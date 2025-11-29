# PhishGuard — AI / ML-Powered Phishing Detector (Website & App Extension) 

<img width="200" height="200" alt="phishguard_logo" src="https://github.com/user-attachments/assets/2ffff0c6-8f11-4636-abc9-1edd2b31946f" />

[![Flask](https://img.shields.io/badge/Flask-2.0+-green.svg)](https://flask.palletsprojects.com/)
[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/ML-RandomForest-orange.svg)](https://scikit-learn.org/)
[![Chrome Extension](https://img.shields.io/badge/Chrome-Manifest_V3-yellow.svg)](https://developer.chrome.com/docs/extensions/)
[![Render](https://img.shields.io/badge/Deployment-Render-purple.svg)](https://render.com/)

**Repository:** [github.com/bez-git/phishguard](https://github.com/bez-git/phishguard)

##  Overview
- PhishGuard is a full-stack security tool designed to protect users from phishing attacks in real-time. It consists of a **Chrome Extension (MV3)** frontend that inspects URLs and a **Flask REST API** backend that scores them using a trained **RandomForest Machine Learning model**.

- The system extracts features from a URL, imputes missing values, runs the model, and returns a label (`phish`, `suspicious`, `legit`) along with a probability score. The platform also includes a web dashboard (Flask + Jinja) for user management, reporting, and statistics.

---


## Architecture

The system operates via a secure loop between the client extension and the cloud-hosted API.

```
[Chrome Extension (MV3)]
 ├─ popup.js:      User interaction, Login, Scan Trigger
 ├─ background.js: Event listeners, Context actions
 └─ config.js:     Environment toggles (Dev/Prod)
       ⇅
 [ HTTPS + JWT + CORS ]
       ⇅
[Flask API (Render)]
 ├─ app/api/:         REST Endpoints (/check, /report)
 ├─ app/auth/:        User Auth (Register/Login/Confirm)
 ├─ app/predictions/: ML Pipeline (Feature Extract → RF Model)
 └─ app/models.py:    SQLAlchemy Models (User, Report)
       ⇅
[PostgreSQL Database]
```

 ## Feature Extraction Pipeline (Flask → ML Model)
 
 PhishGuard uses a custom feature extraction module located at:
 ```
app/predictions/extract_features.py
```
The pipeline computes:

* Lexical Features
* URL length
* Number of dots / subdomains
* Presence of @
* Presence of IP addresses
* Count of suspicious characters (-, _, //, %)
* Length of domain vs. path

Domain / WHOIS Features
- Domain age (in days)
- Remaining days before domain expiry
- Presence of HTTPS
- TLD classification
- Model Input Standardization
- A feature_order.json file (Git LFS) ensures consistent feature ordering.


 ## File Directory
<img width="200" height="1000" alt="Screenshot 2025-11-28 200952" src="https://github.com/user-attachments/assets/f088ea9c-eed1-421d-862c-b6d1f9d4cbe0" />

## Website Login 
<img width="600" height="425" alt="Screenshot 2025-11-28 195900" src="https://github.com/user-attachments/assets/f989fc8c-2068-42cd-8cb4-002600adbc6b" />

## Website Dashboard Data
<img width="500" height="300" alt="Screenshot 2025-11-28 200147" src="https://github.com/user-attachments/assets/96028f43-0159-45b3-84c5-fdbc3eb444dd" />

## Chrome Extension
<img width="400" height="400" alt="Screenshot 2025-11-28 201638" src="https://github.com/user-attachments/assets/ce117e6e-87ca-47de-aabf-c3fea1bc2b28" />
<img width="200" height="400" alt="Screenshot 2025-11-28 201801" src="https://github.com/user-attachments/assets/a2bc5f66-a717-4a5a-b9f9-36804ee2c4e4" />
<img width="400" height="400" alt="Screenshot 2025-11-28 201810" src="https://github.com/user-attachments/assets/c8194d23-7d01-4115-bc89-3f9882eb5d42" />

## MailTrap
<img width="700" height="311" alt="Screenshot 2025-11-28 201717" src="https://github.com/user-attachments/assets/e0666ce8-82ea-44d0-8ada-537f159266a2" />


##  Key Features
- Real-time Analysis: Instant URL scoring via REST API (/api/check) using a serialized ML model.
- Chrome Extension (MV3) with background worker
- Secure Authentication: JWT for API access (Extension) and Flask-Login for Web Dashboard sessions.
- User Management: Full registration flow with Email Verification and Password Reset using Mailtrap.
- Dashboard: Visual interface built with Jinja2 to view scan history and attack statistics.
- Data Integrity: SQLAlchemy ORM with Alembic migrations for database version control.
- ML Ops: Git LFS integration for managing model artifacts (*.joblib).
- Fully containerized deployment using Render + Gunicorn


## Dataset & Training (ML)
- The RandomForest model was trained using a hybrid dataset combining multiple reputable phishing & legitimate URL sources.

Phishing Data Sources
- Kaggle: Phishing Websites Dataset
- Kaggle: Malicious URLs Dataset
- PhishTank Verified Feed (daily updates)
- OpenPhish Community Feed

Legitimate URL Sources
- Alexa 
- Tranco List
- Popular trusted domains (gov, edu, major tech companies)

Data Cleaning Steps
- Remove unreachable URLs
- Normalize URL text
- Lowercase & encoding fixes
- Remove duplicates
- Verify domain DNS resolution
- Balance classes (phish:legit ~ 50/50)


## Google Colab Training Process
- The ML training notebook (training_notebooks/phishguard_train.ipynb) was executed on Google Colab GPU/CPU.

Training Steps
- Load all phishing + legit datasets
- Merge into a unified DataFrame
- Generate lexical features
- Pull WHOIS data (when rate limits allowed)
- Impute missing values
- Train RandomForest with parameters:

Model Evaluation
- Accuracy: ~94–97%
- Precision: High for phishing detection
- ROC AUC: ~0.97
- Confusion matrix included in images

Exported Artifacts (Git LFS) These are loaded by the Flask app at runtime.
- phish_rf.joblib
- imputer_phi.joblib
- feature_order.json
- threshold.json


## Machine Learning Pipeline & Model
The detection logic resides in app/predictions/predict.py. 
- Artifacts are managed via Git LFS.
- Feature Extraction: Parses the URL string (length, special chars, domain age, etc.).
- Imputation: Fills missing data points using imputer_phi.joblib.
- Prediction: Runs the phish_rf.joblib (RandomForest) model to generate a probability.
- Heuristics: Applies post-processing logic (thresholds) to assign final labels.

End-to-End Prediction Flow
- Parse raw URL
- Extract features (lexical + domain)
- Load imputer & transform inputs
- Run RandomForest inference
- Apply threshold heuristics
- Return {score, label}

Artifacts managed via Git LFS:
- phish_rf.joblib
- imputer_phi.joblib
- feature_order.json
- threshold.json

## Model
<img width="350" height="100" alt="Screenshot 2025-11-28 201913" src="https://github.com/user-attachments/assets/1cc6076f-ea7e-4561-8da4-b25c79371602" />
<img width="200" height="400" alt="Screenshot 2025-11-28 202037" src="https://github.com/user-attachments/assets/f7725ecf-1131-4ecc-b5cf-f914da135910" />
<img width="200" height="400" alt="Screenshot 2025-11-28 202043" src="https://github.com/user-attachments/assets/9a24ab66-0764-4718-9237-501a99596a0f" />
<img width="500" height="250" alt="Screenshot 2025-11-28 202100" src="https://github.com/user-attachments/assets/6899456a-9197-43c2-a7c6-3d5c5f06c1b9" />


##  Quick Start (Local Dev)

```text
# Clone the repo
git clone [https://github.com/bez-git/phishguard.git](https://github.com/bez-git/phishguard.git)
cd phishguard

# Virtual Environment
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# Install Dependencies
pip install -r requirements.txt

# Configure Environment
# Create a .env file in the root directory with the following:
@"
FLASK_ENV=development
FLASK_DEBUG=1
SECRET_KEY=dev-secret-key
SQLALCHEMY_DATABASE_URI=sqlite:///phishguard.sqlite3
MAIL_SUPPRESS_SEND=true
JWT_SECRET_KEY=dev-jwt-key
PHISH_THRESHOLD=0.90
"@ | Out-File .env -Encoding UTF8

# Database Init
$env:FLASK_APP="wsgi.py"
python -m flask db upgrade

# Run Server
python -m flask --app wsgi.py run
```

## Chrome Extension Setup
Open Chrome and navigate to chrome://extensions.
Enable Developer Mode (top right).
Click Load unpacked and select the phishguard_chrome_extension_v2/ folder.
Optional: Edit config.js if your local API is not running on port 5000.

## API Reference
Method,Endpoint,Auth,Description
GET,/api/health,None,Checks if model and API are online.
POST,/api/login,None,"Returns {access_token, refresh_token}."
GET,/api/me,JWT,"Returns user profile { id, email, confirmed }."
POST,/api/check,JWT,"Scans URL. Returns { url, score, label }."
POST,/api/report,JWT,Persists a scan result to the DB.

## PowerShell One-Liner Test:
$base="[http://127.0.0.1:5000](http://127.0.0.1:5000)"; $t=(iwr -Method POST "$base/api/login" -Body (@{email="user@example.com";password="password"} | ConvertTo-Json) -ContentType "json").Content | ConvertFrom-Json; iwr -Headers @{Authorization="Bearer $($t.access_token)"} -Method POST "$base/api/check" -Body (@{url="[http://example.com](http://example.com)"} | ConvertTo-Json) -ContentType "json"


## Deployment (Render)
PhishGuard is deployed on Render.com using:
- Custom Git-LFS build script
- Gunicorn production server
- Auto Alembic migrations on startup
- HTTPS with Hostinger DNS
- Mailtrap with full DKIM/DMARC validation

Build Command: bash scripts/render-build.sh
Start Command: PYTHONPATH=. FLASK_APP=wsgi.py python -m flask db upgrade && python -m gunicorn --preload ... wsgi:app
Email: Configured via Mailtrap with Hostinger DNS records (CNAME, DKIM, DMARC) for reliable delivery.
<img width="400" height="250" alt="image" src="https://github.com/user-attachments/assets/3c522313-268f-49f3-8f1c-58bb3ff1b427" />
<img width="500" height="200" alt="Screenshot 2025-11-28 201505" src="https://github.com/user-attachments/assets/39a98ad9-e56b-4d32-bf74-d2bdb09b991f" />



## PSQL Database Access Data
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/3e31c81b-034d-40df-a839-18c41775408d" />


## Hostinger DNS / MailTrap Configuration
<img width="600" height="300" alt="test" src="https://github.com/user-attachments/assets/6add9bd4-b120-482c-bcda-ab3f37d49894" />
<img width="500" height="500" alt="Screenshot 2025-11-28 202233" src="https://github.com/user-attachments/assets/cc47143c-d775-4375-83e5-4e7e4bd80eb5" />


## System / UML Diagrams
<img width="300" height="400" alt="Screenshot 2025-11-28 201943" src="https://github.com/user-attachments/assets/0f0f65b9-c464-4123-b3a7-d232d1bc5cda" />
<img width="300" height="300" alt="Screenshot 2025-11-28 201933" src="https://github.com/user-attachments/assets/a4cfdca9-a21b-4553-be9c-3958b015fbc1" />
<img width="600" height="400" alt="Screenshot 2025-11-28 195759" src="https://github.com/user-attachments/assets/b0fc8a80-69ea-4819-aefe-8fdcb135d3c5" />



## Bezalel Sabu - Full Stack Developer & ML Engineer
Backend API & Database Design
Chrome Extension Development (MV3)
Machine Learning Model Training & Integration
DevOps & Deployment (Render, DNS, Mailtrap)
