# Secretify CLI

## Installing Secretify CLI on Linux
### Step 1: Download Secretify binary with curl

Download the latest release of Secretify for Linux using the following command:

```bash
curl -LO "https://www.secretify.io/release/bin/linux/amd64/secretify"
```

### Step 2: Validate the binary (optional)  

Download the secretify checksum file:

```bash
curl -LO "https://www.secretify.io/release/bin/linux/amd64/secretify.sha256"
```

Validate the secretify binary against the checksum file:

```bash
echo "$(cat secretify.sha256) secretify" | sha256sum --check
```

If valid, the output is:

```bash
secretify: OK
```

If the check fails, `sha256` exits with nonzero status and prints output similar to:

```bash
secretify: FAILED
sha256sum: WARNING: 1 computed checksum did NOT match
```

### Step 3: Install secretify

To install Secretify system-wide, use the following command:

```bash
sudo install -o root -g root -m 0755 secretify /usr/local/bin/secretify
```

If you do not have root access on the target system, you can still install secretify to the `~/.local/bin` directory:

```bash
chmod +x secretify
mkdir -p ~/.local/bin
mv ./secretify ~/.local/bin/secretify
# and then append (or prepend) ~/.local/bin to $PATH
```

### Step 4: Test Secretify installation

After installation, verify that Secretify is installed correctly by running:

```bash
secretify --version
```

This command should output the version of Secretify installed.

## Usage

```bash
dario@quasar:~$ secretify help
Usage:
  secretify [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  create      Create a new secret link
  help        Help about any command
  login       Login with username and password
  logout      Log out from a Secretify API
  reveal      Reveal an entry

Flags:
  -h, --help      help for secretify
  -v, --version   version for secretify

Use "secretify [command] --help" for more information about a command.
```

### Login

To login, run the following command:

```bash
secretify login https://example.secretify.io/api/v1 -u YOUR_USERNAME
```

You will be prompted to enter your password. Once authenticated, your credentials will be securely stored. If a keyring is available, your credentials will be saved there; otherwise, they will be stored in a newly created file located at `~/.secretify/.netrc`.

### Logout

To logout, run the following command:

```bash
secretify logout https://example.secretify.io/api/v1
```

**Note**: You might encounter an error regarding the keyring. Ignore it for now; what's important is that  `~/.secretify/.netrc` will be removed.

### Creating a secret

To create a new secret, use the following command:

```bash
secretify create text --set message=v3ryS3ecure$
```

Upon successful creation, you will receive output similar to the following:

```bash
{
    "link": "https://example.secretify.io/s/asdfasdfasfdafsfd#asdfasdfasdfasdf"
}
```

(optional) Use `jq` to parse the link:

```bash
secretify create text --set message=v3ryS3ecure$ | jq -r '.link'
```

### Revealing a secret

**Note**: This feature is currently under construction and will be available soon.
