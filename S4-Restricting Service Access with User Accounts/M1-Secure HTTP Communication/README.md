This module contains two videos:

1. Man in the Middle Attacks & Public Key Infrastructure (https://www.youtube.com/watch?v=J9CgCsZrok0&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=51)

- Let's say we have a server to which a client is talking to over HTTP, all the data comprised in HTTP is textual information, this is highly unsafe, as almost any attacker can intercept the information from the request. This attacker can be someone in the network, or maybe anyone on the Internet.
- To stop this from happening, HTTPS (secure version of HTTP) encrypts all the plain text that was being transferred over requests. It is based on a cryptography known as Public Key Encryption.
- When a client wants to communicate with the server, the server provides it with a encrypting key, which the client uses to encypt its text data and transfers all the requests encypted in this key. 
- Man in the middle attack occurs when someone in the network changes the encryption key from the original key to attacker's key, and thus client would not be able to process the response. How to make sure key that encryption key used to send the response is same as the one assigned to the client?
- Public Key Cryptography - Having data that can be encrypted with one key, that can only be decrypted with only one other specific key. These maybe known as public and private keys. Only a private key can decrypt a text encrypted in public key and vice versa.

2. HTTPS (https://www.youtube.com/watch?v=4ttFCW0rBzM&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=51)

- We agree on a authority (CA) that contains a private key that we trust, and let the client and server contain the corresponding public key, the server send the public key to the CA, looking for validation of its public key and its domain. CA encrypts both of that data in private key (into a box) and sends it back to the server.
- Anytime, the server needs to give its public key to the client, to check validity. The server sends the box sent by CA to the client. Now the client can guarantee and check that the box provided is correct one as it can unlock the box using its public key. Now the client knows that it can use that public key to communicate with the server, that noone else can intercept.
- Now that the connection with one client has been started, the session starts. This process is done for all other clients.
- If the attacker tries to change the box, the client would not be able to decode the box, and hence would detect malicious activity.
- Encrypted conversation between a client and server can be referred to as a session.
- When to use HTTPS rather than HTTP? When we want to freely share data with everyone, anyone can write or read, this can be achieved by HTTP. Whenever there is a single person that we need to restrict access from, any client that shouldn't read or write all of the data, we should use HTTPS.

