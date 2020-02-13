
# Generating a RSA key
    openssl genrsa -out person.key 4096

# Generating a DSA key
    openssl gendsa -out person.key 4096

# Generating an EC key
    openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:P-384 -out myusername.id_ec
https://apps.nsa.gov/iaarchive/programs/iad-initiatives/cnsa-suite.cfm
ec_paramgen_curve:
 - P-256 (prime256v1) * less trouble
 - P-384 (secp384r1)  * nsa reccomended
 - P-521 (secp521r1)
ec_param_enc:
 - named_curve (default)
 - explicit

## Check out that spectacular key
    openssl ec -in myusername.id_ec -text


# Generate a CSR
    # if it complains about you missing the ~/.rnd just "touch" it.
    openssl req -new -key /home/myusername/.ssh/id_ec -out myusername.csr -subj "/CN=myusername/O=system:masters"

# Check out that glorious csr
    openssl req -in myusername.csr -text
