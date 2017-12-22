---
layout: post
title: "Another Blockchain Post"
excerpt: "Are you tired of hearing about it yet? Here's more."
categories: blog
date: 2017-12-22T02:00:00-06:00
---

<center><figure>
<img src="https://cdn.pixabay.com/photo/2016/11/10/05/09/bitcoin-1813503_1280.jpg">
<figcaption><a href="https://cdn.pixabay.com/photo/2016/11/10/05/09/bitcoin-1813503_1280.jpg">Source: Pixabay</a></figcaption>
</figure></center>

It's been in the news a lot lately, so I feel like I would be remiss if I didn't address the idential Blockchain elephant in all our rooms.  I'll hop on the bandwagon, and tell you even more about the cryptocurrency craze, probably most of which you already know.

I will not try to address the finances behind it, and I certainly will endorse neither purchasing nor abstention from purchasing cryptocurrency.  You can read the financial implications for yourself. If you want to invest, that's really your personal decision -- and I won't try to dissuade you.  I will however tell you that right now, the mania is INSANE, and I recommend not getting caught up in the fever or soon you'll be hanging people as witches.

I will tell you: <a href="https://www.vice.com/en_us/article/d3xywq/buy-bitcoin-with-debt-insane?utm_source=vicetwitterus">don't go into debt to buy bitcoin</a>, that mining for cryptocurrencies <a href="http://www.businessinsider.com/bitcoin-is-ruining-the-planet-2017-12?r=US&IR=T">requires huge amount of resources detrimental to our environment</a>, that the price is so volatile, <a href="https://arstechnica.com/tech-policy/2017/12/bitcoin-fees-rising-high/">transaction fees are insane</a> and Goldman requires an <a href="https://www.bloomberg.com/news/articles/2017-12-14/goldman-said-to-seek-100-margin-on-some-bitcoin-futures-trades">100% margin on futures trading</a>, that by simply <a href="https://techcrunch.com/2017/12/21/long-island-iced-tea-shares-went-gangbusters-after-changing-its-name-to-long-blockchain/">changing the name of something you can apparently make a fortune right now</a>, and finally that <a href="https://steamcommunity.com/games/593110/announcements/detail/1464096684955433613">Steam no longer accepts Bitcoin</a>.

Let's talk about the fundamentals of what Bitcoin, LiteCoin, Ethereum, <a href="https://www.cryptokitties.co/">Crypto Kitties</a>, etc etc (there were over 40 in August, so there are probably thousands now) do.

If you are interested in learning more, I recommend you read the resources I've put at the end of this article, as I am by no means an expert.

Cryptocurrencies leverage a technology called Blockchain[^1] to do what it does.  Blockchain technology has been around for a while -- because Bitcoin has been around for a while.  Bitcoin was initially introduced in 2009 using this technology.  And my own previous company, Overstock, has spent several years working on their Blockchain platform, <a href="https://www.tzero.com/">t0</a>.

Blockchain is called a <a href="https://www.investopedia.com/terms/d/distributed-ledgers.asp">'distributed ledger technology'</a>.  There are different types of blockchain, public, federated, and private.[^2]  The whole point behind the blockchain, and other ledger technologies are this: instead of relying on a central repository for all your information, copies of it are laid out all over the world.[^3]

Think of it this way.  Imagine your Grandma had some special holiday recipe.  She was the gatekeeper of this recipe, so if you wanted to get it, you had to go ask Grandma directly, she had to agree, and she had to provide it to you, whenever she got around to it.

<center><figure>
<img src="/images/recipe.jpg">
</figure></center>

Now, imagine instead her recipe is stored on the Blockchain.  Your entire family has the recipe in their recipe book.  Now, when you ask Grandma for her recipe, if she agrees -- you can get it from your Aunt, your Father, your Cousin with the mullet.  You even have direct access to it yourself!  You don't have to wait however long it may take her to get around to writing it down, handing it to you.

<center><figure>
<img src="/images/recipe_2.jpg">
</figure></center>

Each node involved gets a copy of the blockchain, and is updated as a node updates their portion of the blockchain.  In some ways, it's similar to the concept of Google Docs.  If you've ever used Google Documents, you know that you can have multiple people working and editing the same file at the same time, and the changes get updated as you are working on both sides.

