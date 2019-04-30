cred_param:
	echo credential challenge | openssl sha256 -binary | base64 > cred_param
	echo relying party >> cred_param
	echo user name >> cred_param
	dd if=/dev/urandom bs=1 count=32 | base64 >> cred_param

cred: cred_param
	fido2-cred -M -i cred_param /dev/hidraw0 | fido2-cred -V -o cred

assert_param: cred
	echo assertion challenge | openssl sha256 -binary | base64 > assert_param
	echo relying party >> assert_param
	head -1 cred >> assert_param

pubkey: cred
	tail -n +2 cred > pubkey

verify: assert_param pubkey
	fido2-assert -G -i assert_param /dev/hidraw0 | fido2-assert -V pubkey es256

clean:
	-rm cred_param cred assert_param pubkey
