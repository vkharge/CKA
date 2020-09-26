# TLS Basics
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808254)
  
In this section, we will take a look at TLS Basics

## Certificate
- A certificate is used to gurantee trust between 2 parties during a transaction.
- Example: when a user tries to access web server, tls certificates ensure that the communication between them is encrypted and the server is who it says it is.

  ![cert1](../../images/cert1.PNG)

- Let’s take a look at a scenario. Without secure connectivity, If a user were to access his online banking application the credentials he types in would be sent in
 a plain text format. The hacker sniffing network traffic could easily retrieve the credentials and use it to hack into the user's bank account. Well that's obviously not safe.
  
## Symmetric Encryption

- So you must encrypt the data being transferred using encryption keys, the data is encrypted using a key which is basically a set of random numbers and alphabets you add the random number to your data and you encrypted into a format that cannot be recognized, the data is then sent to the server.

- The hacker sniffing the network gets the data but can't do anything with it. However the same is the case with the server receiving the data it cannot decrypt that data without the key. So a copy of the key must also be sent to the server so that the server can decrypt and read the message

- since the key is also sent over the same network. The attacker can sniff that as well and decrypt that data with it. This is known as SYMMETRIC ENCRYPTION. 

- It is a secure way of encryption, but it uses the same key to encrypt and decrypt the data and the key has to be exchanged between the sender and the receiver, there is a risk of a hacker gaining access to the key and decrypting the data.

- It is a secure way of encryption but since it uses the same key to encrypt and decrypt the data and since the key has to be exchanged between the sender and the receiver there is a risk of a hacker gaining access to the key and decrypting the data 

  ![cert2](../../images/cert2.PNG)
  
## Asymmetric Encryption

- Instead of using a single key to encrypt and decrypt data asymmetric encryption uses a pair of keys, a private key and a public key well they are private and public keys. But for the sake of this example we will call it a private key and a public lock we will get back to that at the end.

- The trick here is if you encrypt or lock the data with your lock you can only open it with the associated key. So your key must always be secure with you and not be shared with anyone else. It's private.

- let’s look at an even simpler use case of securing SSH access to servers through key pairs. You have a server in your environment that you need access to. You don't want to use passwords as they're too risky. So you decide to use key pairs you generate a public and private key pair.
  ```
  ssh-keygen 
  id_rsa     id_rsa.pub
  ```

- Then you add your public keys in the server .ssh authorized_keys file.  Now when you ssh the server, you pass the private key 
  ```
  ssh -i id_ras user1@server1
  ```
  
 - with symmetric encryption was that the key used to encrypt data had to be sent to the server over the network along with the encrypted data. And so there is a risk of the hacker getting the key to decrypt the data.
 
 - To securely transfer the key from the client to the server, we use Asymmetric Encryption.
 
 - So, we generate a public and private key pair on the server.
 ```
 openssl genrsa -out my-bank.key 1024
 my-bank.key
 
 
 openssl rsa -in -out  my-bank.key -puout > mybank.pem
 my-bank.key  mybank.pem
 ```

- Instead of using single key to encrpyt and decrypt data, asymmetric encryption uses a pair of keys, a private key and a public key.

  ![cert3](../../images/cert3.PNG)
  
  ![cert4](../../images/cert4.PNG)
  
  ![cert5](../../images/cert5.PNG)
  
  ![cert6](../../images/cert6.PNG)
  

## With asymmetric encryption, We have successfully transferred the symmetric keys from the user to the server
  
  - User/Browser has a private and public key
  - Browser also has a public key of the website/server and server private is on the server itself

  - How the symetric keys are successfully transfered?
    - Browser encrypts the browser public key with the public key of the server
    - It sends the data to the server 
    - Server receives the data, to decrypt the data, server uses its private key and get the public key of the user/browser
    -  With asymmetric encryption, We have successfully transferred the symmetric keys from the user to the server

#### How do you look at a certificate and verify if it is legit?
- who signed and issued the certificate.
- If you generate the certificate then you will have it sign it by yourself, that is known as self-signed certificate.
- All of the web browsers are built in with a Certificate validation mechanism, wherein the browser checks the certificate received from the server and validates it to make sure it is legitimate if it identifies it to be a fake certificate then it actually warns you.

  ![cert7](../../images/cert7.PNG)
  
#### How do you generate legitimate certificate? How do you get your certificates singed by someone with authority?
- That's where **`Certificate Authority (CA)`** comes in for you. some of the popular ones are symantic, DigiCert, Comodo, GlobalSign etc.

- The CA is themselves have a set of public and private key pairs. The CA is use their private keys to sign the certificates the public keys of all the CAs are built in to the browsers. The browser uses the public key of the CA to validate that the certificate was actually signed by the CA themselves.

  ![cert8](../../images/cert8.PNG)
  
  ![cert9](../../images/cert9.PNG)
  
  ![cert10](../../images/cert10.PNG)
  
## Public Key Infrastructure
 - For this the server first sends a certificate signing request to a CA.

 - The CA uses its private key to sign the CSR.

 - Remember all users have a copy of the CAs public key.

 - The signed certificate is then sent back to the server the server configure is the web application with the signed certificate.

- Whenever a user accesses the web application the server first sends the certificate with its public key.

- The user or rather the user's browser reads the certificate and uses the CA's public key to validate and retrieve the servers Public key

- It then generates a symmetric key that it wishes to use going forward for all communication.

- The symmetric key is encrypted using the server as public key and sent back to the server

- The server uses its private key to decrypt the message and retrieve the symmetric key.   
   
   ![pki](../../images/pki.PNG)
   
- administrator generates a key pair for securing SSH

- the web server generates a key pair for securing the web site with HTTPS

- the Certificate Authority generates its own set of key pair to sign certificates.

- The end user though only generates a single symmetric key.

- Once he establishes trust with the Web site he uses his username and password to authenticate the Web server with the servers key pairs.

- The client was able to validate that the server is who they say they are but the server does not for sure know if the client is who they say they are.

- So what can the server do to validate that the client is who they say they are for this as part of the initial trust building exercise.

- The server can request a certificate from the client and so the client must generate a pair of keys and a signed certificate from a valid CA, the client then sends the certificate to the server for it to verify that the client is who they say they are.

- Now you must be thinking you have never generated a client's certificate to access a Web site.? Well that's because TLS client certificates are not generally implemented on web servers

- the process of generating, distributing and maintaining digital certificates is known as public key infrastructure or PKI 
   
## Certificates naming convention

  ![cert11](../../images/cert11.PNG)
  
  

  
   

  
  
  

  
  
  
  
  
  

  
  
