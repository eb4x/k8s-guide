
# Generating and viewing a RSA key
    openssl genrsa -out myusername.id_rsa 4096
    openssl rsa -in myusername.id_rsa -text

# Generating and viewing a DSA key
    openssl dsaparam -out dsaparam.pem 4096
    openssl gendsa -out myusername.id_dsa dsaparam.pem
    openssl dsa -in myusername.id_dsa -text

# Generating and viewing an EC key
    openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:P-384 -out myusername.id_ecdsa
    openssl ec -in myusername.id_ecdsa -text

[Commercial NSA Suite](https://apps.nsa.gov/iaarchive/programs/iad-initiatives/cnsa-suite.cfm)

ec_paramgen_curve:
 - P-256 (prime256v1) * less trouble
 - P-384 (secp384r1)  * nsa reccomended
 - P-521 (secp521r1)
ec_param_enc:
 - named_curve (default)
 - explicit



# Generate a CSR
    # if it complains about you missing the ~/.rnd just "touch" it.
    openssl req -new -key /home/myusername/.ssh/id_ecdsa -out myusername.csr -subj "/CN=myusername/O=system:masters"

# Check out that glorious csr
    openssl req -in myusername.csr -text
