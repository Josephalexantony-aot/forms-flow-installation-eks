---
sidebar_position: 1
---

# Windows

**Estimate time: 20 minutes (approximate)**

## Prerequisites

Before starting the installation, ensure your system meets the following requirements:

### 1. Software Installation & Configuration
* **Docker Desktop**: Download and install [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/).
* **WSL 2 Backend (Recommended)**: Ensure WSL 2 is installed and enabled in Windows Features. Configure Docker Desktop to use the WSL 2 backend for better performance.
* **Docker Compose**: Included automatically with Docker Desktop. Verify it is active.

### 2. System Resource Requirements
Running the entire formsflow.ai stack requires sufficient system resources. Ensure Docker Desktop is allocated at least:
* **Memory (RAM)**: Minimum **4 GB** (8 GB or more recommended). *Your PC should have at least 8 GB-16 GB total RAM.*
* **CPUs**: Minimum **4 Cores**.
* **Disk Space**: At least **10 GB** of free disk space.

### 3. General Requirements
* **Active Internet Connection**: Required to download the installer ZIP and pull Docker images (approx. 2-3 GB).
* **Web Browser**: A modern web browser (e.g., Google Chrome, Mozilla Firefox, Microsoft Edge) to access the formsflow.ai UI.

## Step 1: Download the GitHub Repository

In this initial step, download the **Forms Flow AI Deployment** GitHub repository by clicking [**here**](https://github.com/AOT-Technologies/forms-flow-ai-deployment/archive/refs/heads/release/v7.3.0.zip).

A zip file will be downloaded.

## Step 2: Extract the downloaded .zip file

![extracted folder preview](../static/img/extracted-file.png)

**Now double-click to open the extracted folder and go to the `scripts` folder:**

![scripts folder preview](../static/img/scripts-folder.png)

There you can see the **install.bat** file:

![.bat file preview](../static/img/install-file.png)

## Step 3: Install using the install.bat file

a) If you encounter this warning from **Microsoft Defender**, click **Run anyway** to proceed with the installation:

![warning preview](../static/img/microsoft-defender-warning.png)

b) The installation process will start and you will be presented with an IP address verification question. Verify that the IP address shown is correct. If it is correct, enter **`y`**, otherwise enter **`n`** to provide the correct IP:

![IP address prompt](../static/img/7.3.0/latest/ip-address-prompt.png)

c) When asked whether to install **Analytics**, enter **`y`** to include Redash, or **`n`** to skip. If installing Analytics, you will be asked to provide the Redash API key later:

![analytics installation prompt](../static/img/7.3.0/latest/analytics.png)

d) The installer displays a summary of the configuration options you selected. Enter **`y`** to start the installation, or **`n`** to cancel:

![installation summary](../static/img/7.3.0/latest/installation-summary.png)

e) The installer will then ask for a **Redash API Key**:

![Redash API key prompt](../static/img/7.3.0/latest/redash-api.png)

- The Redash application should be available for use at port defaulted to 7000. Open http://localhost:7001/ on your machine and register with any valid credentials:
  ![redash landing page](../static/img/redash-landing.png)

- The API Key can be found at **Settings > Account > API Key**. Copy it and paste it into the command prompt to continue the installation:
  ![redash settings page](../static/img/redash-settings-page.png)

f) Once the installation is complete, the command prompt displays **formsflow.ai is successfully installed**. Press any key to finish. Docker Desktop will show all the installed containers running:

![successful installation](../static/img/7.3.0/latest/completed.png)

Once successfully installed, you can access the formsflow.ai web application in your browser via `http://localhost:3000` or `http://{your-ip-address}:3000`, and log in with the default admin credentials `formsflow-admin` / `changeme`.

![web application](../static/img/web-application.png)

---

## Step 4: Mail Configuration (Optional)

For the **email configuration**, follow the steps below:

![configuration folder](../static/img/configuration-folder.png)

Create a folder inside the configuration folder (inside the docker-compose directory) named **bpm-mail-config**.

![mail configuration file](../static/img/mail-config-file.png)

Create a file named **mail.config.properties** inside the **bpm-mail-config** folder you just created, and copy the contents below, updating the values as needed:

```bash
# Send mails via SMTP. The given settings are for Gmail
mail.transport.protocol=smtp

mail.smtp.host=smtp.gmail.com
mail.smtp.port=465
mail.smtp.auth=true
mail.smtp.ssl.enable=true
mail.smtp.socketFactory.port=465
mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory

# Poll mails via IMAPS.
mail.store.protocol=imaps
mail.imaps.host=imap.gmail.com
mail.imaps.port=993
mail.imaps.timeout=10000

mail.sender=donotreply
mail.sender.alias=DoNotReply

mail.attachment.download=true
mail.attachment.path=attachments

# Credentials
mail.user=CHANGEME@gmail.com
mail.password=CHANGEME

```

- Now restart the container to apply the changes.

## Verifying the Installation status

