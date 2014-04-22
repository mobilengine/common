#iOS Push notifications

Seguir el manual de Ray wenderlich: <http://www.raywenderlich.com/32960/apple-push-notification-services-in-ios-6-tutorial-part-1>

* En el momento que generas un *CertificateSigningRequest.certSigningRequest* **NO LO BORRES!** lo necesitarás mas adelante.
* Cuando llegues a "Making a PEM file" para, nuestro servidor java requiere un p12 y no un pem. Esa sección la sustituiremos por nuestro "Generar p12 file" indicado a continuación


##Generar p12 file

Nuestro servidor requiere el certificado + la llave en un formato especial p12, que **no** es el que se genera exportando la llave junto al certificado desde el keychain.

Seguir los siguientes pasos:

1. 
openssl x509 -in aps_developer_identity.cer -inform DER -out aps_developer_identity.pem -outform PEM}

Where aps_developer_identity.cer is the file you download from the portal

2.

openssl pkcs12 -nocerts -out APSCertificates.pem -in APSCertificates.p12

Where APSCertificates.p12 is a file you export from the Mac Keychain. This is critical, you must import the certificate from the portal into keychain. Find it in My Certificates, open the disclosure triangle and highlight both the certificate and the private key, then right click and export them. Give them a password and save them to a p12 file.

3.

openssl pkcs12 -export -in aps_developer_identity.pem -out aps_developer_identity.p12 -inkey APSCertificates.pem

You will be prompted a few times for the password you used to export the certificate and private key in Keychain and prompted again for new passwords to re-encrypt it all, but in the end you will have the file aps_developer_identity.p12 which you need to move to windows, then import it into both the Personal and Trusted Root sections of certificate manager in MMC. Then in C# when you use MoonAPNS and call the PushNotification? class you give it a path to that certificate. Also make sure to remove spaces from the device token.
