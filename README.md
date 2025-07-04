# SumAI 
SumAI adalah sistem berbasis web yang dirancang untuk menyederhanakan konsumsi berita melalui ringkasan otomatis menggunakan teknologi Natural Language Processing (NLP). Proyek ini mengimplementasikan mekanisme autoscaling untuk mengatasi keterbatasan skalabilitas dan meningkatkan efisiensi penggunaan sumber daya.
Sistem ini terdiri dari dua komponen utama yang di-deploy secara terpisah dengan mekanisme autoscaling masing-masing:
#  Repository Components:
- SumAI App (Laravel): https://github.com/Alvino08/Capstone.git
- SumAI Model (FastAPI): https://github.com/HanifahPutriNavyta/Model-SumAI-Capstone.git

# Tujuan
- Mengimplementasikan autoscaling untuk penyesuaian kapasitas sumber daya secara otomatis
- Meningkatkan stabilitas performa dan efisiensi penggunaan sumber daya
- Memisahkan autoscaling antara aplikasi web dan model NLP untuk optimasi yang lebih baik

# Arsitektur Sistem & Teknologi
- Git/GitHub --> Version control dan deployment integration
- Laravel Application (PHP) --> Framework web application (PHP) utama
- FastAPI Service (Python) --> Layanan API untuk NLP processing
- Nginx --> Reverse proxy dan web server
- Docker & Docker Compose --> Containerization aplikasi dan model
- AWS EC2 --> Hosting dan infrastruktur utama
- AWS Auto Scaling Group --> Autoscaling mechanism
- AWS CloudWatch --> Monitoring dan metrics
- AWS Elastic Load Balancer --> Load balancing

#  Fitur Autoscaling
**Auto Scaling Groups:**
1. ASG SumAI-App
- Metrik Utama: CPU Utilization
- Scaling Policy: Target tracking berdasarkan aplikasi web traffic
- Min/Max Instances: Disesuaikan dengan kebutuhan web traffic
- Health Check: HTTP endpoint checking

2. ASG SumAI-Model (FastAPI NLP Service)
- Metrik Utama: CPU Utilization
- Scaling Policy: Target tracking berdasarkan NLP processing load
- Min/Max Instances: Disesuaikan dengan kompleksitas model processing
- Health Check: API endpoint checking

**Kebijakan Scaling**:
- Scale Up: Otomatis menambah instance saat beban tinggi
- Scale Down: Otomatis mengurangi instance saat beban rendah
- Target Tracking: Mempertahankan target metrics tertentu

# Instalasi dan Konfigurasi
**Prerequisites**:
- AWS Account dengan akses ke EC2, ASG, CloudWatch
- Docker dan Docker Compose
- Git untuk version control

# Setup Steps
**1. Clone Repository**
#Clone SumAI App (Laravel) 

git clone https://github.com/Alvino08/Capstone.git sumai-app

cd sumai-app

**# Clone SumAI Model (FastAPI)\**

git clone https://github.com/HanifahPutriNavyta/Model-SumAI-Capstone.git sumai-model

cd sumai-model

**2. Setup SumAI App**
bash


cd sumai-app

cp .env.example .env

**# Edit file .env sesuai konfigurasi database dan AWS\**

composer installphp artisan key:generate

php artisan migratedocker-compose up -d

**3. Setup SumAI Model**
bashcd sumai-model
cp .env.example .env
**# Edit file .env sesuai konfigurasi model dan AWS\**
pip install -r requirements.txt
docker-compose up -d

**4. Setup AWS Infrastructure**
A. Create AMI untuk SumAI App & Model:
- Launch EC2 instance dan setup aplikasi Laravel
- Install dependencies dan konfigurasi environment
- Create AMI dari instance yang sudah dikonfigurasi

B.  Setup Launch Templates:
- Launch Template SumAI App: Gunakan SumAI-App-AMI
- Launch Template SumAI Model: Gunakan SumAI-Model-AMI

C. Create Auto Scaling Groups:
- SumAI-App-ASG: Auto scaling untuk aplikasi web
- SumAI-Model-ASG: Auto scaling untuk model NLP

D. Setup Load Balancers:
- ALB for SumAI App: Load balancer untuk aplikasi web
- ALB for SumAI Model: Load balancer untuk model API

E. Configure CloudWatch:
- Setup monitoring untuk kedua komponen
- Configure alerts dan notifications

# Alerts Configuration
**App Alerts:**
- High CPU utilization (>60%) → Scale up App instances
- Low CPU utilization (<50%) → Scale down App instances
- High response time (>3m) → Scale up App instances
- Instance health failures → Replace unhealthy instances

**Model Alerts:**
- High CPU utilization (>70%) → Scale up Model instances
- Long processing queue (>10 requests) → Scale up Model instances
- Model inference timeout → Scale up Model instances
- Low utilization (<50%) → Scale down Model instances

# Testing
**CPU stress test pada instances**

sudo stress-ng --cpu 2 






