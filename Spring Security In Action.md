## JdbcUserDetailsManager
1. expects three columns in the users table: a username, a password, and enabled, which you can use to deactivate the user

```sql

CREATE TABLE IF NOT EXISTS 'spring'.'users' (
    'id' INT NOT NULL AUTO_INCREMENT,
    'username' VARCHAR(45) NOT NULL,
    'password' VARCHAR(45) NOT NULL,
    'enabled' INT NOT NULL,
    PRIMARY KEY ('id')
);

```
## KeyGenerators
1. can used to generate salt value for hashing 
2. The generator creates an 8-byte key, and it encodes that as a hexadecimal string
``` java

StringKeyGenerator keyGenerator = KeyGenerators.string();
String salt = keyGenerator.generateKey();

```
3. To generate a 8-byte byte[] salt
``` java
BytesKeyGenerator keyGenerator = KeyGenerators.secureRandom();
byte [] key = keyGenerator.generateKey();
int keyLength = keyGenerator.getKeyLength();
```

## Spring Security Encryption
1. BytesEncryptor for byte[] data
    1. Encryptors.standard() for 256-byte AES encryption + cipher block chaining (CBC)
    ``` java
    String salt = KeyGenerators.string().generateKey();
    String password = "secret";
    String valueToEncrypt = "HELLO";

    BytesEncryptor e = Encryptors.standard(password, salt);
    byte [] encrypted = e.encrypt(valueToEncrypt.getBytes());
    byte [] decrypted = e.decrypt(encrypted);
    ```
    2. Encryptors.stronger() for 256-byte AES encryption + Galois/Counter Mode (GCM)
    ``` java
    String salt = KeyGenerators.string().generateKey();
    String password = "secret";
    String valueToEncrypt = "HELLO";

    BytesEncryptor e = Encryptors.stronger(password, salt);
    byte [] encrypted = e.encrypt(valueToEncrypt.getBytes());
    byte [] decrypted = e.decrypt(encrypted);
    ```
3. TextEncryptor for String data
    1.   Encryptors.text()
    ```java
    String salt = KeyGenerators.string().generateKey();
    String password = "secret";
    String valueToEncrypt = "HELLO";
    
    TextEncryptor e = Encryptors.text(password, salt);
    String encrypted = e.encrypt(valueToEncrypt);
    String decrypted = e.decrypt(encrypted);
    ```
    2.   Encryptors.delux() like stronger() in BytesEncryptor
    3.   Encryptors.queryableText()
            -  This encryptor guarantees that sequential encryption operations will generate the same output for the same input
            -  Used in OAuth API key

## Principal
- The user requesting access to the application is called a principal

