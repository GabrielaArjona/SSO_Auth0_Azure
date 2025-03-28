# GreenRetail SSO Project: Auth0 (IdP) + Azure AD (SP)

**Project Context**  
GreenRetail is a fictional company used for a **practice scenario** involving Single Sign-On (SSO) with **Auth0** and **Azure AD**. Both accounts (Auth0 and Azure) are **trial/free** editions, which means certain advanced features (like Conditional Access) were not available. Additionally, there's no real production web page to redirect to after authentication; this was purely **an exercise** to explore SSO configurations.

## Table of Contents
1. [Overview](#overview)
2. [Objectives](#objectives)
3. [Architecture & Components](#architecture--components)
4. [Implementation Steps](#implementation-steps)
   - [A. Create a Non-Gallery App in Azure AD](#a-create-a-non-gallery-app-in-azure-ad)
   - [B. Configure Auth0 as IdP (SAML2 Add-On)](#b-configure-auth0-as-idp-saml2-add-on)
   - [C. Assign Users in Auth0 & Azure AD](#c-assign-users-in-auth0--azure-ad)
   - [D. Testing the SSO Flow](#d-testing-the-sso-flow)
5. [Common Issues & Troubleshooting](#common-issues--troubleshooting)
6. [Screenshots](#screenshots)
7. [Conclusions & Learnings](#conclusions--learnings)

---

## Overview
GreenRetail aims to set up a **Single Sign-On** solution without a real production app. Instead, we used **Auth0** (as IdP) and **Azure AD** (as SP) purely for **educational** purposes. The focus was on basic SAML integration and verifying that a user could log in once (in Auth0) and have Azure AD accept the SAML response.

---

## Objectives
1. Understand how **Auth0** issues SAML tokens as the IdP.  
2. Configure **Azure AD** as the Service Provider to consume those tokens.  
3. Demonstrate both **IdP-Initiated** and **SP-Initiated** flows.  
4. Use **trial/free** accounts to learn core SSO concepts without a production environment.

---

## Architecture & Components
- **Auth0 (IdP)**: Stores user credentials and provides SAML assertions upon successful login.  
- **Azure AD (SP)**: Validates Auth0’s SAML responses and grants or denies access.  
- **User**: A test account with matching email in both Auth0 and Azure AD.

No actual web page is used for final redirection; we tested sign-ins using built-in URLs (Auth0’s “Usage” link, My Apps portal in Azure, etc.).

---

## Implementation Steps

### A. Create a Non-Gallery App in Azure AD
1. In **Azure Portal** → **Azure AD** → **Enterprise Applications** → **New application**.  
2. Choose **Create your own application** and name it `Auth0_IdP_Federation`.  
3. Select **Single sign-on (SAML)** and configure:
   - **Identifier (Entity ID)** = e.g. `https://login.microsoftonline.com/<TenantID>/callback-id`  
   - **Reply URL (ACS)** = e.g. `https://login.microsoftonline.com/<TenantID>/callback`

### B. Configure Auth0 as IdP (SAML2 Add-On)
1. In the **Auth0 Dashboard**, pick your **Application** (Regular Web App).  
2. Go to **Addons** → **SAML2 Web App**:
   - **Application Callback URL** = The same Reply URL from Azure AD, e.g. `https://login.microsoftonline.com/<TenantID>/callback`  
   - **audience** = The Identifier from Azure AD, e.g. `https://login.microsoftonline.com/<TenantID>/callback-id`   
   - **recipient** / **destination** = The Reply URL, e.g. `https://login.microsoftonline.com/<TenantID>/callback`  
   - Set `"signResponse": true` in the JSON.  
3. Save and check the **Usage** tab for the **IdP Login URL**.

### C. Assign Users in Auth0 & Azure AD
1. **Auth0**: Create a user `greenretailone@...` (or “greenretailtest@...”) with a password.  
2. **Azure AD**: Create or ensure the same email user is active. (Non-Gallery App, assign the user.)  
3. No advanced features like Conditional Access are used since these are trial accounts.

### D. Testing the SSO Flow
1. **IdP-Initiated**: From Auth0’s **Usage** link (SAML2 Web App), enter the user’s email/password.  
2. **SP-Initiated**: Attempt to log in via [myapps.microsoft.com](https://myapps.microsoft.com).  
3. **Observe** logs in Auth0 (successful login) and in Azure AD (interactive and non-interactive sign-ins).

---

## Common Issues & Troubleshooting
- **No sign-in log in Azure AD**: IdP-Initiated SSO can be deemed non-interactive.  
- **Audience mismatch**: Ensure "audience" in Auth0 matches Azure AD’s Identifier.  
- **User not found**: Azure AD must have the same exact email as Auth0.  
- **HTTP 404**: If you used a placeholder Reply URL with no actual page, you may see 404.

---

## Screenshots
1. **Azure AD Non-Gallery**: SAML configuration (Identifier, Reply URL).  
2. **Auth0 SAML2**: JSON snippet with audience, recipient, etc.  
3. **Auth0 Logs**: Show a successful login for `greenretailtest@...`.  
4. **Azure AD Sign-In Logs**: Displaying the user sign-in.  
5. **Auth0 Sign-In**:The sign-in page.

---

## Conclusions & Learnings
1. **Core SSO Concepts**: Successfully configured Auth0 as IdP and Azure AD as SP using SAML, proving the fundamentals of claims, audience, and recipient/destination matching.  
2. **Trial Environments**: Because both Auth0 and Azure AD accounts were free/trial, certain advanced features (e.g., Conditional Access) were unavailable.  
3. **No Production Redirect**: We used placeholders and tested with built-in endpoints for sign-in. This was purely a practice lab, not a production deployment.  
4. **Auth0 & Azure AD Behavior**: Learned how Azure AD classifies sign-ins (interactive vs. non-interactive).  
5. **Key Takeaway**: Even on free tiers, you can explore major SSO flows, gather logs, and understand the synergy between **IdP** (Auth0) and **SP** (Azure AD).
