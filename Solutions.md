# Docker Practical Assessment: Core Container Operations

**Student Name:** Tanish Bhandari
**Date:** 26th Aug 2026

## Part 1: The "No Magic" Rule - Researching Configurations & Ports

Before deploying, you must determine how to configure your containers. Environment variables and default ports are not magic; they are explicitly documented by the image maintainers in the documentation and the `Dockerfile`.

**Task 1: MariaDB Environment Variables**
Go to the official Docker Hub page for the `mariadb` image. Find the **Environment Variables** documentation section.
1. What environment variable is used to instruct the container to generate a random root password?
   * **Answer:** MARIADB_RANDOM_ROOT_PASSWORD=yes , to use add -e in command

**Task 2: Finding Default Ports via Dockerfile**
Not all documentation lists the default ports right at the top. Sometimes you need to check the source. On Docker Hub, find the supported tags for `nginx` and `redis`, click on the link to view their `Dockerfile` (usually hosted on GitHub), and look for the `EXPOSE` instruction.
1. What default port is exposed in the `nginx` Dockerfile?
   * **Answer:** 80
2. What default port is exposed in the `redis` Dockerfile?
   * **Answer:** 6379

---

## Part 2: Infrastructure Deployment

You are provisioning infrastructure for a new web application. Deploy the following services directly from your command line. *Write the exact command you used below each prompt.*

**Task 3: Deploy the Web Server**
* **Image:** `nginx`
* **Container Name:** `frontend-web`
* **Requirements:** Run in the background (detached). Map port `8080` on your host machine to the default `nginx` port you discovered in Part 1.
* **Command:**
  ```bash
	docker container run -d --name frontend-web -p 8080:80 nginx
  ```

**Task 4: Deploy the Database**
* **Image:** `mariadb`
* **Container Name:** `backend-db`
* **Requirements:** Run in the background (detached). Use the environment variable you found in Part 1 to generate a random root password. Do NOT map any ports to the host.
* **Command:**
  ```bash
	docker container run -d --name backend-db -e MARIADB_RANDOM_ROOT_PASSWORD=yes mariadb
  ```

**Task 5: Deploy the Cache Layer**
* **Image:** `redis`
* **Container Name:** `session-cache`
* **Requirements:** Run in the background (detached). Map port `6379` on your host machine to the default `redis` port you discovered in Part 1.
* **Command:**
  ```bash
	docker container run - d --name session-cache -p 6379:6379 redis
  ```

---

## Part 3: Verification & Inspection

Now that your infrastructure is running, you need to verify its status and extract important operational data.

**Task 6: Check Container Status**
* Write the command used to list all currently active, running containers to verify your deployments.
* **Command:**
  ```bash
	docker ps
  ```

**Task 7: Extract the Database Password**
* Because you used the random password environment variable, MariaDB generated a secure password on boot and printed it to the standard output. Write the command to view the container's logs so you can find this password.
* **Command:**
  ```bash
	docker logs - f <container-id/container name> and do a grep
  ```
* **Extracted Password (write what you found in the logs):**
  __________________________________________________

**Task 8: Monitor Container Resources**
* Write the command to display a live stream of resource usage statistics (such as RAM, CPU, and Block I/O) for all running containers.
* **Command:**
  ```bash
	docker stats
  ```

**Task 9: Inspect Running Processes**
* Write the command to display the running processes specifically inside the `frontend-web` container.
* **Command:**
  ```bash
	docker container top frontend-web
  ```

---
*End of Assessment*
