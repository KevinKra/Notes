- **Client-side encryption** is the act of encrypting data _before_ sending it to S3. 
- There are two options to enable client-side encryption: **AWS-managed customer key or use a client-side master key.**
- When you use an AWS KMS-managed customer master key to enable client-side data encryption, _you provide an AWS KMS customer master key ID (CMK ID) to AWS._
- When you use a client-side master key, for client-side data encryption, your client-side master keys and your encrypted data are **_never_** sent to AWS.
- When you provide a client-side master key to the **Amazon S3 encryption client**. **The S3 encryption client uses the master key only to encrypt the data encryption key** that it generates randomly.

### S3 Encryption Client Steps

> This is how client-side encryption using client-side master key works:

#### Uploading

1. The Amazon S3 encryption client generates a one-time-use **symmetric key** (also known as a **data encryption key** or **data key**) locally. 
1. It uses the symmetric key to encrypt the data of a **single Amazon S3 object**.**The S3 encryption client generates a separate data key for each object.**
1. The S3 encryption client encrypts the data encryption key using the master key that you provide.
1. The S3 encryption client uploads the encrypted data key and its material description as part of the object metadata.
1. The S3 encryption client uses the material description to determine which client-side master key to use for decryption.
1. The S3 encryption client uploads the encrypted data to Amazon S3 and saves the encrypted data key as object metadata (`x-amz-meta-x-amz-key`) in Amazon S3.

- S3 encrypt client generates a data-key, uses that key to encrypt the data of a single s3 object, it then encrypts the data-key based on your master key, then it uploads the newly made encrypted data-key using its material description as metadata -- this metadata is used by the s3 encryption client to determine which client-side master key to use for decryption, it then uploads the encrypted data to S3 and saves the encrypted data-key as object metadata in S3. 

#### Downloading

1. Client downloads the encrypted object from S3.
1. Using the material description from the object's metadata, the client determines which master key to use for decrypting the data-key.
1. The master-key is used to decrypt the data-key, then the decrypted data-key is used to decrypt the object.

- encrypted object is downloaded from S3, local customer-key is used to decrypt data-key, then the decrypted data-key is used to decrypt the object.


--- 

- SSE: Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects.
- CSE: encrypting your data locally to ensure its security as it passes to the Amazon S3 service. The Amazon S3 service receives your encrypted data; it does not play a role in encrypting or decrypting it.
- Client-side encryption with a KMS-managed customer master key, you provide an AWS KMS customer master key ID (CMK ID) to AWS. AWS now has the master-key.
- S3 server-side encryption potentially results in unencrypted traffic being sent to AWS (if SSL/TLS isn't used). This is a data _in-transit_ vulnerability.*
- When using S3 server-side encryption with a customer-provided key (SSE-C), **you actually provide the encryption key as part of your request to upload the object to S3.**
- You can use **SSL/TLS (Secure Socket Layer or Transport Layer Security)** or client-side encryption, to protect data in transit.

#### AWS Encryption SDK

- The AWS Encryption SDK is a client-side encryption library that is separate from the language–specific SDKs. You can use this encryption library to more easily implement encryption best practices in Amazon S3.
- Unlike the Amazon S3 encryption clients in the language–specific AWS SDKs, the AWS Encryption SDK is not tied to Amazon S3 and can be used to encrypt or decrypt data to be stored anywhere.