---
layout: post
title: "Kubernetes - Part 2 - Tips and Tricks "
excerpt: "I'm still being buried alive by webpages"
categories: blog
date: 2017-11-24T02:00:00-06:00
---

<center>
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/zachd1_618/14339201176/in/photolist-nR79Wu-5ytW11-7fATyR-7fEMHh-92xZLG-8Pojj4-7fENfG-GboRz-7fEKM7-5ytVGU-5ytNQs-5ytTjS-7fENbC-5ytLDL-anb1R8-5ytYsW-r5k9qp-5ypAbV-pKsW87-4Y248s-7fARDx-33bUb5-5ytQkq-qNtFic-5jHBoS-oDtuLk-G8f56p-tLwgdv-5WxZim-5UXTEf-dYoG8k-b4UQgB-R4nWWq-pvXaxU-7AxJVH-76RUue-nBkhtu-4XWNs4-3pcVW-5ytPkL-7chkZE-5ytXzE-5MRjeb-5ynLGt-jqpEnT-7Bn8Bj-5ytRqb-eQVokk-5ytNaE-6iefHx" title="Wheelie Portrait"><img src="https://farm3.staticflickr.com/2911/14339201176_f6332ecaef.jpg" width="500" height="333" alt="Wheelie Portrait"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>
</center>

Last week, I gave a brief introduction to Kubernetes.  This week, I'm going to cover some tips and tricks I've learned from poking around with Kubernetes over the past few weeks.  I'll cover a few major categories -- random, kubectl, minikube, Charts (Helm), and References.

I'm not going to explain basic usage, because there are literally thousands upon thousands of pages that will help you create your very own "hello-world" kubernetes cluster.

<font color="cyan" size="+3"><b>Random</b></font>

##### Tip #1 - Autocomplete
All the requisite applications have autocompletion, but they don't work for me in the standard way that they recommend.  Instead, I had to put it in my .bash_profile or in the special location on a Mac that will load itself.  Both options included below:

```bash
##this way didn't work for me
~: source <(kubectl completion bash)
##this way did
~: kubectl completion bash > .kubectl
##do one or the other below
~: echo "source ~/.kubectl" >> .bash_profile
~: mv .kubectl /usr/local/etc/bash_completion.d/kubectl
```

##### Tip #2 - YAML Validation
You can validate all your YAML files prior to loading them into kubectl with python
```bash
$ python -c 'import yaml,sys;yaml.safe_load(sys.stdin)' < \
test-application.deployment.yaml
```

<font color="cyan" size="+3"><b>kubectl</b></font>

##### Tip #3 - Context
kubectl allows multiple contexts.  Unless you are only planning on running your cluster in minikube, you're going to have to switch contexts.  Here's how you change contexts:
```bash
$ kubectl config get-contexts
CURRENT   NAME            CLUSTER         AUTHINFO        NAMESPACE
          dev.k8s.local   dev.k8s.local   dev.k8s.local
*         minikube        minikube        minikube
$ kubectl config use-context dev.k8s.local
$ kubectl config get-contexts
CURRENT   NAME            CLUSTER         AUTHINFO        NAMESPACE
*         dev.k8s.local   dev.k8s.local   dev.k8s.local
          minikube        minikube        minikube
##This doesn't set a context, it creates a context.  
##It can however, overwrite contexts you already have.  
$ kubectl config set-context dev.k8s.local
```

##### Tip #4 - Validation
Validate your kubectl configuration before implementation by using a --dry-run feature
```bash
$ kubectl create -f test-application.deploy.yaml --dry-run --validate=true
```

##### Tip #5 - Node Assignment
You can easily figure out what node your pod is running on (or other cool information for other commands).
```bash
$ kubectl get pods -o wide
nginx-deployment-431080787-dsl0h                        1/1       Running   0          21d       100.109.0.6     ip-10-10-0-209.ec2.internal
nginx-deployment-431080787-j86tx                        1/1       Running   0          20d       100.109.0.10    ip-10-10-0-209.ec2.internal
nginx-deployment-431080787-j86tx                        1/1       Running   0          20d       100.109.0.10    ip-10-10-0-14.ec2.internal
```

<font color="cyan" size="+3"><b>minikube</b></font>

##### Tip #6 - Addons
Minikube is great, but documentation is seriously lacking with their addons.  They are fabulous and they can help you with many things -- including downloading from a private repository (registry-creds).  Kube-dns and ingress (local nginx ingress controller) are also useful to have.
```bash
$ minikube addons list
- heapster: disabled
- registry-creds: disabled
- dashboard: enabled
- default-storageclass: enabled
- coredns: disabled
- kube-dns: enabled
- ingress: disabled
- registry: disabled
- addon-manager: enabled
$ minikube addons enable registry-creds
$ minikube addons configure registry-creds
##has to be restarted to take effect
$ minkube stop && minikube start 
```

