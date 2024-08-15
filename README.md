<p align="center">
<img src="https://i.imgur.com/xJ7Nzqj.png"/>
</p>

<h1>Installing Peppermint Ticketing System on Ubuntu</h1>
This guide will walk you through the process of installing the <a href="https://github.com/bztoothman/Installing-Docker-and-Docker-Compose">Peppermint ticketing system </a> using Docker and Docker Compose on an Ubuntu machine. By the end of this guide, you will have a fully functional Peppermint ticketing system accessible via your web browser.

<h2>Prerequisites</h2>

- Ubuntu 24.04 LTS Server (Virtual Machines/Compute)
- <a href="https://github.com/bztoothman/Installing-Docker-and-Docker-Compose"> Docker and Docker Compose installed

<h2>Operating Systems Used </h2>

- Ubuntu 24.04 LTS Server (terminal only): <a href="https://ubuntu.com/download/server">Ubuntu Server 24.04 LTS</a>.

<h2>Step 1: Find the Machine's IP Address</h2>

<p>
</p>
<p>
First, you'll need to identify the IP address of your machine, which will be used to access the Peppermint web interface.

Run the following command to find the IP address:

 ```bash
ip -4 addr show | grep inet
```
You should see an IP address like 192.168.0.10. Note this down, as it will be used later.
</p>
<br />

<h1>Step 2: Set Up the Project Directory</h1>
 
<p>
Next, create a directory for Peppermint and navigate into it:
</p>
<p>
 
 ```bash
mkdir peppermint
cd peppermint
 ```
<h1>Step 3: Create the Docker Compose File</h1>
<p>
 Inside the peppermint directory, create a new file named docker-compose.yml using nano or your preferred text editor:
  
 ```bash
nano docker-compose.yml
 ```
Copy and paste the following configuration into the file:

 ```bash
version: "3.1"
 
services:
  peppermint_postgres:
    container_name: peppermint_postgres
    image: postgres:latest
    restart: always
    ports:
      - 5432:5432
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: peppermint
      POSTGRES_PASSWORD: 1234
      POSTGRES_DB: peppermint
 
  peppermint:
    container_name: peppermint
    image: pepperlabs/peppermint:latest
    ports:
      - 3000:3000
      - 5003:5003
    restart: always
    depends_on:
      - peppermint_postgres
    healthcheck:
      test: ["CMD", "sh", "-c", "wget --spider $$BASE_URL"]
      interval: 30s
      timeout: 10s
      retries: 3
    environment:
      DB_USERNAME: "peppermint"
      DB_PASSWORD: "1234"
      DB_HOST: "peppermint_postgres"
      SECRET: 'peppermint4life'
      API_URL: "http://server-ip:5003"
 
volumes:
 pgdata:
 ```
</p>
<br />

<h2>Important:</h2>
<p>Replace 192.168.0.10 in API_URL with the IP address you obtained in Step 1.</p>
Save the file by pressing CTRL + X, then Y to confirm, and ENTER to exit.

<h1>Step 4: Start the Peppermint Ticketing System</h1>
<p>
Now that your Docker Compose file is ready, start the Peppermint system by running:
  
 ```bash
docker-compose up -d
```
Docker will download the necessary images and start the containers in detached mode.
</p>
<h1>Step 5: Access the Peppermint Web Interface</h1>
<p>Once the containers are up and running, you can access the Peppermint ticketing system via any web browser.

Type the following URL into your browser, replacing 192.168.0.10 with your machine's IP address:

 ```bash
http://192.168.0.10:3000
```
</p>
<h1>Conclusion</h1>
<p>
You should now have a fully functional Peppermint ticketing system running on your Ubuntu machine. If you encounter any issues, ensure that the IP address is correct and that all services are running properly.

To log in to the Peppermint system, use the following credentials:

- Username: admin@admin.com
- Password: 1234
</p>
<br />
