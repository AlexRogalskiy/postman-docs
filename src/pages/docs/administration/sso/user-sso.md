---
title: "Logging in to an SSO team"
order: 133
updated: 2021-02-08
page_id: "user_sso"
warning: false
contextual_links:
  - type: section
    name: "Prerequisites"
  - type: link
    name: "Intro to SSO"
    url: "/docs/administration/sso/intro-sso/"
---

> __[SSO is available on Postman Professional and Enterprise plans.](https://www.postman.com/pricing)__

When your team admin has enabled single sign-on (**SSO**) for Postman, you can sign in to Postman with a [configured Identity provider](/docs/administration/sso/intro-sso/).

Your team admin can provide a **Login URL** that was generated during the SSO configuration. The **Login URL** will automatically redirect you to your configured Identity Provider.

To sign into Postman:

1. Select **Sign In** in the upper-right corner of Postman
1. Select **Home** in the upper-left, then select **Sign in**
1. Open [Postman's website](https://www.postman.com/) and select **Sign In** in the upper-right corner

<img src="https://assets.postman.com/postman-docs/create-account-or-sign-in.jpg" alt="Create account or sign in"/>

When you opt to sign into the desktop version of Postman, you will see the following screen and your default browser will automatically open a webpage asking you to sign into Postman.

> Your browser will open within a few seconds. If it doesn't, select **open your browser** in Postman.

<img src="https://assets.postman.com/postman-docs/sign-in-with-web-browser.jpg" alt="Sign in with web browser"/>

> You must finish signing in within five minutes once you initiate the process from Postman. If you go beyond this time, you must return to Postman and restart the sign in process.

Sign into Postman by selecting **Sign in with Single Sign-On (SSO)** in the upper-right.

<img src="https://assets.postman.com/postman-docs/sign-in-full-page-20.jpg" alt="Sign in"/>

Enter your team domain and **Continue**.

> Check **Keep me signed in** if you would like to remain signed in after your current session for 30 days before re-authenticating. If you do not want to remain signed in on the computer you are working on, uncheck this option. Note that you will be prompted to sign in again after 30 minutes.

<img src="https://assets.postman.com/postman-docs/sso-login-continue.jpg" alt="Sign in with SSO" width="350px"/>

Postman will redirect you to the configured SSO provider. Enter your SSO details to sign in.

> The first time you sign in to Postman, you may need to carry out additional steps to set up access to your identity provider account. Sign in using SSO and follow your provider's steps to connect the account to Postman.

Upon signing in, you will see a confirmation in your browser and you will be redirected back to Postman.

<img src="https://assets.postman.com/postman-docs/youre-signed-in-confirmation.jpg" width="350px" alt="Signed in confirmation"/>
