# Use a valid token, eg
# make toc.json TOKEN=6d6b44d78b09fed0c5559e34c71db291d0d322d4d4de0000
# or fill in yout token here:
TOKEN=FillInYourTokenHerePlease

# Using tools wget and step
# https://smallstep.com/docs/cli/certificate/verify/
# macos: brew install step wget

all: toc.json

toc.jwt:
	curl https://mds2.fidoalliance.org/?token=${TOKEN} --silent --output toc.jwt

# verify certificate chain

Root.cer:
	wget --quiet https://mds.fidoalliance.org/Root.cer
MetadataTOCSigner3.pem: toc.jwt
	step crypto jwt inspect --insecure < toc.jwt | jq -r '.header.x5c[0]' | base64 -D | openssl x509 -inform der -out MetadataTOCSigner3.pem
CA-1.pem: toc.jwt
	step crypto jwt inspect --insecure < toc.jwt | jq -r '.header.x5c[1]' | base64 -D | openssl x509 -inform der -out CA-1.pem

verify-chain: Root.cer CA-1.pem MetadataTOCSigner3.pem
	openssl verify -CAfile Root.cer CA-1.pem 
	openssl verify -CAfile Root.cer -untrusted CA-1.pem MetadataTOCSigner3.pem 

# verify JWT

MetadataTOCSigner3.pub: MetadataTOCSigner3.pem
	openssl x509 -in MetadataTOCSigner3.pem -noout -pubkey > MetadataTOCSigner3.pub
CA-1.pub: CA-1.pem
	openssl x509 -in CA-1.pem -noout -pubkey > CA-1.pub

toc.json: verify-chain toc.jwt MetadataTOCSigner3.pub
	step crypto jwt verify --key MetadataTOCSigner3.pub --subtle < toc.jwt | jq .payload > toc.json
clean:
	-rm CA-1.pem MetadataTOCSigner3.pem MetadataTOCSigner3.pub Root.cer toc.json toc.jwt