Using something like the blockchain requires that there are enough people in the conversation for sharing of the data -- meaning there must be more than 2.[^4]  There always has to be a third external party present for the integrity of the data.  These third parties have to provide quorum, or enough external support to validate the transaction is happening.  In our recipe example, they would have to validate the authorization Grandma made in sharing the recipe on their personal copy, before you would have access to it.

All these transactions are stored in an encrypted manner on your node.  They are sealed (mined) with a unique key created -- a hash.  Basically, once data is announced (like Grandma's Recipe), everybody listening writes it down in their copy of the recipe book, and does a math problem to encrypt it.[^5]  This hash ensures the data doesn't change once it's created, because it's tied both to the specific data that exists when that block was added, and also to all the blocks already in existence on the chain.[^6]

Let's go back to the recipe example.  Let's say Grandma's recipe for Macaroni and Cheese is the first one added to our blockchain, and it gets sealed and a hash is created for it.  Then, your great Aunt Ida's Beef Bourguignon recipe gets added as the second part of the blockchain.  It too gets sealed and has a hash created for it.  Then Uncle Joey's recipe for Chocolate Chip Cookies gets added, and sealed.  These are all sealed with a certain number of leading zeros (called a nonce).

So we have a recipe blockchain with 3 items, Mac 'n Cheese → Beef → Cookies.  

Now say your mullet cousin wants to try and Change Grandma's Mac 'n Cheese Recipe. This is what happens:

```
A. The recipe, or data, in Mac N' Cheese has changed.

B. The Mac N' Cheese hash changes because the data used to calculate the hash is 
different.  The Mac N' Cheese recipe is now invalid because its hash no longer 
has the correct number of leading zeros.

C. The hash changes for the Beef recipe because the hash from Mac N' Cheese was
used to calculate its hash.  The Beef recipe becomes invalid because its hash
no longer has the right number of leading zeros.

D. The Cookie hash changes because the has from the Beef recipe was used to 
calculate it.  So now, the Cookie recipe becomes invalid because its hash no 
longer meets the requirements.
```

So in order to make a change to one value, the entire chain after it would have to be recalculated.  This means that Grandma's Recipe, once added to the Blockchain, is immutable.  It will live on forever as it did in that moment of creation.

The beauty of this sort of technology is threefold.  First, if a node goes down, you still have other copies available, so things don't fail.  It's a high availability model.  Second, all the data is transparent.  Since everybody has access to the data, you can all view the entire thing. Third, the data is not easily corruptible.  Once things are added to the chain, it is very difficult, and require INCREDIBLE amounts of energy to modify.

This technology can be used for many things beyond currency, and there are many companies looking at how they can leverage this de-centralized data.  In fact, <a href="https://blockgeeks.com">blockgeeks.com</a> has a long list of what blockchain can be used for in the world, and how it will <a href="https://www.youtube.com/watch?v=RplnSVTzvnU">transform the economy</a>.  (Here's a hint, it's basically everything in the world).

So that's it from me, Tracy Blockchain Cryptocurrency.  Tune in next week, when I attempt to change my name to reflect my inner tech spirit.

<font color="cyan"><u>Resources</u></font>
[^1]: <a href="https://www.investopedia.com/terms/b/blockchain.asp"> Investopedia: Blockchain</a>
[^2]:<a href="https://blockchainhub.net/blockchains-and-distributed-ledger-technologies-in-general/">Blockchains and Distributed Ledger Technologies</a>
[^3]:<a href="https://blockgeeks.com/guides/what-is-blockchain-technology/">What is Blockchain Technology</a>
[^4]: <a href="https://hackernoon.com/wtf-is-the-blockchain-1da89ba19348">WTF is the Blockchain</a>
[^5]: <a href="https://hackernoon.com/your-company-will-use-blockchain-in-less-than-10-years-heres-how-6d9da452fa8d">Every Company Will Use Blockchain by 2027</a>
[^6]: <a href="https://medium.freecodecamp.org/how-does-blockchain-really-work-i-built-an-app-to-show-you-6b70cd4caf7d">How Does Blockchain Really Work? I Built an App to Show You</a>
