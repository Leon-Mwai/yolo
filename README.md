# YOLO E-Commerce Deployment (Ansible + Docker + Vagrant)

##  Project Overview
This project automates the deployment of a containerized e-commerce platform using **Ansible**, **Docker**, and **Vagrant**.  
The application stack consists of three main components:
- **MongoDB** — Database
- **Backend (Node.js)** — API server
- **Frontend (React)** — User interface

The deployment is fully automated through **Ansible playbooks** and **roles**, making the setup reproducible, clean, and easy to manage.

---

## Technologies Used
- **Ansible** — Configuration management & orchestration  
- **Docker** — Containerization  
- **Vagrant** — Virtual machine provisioning  
- **Ubuntu 20.04** — Base server environment  
- **Node.js + MongoDB + React** — Application stack  

---

## ⚙️ Project Structure

yolo/
├── Vagrantfile
├── playbook.yml
├── roles/
│ ├── setup-mongodb/
│ ├── backend-deployment/
│ ├── frontend-deployment/
│
├── vars/
│ └── main.yml
├── README.md
└── explanation.md

---

## How to Run the Project

### 1️ Start the Virtual Machine
```bash
vagrant up
ansible-playbook playbook.yml

The playbook will:

Clone the project from GitHub

Pull the necessary Docker images

Start the following containers:

mongo on port 27017

yolo-backend on port 5000

yolo-frontend on port 3000
 Roles Description
setup-mongodb

Installs Docker if missing

Pulls the official MongoDB image

Creates and runs the Mongo container

backend-deployment

Clones the backend repository

Builds and runs the Node.js API container

Connects it to the MongoDB service

frontend-deployment

Clones the frontend repository

Builds and runs the React container

Links it with the backend API endpoint

Verification

Once the playbook completes:

Access the frontend via http://localhost:3000

Add a new product to verify database persistence

Confirm backend connectivity at http://localhost:5000