###### Tip #7 - Sharing the Docker Daemon
This has been covered in a few places, but you can speed up your minikube experience by sharing the docker daemon across docker and minikube.  This way if you build a local docker image, it will be available via minikube as well, and running docker ps will show your kubernetes pods also!  <font color="green">Profit.</font>
```bash
$ eval $(minikube docker-env)
$ docker ps
```

<font color="cyan" size="+3"><b>Charts\Helm</b></font>

I started by just creating random files for my kubernetes deployment, but quickly moved over to Helm.  Helm is great because it helps organize your files and you can test all your deployments before you do things.  It can be a bit tricky to debug, but there are great references out there that help you.  Some basic commands:
```bash
$ brew install kubernetes-helm
$ helm init
##verify directory for helm is clean:
$ helm lint test-app
## helm will install to whatever your current context is
$ helm install --debug --name test-app
##if you don't purge, you can't re-use the name
helm delete --purge test-app
```

##### Tip #8 - Files in ConfigMaps
One of my favorite things that I've found in helm so far is I can load files in ConfigMaps.  You can do this in kubectl through the command line, but it's so nice to be able to have it in a file!  See the information <a href="https://github.com/kubernetes/helm/blob/master/docs/chart_template_guide/accessing_files.md">here</a>!

```bash
##here's the basic structure of my helm chart
test-app$ ls
Chart.yaml       NOTES.txt        charts           configs
 templates        values.yaml
##create the volume mount in the deployment first
test-app$ grep -r -A 3 volume templates/raw-s3-loader-deployment.yaml
templates/raw-s3-loader-deployment.yaml:          volumeMounts:
templates/raw-s3-loader-deployment.yaml-            - mountPath: /snowplow/config
templates/raw-s3-loader-deployment.yaml-              name: s3-config
--
templates/raw-s3-loader-deployment.yaml:      volumes:
templates/raw-s3-loader-deployment.yaml-        - name: s3-config
templates/raw-s3-loader-deployment.yaml-          configMap:
templates/raw-s3-loader-deployment.yaml-            name: {{ template "snowplow.rawS3.fullname" . }}
## in the config map, point to the file
test-app$ grep -A 3 data templates/raw-s3-loader-configmap.yaml
data:
  rawS3loader.conf: |
## It doesn't show with two so here's the first one: {
{ .Files.Get .Values.rawS3.rawS3conf || indent 4 }}

## Here's where I have the value that points to the actual file
## in my helm chart directory
test-app$ grep rawS3conf values.yaml
  rawS3conf: configs/raw-s3-loader.staging.conf
```

<font color="cyan" size="+3"><b>References</b></font>

Here are some links to things I've spent some time clicking around for.  Mostly these are tools to help make your Kubernetes deploy just a little bit easier.  They are also mostly for use with AWS.

-#1-  Working with IAM permissions for Kubernetes pods in different namespaces: <a href="https://github.com/jtblin/kube2iam">kube2iam</a>

-#2-  Horizontal autoscaling within Kubernetes, now a part of the project itself: <a href="https://github.com/kubernetes/autoscaler">autoscaler</a>

-#3-  Storing registry creds to private (AWS, GCE) engine since tokens expire every 12 hours.  This is an addon in minikube: <a href="https://github.com/upmc-enterprises/registry-creds">registry-creds</a>

-#4-  Configuring getting your logs leveraging fluentd: <a href="https://blog.codersociety.com/setting-up-logging-within-kubernetes-on-aws-9840c72208c7">Setting up Logging Within Kubernetes on AWS</a>

-#5-  Prometheus Chart for your Kubernetes Cluster: <a href="https://github.com/kubernetes/charts/tree/master/stable/prometheus">Charts - Prometheus</a>

-#6-  Ingress Controllers that aren't NGINX on AWS: <a href="https://github.com/coreos/alb-ingress-controller">alb-ingress-controller</a>,<a href="https://github.com/zalando-incubator/kube-ingress-aws-controller">kube-ingress-aws-controller</a>

-#7-  Different ways you can configure external access to your kubernetes pods (this is one of my favorites): <a href="http://alesnosek.com/blog/2017/02/14/accessing-kubernetes-pods-from-outside-of-the-cluster/">Accessing pods from outside of the cluster</a>