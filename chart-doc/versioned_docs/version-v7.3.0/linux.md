---
sidebar_position: 1
---

# Linux / MAC

**Estimate time: 20 minutes (approximate)**

## Prerequisites

Before starting the installation, ensure your system meets the following requirements:

### 1. Software Installation & Configuration
* **Docker**: Install Docker Engine. Follow the official guides for [Linux](https://docs.docker.com/desktop/install/linux/) or [MAC](https://docs.docker.com/desktop/install/mac/).
* **Docker Compose (V2)**: Ensure `docker compose` is installed and accessible.
* **Permissions**: Ensure your user account has permissions to run Docker commands (added to the `docker` group) or you have `sudo` access.

### 2. System Resource Requirements
Running the entire formsflow.ai stack requires sufficient system resources. Ensure Docker is allocated at least:
* **Memory (RAM)**: Minimum **4 GB** (8 GB or more recommended). *Your machine should have at least 8 GB-16 GB total RAM.*
* **CPUs**: Minimum **4 Cores**.
* **Disk Space**: At least **10 GB** of free disk space.

### 3. General Requirements
* **Active Internet Connection**: Required to download the installer ZIP and pull Docker images (approx. 2-3 GB).
* **Web Browser**: A modern web browser (e.g., Google Chrome, Mozilla Firefox, Safari) to access the formsflow.ai UI.

## Step 1: Download the GitHub Repository

In this initial step, download the **Forms Flow AI Deployment** GitHub repository by clicking [**here**](https://github.com/AOT-Technologies/forms-flow-ai-deployment/archive/refs/heads/release/v7.3.0.zip).

A zip file will be downloaded.

## Step 2: Extract the downloaded .zip file

> [!NOTE]
> The steps in this section show a Linux file manager (e.g. Nautilus). macOS users follow the same flow using **Finder** — the folder structure and script names are identical.

![extracted folder preview](../static/img/linux/extracted.png)

**Now double-click to open the extracted folder and go to the `scripts` directory:**

![scripts dir preview](../static/img/linux/scripts-dir.png)

There you can see the **install.bash** file:

![install file preview](../static/img/linux/install-file.png)

Now open a terminal inside the `scripts` directory:

- **Linux:** Right-click anywhere in the file manager and click **Open in Terminal**.
- **macOS:** Right-click the `scripts` folder in Finder and select **New Terminal at Folder**. If this option is not visible, open the **Terminal** app (Applications > Utilities) and run:
  ```bash
  cd /path/to/extracted-folder/scripts
  ```

![terminal preview](../static/img/linux/open-in-terminal.png)

Now type `sudo su` in the terminal to gain elevated privileges, which are required for the installation process:

> [!NOTE]
> **Linux:** `sudo su` switches you to the root user. If prompted, enter your user password.
>
> **macOS:** `sudo su` works the same way. Alternatively, you can prefix each command with `sudo` instead of switching to root.

![sudo permissions](../static/img/linux/sudo-su.png)

- You can type `ls` to see the **install.bash** file there.

## Step 3: Install using the install.bash file

First, give the script executable permission, then run it:

```bash
chmod +x install.bash
./install.bash
```

- It may display a **Do you want to continue?** prompt if the script has not been tested against your Docker version. Enter **`y`** to proceed with the installation.

![installation start](../static/img/linux/installation-start.png)

a) Verify the IP address shown is correct. If it is correct, enter **`y`**, otherwise enter **`n`** to provide the correct IP:

![IP address prompt](../static/img/linux/ip-address-prompt.png)

b) When asked whether to install **Analytics**, enter **`y`** to include Redash, or **`n`** to skip. (To learn more, visit [Redash](https://redash.io/help/).)

![Redash analytics prompt](../static/img/linux/analytics-prompt.png)

c) The installer will then ask for a **Redash API Key**:

![redash api key prompt](../static/img/7.3.0/linux/redash-prompt.png)

- The Redash application should be available for use at port defaulted to 7000. Open http://localhost:7001/ on your machine and register with any valid credentials:
  ![redash landing page](../static/img/7.3.0/redash-landing.png)

- The API Key can be found at **Settings > Account > API Key**:
  ![redash api page](../static/img/7.3.0/linux/redash-api-page.png)

d) Once the installation is complete, the terminal displays **formsflow.ai is successfully installed**.

![completed](../static/img/linux/complete.png)

Once successfully installed, you can access the formsflow.ai web application in your browser via `http://localhost:3000` or `http://{your-ip-address}:3000`, and log in with the default admin credentials `formsflow-admin` / `changeme`.

![web application](../static/img/web-application.png)

---

## Step 4: Mail Configuration (Optional)

For the **email configuration**, follow the steps below:

![configuration folder](../static/img/linux/config-dir.png)

Create a folder inside the configuration folder (inside the docker-compose directory) named **bpm-mail-config**.

![mail configuration file](../static/img/linux/config.png)

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

1. Go to the folder you extracted earlier and navigate to the `scripts` directory. You will see the **uninstall.bash** file there.
2. Open a terminal in that directory:
   - **Linux:** Right-click anywhere in the file manager and click **Open in Terminal**.
   - **macOS:** Right-click the `scripts` folder in Finder and select **New Terminal at Folder**.
3. Give the script executable permission and run it:
   ```bash
   chmod +x uninstall.bash
   ./uninstall.bash
   ```
   - If you run `ls` you can see **uninstall.bash** highlighted in green, confirming it now has executable permission.
4. When prompted, enter **`y`** to confirm and proceed with the uninstallation.

![uninstall formsflow](../static/img/linux/uninstall.png)

If you face any issues while installing, please connect with [us](https://github.com/AOT-Technologies/forms-flow-ai/issues).

---

## Frequently Asked Questions (FAQ)

This section covers common issues, error messages, and troubleshooting steps encountered during the installation and setup of formsflow.ai.

### Q: "Port is already allocated" or connection errors (e.g., port 3000, 8080, 3001)
> [!WARNING]
> This error occurs when another process or service is already running on one of the ports formsflow.ai requires.

* **Possible Cause**: You have a local database, a web application, or another Docker container running on ports like `8080` (Keycloak), `3000` (Web app), `3001` (Forms), or `7001` (Analytics).
* **Solution**:
  1. Find and terminate the process using the port (works on both **Linux** and **macOS**):
     ```bash
     sudo lsof -i :3000
     # Kill the process using the PID found above
     kill -9 <PID>
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
