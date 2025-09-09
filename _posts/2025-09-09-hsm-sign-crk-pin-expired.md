---
layout: post
title: CKR_PIN_EXPIRED fix
date: 2025-09-09 12:04:02 +0700
categories: build
---

From [6/2023](https://knowledge.digicert.com/general-information/new-private-key-storage-requirement-for-standard-code-signing-certificates-november-2022), every code sign ceritifcate need new storage for private key so I have to build an API to sign executables files. 

This blog is just a quick memo to save the fix of the error 'CKR_PIN_EXPIRED' because I can not find the solutions elsewhere. 

Digicert provides Safenet Token, after provisioning and run for a couple of days (17) I've got that error. I've used jsign to build the API then the error looked like this:

{% highlight java %}
java.security.KeyStoreException: Unable to load the ETOKEN keystore
        at net.jsign.KeyStoreType.getKeystore(KeyStoreType.java:653)
        at net.jsign.KeyStoreBuilder.build(KeyStoreBuilder.java:285)
        at net.jsign.SignerHelper.build(SignerHelper.java:327)
        at net.jsign.SignerHelper.sign(SignerHelper.java:450)
        at net.jsign.SignerHelper.execute(SignerHelper.java:305)
        at net.jsign.JsignCLI.execute(JsignCLI.java:213)
        at net.jsign.JsignCLI.main(JsignCLI.java:57)
Caused by: java.io.IOException: load failed
        at jdk.crypto.cryptoki/sun.security.pkcs11.P11KeyStore.engineLoad(P11KeyStore.java:778)
        at java.base/java.security.KeyStore.load(KeyStore.java:1500)
        at net.jsign.KeyStoreType.getKeystore(KeyStoreType.java:650)
        ... 6 more
Caused by: javax.security.auth.login.LoginException
        at jdk.crypto.cryptoki/sun.security.pkcs11.SunPKCS11.login(SunPKCS11.java:1693)
        at jdk.crypto.cryptoki/sun.security.pkcs11.P11KeyStore.login(P11KeyStore.java:878)
        at jdk.crypto.cryptoki/sun.security.pkcs11.P11KeyStore.engineLoad(P11KeyStore.java:765)
        ... 8 more
Caused by: sun.security.pkcs11.wrapper.PKCS11Exception: CKR_PIN_EXPIRED
        at jdk.crypto.cryptoki/sun.security.pkcs11.wrapper.PKCS11.C_Login(Native Method)
        at jdk.crypto.cryptoki/sun.security.pkcs11.wrapper.PKCS11$SynchronizedPKCS11.C_Login(PKCS11.java:1782)
        at jdk.crypto.cryptoki/sun.security.pkcs11.SunPKCS11.login(SunPKCS11.java:1676)
        ... 10 more
{% endhighlight %}

So the fix is just simple change-pin using pkcs11-tool
{% highlight bash %}
pkcs11-tool --module /usr/lib/libeTPkcs11.so --change-pin
{% endhighlight %}
Update new pin to API then it's work and preparing to change again when it comes.
