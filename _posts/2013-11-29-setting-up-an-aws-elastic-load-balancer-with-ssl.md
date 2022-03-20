---
layout: post
title: Setting up an AWS Elastic Load Balancer with SSL
date: 2013-11-29 15:45:00.000000000 -08:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
To set up SSL with an Amazon Web Services( AWS ) Elastic Load Balancer ( ELB ) follow these simple steps. These were run on a Mac OSX computer. You may need to install openssl if you don't have it already installed.

#### Step 1: Get your certificate

In a console:   
`  
$ openssl genrsa 2048 > private-key.pem  
 $ openssl req -new -key private-key.pem -out csr.pem  
`

The script will ask you a bunch of questions. Most of these fields are simple, but **Common Name** is the exact opposite. Common Name is more simply the url of the webpage. For a single url domain, your common name is www.your-url.com. In my case, that of the unlimited subdomain, it is *.your-url.com.

#### Step 2: Register your certificate

Now its time to get register your csr with a certificate authority. When you request a certificate, it will prompt you for the csr.pem. Submit the csr.pem to namecheap when requesting your Comodo SSL. MAKE SURE TO KEEP all of those files you generated. You will need them later.

You will receive an email at your administrator email to confirm you own the domain. Just a heads up, this email may take up to 45 minutes to be received. It scared me a bunch the first time, but it will come. Once confirmed, you will receive your tickets. You will receive three files:

1.  STAR_URL.crt
2.  PositiveSSLCA2.crt
3.  AddTrustExternalCARoot.crt

#### Step 3: Save to the ELB

For the ELB, you will need to convert the crt file to a pem file. Back in the console, run the following commands:

`  
$ openssl x509 -in STAR_URL.crt -out url.der -outform DER  
 $ openssl x509 -in url.der -inform DER -out url.pem -outform PEM  
`

Don't ask me why its a two step conversion, just go with the flow. Trust me on this one. I tried many different ways, and this is the only one that worked!

Now we have the public and private keys, we just need to make the certificate chain. Its as easy as:

`  
$ cat PositiveSSLCA2.crt AddTrustExternalCARoot.crt > url.ca-bundle  
`

Now, when you create your ELB it will ask you for your public key, private key, and certificate chain.

*   Public Key: url.pem
*   Private Key: private-key.pem
*   Certificate Bundle: url.ca-bundle

And with that, we have properly configured an Elastic Load Balancer for SSL. It was a nightmare for me to set up. Hope it was a bit easier for you!

Hope it helps.
