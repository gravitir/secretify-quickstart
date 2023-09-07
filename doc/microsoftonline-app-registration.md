# Microsoftonline App Registration Guide

10 min.

In this guide, you will learn how to register a new app in Entra (formerly Azure AD) to use OAuth 2.0 with Secretify.

## Prerequisite

Before you begin, make sure you have the following:

* A Microsoft Azure Tenant.
* A running instance of Secretify.

## Setup

> **_NOTE:_**   For ease of readability, we will use the URL `https://example.secretify.io` as the configured instance in this guide. Replace it with the actual URL of your running instance (e.g. `https://secretify.example.com`).

### Step 1: Create a New App Registration

1. Open [entra.microsoft.com](https://entra.microsoft.com) in your web browser.
2. Navigate to `Applications` > `App registrations` and click on `+ New registration`.
3. Give it a reasonable name, such as `secretify-companyname`.
4. Under `Redirect URI (optional)`, select `web` as the platform and enter `https://example.secretify.io/callback`.
5. Press `Register`

You will be redirected to the newly created app registration.

### Step 2: Create client secret

1. Navigate to `Certificates & secrets` and click on `+ New client secret`.
2. Give it a name, such as `secretify-companyname_secret`
3. Click `Add`.
4. Copy the `Secret ID` and `Value` and save them. Note that you can't obtain the `Values` again after leaving the page.

> **_NOTE:_**   The client secret has an expiration date, so remember to update it in advance to avoid login issues.

### Step 3: Expose an API

1. Navigate to `Expose an API` and click on `+ Add a scope`.
2. Edit the already provided Application ID URL by adding the domain right after the scheme (e.g. from `api://d2254856-cdd5-4c48-be5e-4edc2aff353a` to `api://secretify.localhost/d2254856-cdd5-4c48-be5e-4edc2aff353a`).
3. Click `Save and continue`. You will be redirected to the `Add a scope` form.
4. Configure the scope as follows:
   * Scope name: `access_as_user`
   * Admin consent display name: `Admins and users`
   * Admin consent display name: `Access as user`
   * Admin consent description: `Allows the app to access as a user`
5. Click `Add scope`.

#### Step 3b: Add a Client Application for Outlook (Optional)

If you do not intend to use the Outlook plugin, you can skip this step. This step is required if you want to use SSO with the Secretify Outlook plugin.

1. Click `+ Add a client application` and enter the Client ID `ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (use actually excactly this client id).
2. Select your newly created scope.
3. Click `Add application`.

### Step 4: API Permissions

1. Navigate to `API permissions` and click on `+ Add a permission`.
2. Select `Microsoft Graph` > `Delegated permissions` and select the following permissions: `openid` and `profile`.
3. Click `Add permissions`.
4. Click `+ Add a permission` once again, switch to the `APIs my organization uses` tab, search for your app name (e.g. `secretify-companyname`), check `access_as_user`, and click `Add permissions`.

Finally, click the `Grant admin consent for COMPANY` button and confirm with `Yes`.

### Step 5: Update manfiest version

1. Navigate to `Manifest` and change `"accessTokenAcceptedVersion": null` to `"accessTokenAcceptedVersion": "v2"`.
