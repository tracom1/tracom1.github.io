---
layout: post
title: "Vault and Consul"
excerpt: "Setting up a HA non-container setup"
categories: blog
date: 2017-07-13T05:00:00-06:00
---

<center><figure>
<img src="/images/vault.png">
</figure></center>
I spent last week configuring and setting up Vault HA, with a Consul backend.  It took me a bit longer to configure than a normal install because 1) we wanted it HA 2) we didn't want to run it in containers.

For a brief background, Consul is a tool by Hashicorp and can be used for a few different things, but I'm going to be focusing on two main ones right now.  It can aide in service discovery, or basically the registration of apps to your system.  It also can be used for a key/value configuration storage system.  This can be used for storing such things as app authentication, configuration flags, etc.

When Vault is coupled with a Consul back-end, it helps make your key/value configuration more secure.  Vault's pitch is that it helps store "secrets", whether those are API authentication tokens, username/password key value pairs.  The tool itself requires authentication to be able to access all the secret goodies stored inside.

<center>
	<iframe src="https://giphy.com/embed/xBVH2wE9eUUtq" width="480" height="271" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/excited-big-bang-theory-goody-xBVH2wE9eUUtq">via GIPHY</a></p>
</center>

Management of tokens through Vault can also allow you to create temporary access tokens to vault, either by time or usage, so applications can have only temporary access to those secrets.  For example: a lot of applications need a username/password for some db auth parameters only upon startup, and then it's stored in memory.  By creating limited authentication tokens, you can prevent an application from requesting credentials while running.

Before I could stand up my 3 server instance of Vault, I had to get my back-end up and configured.  I did the first few steps in their <a href="https://www.consul.io/intro/getting-started/install.html">documentation</a>, and played around a little bit with the dev.  Their documentation is pretty good, but not always housed in one location.  I expected getting-started would include everything I needed, but some of the more complex things figured out.  For that, I referred to the DigitalOcean page <a href="https://www.digitalocean.com/community/tutorials/how-to-configure-consul-in-a-production-environment-on-ubuntu-14-04">here</a>.

Here are some of the basic commands I had to do to get my application configured the way I wanted it:
```bash
adduser consul
mkdir -p /etc/consul.d/{bootstrap,server,client}
mkdir /var/consul
chown consul:consul /var/consul
touch /etc/systemd/system/consul.service
```

Then I had to figure out how I wanted to configure my servers to run, i.e., what configuration they should have on startup.  I needed my service to be available externally, rather than bootstrapping to the standard <font color="cyan"><i>127.0.0.1:8500</i></font>, so I needed to advertise it differently.  I also wanted each server to know that it was expecting 2 other servers in it's quorum.  Finally, I wanted a UI to be available. I opted for a standard json configuration versus HCL, but it looked something like this:

```bash
{
    "bootstrap" : false,
    "server" : true,
    "data_dir" : "/var/consul",
    "encrypt" : "secret stuff",
    "log_level" : "INFO",
    "enable_syslog" : true,
    "addresses" : {
        "http" : "0.0.0.0"
    },
    "start_join" : ["10.20.X.X","10.20.X.X"],
    "ui" : true
}
```

Now that I had my configuration set up, I had to set up a systemd script so I could daemonize when it actually ran.  The systemd script was simple, and basically just involved the invocation of the command line.  To keep it separate however, I put that in a different file:

```bash
[root@host ~]# cat /etc/systemd/system/consul.service
[Unit]
Description=Run the consul service as a daemon

[Service]
User=consul
Group=consul
ExecStart=/etc/consul.d/start-consul.sh

[Install]
WantedBy = multi-user.target
[root@host ~]# cat /etc/consul.d/start-consul.sh
#!/bin/sh
consul agent -server -config-dir=/etc/consul.d/server/
[root@host ~]# systemctl daemon-reload
```
And that was it for the server configuration!  I had to repeat the config.json step for my <font color="cyan"><i>/etc/consul.d/bootstrap</i></font>, with slightly different modifications (detailed in the link above).

