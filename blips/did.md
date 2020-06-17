# Decentralized Identifiers (draft)

## Introduction
W3C's Decentralized Identifier (DID) specification [(link)](https://www.w3.org/TR/did-core/) provides a means to manage the identities of trusted authorities that are issuing research object certificates [(link)](blips/blip-2-researchcertificate.md). 
A user's decentralized identifier is determined by them (as opposed to being assigned by a centralized authority) and its registration and later interaction with the service is based on public key cryptography. For privacy reasons, it is recommneded that the registering entity creates a DID per service in order to prevent correlation between various services.

The DID is a string consisting of three parts

~~~
did:<method_name>:<method_string>
~~~

where the method name is service specific and the method string is a randomly generated alphanumeric string of fixed length.

**DID Example**
~~~
did:bloxberg:l7T1FC90SwGYj89K79WgZJ
~~~

The DID's counterpart is the DID document (DDO), which in its basic form contains a list of public keys, a list of authentication means and a list of service endpoints.

An example of a DDO that uses an RSA key for authentication and lists two service endpoints, one providing information about the user, and one showing the current certificates issued by them looks as follows:

**DID Document Example<a name="ddo_example"></a>**
~~~
{
	"@context": "https://www.w3.org/ns/did/v1",
	"id": "did:bloxberg:l7T1FC90SwGYj89K79WgZJ",
	"publicKey": [
		{
			"id": "did:bloxberg:l7T1FC90SwGYj89K79WgZJ#key-1",
			"type": "RSA",
			"publicKeyPem": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsgBdWNwfHBVoEBG07GHQ\n7ZD4htyot1AM2udDAkI8zRA/+lLQQDSw+vpP8cDZYz1dpytxsXj6TqWiPB45Zxu/\nRAJOdJ91+BhTLoaO6HldUsuPljh7cyMBQGJ9Co5yazjcI39jrunBEzambd6sVbvL\nZrdFHgETnWMMcknovtrkep4S3wi073L3YFR8iC+RAzSezqZjskjpnG27HgEto3F4\nzDK1dkjA4xOiigPCIZTjnQ+g/lUp3fe0B0MEH0VJuaUMRyCYICXhEeEqLfUwBaMi\nQdEQPQjEWsCIt8yVEW3Z/aotLsVGIoZfKB7LdTSBnjlth4skmNDMSMLOU44NJApr\nVQIDAQAB\n-----END PUBLIC KEY-----"
		}
	],
	"authentication": [
		"#key-1"
	],
	"service": [
		{
			"id": "did:bloxberg:l7T1FC90SwGYj89K79WgZJ#user",
			"type": "UserMetadataService",
			"serviceEndpoint": "http://127.0.0.1:8080/didapi/metadata/user/did:didapi:l7T1FC90SwGYj89K79WgZJ"
		},
		{
			"id": "did:bloxberg:l7T1FC90SwGYj89K79WgZJ#certificates",
			"type": "CertificateMetadataService",
			"serviceEndpoint": "http://127.0.0.1:8080/didapi/metadata/certificates/did:didapi:l7T1FC90SwGYj89K79WgZJ"
		}
	]
}
~~~

A service that manages DIDs would implement the associated CRUD operations:
* create the DID (on behalf of the user)
* retrieve the DID
* update the DID 
* deactivate the DID (deletion of DIDs is by design not supported)


### Creation
Corresponds to the generation of the DID string and its registration with the service. The information provided here is typically:
* a public key (mandatory)
* user's signature with the aforementioned key (mandatory) 
* user metadata (optional).

**Example request**

~~~
curl -X POST -H 'Content-Type: application/json' -i http://localhost:8080/didapi/register --data '{
    "name": "Max Mustermann",
    "email_address": "max@mustermann.de",
    "signature": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJuYW1lIjoiTWF4IE11c3Rlcm1hbm4iLCJlbWFpbF9hZGRyZXNzIjoibWF4QG11c3Rlcm1hbm4uZGUifQ.kjQ1yrCdV_EA6qToOlEkw5zObF9RqaW-BeedTSMQ1RQkXAQJlT3JdyrdqzQ1G7G4ZuNSlQSAZrA85Rfs9FzL_R_M-jOjUtwbkBC7PbiEKBGavuqZbHh-fsO42Uvc9bWTvOzQtY-ujfa2jUMsIkUU0E_77C0rEdlE43beKJoZd8UP8f9ak1r0_NM8RTplqX3v8va_JWSGP9RSlNz1vbICN09JyKyMOy1jgbCk-ss-lPrVhJvNyIYougBdrgl7jxjPi9nvC1i5SUvhmiwt3idGpa6olejCtEgkoA4eHww5zboreZVdzeVWmVh1DGnc_ibe0B6EotpPysnHZxrL7e78Ow",
    "pub_key": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsgBdWNwfHBVoEBG07GHQ\n7ZD4htyot1AM2udDAkI8zRA/+lLQQDSw+vpP8cDZYz1dpytxsXj6TqWiPB45Zxu/\nRAJOdJ91+BhTLoaO6HldUsuPljh7cyMBQGJ9Co5yazjcI39jrunBEzambd6sVbvL\nZrdFHgETnWMMcknovtrkep4S3wi073L3YFR8iC+RAzSezqZjskjpnG27HgEto3F4\nzDK1dkjA4xOiigPCIZTjnQ+g/lUp3fe0B0MEH0VJuaUMRyCYICXhEeEqLfUwBaMi\nQdEQPQjEWsCIt8yVEW3Z/aotLsVGIoZfKB7LdTSBnjlth4skmNDMSMLOU44NJApr\nVQIDAQAB\n-----END PUBLIC KEY-----",
    "pub_key_type": "RSA"
}'
~~~

The resulting DDO (including DID) for this example is the json document above [(link)](#ddo_example)

### Retrieval

### Update

### Deactivation

