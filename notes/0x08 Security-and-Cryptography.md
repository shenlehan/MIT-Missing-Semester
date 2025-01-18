## 1. Entropy

Entropy is a concept used to measure or quantify the randomness and security of a subject.

The unit is $\text{bit}$.

$$\text{Entropy}=\log_2(The\ total\ number\ of\ all\ possible\ solutions)$$

### 2. Hash functions

Hash function takes in a random-length string and maps it to a fixed size output.

For instance, the `SHA-1` hash function used in git.

* non-invertible
* collision-resistant

### 3. Key derivation functions

* Symmetric key cryptography
	* `keygen()`: generates a key
	* `encrypt(plaintext,key)`: receive plain text and key and produce the ciphertext
	* `decrypt(cyphertext,key)`: produce the plain text
![[Photos/Pasted image 20250118161732.png]]

* Salt value: a randomized value added to the encrypted hash value, in defense to the "rainbow table"

### 4. Asymmetric key cryptography

* Only one difference to the symmetric cryptography: The `keygen()` creates not one single key but a pair.
![[Photos/Pasted image 20250118162437.png]]

Encrypt with the public key and decrypt with the private key.

* Signing and verifying:
	* `sign(message, private key)`: generates the signature
	* `verify(message, signature, public key)`: verify