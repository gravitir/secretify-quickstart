<img src="https://app.secretify.io/assets/logo.svg" width="100">

A secure way to transfer or share secrets.

# Get Started

10 min.

In this unit you will learn how to install Secretify using Docker.

At the end of this section, you will have a working Secretify instance behind a reverse proxy. The reverse proxy used in this guide is [traefik](https://traefik.io/), but any other preferable option would work fine.

Take a look at the available SDK's [Secretify SDK's](./doc/sdk/README.md).

## Prerequisites

Before you begin, make sure you have the following tools installed:

* [Docker](https://docs.docker.com/engine/install/) - Containerization platform for running Secretify.
* [Docker Compose](https://docs.docker.com/compose/install/) - Used to manage multi-container Docker applications (only necessary for older Docker versions; Compose V2 is included in newer versions).
* Git - You'll need Git to clone the Secretify Quickstart GitHub repository to your working directory.

```bash
git clone https://github.com/gravitir/secretify-quickstart.git
```

## Installation

This step-by-step installation guide is intended for testing and development purposes and should not be used in a production environment.

If you're using Docker in a local environment, you can proceed with the intended guide. However, if you plan to expose your Secretify instance publicly, follow the secure variant as Secretify requires HTTPS for proper functionality in a public setting.

### Step 1: Login to the Private Docker Registry

As this is a private Docker registry, you will require valid credentials. If you don't have any credentials, please reach out to us at <a href="mailto:hello@secretify.io">hello@secretify.io</a> to request a trial account. Once you have the necessary credentials, proceed to log in to our Docker registry. You will be prompted to enter your password.

```bash
docker login registry.gravitir.io -u YOUR_USERNAME
```

**Verification**

After you enter the password, you should see the following output:  
*Login Succeeded*

### Step 2: Pull the Docker image

Open a terminal session and browse to `secretify-quickstart`. Pull the Docker images defined in `docker-compose.yml`:

```bash
docker-compose pull
```

**Verification**

When the required images are successfully pulled, the terminal will return the following:  
*Pulling traefik       ... done*  
*Pulling secretify-api ... done*  
*Pulling secretify-ui  ... done*

### Step 3: Configure Host IP and DNS Record

Depending on your setup, you need to do this differently.

Open the `.env` file and set the `HOST_IP` to your host IP address and `FQDN` (Fully Qualified Domain Name): 

```bash
HOST_IP="172.16.3.58"
FQDN="secretify.localhost"
```

`HOST_IP`: Set this to your host's IP address, which you can find by running a command like `ipconfig` (Windows) or `ifconfig` (Linux/macOS) in your terminal. Make sure it matches your active network adapter's IPv4 address.

Next, add a DNS record for your `FQDN` (e.g. `secretify.localhost`) in your DNS configuration. This step might vary depending on your network setup and DNS server.

> **_NOTE:_**   Ensure that you do not use a loopback address (e.g., 127.0.0.1) for the `HOST_IP` in the `.env` file. This is because the API instance needs to reach the UI, and using a loopback address would prevent proper communication.

### Step 3b: Additonal Configuration for Secure Variant

Set the `SCHEME` to `https://` in the `.env` file. Additionally, make sure to define a valid email address in `POSTMASTER_EMAIL` which used by traefik for issuing Let's Encrypt certificates.

### Step 5: Start the Secretify Instance

To start the Secretify instance, use one of the following commands depending on your chosen variant:

For the local variant:

```
docker-compose up -d
```

For the secure variant:

```
docker-compose -f docker-compose.secure.yaml up -d
```

When Secretify starts, the terminal will return the following:  
*Creating network "secretify-quickstart_default" with the default driver*  
*Creating volume "secretify-quickstart_db" with default driver*  
*Creating volume "secretify-quickstart_files" with default driver*  
*Creating traefik       ... done*  
*Creating secretify-api ... done*  
*Creating secretify-ui  ... done*  

**Verification**

Run the following command to see a list of running containers:

```
docker ps -a
```

### Step 6: Open Secretify in Your Browser

Open a browser and navigate to http://secretify.localhost

## Tweak configuration

Now you can customize various configuration parameters by editing the `.env` file. For example, you can change the primary branding color by editing `BRANDING_PRIMARY_COLOR="#EA2027"`. After making changes, restart Secretify using again:

For the local variant:

```
docker-compose up -d
```

For the secure variant:

```
docker-compose -f docker-compose.secure.yaml up -d
```

You should see the changes reflected in the Secretify interface.

> **_NOTE:_**   If you have made changes to `config/secretiy.yaml` and want to see your changes take effect, you need to run `docker restart secretify-api` instead.

### Generel Configuration

| Env                | Value   | Description                 |
| ------------------ | ------- | --------------------------- |
| `LOG_LEVEL`        | `info`  | Logging verbosity level     |
| `DEBUG`            | `false` | Enable debugging mode       |
| `ACTIVITY_ENABLED` | `true`  | Enable activity tracking    |
| `HELP_ENABLED`     | `false` | Enable or disable help page |
| `META_NAME`        | `""`    | Name metadata               |
| `META_ADDRESS`     | `""`    | Web URI metadata            |
| `META_EMAIL`       | `""`    | Email metadta               |

### Branding Configuration

| Env                      | Value      | Description                                           |
| ------------------------ | ---------- | ----------------------------------------------------- |
| `BRANDING_SERIOUS`       | `false`    | Enable or disable serious branding mode               |
| `BRANDING_PRIMARY_COLOR` | `#079992`  | Primary color                                         |
| `BRANDING_LOGO`          | `logo.png` | Relative path to the logo file in the `static` folder |


#### Editing Content Files

There are two content files you can edit.

| Name    | Content file path           | URI path   | Note                              |
| ------- | --------------------------- | ---------- | --------------------------------- |
| Imprint | `static/content/imprint.md` | `/imprint` |                                   |
| Privacy | `static/content/privacy.md` | `/privacy` |                                   |
| Help    | `static/content/help.md`    | `/help`    | `HELP_ENABLED` needs to be `true` |

### Password Generator Configuration

| Env                        | Value | Description                                    |
| -------------------------- | ----- | ---------------------------------------------- |
| `PASSWORDGENERATOR_LENGTH` | `30`  | Desired password length (e.g., 30 characters). |

### Authentication Microsoftonline Configuration

> **_NOTE:_**   The default (insecure via HTTP) variant works only if you use `localhost` as your FQDN instead of `secretify.localhost` in some of the examples.

Before configuring Microsoftonline Authentication, you need to create an app registration in your Azure Tenant. Follow this guide [Microsoftonline App Registration Guide](./doc/microsoftonline-app-registration.md).

| Env                                     | Description                                                                                                                                                                                                                                               |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AUTH_ENABLED`                          | Set to `true` to enable authentication.                                                                                                                                                                                                                   |
| `AUTH_STRICT`                           | Set to `true` if you want to enforce authentication for creating secrets (revealing secrets will still be publicly available).                                                                                                                            |
| `AUTH_RANDOM_GUESTNAME`                 | Set to `true` if you want to enable random guest names. This flag is only being used if authentication is enabled.                                                                                                                                        |
| `AUTH_SESSIONSECRET`                    | Do not set this in the `.env` file since it is a secret.<br><br>Set it from the terminal using: `export AUTH_SESSIONSECRET=YOUR_SESSIONSECRET`.<br><br>You should generate the secret only once (e.g. `openssl rand -base64 32`) and reuse it afterwards. |
| `AUTH_MICROSOFTONLINE_ENABLED`          | Set to `true`   to enable Microsoft Online Authentication.                                                                                                                                                                                                |
| `AUTH_MICROSOFTONLINE_SCOPES`           | Obtain from your app registration under `Expose an API` > `Scopes`.<br><br>e.g. `api://secretify.localhost/d2254856-cdd5-4c48-be5e-4edc2aff353a/access_as_user`                                                                                           |
| `AUTH_MICROSOFTONLINE_CLIENTID`         | Obtain from your app registration under `Overview` > `Application (client) ID`.<br><br>e.g. `d2254856-cdd5-4c48-be5e-4edc2aff353a`                                                                                                                        |
| `AUTH_MICROSOFTONLINE_CLIENTSECRET`     | Do not set this in the `.env` file since it is a secret.<br><br>You should have copied the client secret value when you created it. Set it from the terminal using: `export AUTH_MICROSOFTONLINE_CLIENTSECRET=YOUR_CLIENTSECRET`.                         |
| `AUTH_MICROSOFTONLINE_TENANT`           | Obtain from your app registration under `Overview` > `Directory (tenant) ID`.<br><br>e.g. `c32fa489-fff8-4d11-a1a9-ab50112c3be8`                                                                                                                          |
| `AUTH_MICROSOFTONLINE_REDIRECTURL`      | Obtain from your app registration under `Authentication` > `Platform configurations` > `Web - Redirect URIs`.<br><br>e.g. `http://secretify.localhost/callback`                                                                                           |
| `AUTH_MICROSOFTONLINE_APPLICATIONID`    | Obtain from your app registration under `Overview` > `Application (client) ID`.<br><br>e.g. `d2254856-cdd5-4c48-be5e-4edc2aff353a`                                                                                                                        |
| `AUTH_MICROSOFTONLINE_APPLICATIONIDURI` | Obtain from your app registration under `Expose an API` > `Application ID URI`<br><br>e.g. `api://secretify.localhost/d2254856-cdd5-4c48-be5e-4edc2aff353a`                                                                                               |

### Outlook Plugin Configuration (only supported with secure variant)

Before install the Secretify Outlook Plugin, you need to adjust your configuration. After that, follow the instructions in the [Secretify Outlook Plugin Setup Guide](./doc/secretify-outlook-plugin.md).

| Env                         | Description                                                                                                                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `PLUGIN_OUTLOOK_ENABLED`    | Set to `true` to enable the Outlook plugin.                                                                                                                                        |
| `PLUGIN_OUTLOOK_APPID`      | Generate this app ID only once (e.g., using `uuidgen` or any other GUID generator tool) and reuse it. It needs to be a GUID in the form of `f825c0bf-76eb-4e0c-8038-b46003c9d9ca`. |
| `PLUGIN_OUTLOOK_APPVERSION` | Set to `1.0.0.0`.                                                                                                                                                                  |

### Storage configuration

Currently, only the filesystem storage option is supported.

| Env                             | Description                                                                                                           |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `STORAGE_ENABLED`               | Set to `true` to enable storage.                                                                                      |
| `STORAGE_FILESYSTEM_ENABLED`    | Set to `true` to enable filesystem storage.                                                                           |
| `STORAGE_SPACELEFT_HEALTHCHECK` | Set to `true` to enable space left healthcheck.                                                                       |
| `STORAGE_SPACELEFT_THRESHOLD`   | Set to a byte value (e.g. `1073741824` for 1GB). If there is less than 1GB space left, file uploads will be disabled. |

### Mail STMP configuration

Below are the configuration options for SMTP email settings:

| Env                  | Description                                                                                                           |
| -------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `MAIL_ENABLED`       | Set to `true` to enable email functionality.                                                                          |
| `MAIL_SMTP_FROM`     | Specify the email address from which emails will be sent.                                                             |
| `MAIL_SMTP_USERNAME` | Enter the SMTP username for authentication (if required).                                                             |
| `MAIL_SMTP_PASSWORD` | Set the SMTP password. You can set it from the terminal using the command: `export MAIL_SMTP_PASSWORD=YOUR_PASSWORD`. |
| `MAIL_SMTP_HOST`     | Specify the SMTP server host address or hostname.                                                                     |
| `MAIL_SMTP_PORT`     | Specify the SMTP server port number.                                                                                  |

#### Microsoft 365 SMTP configuration

To configure outgoing SMTP for secretify as application in Microsoft 365 see the following manual:
[How to set up a multifunction device or application to send email using Microsoft 365 or Office 365](https://learn.microsoft.com/en-us/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365)

### Additional Configuration via `config/secretify.yaml`

You can customize your own secret types by editing the `config.secretify.yaml` file. Below is an example of how to add a secret type for certificates (you need to run `docker restart secretify-api` to see your changes take effect):

```yaml
# types:
# ...
# other types
# ...
  - active: true
    identifier: certificate
    name:
      en: Certificate
    description:
      en: This secret type allows you to securely upload certificates and private keys with optional details.
    schema:
      - property: certificate
        type: file
        required: true
        maxFilesize: 4096000 # in bytes
        allowedFileExtensions:
          - .crt
      - property: privatekey
        type: file
        required: true
        maxFilesize: 4096000 # in bytes
        allowedFileExtensions:
          - .key
      - property: details
        type: string
        required: false
        secretMaxLength: 200
    uiSetting:
      order: 3
      secretRevealDuration: "120s"
      secretExpiresInOptions: # valid are s (seconds), m (minutes) and h (hours)
        - "5m"
        - "1h"
        - "24h"
        - "168h"
      displayName:
        certificate:
          en: Certificate
        privatekey:
          en: Private Key
        details:
          en: Details
      description:
        details:
          en: Add an optional message related to the uploaded certificate and private key.
      placeholder:
        details:
          en: Details to go along with the certificate and private key
    policy:
      defaultSecretMaxLength: 500

```

## Troubleshooting

Start by checking if the containers are starting successfully.

```bash
docker ps
```

If they are, proceed to examine the Docker logs, as you may find error messages that can provide insights into any issues.

```bash
docker logs secretify-api
```

## Acknowledgments

Our thanks are extended but not limited to the following people or organizations:

* [Freepik](https://www.flaticon.com/free-icons/star-wars) - Star wars icons created by Freepik - Flaticon
