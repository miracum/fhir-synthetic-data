
# FHIR synthetic data

---

## 🚀 External Dependencies

This project suggests to use data generated from Synthea.
Following the instructions to generate your dataset 

Dependencies


We currently use the last documented release from Synthea, **Synthea V3.3.0** (Sep 6th, 2024) to ensure compatibility and stability.
Further releases info available in: **[https://github.com/synthetichealth/synthea/releases/tag/v3.3.0](https://github.com/synthetichealth/synthea/releases/tag/v3.3.0)**

---

## 📝 Prerequisites
 From the **[Synthea Toolkit](https://synthetichealth.github.io/spt/#/customizer)** , generate your tailored 
synthetic dataset according to your customized requirements for population, geographic and demographic settings along with 
your own seed and download your generated *Dockerfile*. 

Build and run your Docker image.

```
 docker build -t syntheadocker -f Dockerfile .
 docker run -v ./synthea_output:/output -it syntheadocker
```


## 📦 Installation and Setup

### Clone This Repository
```bash
git clone https://github.com/miracum/fhir-synthetic-data.git
```

### Setting up  of FHIR server: 

To set up a simple security layer, create a *httpass* by running:
```
docker run --rm -it -v ${PWD}\auth:/auth httpd:2.4 htpasswd -c /auth/.htpasswd <username>
```
> NOTE: After auth folder is created, create 'env' folder and place 'auth' credentials.
 
Run to set up the containers: 

```
docker-compose build
docker-compose up 
```

After populate the servers:

```
blazectl count-resources --server http://localhost/fhir --user <username> --password <secret>
```