> The following applications will be started and can be accessed in your browser.

 Srl No | Service Name | Usage | Access | Default credentials (userName / Password)|
--- | --- | --- | --- | ---
1|`forms-flow-web`|formsflow Landing web app|`http://localhost:3000`| `formsflow-admin/changeme`
2|`Keycloak`|Authentication|`http://localhost:8080`| `admin/changeme`
3|`forms-flow-forms`|form.io form building (Note: Form.io UI is disabled by default). This must be started earlier for resource role IDs creation|`http://localhost:3001`|`admin@example.com/changeme`
4|`forms-flow-analytics`|Redash analytics server, This must be started earlier for redash key creation|`http://localhost:7001`|Use the credentials used for registration / [Default user credentials](https://github.com/AOT-Technologies/forms-flow-ai-deployment/blob/main/docs/forms-flow-ai-properties.md)
5|`forms-flow-api`|API services|`http://localhost:5001`|`Authorization token from keycloak role based user credentials`
6|`forms-flow-bpm`|Camunda integration|`http://localhost:8000/camunda`| [Default user credentials](https://github.com/AOT-Technologies/forms-flow-ai-deployment/blob/main/docs/forms-flow-ai-properties.md)
7|`forms-flow-data-layer`|GraphQL integration|`http://localhost:5500/queries`|

## Uninstall Formsflow (Optional)

Uninstalling is optional and can be performed whenever you wish to remove the formsflow.ai stack from your machine. To uninstall:

1. Go to the folder you extracted earlier and navigate to the `scripts` directory. You will find the **uninstall.bat** file there.
2. Double-click **uninstall.bat** to run it.
3. When prompted, enter **`y`** to confirm and proceed with the uninstallation.

![uninstall formsflow](../static/img/uninstall.png)

If you face any issues while installing, please connect with [us](https://github.com/AOT-Technologies/forms-flow-ai/issues).

---

## Frequently Asked Questions (FAQ)

This section covers common issues, error messages, and troubleshooting steps encountered during the installation and setup of formsflow.ai.

### Q: "Port is already allocated" or connection errors (e.g., port 3000, 8080, 3001)
> [!WARNING]
> This error occurs when another process or service is already running on one of the ports formsflow.ai requires.

* **Possible Cause**: You have a local database, a web application, or another Docker container running on ports like `8080` (Keycloak), `3000` (Web app), `3001` (Forms), or `7001` (Analytics).
* **Solution**:
  1. Find and terminate the process using the port:
     ```powershell
     netstat -ano | findstr :3000
     # Stop the process using the PID found
     Stop-Process -Id <PID> -Force
     ```
  2. Alternatively, modify the port mapping in the `docker-compose.yml` file under the respective service.

### Q: Container exits with Code 137 (Out of Memory)
> [!IMPORTANT]
> Running the entire formsflow.ai stack requires minimum resource allocations in Docker.

* **Possible Cause**: Docker Desktop is running with default low memory configurations (e.g., 2 GB), causing database or Camunda containers to get killed by the OOM killer.
* **Solution**:
  1. Open Docker Desktop settings.
  2. Navigate to **Resources** > **Advanced** (or resource allocation settings).
  3. Increase Memory allocation to at least **4 GB to 8 GB** and CPUs to **4**.
  4. Click **Apply & restart** and run the install script again.

### Q: "Invalid redirect_uri" error on Keycloak login page
* **Possible Cause**: The application client redirect URLs registered in Keycloak do not match the IP address or host domain name you are currently using to access the site.
* **Solution**:
  1. Access the Keycloak Admin Console at `http://localhost:8080/auth/admin` (default credentials: `admin/changeme`).
  2. Navigate to **Clients** > select the client (e.g., `forms-flow-web`).
  3. Under **Valid Redirect URIs**, add your current access URL (e.g., `http://{your-ip-address}:3000/*` or `http://localhost:3000/*`).
  4. Add the URL to **Web Origins** as well to prevent CORS issues.
  5. Click **Save**.

### Q: Services fail to authenticate or token validation fails
* **Possible Cause**: The host IP or domain configured in Keycloak does not resolve correctly inside the Docker network.
* **Solution**:
  * Double-check your `.env` settings. Ensure `KEYCLOAK_URL` is set to the host IP (e.g., `http://{your-ip-address}:8080`) instead of `http://localhost:8080` if containers need to reach it externally.

### Q: "Connection refused" or database migration fails on startup
> [!NOTE]
> DB startup order is managed, but slow disk I/O can occasionally cause other services to start before the database is ready.

* **Possible Cause**: PostgreSQL or MongoDB has not finished initializing schema structures when Camunda or the Web API starts up.
* **Solution**:
  1. Simply restart the containers using:
     ```bash
     docker compose restart forms-flow-bpm forms-flow-api
     ```
  2. If the database credentials were changed, ensure the corresponding variables (`POSTGRES_USER`, `POSTGRES_PASSWORD`, etc.) are updated and match across all dependent services in `docker-compose.yml`.
