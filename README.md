# Kindergarten Project – Run Locally
 
**Objective:** Run all services (Student, Teacher, Employee) and the frontend locally. Verify that the system works.
 
----------
 
## **Step 1: Prepare MongoDB**
 
1.  Decide whether to run **MongoDB locally** or via **Docker**.
    
2.  **Local installation:**
    
    -   Download the latest MongoDB from the official website.
        
    -   Install and start the MongoDB service.
        
    -   Make sure it runs on the default port `27017`.
        
3.  **Docker option:**
    
    -   Pull the latest MongoDB image.
        
    -   Run a MongoDB container.
        
    -   Ensure it is accessible on port `27017`.
        
> **Goal:** other services should be able to connect to MongoDB before starting any backend.
 
 
 
 
## **Step 2: Run Employee Service (Python)**
 
**Objective:** Start the Python backend service so the frontend can communicate with it.
 
----------
 
### **Requirements**
 
-   Must have **Python 3** installed on your machine.
    
-   MongoDB should be running (from Step 1).
    
----------
 
### **Instructions**
 
1.  **Navigate to the Employee Service folder**
    
    -   Open your terminal and go to the `employeeservice` directory.
        
2.  **Create a virtual environment** _(only needed if running locally, not in Docker)_
    
    
-   **Install dependencies**
    
-   `pip install -r requirements.txt` 
    
-   **Set required environment variables**
    
`export MONGODB_URI="mongodb://myUser:myPassword@mongo:27017/kindergarten?authSource=admin"` 
 
`export DATABASE_NAME="kindergarten"` 
 
-   **Run the Python application**
    
1.  `python3 app.py` 
    
2.  **Verify the service is running**
    
    -   The terminal should show that the backend is listening on a port ( `5003`).
       
----------
 
 
 
## **Step 3: Run Frontend (React)**
 
**Objective:** Start the React frontend and verify that it can communicate with all backend services (Student, Teacher, Employee).
 
----------
 
### **Requirements**
 
-   Node.js and npm installed on your machine. version `24` recommended 
    
-   backend services running from previous steps.
    
----------
 
### **Instructions**
 
1.  **Navigate to the frontend folder**
    
    -   Open your terminal and go to the `frontend` directory.
        
2.  **Install dependencies**
    
-   `npm install` 
    
-   **Start the frontend application**
    
1.  `npm start` 
    
    -   This will start the frontend server, usually on `http://localhost:3000` (or Vite default port if using Vite: `5173`).
        
2.  **Verify communication with backend services**
    
    -   Open the frontend in a browser.
        
    -   Go to the **Employee, Teacher, and Student tabs**.
        
    -   Make sure the frontend is able to fetch and display data from each backend.
        
3.  **Check the URLs used by the frontend**
    
    -   Verify that the **Employee service URL** matches your local backend URL (for example, `http://localhost:5003`).
        
    -   If needed, adjust the frontend configuration so that it points to the correct backend URLs.
        
----------
 
> ✅ **Checkpoint:**
> 
> -   Frontend loads successfully.
>     
> -   Each tab fetches data correctly from the corresponding backend service.
>     
> -   Green/red status indicators (if implemented) show the service health.
>     
 
----------
 
----------
 
## **Step 4: Run Student Service (Go)**
 
**Objective:** Start the Go backend for the Student service so the frontend can fetch student data.
 
----------
 
### **Requirements**
 
-   Latest **Go** installed on your machine.
    
-   MongoDB running (from Step 1).
    
----------
 
### **Instructions**
 
1.  **Navigate to the Student Service folder**
    
    -   Open your terminal and go to the `studentservice` directory.
        
2.  **Install dependencies**
    
-   `go mod tidy` 
    
    -   This will download and install all required Go modules.
        
-   **Set required environment variables**
    
-   `export MONGODB_URI="mongodb://myUser:myPassword@172.22.0.2:27017/kindergarten?authSource=admin"  export DATABASE_NAME="kindergarten"` 
    
-   **Run the Go application**
    
-   `go run main.go` 
    
    -   The backend should start and listen on a port ( `5001`).
        
-   **Verify service availability**
    
    -   Open the frontend in a browser.
        
    -   Go to the **Student tab**.
        
    -   Confirm that the frontend can fetch and display student data.
