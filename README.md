# internal-ca-certificates

Interne CA-rotsertifikater for Statens Pensjonskasse.

Tilgjengeliggjøres offentlig for å enkelt kunne bruke de i bygg.

## Bruk

Curl'es typisk ned med

`curl -O https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-current.pem`

og

`curl -O https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-previous.pem`

### Dockerfile (Rockylinux/Redhat 9)

```Dockerfile
# Henter gjeldende rotsertifikat
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-current.pem /etc/pki/ca-trust/source/anchors/
# Henter gammelt rotsertifikat hvis vi er i en overgangsperiode. Hvis ikke så er dette en symlink som peker til samme sertifikat som over.
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-previous.pem /etc/pki/ca-trust/source/anchors/

# Installerer rotsertifikater
RUN update-ca-trust extract
```

### Dockerfile (Debian)

```Dockerfile
# Henter gjeldende rotsertifikat
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-current.pem /usr/local/share/ca-certificates/
# Henter gammelt rotsertifikat hvis vi er i en overgangsperiode. Hvis ikke så er dette en symlink som peker til samme sertifikat som over.
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-previous.pem /usr/local/share/ca-certificates/

# Ubuntu krever at sertifikatene slutter med .crt og ikke .pem
RUN for file in /usr/local/share/ca-certificates/*.pem ; do \
        mv -v -- "$file" "${file%.pem}.crt" ; \
    done

# Installerer rotsertifikater
RUN update-ca-certificates
```

### Dockerfile (Alpine)

```Dockerfile
# Henter gjeldende rotsertifikat
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-current.pem /usr/local/share/ca-certificates/
# Henter gammelt rotsertifikat hvis vi er i en overgangsperiode. Hvis ikke så er dette en symlink som peker til samme sertifikat som over.
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/spk-internal-ca-certificates/refs/heads/main/spk-root-ca-previous.pem /usr/local/share/ca-certificates/

# Installerer rotsertifikater
RUN apk -U add ca-certificates
RUN update-ca-certificates
```
