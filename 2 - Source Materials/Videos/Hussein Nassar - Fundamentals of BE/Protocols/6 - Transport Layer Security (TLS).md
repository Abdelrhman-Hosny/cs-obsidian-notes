
Https is Http over TLS.

---
Https works by sending a symmetric key and using that key to xor the message bits, then anyone with the same key can xor them again to get the original message.

Symmetric encryption ~ O(xor)
Asymmetric encryption ~ O(exponential )

So symmetric is way faster which is why it is used.

The real question is how to the exchange that key between the client and the server without someone getting a hold of it.


---
## TLS

TLS is not only related to http but can be used with any protocol.

It has these steps
1. Exhange a symmetric key using asymmetric key approach
2. Use symmetric key for encryption of messages.


People now only use versions 1.2 or 1.3 as those are the secure versions, versions lower than that shouldn't be used.

---

### TLS 1.2



### TLS 1.3

