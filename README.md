# Installing Trivy 

### 1. Masuk ke Server via SSH

```bash

ubuntu@<ip,123.123.123.123>

```

### 2. Jalankan Perintah Instalasi Trivy

```bash

sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y

```

### 3. Verifikasi Instalasi

```bash

trivy --version

```

## Installing Trivy in the Jenkins container

### 1. Masuk ke Server via SSH

```bash

ubuntu@<ip,123.123.123.123>

```

### 2. Install Trivy ke dalam Kontainer Jenkins

```bash

sudo docker exec -u root jenkins apt-get update
sudo docker exec -u root jenkins apt-get install -y wget apt-transport-https gnupg lsb-release
sudo docker exec -u root jenkins wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo docker exec -i -u root jenkins gpg --dearmor -o /usr/share/keyrings/trivy.gpg
sudo docker exec -u root jenkins sh -c 'echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" > /etc/apt/sources.list.d/trivy.list'
sudo docker exec -u root jenkins apt-get update
sudo docker exec -u root jenkins apt-get install -y trivy

```

### 3. Verifikasi Instalasi di Kontainer

```bash

sudo docker exec -u root jenkins trivy --version

```

### 4. Gunakan di Jenkinsfile

```bash

stage('Security Scan (Trivy)') {
    steps {
        echo "Scanning Docker image for vulnerabilities using Trivy..."
        sh "trivy image --severity HIGH,CRITICAL --exit-code 0 ${images}"
    }
}

```

# **Full Dokumentasi** https://trivy.dev/








