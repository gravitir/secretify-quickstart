# Secretify Outlook Plugin Setup Guide

10 min.

In this guide, you will learn how to setup the Secretify Outlook Plugin.

## Prerequisites

Before you begin, make sure you have the following:

* A Microsoft Azure Tenant.
* A running instance of Secretify with Outlook Properties already configured.

## Setup

> **_NOTE:_**   For ease of readability, we will use the URL `https://example.secretify.io` and Outlook plugin app ID `f825c0bf-76eb-4e0c-8038-b46003c9d9ca` as placeholders. Replace them with the actual URL of your running instance (e.g. `https://secretify.example.com`) and your configured environment variable `PLUGIN_OUTLOOK_APPVERSION`.

### Step 1: Upload the Custom App

1. Open [admin.microsoft.com](https://admin.microsoft.com/) in your web browser.
2. Navigate to `Settings` > `Integrated apps` and click `Upload custom apps`.
3. Choose `Office Add-in` as the App type and check the option for `Provide link to manifest file`.
4. Enter the manifest file link: `https://example.secretify.io/api/v1/outlook/_manifest_f825c0bf-76eb-4e0c-8038-b46003c9d9ca`, and click `Validate`.
5. If it shows as green, click `Next`.
6. Specify the users who should have access to the plugin, which can be yourself, your entire organization, or specific users/groups. Click `Next`.
7. Click `Accept permissions`. You will need to grant permissions for the app to proceed further; a pop-up will appear. Click `Accept`.
8. Click `Next` and then `Finish deployment`.
9. At the end, you should see `Deployment completed`. Click `done` and **wait up to six hours for the app to appear in your Outlook**.