Once I confirmed I could get an app to start with systemctl, which took some time because I kept forgetting to <font color="cyan"><i>chmod +x /etc/consul.d/start-consul.sh</i></font> on pretty much every server, I was ready to start up my cluster.  I logged onto my first server and started it in bootstrap mode just from the command line <font color="cyan"><i>consul agent -config-dir /etc/consul.d/bootstrap</i></font>.  

Then I logged onto my other two servers and started them normally using my <font color="cyan"><i>systemctl start consul</i></font>.

Finally, I went back to my terminal in the first server, stopped the bootstrap instance, and started that up as I did servers 2 and 3.  BAM!  HA Consul cluster.

The vault configuration I was worried would be more complicated because they don't really explain HOW to run Vault in HA, but it ended up being easier.  Vault doesn't run like all applications do in HA, instead it runs in a active/standby mode, with only one server being active at a time.  I ran through the initial documentation <a href="https://www.vaultproject.io/intro/getting-started/install.html">here</a>, but skipped a bunch of portions about things I didn't care about, like github and aws mounting.  I also don't have mine currently enabled for tls, because this was a POC.

If you configure Vault with a Consul back-end which is already running HA, it will automagically configure Vault the same way.  That wasn't very evident in the documentation, so I referred to a great <a href="https://groups.google.com/forum/#!topic/vault-tool/yewOwOMmKG8">Google group post</a> post that really clarified things for me.

Because I didn't have to do anything special for making Vault HA, the main steps I had to take were: (1)set up my Vault json configuration (2) and my systemd script to daemonize.  I followed a similar process to consul above, just using /etc/vault.d/config.json.  Again, I needed it to be able to advertise beyond localhost.  Vault recommends that you point to a locally running instance of Consul, but I have my nodes configured two separate ways: the first two are configured this way, and the third is configured to point to a consul vip.  Either way works.

```bash
{
    "storage": {
        "consul": {
            "address": "10.20.X.X:8500",
            "advertise_addr": "http://<<host>>:8200",
            "path": "vault"
        }
    },
    "listener": {
        "tcp": {
            "address": "0.0.0.0:8200",
            "tls_disable": 1
        }
    }
}
```

My terminal command in the systemd script is also simple: 
<font color="cyan"><i>vault server -config=/etc/vault.d/config.json</i></font>

Then all you have to do is start your vault servers.  You will need to export your address, <font color="cyan"><i>export VAULT_ADDR=http://0.0.0.0:8200</i></font> before you do anything. Then, you start one and initiate it, which gives you the keys and root token.  <b>Make sure to store them somewhere!</b>  You need to unseal vault every time it starts, and you need to use these same keys across all your Vault instantiations.  Export the address and start vault on your other two servers.  Then you need to go through the arduous task of unsealing them, which takes time.

Once you've completed that though, Vault is up and running in HA!

```bash
[root@host1 ~]# vault status
Sealed: false
Key Shares: 5
Key Threshold: 3
Unseal Progress: 0
Unseal Nonce:
Version: 0.7.3
Cluster Name: vault-cluster-975049e2
Cluster ID: 6efde828-3c74-a994-ed47-32e5d8451f3d

High-Availability Enabled: true
	Mode: active
	Leader: http://host1:8200

[root@host2 ~]# vault status
Sealed: false
Key Shares: 5
Key Threshold: 3
Unseal Progress: 0
Unseal Nonce:
Version: 0.7.3
Cluster Name: vault-cluster-975049e2
Cluster ID: 6efde828-3c74-a994-ed47-32e5d8451f3d

High-Availability Enabled: true
	Mode: standby
	Leader: http://host1:8200
```

Standby nodes, if you're advertising correctly, will automatically forward traffic to your active nodes.

Now you can get to the task of creating and storing secrets, which is <i>REALLY</i> the tricky part!