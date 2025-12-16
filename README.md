# internal-ca-certificates

Interne CA-rotsertifikater for Statens Pensjonskasse.

Tilgjengeliggjøres offentlig for å enkelt kunne bruke de i bygg.

## Bruk

Curl'es typisk ned med

`curl -O https://raw.githubusercontent.com/statens-pensjonskasse/internal-ca-certificates/refs/heads/main/spk-root-ca.pem`

### Dockerfile (Rockylinux/Redhat 9)

```Dockerfile
# Henter gjeldende rotsertifikat
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/internal-ca-certificates/refs/heads/main/spk-root-ca.pem /etc/pki/ca-trust/source/anchors/

# Installerer rotsertifikater
RUN update-ca-trust extract
```

### Dockerfile (Debian)

```Dockerfile
# Henter gjeldende rotsertifikat
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/internal-ca-certificates/refs/heads/main/spk-root-ca.pem /usr/local/share/ca-certificates/

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
ADD --chmod=644 https://raw.githubusercontent.com/statens-pensjonskasse/internal-ca-certificates/refs/heads/main/spk-root-ca.pem /usr/local/share/ca-certificates/

# Installerer rotsertifikater
RUN apk -U add ca-certificates
RUN update-ca-certificates
```
