# Cars Dealership - Full Stack Application Development Capstone

Best Cars is a national car retailer in the U.S. This project is a web application that lists the dealership branches across the country, shows the reviews for each dealer, and lets registered users post their own reviews. Every review goes through a sentiment analyzer so visitors can quickly see if it is positive, neutral or negative.

## Features

- List all dealerships, or filter them by state
- View dealer details and the reviews for each dealer
- User sign up, login and logout
- Logged in users can post a review for a dealer
- Sentiment analysis on every review (positive / neutral / negative)
- Admin page to manage car makes and car models

## Tech Stack

| Part | Technology |
| --- | --- |
| Frontend | React, HTML, CSS, Bootstrap |
| Main app | Django (SQLite for users, car makes and models) |
| Dealers & reviews API | Node.js, Express, MongoDB (Docker) |
| Sentiment analyzer | Flask + NLTK, deployed on IBM Cloud Code Engine |
| CI/CD | GitHub Actions (flake8 + JSHint) |
| Deployment | Docker, Kubernetes |

## Project Structure

```
server/
  djangoproj/          Django project settings and urls
  djangoapp/           Django app (views, models, proxy services)
    microservices/     Flask sentiment analyzer
  database/            Node.js + MongoDB backend for dealers and reviews
  frontend/            React app + static pages (Home, About, Contact)
  deployment.yaml      Kubernetes deployment
  Dockerfile
```

## Running Locally

**1. Database service (Node + MongoDB)**

```bash
cd server/database
docker build . -t nodeapp
docker compose up
```

**2. Sentiment analyzer**

```bash
cd server/djangoapp/microservices
docker build . -t senti_analyzer
docker run -p 5050:5000 senti_analyzer
```

**3. Frontend build**

```bash
cd server/frontend
npm install
npm run build
```

**4. Django app**

```bash
cd server
pip install -r requirements.txt
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py createsuperuser
python3 manage.py runserver
```

Set `backend_url` and `sentiment_analyzer_url` in `server/djangoapp/.env` before running.

## API Endpoints

| Endpoint | Description |
| --- | --- |
| `/djangoapp/login` | Login user (POST) |
| `/djangoapp/logout` | Logout user |
| `/djangoapp/register` | Register new user (POST) |
| `/djangoapp/get_cars` | All car makes and models |
| `/djangoapp/get_dealers` | All dealers |
| `/djangoapp/get_dealers/<state>` | Dealers in a state |
| `/djangoapp/dealer/<id>` | Dealer details |
| `/djangoapp/reviews/dealer/<id>` | Reviews for a dealer with sentiment |
| `/djangoapp/add_review` | Post a review (POST, logged in users only) |
