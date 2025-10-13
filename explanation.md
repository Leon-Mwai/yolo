# explanation.md

## 1. Choice of Base Images
- **Frontend (React)**:  
  Used `node:18-alpine` as the base image — it’s lightweight, secure, and optimized for production Node environments.  
  Multi-stage build ensures only the final static files are shipped in the final image, reducing size.
  
- **Backend (Node.js + Express)**:  
  Used a two-stage build:
  - First stage: `node:18-alpine` to install dependencies and build the project.  
  - Second stage: `alpine:3.16` as a minimal runtime environment with Node.js installed — this keeps the image small and secure.

- **Database (MongoDB)**:  
  Used the official `mongo` image from Docker Hub. It’s production-ready and supports data persistence through mounted volumes.

---

## 2. Dockerfile Directives Used
### Frontend
- `WORKDIR /usr/src/app` → defines working directory for consistency.  
- `COPY package*.json ./` → ensures cached layer for dependency installation.  
- `RUN npm install` → installs dependencies.  
- `RUN npm run build` → builds the React app into static files.  
- Second stage uses `RUN npm install -g serve` → installs a lightweight static file server.  
- `CMD ["serve", "-s", "build", "-l", "3000"]` → runs the app on port 3000.

### Backend
- Stage 1 installs production dependencies and copies the project.  
- Stage 2 runs on Alpine and installs Node.js runtime only (no dev deps).  
- `CMD ["node", "server.js"]` → launches the Express API on port 5000.

---

## 3. Docker-Compose Networking
- Defined a custom **bridge network** (`app-net`) for inter-container communication.  
- This lets containers talk to each other using **service names** instead of IPs.  
- Each service exposes its port:
  - **Frontend** → `3000:3000`
  - **Backend** → `5000:5000`
  - **MongoDB** → `27017:27017`
- The environment variable `MONGO_URL=mongodb://mongo:27017/yolo` ensures both the backend and frontend can reach the Mongo container via Docker’s internal DNS.

---

## 4. Volumes
- Created a named volume `mongo_data` in docker-compose:
  ```yaml
  volumes:
    - mongo_data:/data/db
  ```
- This ensures data persistence — added products remain even after containers are stopped or rebuilt.

---

## 5. Git Workflow
- Forked and cloned the starter repository.  
- Created a new feature branch `docker-setup`.  
- Incrementally committed Dockerfiles, tested containers locally, and refined configurations.  
- Merged to `main` after verifying the frontend and backend communicate correctly.  
- Pushed the final version to GitHub for review.

---

## 6. Debugging and Troubleshooting
- Used `docker logs <container_name>` to check for errors.  
- Verified API connection using:
  ```bash
  curl http://localhost:5000/api/products
  ```
  Confirmed database persistence after restarting containers.  
- Checked network issues with `docker network inspect app-net`.  
- Rebuilt containers using:
  ```bash
  docker-compose down -v
  docker-compose up --build
  ```
  to ensure a clean, reproducible build.

---

## 7. Good Practices
- Used clear image tags for version control:  
  - `leonmwai/yolo-frontend:v1`  
  - `leonmwai/yolo-backend:v1`
- Added `.dockerignore` to exclude unnecessary files (`node_modules`, `.env`, `build`, etc.).  
- Multi-stage builds for minimal image size.  
- Consistent use of `alpine` images for performance and security.  
- Environment variables declared explicitly in `docker-compose.yml` for clarity.



---

##  Summary
This setup successfully runs:
- **Frontend** → React app on port 3000  
- **Backend** → Express API on port 5000  
- **MongoDB** → Persistent data store on port 27017  

All containers communicate over a shared bridge network, and data persists through volumes.  
Anyone cloning your repo can run the entire e-commerce platform with one command:

```bash
docker-compose up --build
```
