# MultienvApp

Deploying a multi-environment ticket management application on your local computer. The application consists of two Flask services: development backend, production backend, and a frontend service (React) that interacts with both environments.

## Infrastructure Setup

- Start your Docker Engine.
- Set up your environment variable with MONGO_URI, copy your connection string from Mongo_DB Compass.

## Docker Configuration

- Create Dockerfile for backend development environment using below code.
  
      FROM python:3.9-slim
      WORKDIR /app
      COPY . .
      RUN pip install --no-cache-dir -r requirements.txt
      EXPOSE 3001
      CMD ["python","app.py"]


   <img width="602" height="302" alt="image" src="https://github.com/user-attachments/assets/7e90d807-c82e-4f8f-9f91-eb129f21a607" />


- Create Dockerfile for backend production environment using below code.

      FROM python:3.9-slim
      WORKDIR /app
      COPY . .
      RUN pip install --no-cache-dir -r requirements.txt
      EXPOSE 3002
      CMD ["python","app.py"]

  <img width="602" height="305" alt="image" src="https://github.com/user-attachments/assets/80d5e72f-4389-4785-91ac-8558e8c63223" />


- Create Dockerfile for frontend service using below code.

      FROM node:18-alpine
      WORKDIR /app
      COPY package*.json ./
      RUN npm install
      COPY . .
      RUN npm run build
      EXPOSE 3000
      CMD ["npm","start"]

  <img width="602" height="308" alt="image" src="https://github.com/user-attachments/assets/a1ecb6ac-281c-4fa5-b874-0eda8af0edf6" />

  
- Create docker-compose.yml file and ensure proper environment variable configuration.

      services:
      backend-dev:
      environment:
        - MONGO_URI=mongodb+srv://<username>:<password>@hv-mongo-cluster.e9hkegp.mongodb.net/devdb?retryWrites=true&w=majority
      image: backend-dev
      build:
        context: ./backend/dev
        dockerfile: Dockerfile
      ports:
        - "3001:3001"

      backend-prod:
      environment:
        - MONGO_URI=mongodb+srv://<username>:<password>@hv-mongo-cluster.e9hkegp.mongodb.net/proddb?retryWrites=true&w=majority
      image: backend-prod
      build:
       context: ./backend/prod
        dockerfile: Dockerfile
      ports:
        - "3002:3002"

      frontend:
      image: frontend
      build:
        context: ./frontend
        dockerfile: Dockerfile
      ports:
        - "3000:3000"

  ## Application Deployment

- Run the docker-compose file using docker compose up command.
 
      <img width="602" height="256" alt="image" src="https://github.com/user-attachments/assets/2ff36150-912d-4ad3-86b5-6099f5ebe2ab" />

- Check if all the images are running in Docker Desktop.

      <img width="1606" height="561" alt="image" src="https://github.com/user-attachments/assets/451371f7-470f-4eec-afa8-a790ea594b36" />
  




    
