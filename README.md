# Reverse Proxy (NGINX)
 
Added NGINX as a reverse proxy so all traffic goes through a single entry point on port 80. The frontend and backend services are no longer exposed on their own ports. The browser only talks to `http://localhost`, and NGINX routes each request to the right container over the Docker network.
 
## Diagram
 
```mermaid
flowchart TD
    B["🌐 Browser<br/>http://localhost"] -->|port 80| N{{"NGINX<br/>reverse proxy"}}
 
    N -->|"/"| F["Frontend<br/>React · :3000"]
    N -->|"/api/students/"| S["Student service<br/>Go · :5001"]
    N -->|"/api/teachers/"| T["Teacher service<br/>Java · :5002"]
    N -->|"/api/employees/"| E["Employee service<br/>Python · :5003"]
    N -->|"/api/profile/"| P["Profile service<br/>Node.js · :5004"]
    N -->|"/storage/"| O[("RustFS<br/>object storage · :9000")]
 
    S & T & E & P --> M[("MongoDB")]
    P -->|upload jobs| R[["RabbitMQ"]]
    R --> W["Thumbnail worker<br/>Python"]
    P & W --> O
 
    classDef proxy fill:#6b5ca5,color:#fff,stroke:#000,stroke-width:3px,font-weight:bold
    classDef svc fill:#fff,color:#000,stroke:#000,stroke-width:3px,font-weight:bold
    classDef infra fill:#fff,color:#000,stroke:#000,stroke-width:3px,font-weight:bold
    class N proxy
    class B,F,S,T,E,P svc
    class O,M,R,W infra
```
 
## Routing
 
| Path | Upstream | Service | Backend receives |
|---|---|---|---|
| `/` | `frontend:3000` | Frontend | `/` |
| `/api/students/` | `studentservice:5001` | Student service | `/students` |
| `/api/teachers/` | `teacherservice:5002` | Teacher service | `/teachers` |
| `/api/employees/` | `employeeservice:5003` | Employee service | `/employees` |
| `/api/profile/` | `profileservice:5004` | Profile service | `/profile/...`, `/photos/...` |
| `/storage/` | `object-storage:9000` | RustFS (photo storage) | `/school-media/...` |
 
The trailing slash in `proxy_pass http://students/;` strips the `/api/students` prefix before the request reaches the service.
 
## Photo storage
 
Photos are kept in private buckets and served through short-lived signed URLs. The profile service sets `S3_PUBLIC_PATH=/storage`, so the browser gets links like `/storage/school-media/...`. NGINX strips `/storage` and sets `Host: object-storage:9000` so the signature still matches when RustFS checks it.
 
## Run and verify
 
```bash
docker compose up -d --build
curl -i http://localhost/api/students/students
```
 
Then open **http://localhost**.
 