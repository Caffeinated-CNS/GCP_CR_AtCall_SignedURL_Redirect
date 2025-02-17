# GCP_CR_AtCall_SignedURL_Redirect

Cloud Run Java app for authenticating incoming calls, translating provided parameters into a GCS object path, generating a signed URL for consumer download, and finally sending a redirect response with the target set to the signed URL.

Test metrics:
- What is the implications of adding any signing to response vs expecting consumer to have an appropriate access token to retireve the GCS object?
- What is the impact of using RSA vs HMAC for generating signed URLs?
- What happens when a signed URL is inactivated or deleted mid-download?

Enhancements List:
- Inactivate key after all generated files are downloaded (tracked via GCS event queue and memorystore?)
- Delete key after all generated files are downloaded (after inactivation, as required for HMAC keys).

References:
- https://cloud.google.com/storage/docs/access-control/iam-permissions 
- https://cloud.google.com/storage/docs/authentication/canonical-requests
- https://cloud.google.com/storage/docs/access-control/signing-urls-manually

Notes:
- Should use v4 signing so query parameters cannot be alter after the URL is signed. [v2 signing process](https://cloud.google.com/storage/docs/access-control/signed-urls-v2)

## Interesting GCS Events


HMAC Specific
- storage.hmacKeys.create
- storage.hmacKeys.delete
- storage.hmacKeys.update

Storage General
- storage.objects.get
- storage.bucketOperations.get
