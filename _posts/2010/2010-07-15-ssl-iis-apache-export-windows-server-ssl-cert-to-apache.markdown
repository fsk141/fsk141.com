---
date: '2010-07-15 09:59:08'
layout: post
slug: ssl-iis-apache-export-windows-server-ssl-cert-to-apache
status: publish
title: '[Tutorial] SSL IIS > Apache (Export Windows Server SSL Cert to Apache)'
wordpress_id: '287'
categories:
- Tutorials
---

Export Keys from a windows server to apache server:




  

1) Startup mmc  

a File > Add/Remove Snap in > Certificates:  
a1 Computer Account > Local Computer (Finish with "OK"  

b Select Certificates >> Personal >> Certificates  

c Select Certificate (\*.domainname.com)  

b1 Right click > All Tasks > Export  
b2 "Yes, Export the private Key"  
b3 next (use defaults \[PKCS #12 no boxes checked})  
b4 password (enter anything) << you are setting this password  
b5 Name your export && Finish

2) Extract the Key & Certificate from the pkcs file & enjoy  

a Export the Private key:  

{% highlight bash %}openssl pkcs12 -in filename.pfx -nocerts -out key.pem{% endhighlight %}  

b Export the certificate file:  

{% highlight bash %}openssl pkcs12 -in filename.pfx -clcerts -nokeys -out cert.pem{% endhighlight %}  

c Remove the passphrase from the key so apache won't prompt for passphrase:  

{% highlight bash %}openssl rsa -in key.pem -out server.key{% endhighlight %}
