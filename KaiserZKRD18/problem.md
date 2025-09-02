The core issue is that we can't access `opam.ocaml.org`,
which is preventing the download of necessary files for
OPAM installation and initialization.
The issue involves OPAM failing to update its default repository during the initial setup. The error message indicates
that the curl command,
used to download the repository index file from https://opam.ocaml.org, exited with code 35, which suggests a problem
related to SSL connections or certificate validation.
As a result, the initial repository download could not be completed.

workarounds tried:

- Used Ubuntu PPA for OPAM: Encountered time zone configuration prompts.
- Built OPAM from GitHub source: Failed due to missing dependencies (dose3, cudf) since opam.ocaml.org was down.
- Manually installed dependencies with OPAM: Still couldn't access the repository.
- Ensure ca-certificates is Up to Date: We tried updating and reinstalling the ca-certificates package to make sure the
  SSL certificates were current.
- Explicitly Set the CA Certificate Path: We suggested setting the CA certificate path manually using:
- Disable SSL Verification (for Testing): We used the environment variable CURL_SSL_NO_VERIFY=true to bypass SSL
  certificate validation temporarily.
