# 🚗 Vehicle Data MLOps Pipeline

An end-to-end Machine Learning Operations (MLOps) project designed to predict vehicle-related metrics. This project encompasses the entire machine learning lifecycle, from data ingestion and MongoDB integration to model training, evaluation, and CI/CD deployment using AWS and Docker.

## 🌟 Key MLOps Features & Architecture

This project is built with production-grade practices to ensure scalability, reliability, and automated deployments.

* **Custom Exception Handling & Logging:** Robust tracking of system behavior and errors.
* **Modular Pipeline Design:** Separated components for Data Ingestion, Validation, Transformation, and Model Training.
* **Cloud Data Storage:** Integration with MongoDB Atlas for NoSQL data management.
* **Model Registry & Tracking:** Automated pushing and pulling of model artifacts using AWS S3.
* **Containerization:** Fully containerized application utilizing Docker.
* **CI/CD Automation:** Continuous Integration and Deployment pipelines configured via GitHub Actions.
* **Cloud Deployment:** Hosted on an AWS EC2 Ubuntu instance via AWS ECR.

## 🛠️ Technology Stack

| Category | Tools & Technologies |
| --- | --- |
| **Language & Environment** | Python 3.10, Virtual Environments (Conda) |
| **Database** | MongoDB Atlas |
| **Cloud Services (AWS)** | S3 (Model Registry), EC2 (Hosting), ECR (Container Registry), IAM |
| **ML Pipeline** | Scikit-Learn, Pandas, Numpy |
| **DevOps & CI/CD** | Docker, GitHub Actions, Self-Hosted Runners |
| **Web Interface** | Flask (HTML/CSS templates) |

---

## 🚀 Quick Start & Local Setup

**1. Clone the repository and navigate to the project directory:**

```bash
# Execute the template setup
python template.py

```

**2. Create and activate a virtual environment:**

```bash
conda create -n vehicle python=3.10 -y
conda activate vehicle

```

**3. Install local dependencies:**

```bash
pip install -r requirements.txt
pip list # Verify local packages (setup.py/pyproject.toml) are installed

```

---

## 🗄️ Database Setup (MongoDB Atlas)

This project uses MongoDB to store and retrieve dataset records.

1. Create a free **M0 Cluster** on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
2. Configure **Network Access** to allow requests from anywhere (`0.0.0.0/0`).
3. Generate a database user and fetch the Python Driver connection string.
4. Set up your environment variable:

**For Mac/Linux (Bash):**

```bash
export MONGODB_URL="mongodb+srv://<username>:<password>@cluster.mongodb.net/..."

```

**For Windows (Powershell):**

```powershell
$env:MONGODB_URL="mongodb+srv://<username>:<password>@cluster.mongodb.net/..."

```

*(Note: Ensure your data ingestion notebooks push the initial dataset to the Atlas cluster in key-value format prior to running the pipeline).*

---

## ☁️ AWS Cloud Setup

To enable the Model Evaluation and Pusher components, AWS S3 is used as a model registry.

**1. IAM Configuration:**

* Create an IAM user with `AdministratorAccess`.
* Generate CLI Access Keys.
* Export keys to your environment:

```bash
export AWS_ACCESS_KEY_ID="your_access_key"
export AWS_SECRET_ACCESS_KEY="your_secret_key"

```

**2. S3 Bucket Initialization:**

* Create an S3 bucket named `my-model-mlopsproj` in the `us-east-1` region.
* The `s3_estimator.py` utility will handle pushing valid models to the `model-registry` directory in S3 if their accuracy exceeds the `MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE` (0.02).

---

## ⚙️ CI/CD Pipeline & Deployment

This application is continuously deployed to an AWS EC2 instance using GitHub Actions and Docker.

### Infrastructure Provisioning

1. **AWS ECR:** Created a repository named `vehicleproj` in `us-east-1` to store Docker images.
2. **AWS EC2:** Provisioned a `t2.medium` Ubuntu 24.04 server named `vehicledata-machine`. Port `5080` is exposed in the security group for web traffic.

### Deployment Workflow

The deployment is entirely automated upon pushing to the main branch:

1. **Docker Build:** GitHub Actions builds the Docker image using the provided `Dockerfile`.
2. **ECR Push:** The image is pushed to the private AWS ECR repository.
3. **EC2 Pull & Run:** A self-hosted GitHub runner situated on the EC2 instance pulls the latest image and runs the container.

### Running the App

Once deployed, the Flask application is accessible via:

* **Web UI:** `http://<EC2_PUBLIC_IP>:5080`
* **Trigger Training:** `http://<EC2_PUBLIC_IP>:5080/training`

---

## 👨‍💻 Author

**Rakshan Parashar** Computer Science and Engineering (CSE)
*Passionate about scalable systems, automated pipelines, and full-stack machine learning solutions.*