## **Step 5: Run Teacher Service (Java)**
 
**Objective:** Start the Java backend for the Teacher service so the frontend can fetch teacher data.
 
----------
 
### **Requirements**
 
-   Java (OpenJDK 17) installed.
    
-   Maven installed.
    
-   MongoDB running (from Step 1).
    
----------
 
### **Instructions**
 
1.  **Update system packages**
    
-   `sudo apt update` 
    
-   **Install Java runtime**
    
-   `sudo apt install openjdk-17-jre`
    `java -version` 
    
    -   Ensure Java 17 or higher is installed.
        
-   **Install Maven**
    
-   `sudo apt install maven` 
    
-   **Navigate to the Teacher Service folder**
    
    -   Open your terminal and go to the `teacherservice` directory.
        
-   **Build the project using Maven**
    
-   `mvn clean package` 
    
    -   This will create a `.jar` file in the `target` folder.
 
`export MONGODB_URI="mongodb://myUser:myPassword@mongo:27017/kindergarten?authSource=admin"` 
 
`export DATABASE_NAME="kindergarten"` 
 
        
-   **Run the Java application**
    
1.  `java -jar target/<jar-file-name>.jar` 
    
    -   Replace `<jar-file-name>` with the actual name of the `.jar` generated by Maven.
        
    -   The backend should start and listen on a port ( `5002`).
        
2.  **Verify service availability**
    
    -   Open the frontend in a browser.
        
    -   Go to the **Teacher tab**.
        
    -   Confirm that the frontend can fetch and display teacher data.
        
----------
 
> ✅ **Checkpoint:**
> 
> -   Teacher service runs successfully.
>     
> -   Frontend is able to communicate with the Teacher backend using the correct URL.
>     
 
----------
 
## **Step 6: Run the Whole Project with Docker (Containerized)**
 
**Objective:** Run MongoDB, all backend services (Student, Teacher, Employee), and the frontend together with **Docker Compose**. Every service in this project has been containerized, so you don't need to install Python, Go, Java, Maven, or Node.js on your machine.
 
----------
 
### **What was containerized**
 
| Service | Stack | Container port | Dockerfile location |
|---|---|---|---|
| Frontend | React (built with Node.js `24`, served by nginx) | `3000` | `frontend/Dockerfile` |
| Student Service | Go | `5001` | `studentservice/Dockerfile` |
| Teacher Service | Java 17 + Maven | `5002` | `teacherservice/Dockerfile` |
| Employee Service | Python 3 | `5003` | `employeeservice/Dockerfile` |
| MongoDB | `mongo:7.0` image | `27017` | defined in `docker-compose.yml` |
 
-   Each service has its own **Dockerfile**.
    
-   A **`docker-compose.yaml`** file in the project root starts all containers together on one shared network.
    
----------
 
### **Requirements**
 
-   **Docker** installed on your machine.
    
-   **Docker Compose** installed (included with Docker Desktop).
    
----------
 
### **Instructions**
 
1.  **Navigate to the project root folder**
    
    -   Open your terminal and go to the folder that contains `docker-compose.yaml`.
        
2.  **Build and start all containers**
    
    -   `docker compose up -d --build`
        
3.  **Check that all containers are running**
    
    -   `docker compose ps`
        
    -   All containers should show a status of `Up`.
        
4.  **Open the application**
    
    -   Go to `http://localhost:3000` in your browser.
        
5.  **Note about the MongoDB connection**
    
    -   Inside Docker, services connect to MongoDB using the **service name** instead of `localhost` or an IP address:
        
    -   `mongodb://myUser:myPassword@mongodb:27017/kindergarten?authSource=admin`
        
6.  **View logs** _(if something is not working)_
    
    -   `docker compose logs -f <service-name>`
        
7.  **Stop all containers**
    
    -   `docker compose down`
        
    -   To also remove the MongoDB data: `docker compose down -v`
        
----------
 
> ✅ **Checkpoint:**
> 
> -   All containers show `Up` in `docker compose ps`.
>     
> -   Frontend loads successfully at `http://localhost:3000`.
>     
> -   Each tab (Student, Teacher, Employee) fetches data correctly from its backend service.
>     
 
----------
