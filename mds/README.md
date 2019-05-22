# Using the FIDO Alliance Metadata Service

See
https://fidoalliance.org/metadata/

# Synopsis

In short:

- [Get an access token](https://mds2.fidoalliance.org/tokens/)

- [Accept Metadata Usage Terms](https://mds2.fidoalliance.org/tokens/legalese?v=1.0)

- obtain the metadata TOC:

    curl https://mds2.fidoalliance.org/?token=$TOKEN

- obtain the MDS root certificate to validate the TOC:

    wget https://mds.fidoalliance.org/Root.cer

- verify the jwt signature

You can use a tool like [step](https://smallstep.com/docs/cli/crypto/jwt/)

On macos:

    brew install step

All these steps in sequence:

    wget --quiet https://mds.fidoalliance.org/Root.cer
    curl https://mds2.fidoalliance.org/?token=FillInYourTokenHerePlease --silent --output toc.jwt
    step crypto jwt inspect --insecure < toc.jwt | jq -r '.header.x5c[1]' | base64 -D | openssl x509 -inform der -out CA-1.pem
    step crypto jwt inspect --insecure < toc.jwt | jq -r '.header.x5c[0]' | base64 -D | openssl x509 -inform der -out MetadataTOCSigner3.pem
    openssl verify -CAfile Root.cer CA-1.pem 
    openssl verify -CAfile Root.cer -untrusted CA-1.pem MetadataTOCSigner3.pem 
    openssl x509 -in MetadataTOCSigner3.pem -noout -pubkey > MetadataTOCSigner3.pub
    step crypto jwt verify --key MetadataTOCSigner3.pub --subtle < toc.jwt | jq .payload > toc.json

Alternatively, use the Makefile:

    make toc.json TOKEN=YourToken

# Notes

It is safe to store the token in a file named TOKEN, which is excluded from being commited with a `.gitignore` file.
Set your TOKEN envireonment variable:

    export TOKEN=$(cat TOKEN)

# Examples

```
curl https://mds2.fidoalliance.org/?token=$TOKEN
curl https://mds2.fidoalliance.org/?token=$TOKEN | step crypto jwt inspect --insecure | jq '.payload.entries[].statusReports[].certificationDescriptor'

curl https://mds2.fidoalliance.org/metadata/4e4e%234005/?token=$TOKEN
curl -s https://mds2.fidoalliance.org/metadata/4e4e%234005/?token=$TOKEN | base64 -D | jq -r .icon | cut -d, -f2 | base64 -D > icon.png

```
