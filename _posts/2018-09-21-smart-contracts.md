---
layout: post
title:  Ethereum Blockchain Tutorial - SmartContracts
comments: true
image: https://jonathanmann.github.io/public/img/hashblockpreview.png
excerpt: SmartContracts may be the most revolutionary application blockchain technology has enabled. SmartContracts allow for the creation of trustless verifiable agreements that can be enforced by the network. Because the rules that govern contract execution are written in code rather than human language, no judge or abitrator is required to enforce the contract. Code is law, and, for better or for worse, once a SmartContract is agreed to, it is forever binding.
---

SmartContracts may be the most revolutionary application blockchain technology has enabled. SmartContracts allow for the creation of trustless verifiable agreements that can be enforced by the network. Because the rules that govern contract execution are written in code rather than human language, no judge or abitrator is required to enforce the contract. Code is law, and, for better or for worse, once a SmartContract is agreed to, it is forever binding.

### Introduction
This tutorial walks through the process of creating a SmartContract to store and retrieve hashes in the blockchain using Solidity and Truffle. It assumes that the user has already installed and configured Truffle. Further details on installing Truffle can be found [here](https://truffleframework.com/docs/truffle/getting-started/installation). Although the tutorial does not cover deployment of the SmartContract to the network, it should be a relatively straightforward next step. This tutorial was created as a companion for an in-person workshop, so if there is any missing information please comment below.

### Overview
This tutorial builds an application that can be used for proof of first discovery or composition. An author can hash a digital representation of their work and add the result to the blockchain proving with the certainty of the network that the work was completed by the author by the time of insertion. This hash can later be retrieved as needed to prove the first composition is tied to the author's identity. The source code for this tutorial can be found [here](https://github.com/jonathanmann/hash_factory).

![HASHBLOCK](https://jonathanmann.github.io/public/img/hashblock.png)

### Setup
Truffle is a prerequisite for this tutorial, if npm is already installed, setup is easy:
{% highlight sh %}
    $ npm install -g truffle 
{% endhighlight %}
Now that the prerequisites are out of the way, create a directory called "hash_factory".
{% highlight sh %}
    $ mkdir hash_factory
{% endhighlight %}
Next, let's use truffle to setup some scaffolding for our application:
{% highlight sh %}
    $ truffle unbox react
{% endhighlight %}
We'll make use of react in a later tutorial, but, for now, let's take a look at our contracts.
{% highlight sh %}
    $ cd hash_factory/contracts/
    $ ls
{% endhighlight %}
There should be two Solidity files. Use the following commands to remove the SimpleStorage contract and add a  HashFactory contract.
{% highlight sh %}
    $ rm SimpleStorage.sol
    $ touch HashFactory.sol
{% endhighlight %}

Before we go any futher, let's update the deploy script to point to our new contract. Replace "migrations/2_deploy_contracts.js" with [this code](https://github.com/jonathanmann/hash_factory/blob/master/migrations/2_deploy_contracts.js).

Now we're ready to start working on our SmartContract.

### Contract Coding
The source code for this tutorial is available [here](https://github.com/jonathanmann/hash_factory). Begin by editing the HashFactory contract:

{% highlight sh %}
    $ vim HashFactory.sol
{% endhighlight %}

#### Structure
In the first line, add a pragma to require that the contract be run using version 0.4.18 or higher. Next, declare a contract called "HashFactory" containing a NewHash event that takes an unsigned integer for an insertion id and a string for a hash. Create a public list of type string called "hashes" to store the hashes. Finally declare a public mapping called "hashToAuthor" that can be used to map the insertion id to the author's address.
{% highlight js %}
    pragma solidity ^0.4.18;

    contract HashFactory {
        event NewHash(uint id, string hash);
        string[] public hashes;
        mapping (uint => address) public hashToAuthor;

        // Add functions below
        .
        .
        .
    }
{% endhighlight %}

#### Functions
Now that the basic declarations are out of the way, create the funtions to intereact with the contract. Within the HashFactory contract under the hashToAuthor declaration, create a public createHash function that takes a hash as input. Prepend underscores to function arguments to differentiate them from global variables.
{% highlight js %}
        .
        .
        .
        // Add functions below

        function createHash(string _hash) public {
            uint id = hashes.push(_hash) - 1;
            hashToAuthor[id] = msg.sender; 
            emit NewHash(id,_hash);
        }
        .
        .
        .
    }
{% endhighlight %}

Now we can add hashes to the Blockchain! Next make a "getHash" function to retrieve stored hashes and a "getHashAuthor" function to lookup the hash's author.
{% highlight js %}
        .
        .
        .
        function getHash(uint _hashId) public view returns (string) {
            return hashes[_hashId];
        }

        function getHashAuthor(uint _hashId) public view returns (address) {
            return hashToAuthor[_hashId];
        }
    }
{% endhighlight %}

### Contract Interaction
Now that the smart contract is ready, open the truffle console:
{% highlight sh %}
    $ truffle dev
{% endhighlight %}

Deploy the contract with the following commands:
{% highlight sh %}
    truffle(develop)> migrate --reset
    truffle(develop)> compile
{% endhighlight %}

Something like this will appear:
{% highlight sh %}
    Running migration: 2_deploy_contracts.js
      HashFactory: 0x82d50ad3c1091866e258f...
    Saving successful migration to network...
      ... 0x68410036257ab41a3a6be1cd61faa...
    Saving artifacts...
{% endhighlight %}

Grab the address that appears after HashFactory and assign it to an address variable "a". Then create a contract vaiable "c" and assign it the HashFactory smartcontract from the address stored in "a".
{% highlight sh %}
    a = '0x82d50ad3c1091866e258fd0f1a7cc9674609d254'
    c = HashFactory.at(a)
{% endhighlight %}

In a separate terminal, create a hash of something to store in the blockchain.
{% highlight sh %}
    $ echo -n "hello, world" | md5sum
    e4d7f1b4ed2e42d15898f4b27b019da4  -
{% endhighlight %}

Use the "createHash" function to store the result in the blockchain.
{% highlight sh %}
    truffle(develop)> c.createHash('e4d7f1b4ed2e42d15898f4b27b019da4')
{% endhighlight %}

Since our storage is zero-indexed and this is the first insertion, this hash can be accessed using the following command:
{% highlight sh %}
    truffle(develop)> c.getHash(0)
    'e4d7f1b4ed2e42d15898f4b27b019da4'
{% endhighlight %}

The author can also be looked up using the index.
{% highlight sh %}
    truffle(develop)> c.getHashAuthor(0)
    '0x627306090abab3a6e1400e9345bc60c78a8bef57'
{% endhighlight %}

### Conclusion
This contract can be used to hash a digital representation of work, add the result to the blockchain, and then retrieve the hash as needed to prove the composition is tied to the author's identity. The source code for this tutorial can be found [here](https://github.com/jonathanmann/hash_factory).
