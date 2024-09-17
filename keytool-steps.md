# Keytool - Generate key and self-signed certificate

This document includes instructions for generating a key and self-signed certificate using [Keytool](https://docs.oracle.com/en/java/javase/13/docs/specs/man/keytool.html). 

> :ballot_box_with_check: NOTE: Keytool is already installed with the Java Development Kit. No additional installed is required.
## Generate Key and Self-Signed Certificate

1. Open a terminal/command-prompt window.

2. Move into the directory of your Spring Boot library project.

    ```
    cd spring-boot-library
    ```

3. In the terminal window, run this command to generate the key and certificate. This is one long command, copy/paste in its entirety.

    ```
    keytool -genkeypair -alias Bookbuddy -keystore src/main/resources/Bookbuddy-keystore.p12 -keypass secret -storeType PKCS12 -storepass secret -keyalg RSA -keysize 2048 -validity 365 -dname "C=IN, ST=Karnataka, L=Bangalore, O=Bookbuddy, OU=LibraryApp Backend, CN=localhost" -ext "SAN=dns:localhost"
    ```

    | Argument | Description |
    | --- | --- |
    | -genkeypair | Generates a key pair |
    | -alias | Alias name of the entry to process |
    | -keystore | Name of output keystore file |
    | -keypass | Key password |
    | -storeType | Keystore type |
    | -storepass | Keystore password
    | -keyalg | Key algorithm name |
    | -keysize | Key bit size |
    | -validity | Validity number of days |
    | -dname | Distinguished name |
    | -ext | Add the given X.509 extension |

    > Detailed docs available [here](https://docs.oracle.com/en/java/javase/13/docs/specs/man/keytool.html). 
3. The command generates the file: `src/main/resources/Bookbuddy-keystore.p12` .

## Verify Results

1. View the contents of your certificate.

    ```
    keytool -list -v -alias Bookbuddy -keystore src/main/resources/Bookbuddy-keystore.p12 -storepass secret
    ```

    _Sample Output_
    ```    
    Alias name: Bookbuddy
    Creation date: 17-Sep-2024
    Entry type: PrivateKeyEntry
    Certificate chain length: 1
    Certificate[1]:
    Owner: C=IN, ST=Karnataka, L=Bangalore, O=Bookbuddy, OU=LibraryApp Backend, CN=localhost
    Issuer: C=IN, ST=Karnataka, L=Bangalore, O=Bookbuddy, OU=LibraryApp Backend, CN=localhost
    Serial number: ba51e9be8f54b95f
    Valid from: Tue Sep 17 12:11:40 IST 2024 until: Wed Sep 17 12:11:40 IST 2025
    Certificate fingerprints:
    SHA1: 14:37:76:E6:29:6B:33:3E:D3:5E:90:6F:CE:48:92:3F:FB:A0:8D:07
    SHA256: D2:F6:FC:46:50:00:7C:55:21:77:E2:73:D8:05:59:73:90:86:13:E2:D2:58:CA:C4:0B:EE:CC:1D:C1:F2:6D:72
    Signature algorithm name: SHA256withRSA
    Subject Public Key Algorithm: 2048-bit RSA key
    Version: 3

Extensions:

#1: ObjectId: 2.5.29.17 Criticality=false
SubjectAlternativeName [
DNSName: localhost
]

#2: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: F1 93 9E 36 CD A5 B9 86   20 7D 83 BA 7A 19 EA 74  ...6.... ...z..t
0010: 46 1A 77 A8                                        F.w.
]
]
```

## Spring Boot HTTPS configs

1. Edit your `application.properties` file

1. Add this snippet for Spring Boot SSL configs to the end of your `application.properties` file

    ```
    #####
    #
    # HTTPS configuration
    #
    #####
    # Server web port
    server.port=8443
    # Enable HTTPS support (only accept HTTPS requests)
    server.ssl.enabled=true
    # Alias that identifies the key in the key store
    server.ssl.key-alias=luv2code
    # Keystore location
    server.ssl.key-store=classpath:luv2code-keystore.p12
    # Keystore password
    server.ssl.key-store-password=secret
    # Keystore format
    server.ssl.key-store-type=PKCS12
    ```

Congrats! You have successfully configured your Spring Boot project to use a self-signed certificate for https. You can now return to the videos and continue with the course.