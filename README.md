
# FHIR synthetic data

---

## 🚀 External Dependencies

This project suggests to use data generated from Synthea.
Following the instructions to generate your dataset 

Dependencies


We currently use the last documented release from Synthea, **Synthea V3.3.0** (Sep 6th, 2024) to ensure compatibility and stability.
Release info available in: **[https://github.com/synthetichealth/synthea/releases/tag/v3.3.0](https://github.com/synthetichealth/synthea/releases/tag/v3.3.0)**

---

## 📝 Prerequisites

From the **[Synthea Toolkit](https://synthetichealth.github.io/spt/#/customizer)** , generate your tailored 
synthetic dataset according to your customized requirements for population, geographic and demographic settings along with 
your own seed and download your generated *Dockerfile*.


## 📦 Installation and Setup

Include the generated *Dockerfile* from Synthea toolkit in the project's directory and run:

```
docker build -t syntheadocker -f Dockerfile .
docker run -v "${PWD}/data/synthea-output-data:/output" -it syntheadocker
```

### Create your own certificates and credentials

For an HTTPS certificate run in **Bash**.
```dash
mkdir -p nginx/certs
openssl req -x509 -newkey rsa:4096 -nodes \
  -keyout nginx/certs/privkey.pem \
  -out nginx/certs/fullchain.pem \
  -days 365 \                                                                                           
  -subj "//CN=localhost"
```
Create `.htpasswd` for basic authentication

```dash
docker run --rm -it -v ${PWD}\auth:/auth httpd:2.4 htpasswd -c /auth/.htpasswd <yourfantasticusername>
```
<br>


> 💡 NOTE: Ensure the `privkey.pem`, `fullchain.pem` and `.htpasswd` were successfully generated and 
> placed in the correct subfolders and fit according to you hostname.

> 💡 NOTE: Ensure your *Synthea* data was successfully generated and placed in the correct subfolder.

> 💡 NOTE: Ensure your defined `.env` locally.

### Bring the `docker-compose.yaml` up


Run to set up the containers: 

```
docker-compose up -d
```
Build `blazectl` to upload bundles

```
docker build -f Dockerfile-blazectl -t blazectl .
```
```dash
docker run --rm -v ${PWD}\data\synthea-output-data:/input --network <your-project-network> blazectl blazectl upload --server http://fhir-node:8080/fhir /input/fhir   
```
Open browser and verify your loaded server ‍🚀. 

---
## 🔒 Security Notes

- Basic auth + self-signed certificates are for local testing only.

For production:

- Use OAuth2 / OIDC (Keycloak, Auth0).