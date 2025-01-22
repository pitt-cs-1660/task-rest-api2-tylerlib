[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/lnCVAvxz)
# **Assignment 1: Implementing Task Manager API Using FastAPI, SQLite, and Vagrant**

## **Overview**

In this assignment, you will build a **Task Manager API** using **FastAPI** and **SQLite**. The goal is to provide a consistent development environment using **Vagrant** to avoid "it works on my machine" issues. This setup will help you understand how to use Vagrant for local development and implement RESTful APIs using FastAPI and SQLite.

---

## Additional Documentation
- [vagrant.md](vagrant.md)
- [sqlite.md](sqlite.md)
- [rubric.md](rubric.md)
- [python.md](python.md)

---

## Prequisites 
- [vagrant](https://developer.hashicorp.com/vagrant/docs/installation) install
- [docker](https://docs.docker.com/engine/install/) or [virtualbox](https://www.virtualbox.org/wiki/Downloads) installed
  - use docker if you are on a M1/2/3 mac or arm based architecture
  - use VirtualBox if you are on a Windows machine, any x86 based machine can use VirtualBox

---

## **Learning Objectives**

- Understand how to define API routes.
- Implement different HTTP methods (`GET`, `POST`, `PUT`, `DELETE`) in FastAPI.
- Use Vagrant to create a consistent and portable development environment.

---

## **Assignment Tasks**

You are provided with a python project that is using the FastAPI webserver framework  and a **`Vagrantfile`** that is completed. Your job is to implement the following API routes:
1. **`GET /tasks/`**: Retrieve all tasks from the SQLite database.
2. **`POST /tasks/`**: Create a new task in the SQLite database.
3. **`PUT /tasks/{task_id}/`**: Update an existing task in the SQLite database.
4. **`DELETE /tasks/{task_id}/`**: Delete a task from the SQLite database.
5. Return JSON message `{"message": "Welcome to the Cloud Computing!"}` from the index `/` route .

---

## **Grading Rubric**


### **Development Environment Setup**

We are using **Vagrant** to ensure a consistent local development experience. The Vagrant configuration uses **Docker** as the provider to run an **Ubuntu-based** development environment.

### **Installation Instructions**

1. **Install Docker**:  
   Download and install Docker Desktop from:
   - [Windows/macOS Docker Desktop](https://www.docker.com/products/docker-desktop/)
   - **Linux:** Install using your package manager (e.g., `sudo apt install docker.io`).

2. **Install Vagrant**:  
   Download and install Vagrant from:
   - [Vagrant Downloads](https://developer.hashicorp.com/vagrant/downloads)

3. **Verify Installations**:
   Run the following commands to verify the installations:
   ```bash
   docker --version
   vagrant --version
   ```
4. **Install Vagrant Docker Plugin**:
   Install the Vagrant Docker plugin:
   ```bash
   vagrant plugin install vagrant-docker-compose
   ```
   you can refer to the [vagrant.md](vagrant.md) for more information.
---

## **Using Vagrant for Local Development**

### **Vagrant Workflow**

1. **Start the Development Environment**:
    ```bash
   # note: if you have a arm based computer, M1/2/3 mac. Windows machines experience issues with this setup
   vagrant up --provider=docker
   
   # note: Windows users, or anyone with x86 based architecture
   vagrant up --provider=virtualbox
   ```
   - This command starts the Docker container using the Vagrant configuration.
   - The first time this runs, it will pull the base image and create the container.

2. **SSH into the Development Environment**:
   ```bash
   vagrant ssh
   ```
   - This command opens an SSH session into the running container.

3. **Navigate to the Project Directory**:
   Inside the container:
   ```bash
   cd /home/vagrant/app
   ```

4. **Create a Virtual Environment**:
   Inside the `app` directory:
   ```bash
   python3.11 -m venv .venv
   source .venv/bin/activate
   ```

5. **Install Dependencies**:
   Inside the virtual environment:
   ```bash
   poetry install --no-root
   ```

6. **Run the FastAPI Server**:
   ```bash
   poetry run uvicorn cc_simple_server.server:app --reload --host 0.0.0.0 --port 8000
   ```

   - Access the API documentation at: `http://localhost:8000/docs`.

7. **Stop the Vagrant box**:
   To stop the development environment:
   ```bash
   vagrant halt
   ```

8. **Restart and Reprovision the Environment**:
   If you change the `Vagrantfile` or need to reapply provisioning:
   ```bash
   vagrant reload --provision
   ```

9. **Destroy the Environment**:
   To remove the container entirely:
   ```bash
   vagrant destroy -f
   ```

---

## **Assignment Description**

The `server.py` file contains the FastAPI routes for managing tasks in the SQLite database. You are responsible for implementing the controllers for the following endpoints:

### **1. `GET /tasks/` (Retrieve All Tasks)**

- **Description:** Retrieve a list of all tasks.
- **Implementation Details:**  
  Query the database to get all tasks and return a list of tasks.

---

### **2. `POST /tasks/` (Create a New Task)**

- **Description:** Create a new task with `title`, `description`, and `completed` fields.
- **Implementation Details:**  
  Insert the task into the `tasks` table and return the created task with its `id`.

---

### **3. `PUT /tasks/{task_id}/` (Update an Existing Task)**

- **Description:** Update the `title`, `description`, and `completed` status of a task.
- **Implementation Details:**  
  Check if the task exists, update its values, and return the updated task.  
  If the task does not exist, return `404 Not Found`.

---

### **4. `DELETE /tasks/{task_id}/` (Delete a Task)**

- **Description:** Delete a task by its `id`.
- **Implementation Details:**  
  Check if the task exists and delete it.  
  If the task does not exist, return `404 Not Found`.

---

## **Models Provided**

You are provided with the following Pydantic models for request and response validation:

- **`TaskCreate` (Request Model):**
  ```python
  from pydantic import BaseModel

  class TaskCreate(BaseModel):
      title: str
      description: str
      completed: bool
  ```

- **`TaskRead` (Response Model):**
  ```python
  class TaskRead(TaskCreate):
      id: int
  ```

---

## **Database Utility Functions**

- **`init_db()`**: Initializes the SQLite database and creates the `tasks` table.
- **`get_db_connection()`**: Opens a connection to the SQLite database.

---

## **Running the Application**

1. **Run the FastAPI Server**:
   ```bash
   poetry run uvicorn cc_simple_server.server:app --reload --host 0.0.0.0 --port 8000
   ```

2. **Test the Endpoints From Outside the Vagrant Box**:
   - **Create a Task (`POST /tasks/`)**:
     ```json
     {
       "title": "Finish assignment",
       "description": "Complete all API routes",
       "completed": false
     }
     ```
   - **Get All Tasks (`GET /tasks/`)**:
     ```json
     [
       {
         "id": 1,
         "title": "Finish assignment",
         "description": "Complete all API routes",
         "completed": false
       }
     ]
     ```

---

## **Submission Instructions**

- Implement all API routes (`GET`, `POST`, `PUT`, `DELETE`).
- Ensure your API passes all tests in Github Actions.
- Push your code to the main branch in GitHub and submit the repository URL on Canvas.

---

## **Additional Notes**

- The Vagrant/Docker setup abstracts the environment setup so you can focus on development.
- You can use the provided `pytest` tests to verify your endpoints.

Happy coding! 🚀