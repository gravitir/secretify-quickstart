# Microsoftonline App Registration Guide

10 min.

In this unit you will learn how to register a new app in Entra (previously Azure AD) for using OAuth 2.0 with Secretify.

## Prerequisite

Before you begin, make sure you have the following:

* Microsoft Azure Tenant
* Running Secretify instance

## Installation

> **_NOTE:_**   For easier readability we will use in this guide `https://example.secretify.io` as the configured instance. Replace it with your actual running instance's url e.g. `https://secretify.example.com`.

### Step 1: Create new app registration

Open up [entra.microsoft.com](https://entra.microsoft.com) in a browser. Navigate to `Applications` > `App registrations` and click on new `+ New registration`.

* Give it a reasonable name like `secretify-companyname`.
* Under `Redirect URI (optional)` select `web` as a platform and type `https://example.secretify.io/callback`
* Press `Register`

You will be redirected to the newly created app registration.

### Step 2: Create client secret

Navigate to `Certificates & secrets` and click on `+ New client secret`. Give it a name like `secretify-companyname_secret` () and click on `Add`. Copy the `Secret ID` and `Value` somewhere save, especially the `Values` since you can't obtain it as soon as you leave the page anymore.

> **_NOTE:_**   The secret has an expiration date, so in some time in the future login will not work anymore if you do not update the clientsecret early enough

### Step 3: Expose an API

Navigate to `Expose an API` and click on `+ Add a scope`. Edit the already generated application ID URL while adding the domain right after the schema (e.g. from `api://d2254856-cdd5-4c48-be5e-4edc2aff353a` to `api://secretify.localhost/d2254856-cdd5-4c48-be5e-4edc2aff353a`). Press `Save and continue`. You will be redirected to the `Add a scope` form.

* Scope name: `access_as_user`
* Admin consent display name: `Admins and users`
* Admin consent display name: `Access as user`
* Admin consent description: `Allows the app to access as a user`

Press `Add scope`.

#### Step 3b: Add client application for Outlook

If you do not intend to use the outlook plugin, you can skip this step. This step is required if you want to use SSO with the Secretify Outlook plugin.

Press `+ Add a client application` and type in Client ID `ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` and select your newly created scope. Press `Add application`.

### Step 4: API permissions

Navigate to `API permissions` and click on `+ Add a permission`. Select `Microsoft Graph` > `Delegated permissions` and select following ones `openid` and `profile`. Press `App permissions`. Once again click on `+ Add a permission`, in the tab switch to `APIs my organization uses`, search for your app name e.g. `secretify-companyname`, check `access_as_user` and press `Add a permissions`.

Next press the button `Grant admin consent for COMPANY` and confirm with `Yes`.

### Step 5: Update manfiest version

Navigate to `Manifest` and change the from `"accessTokenAcceptedVersion": null` to `"accessTokenAcceptedVersion": "v2"`.