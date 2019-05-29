RELYING_PARTY=example.com
USERNAME=John

all: verify

### 1 create credentials

# create credential parameters (challenge, rp ID, username, user ID)
cred_challenge:
	dd if=/dev/urandom bs=1 count=64 | openssl sha256 -binary | base64 > cred_challenge
	echo ${RELYING_PARTY} >> cred_challenge
	echo ${USERNAME} >> cred_challenge
	dd if=/dev/urandom bs=1 count=32 | base64 >> cred_challenge

# create credential
cred_response: cred_challenge
	sudo fido2-cred -M -i cred_challenge -o cred_response /dev/hidraw0
# verify response and store credential (credential ID, pubkey)
credential: cred_response
	fido2-cred -V -i cred_response -o credential

### 2 create assertion

# create assertion parameters (challenge, rp ID, and credential ID)
assert_challenge: credential
	dd if=/dev/urandom bs=1 count=64 | openssl sha256 -binary | base64 > assert_challenge
	echo ${RELYING_PARTY} >> assert_challenge
	head -1 credential >> assert_challenge

# create assertion
assert_response: assert_challenge
	sudo fido2-assert -G -i assert_challenge -o assert_response /dev/hidraw0 

# extract pubkey from stored credentials
pubkey: credential
	tail -n +2 credential > pubkey
# verify assertion
verify: assert_response pubkey
	fido2-assert -V -i assert_response pubkey es256

clean:
	-rm cred_challenge cred_response credential assert_challenge assert_response pubkey
