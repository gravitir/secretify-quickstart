<img src="https://app.secretify.io/assets/logo.svg" width="100">

A secure way to transfer or share secrets.

# Get Started

10 min.

In this unit you will learn how to install Secretify using Docker.

At the end of this section, you will have a working Secretify instance behind a reverse proxy. The reverse proxy used in this guide is [traefik](https://traefik.io/), but any other preferable option woudl work fine.

## Prerequisite

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

Since this is a private docker registry, you need credentials. Log in to our docker registry, and you will get a prompt to enter your password.

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
docker-compose up -f docker-compose.secure.yaml -d
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

### Branding Configuration

| Env                      | Value      | Description                                           |
| ------------------------ | ---------- | ----------------------------------------------------- |
| `BRANDING_PRIMARY_COLOR` | `#079992`  | Primary color                                         |
| `BRANDING_LOGO`          | `logo.png` | Relative path to the logo file in the `static` folder |

#### Editing Content Files

There are two content files you can edit.

| Name    | Content file path           | URI path   |
| ------- | --------------------------- | ---------- |
| Imprint | `static/content/imprint.md` | `/imprint` |
| Privacy | `static/content/privacy.md` | `/privacy` |

### Authentication Microsoftonline Configuration


Before configuring Microsoftonline Authentication, you need to create an app registration in your Azure Tenant. Follow this guide [Microsoftonline App Registration Guide](./doc/microsoftonline-app-registration.md).

| Env                                                                                                                      | Description                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AUTH_ENABLED`                                                                                                           | Set to `true` to enable authentication.                                                                                                                                                                                                     |
| `AUTH_STRICT`                                                                                                            | Set to `true` if you want to enforce authentication for creating secrets (revealing secrets will still be publicly available).                                                                                                              |
| `AUTH_SESSIONSECRET`                                                                                                     | Do not set this in the `.env` file since it is a secret. Set it from the terminal using: `export AUTH_SESSIONSECRET=YOUR_SESSIONSECRET`. You should generate the secret only once (e.g. `openssl rand -base64 32`) and reuse it afterwards. |
| `AUTH_MICROSOFTONLINE_ENABLED`                                                                                           | Set to `true`   to enable Microsoft Online Authentication.                                                                                                                                                                                  |
| `AUTH_MICROSOFTONLINE_SCOPES`                                                                                            | Obtain from your app registration under `Expose an API` > `Scopes`.<br><br><br><br>Example: `api://secretify.localhost/d2254856-cdd5-4c48-be5e-4edc2aff353a/access_as_user`                                                                 |
| `AUTH_MICROSOFTONLINE_CLIENTID`                                                                                          | Obtain from your app registration under `Overview` > `Application (client) ID` (e.g. `d2254856-cdd5-4c48-be5e-4edc2aff353a`).                                                                                                               |
| `AUTH_MICROSOFTONLINE_CLIENTSECRET`                                                                                      | Do not set this in the `.env` file since it is a secret. You should have copied the client secret value when you created it. Set it from the terminal using: `export AUTH_MICROSOFTONLINE_CLIENTSECRET=YOUR_CLIENTSECRET`.                  |
| `AUTH_MICROSOFTONLINE_TENANT`<br><br>Example: `c32fa489-fff8-4d11-a1a9-ab50112c3be8`                                     | Obtain from your app registration under `Overview` > `Directory (tenant) ID`                                                                                                                                                                |
| `AUTH_MICROSOFTONLINE_REDIRECTURL`<br><br>Example: `http://secretify.localhost/callback`                                 | Obtain from your app registration under `Authentication` > `Platform configurations` > `Web - Redirect URIs`                                                                                                                                |
| `AUTH_MICROSOFTONLINE_APPLICATIONID`  <br><br>Example: `d2254856-cdd5-4c48-be5e-4edc2aff353a`                            | Obtain from your app registration under `Overview` > `Application (client) ID`                                                                                                                                                              |
| `AUTH_MICROSOFTONLINE_APPLICATIONIDURI`<br><br>Example: `api://secretify.localhost/d2254856-cdd5-4c48-be5e-4edc2aff353a` | Obtain from your app registration under `Expose an API` > `Application ID URI`                                                                                                                                                              |


### Outlook Plugin configuration

*coming soon..*

### Mail STMP configuration

*coming soon..*

### Storage configuration

*coming soon..*

## Acknowledgments

Our thanks are extended but not limited to the following people or organizations:

* [Freepik](https://www.flaticon.com/free-icons/star-wars) - Star wars icons created by Freepik - Flaticon
