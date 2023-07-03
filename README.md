# Doxtream E-Sign note


Difference between a digital signature and a digitized signature

You're mixing something up here: A digital signature is an asymmetric cryptographic algorithm and has nothing to with writing down your name, neither old-school pen-and-paper style nor on a any digital device. It is called digital signature because it can be used with the same purpose: Make sure that data

authentic (i.e. that when the letter says 'Yours, mom', it was actually your mom who wrote it)
original (i.e. that nobody modified your mom's letter's content after your mom wrote it)
Now, what the actual hand-written signature is in analog real-life is the private key in the digital signature world. This definition of digital signature is what for example this US government thing relates to.

The second thing you're talking about is digitizing the handwritten signature. A digitized handwritten signature is not a digital signature.

Anyway, there are lots of reasons to mix these completely different concepts. A digital signature algorithm can be used to make sure that a digitally recorded handwritten signature is authentic and original, for example. This is what the tutorial you've mentioned is about.

That being said, the main question is: What are your requirements? Especially regarding security.

Solution principle

I would start from here: The main goal is to bind a digitized signature to your dataset in a way that the dataset cannot be altered afterwards. As a concept, the following algorithm would achieve this goal:

Given the data, and the digitized signature in whatever format (InkSecureSignature is fine here, the important thing is that the signature needs to be present in a format that prohibits misuse. This means, the quality of the data needs to be "sufficiently bad"):

Define a class which holds the data and the signature an serialize it.

Create a random public and private key pair (APub, APriv).

Use a digital signature algorithm to sign the data (enrypted or original)

Destroy key APriv.

Save the data together with key APub.

Now, with key APub you will be able to check that data and digitized signature have been packed together and have not altered since.

That's not it, of course

The solution above is highly insecure. The worst part is, that, although a record, once stored in the database, cannot be altered, it could be replaced and nobody would notice. To avoid this, you need a certificate, more or less the same as a pair of a private and a public key (I hope I don't get slaughtered for this) which is the only instance with the right to sign data. Security of your system then melts down to making sure your certificate doesn't get in the wrong people's hands. You could consider a webservice which requests signing the data on a central server. You need to secure the communication channel, again with encryption and digital signing. Each device would have its own certificate. That way you could track everything on the server: Which device requested what signature at what time with what certificate. This would probably come close to a system that would be accepted by legal authorities (Don't nail me down to this).

If your software could be hacked

Secondly, one could use a digitized signature which is already present in the database, to sign new data. There is hardly any secure solution, as long as you have to consider somebody to manipulate your software on code level. Alternatively you could make sure, nobody has access rights to the data in the database. I.e. you could encrypt everything and make sure only the CEO has the certificate to encrypt the data ;).

If you cannot be sure that your software will not be hacked, there is only one way to make the system secure: No digitized signature at all, only digital signature. Every customer gets his own digital certificate and is responsible for it. The signature needs to be performed digitally with this certificate. Of course, lots of possibilities for abuse, but you've delegated the risk to the customer.

Conclusion

No system is ever totally secure and it is most important to get the requirements straight. Otherwise it gets expensive quickly. There are always many options for abuse, make sure that your certificates cannot be accessed by people who shouldn't be allowed to.

