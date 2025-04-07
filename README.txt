Here is a complete, ready-to-upload project folder for CI/CD Node.js deployment using GitHub Actions.
 This includes all code, structure, and a short README.md file.

---

Folder Structure:

nodejs-demo-app/
├── app.js
├── package.json
├── Dockerfile
├── README.md
└── .github/
    └── workflows/
        └── main.yml


---

1. app.js :
js
const http = require('http');

const server = http.createServer((req, res) => {
  res.end('Hello from Node.js App!');
});

server.listen(3000, () => {
  console.log('App running on port 3000');
});


---

2. package.json :
json
{
  "name": "nodejs-demo-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "test": "echo \"No tests yet\" && exit 0"
  }
}


---

3. Dockerfile :
dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["node", "app.js"]


---

4. .github/workflows/main.yml :
yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

  docker-deploy:
    runs-on: ubuntu-latest
    needs: build-test
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Log in to DockerHub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Build Docker image
        run: docker build -t sampleuser/nodejs-demo-app:latest .

      - name: Push to DockerHub
        run: docker push sampleuser/nodejs-demo-app:latest


---

5. README.md :
markdown
# Node.js CI/CD with GitHub Actions and Docker

This project demonstrates a basic CI/CD pipeline using GitHub Actions. It builds and deploys a Node.js app as a Docker image to DockerHub.

## Features
- Node.js HTTP server
- Dockerized application
- CI/CD workflow using GitHub Actions
- Automated build, test, and deploy to DockerHub

 Folder Structure :

nodejs-demo-app/
├── app.js
├── package.json
├── Dockerfile
└── .github/
    └── workflows/
        └── main.yml


Steps Performed :

1. Created a basic Node.js server.
2. Added a Dockerfile to containerize the app.
3. Created a GitHub Actions workflow to:
   - Install dependencies
   - Run tests
   - Build Docker image
   - Push it to DockerHub

Setup Instructions :

1. Clone or upload this repo to GitHub.
2. Add the following GitHub secrets:
   - `DOCKER_USERNAME` – your DockerHub username
   - `DOCKER_PASSWORD` – your DockerHub password or access token
3. Push to the `main` branch.
4. GitHub Actions will automatically run and deploy.

 DockerHub Image Example :

docker pull youruser/nodejs-demo-app:latest