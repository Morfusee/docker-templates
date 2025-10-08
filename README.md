# 🐳 Home Server & Development Stack Overview

This document serves as a quick reference for how my **Docker-based home and development server stacks** are organized across different environments.
Each environment runs a separate Docker Compose configuration, typically grouped under distinct repositories or folders.

---

## 🏠 Home Server VM

### Stack: **komodo-traefik**

* **Repository:** `docker-templates`
* **Folder:** `komodo-traefik`
* **Compose file:** `compose.yml`
* **Purpose:**
  Acts as the **main Traefik instance** for the home server VM.
  Handles reverse proxying and routing for all containers running locally.

**Run command:**

```bash
docker compose -f compose.yml up -d
```

---

## 💻 Development Server

### Stack: **traefik-n8n**

* **Repository:** `docker-templates`
* **Folder:** `traefik-n8n`
* **Compose file:** `compose.yml`
* **Purpose:**
  Hosts **n8n** (workflow automation) behind its own **Traefik instance** for isolated testing and development.

**Run command:**

```bash
docker compose -f compose.yml up -d
```

---

## 🚀 Production Server

### Stack 1: **firestore-erd**

* **Repository:** `firestore-erd`
* **Folder:** `server`
* **Compose file:** `compose.bare.yml`
* **Purpose:**
  Runs the **main backend services** for the Firestore ERD production environment.

**Run command:**

```bash
docker compose -f compose.bare.yml up -d
```

### Stack 2: **traefik-standalone-proxy-socket**

* **Repository:** `docker-templates`
* **Folder:** `traefik-standalone-proxy-socket`
* **Compose file:** `compose.yml`
* **Purpose:**
  Provides a **dedicated Traefik proxy** that communicates via Docker socket, handling HTTPS, routing, and certificate management for production services.

**Run command:**

```bash
docker compose -f compose.yml up -d
```

---

## 🧑‍🎓 MMDC Student Portal

### Stack 1: **Backend**

* **Repository:** `mmdc-student-portal`
* **Folder:** `backend`
* **Compose file:** `compose.yml`
* **Purpose:**
  Runs the **backend API and services** for the MMDC Student Portal.

**Run command:**

```bash
docker compose -f compose.yml up -d
```

### Stack 2: **traefik-gateway**

* **Repository:** `docker-templates`
* **Folder:** `traefik-gateway`
* **Compose file:** `compose.yml`
* **Purpose:**
  Acts as the **gateway Traefik proxy** for the Student Portal backend and related services.

**Run command:**

```bash
docker compose -f compose.yml up -d
```

---

## 🧩 Summary Table

| Environment             | Repository / Folder                                | Compose File       | Purpose                    |
| ----------------------- | -------------------------------------------------- | ------------------ | -------------------------- |
| **Home Server**         | `docker-templates/komodo-traefik`                  | `compose.yml`      | Main Traefik reverse proxy |
| **Development Server**  | `docker-templates/traefik-n8n`                     | `compose.yml`      | n8n automation + Traefik   |
| **Production Server**   | `firestore-erd/server`                             | `compose.bare.yml` | Main Firestore ERD backend |
|                         | `docker-templates/traefik-standalone-proxy-socket` | `compose.yml`      | Production Traefik proxy   |
| **OVH VPS** | `mmdc-student-portal/backend`                      | `compose.yml`      | Student Portal backend     |
|                         | `docker-templates/traefik-gateway`                 | `compose.yml`      | Traefik gateway for portal |

---

## 🧠 Notes

* Each Traefik instance is self-contained and manages routing for its own stack.
* All stacks are started using `docker compose up -d` from their respective directories.
* Traefik configurations are stored in the `docker-templates` repo for reuse across environments.