# Azure AD (EntraID) Registration

### Step 1: Register Your Custom API in Azure AD

1. **Sign in to the Azure portal**:
   - Go to the [Azure portal](https://portal.azure.com/).

2. **Navigate to Azure Active Directory**:
   - In the row of icons at the top, select **Microsoft Entra ID**.

3. **Register a new application (API)**:
   - Click "+Add" at the top
   - Click on **App Registration**.
   - Enter a **Name** for the API.
   - Choose the **Supported account types** that apply to your scenario.
   - Enter a **Redirect URI** (optional for now, you can set it later if needed).
   - Click **Register**.

### Step 2: Expose Your API

1. **Expose an API**:
   - After registering the API, you'll be redirected to the app's overview page.
   - In the left-hand menu, click **Manage**, which will open more options. In the new list of options, click **Expose An API**
   - Click on **+Add a scope**.
   - If prompted, click **Save and continue** to set the Application ID URI.
   - Define the scope's details:
     - **Scope name**: For example, `access_as_user`.
     - **Who can consent**: Typically, `Admins and users`.
     - **Admin consent display name**: A friendly name for admin consent.
     - **Admin consent description**: Description for admin consent.
     - **User consent display name**: A friendly name for user consent.
     - **User consent description**: Description for user consent.
     - **State**: Set to **Enabled**.
   - Click **Add scope**.

### Step 3: Register the Client Application

1. **Register a new application (Client)**:
   - Go back to **App registrations**.
   - Click on **New registration**.
   - Enter a **Name** for the client application.
   - Choose the **Supported account types** that apply to your scenario.
   - Enter a **Redirect URI** (optional for now, you can set it later if needed).
   - Click **Register**.

### Step 4: Configure Permissions for the Client Application

1. **Configure API permissions**:
   - After registering the client application, you'll be redirected to the app's overview page.
   - In the left-hand menu, select **API permissions**.
   - Click on **Add a permission**.
   - Select **My APIs**.
   - Choose the API you registered earlier.
   - Select the scope you defined (e.g., `access_as_user`).
   - Click **Add permissions**.
   - If needed, click **Grant admin consent** for the permissions (requires admin privileges).

### Step 5: Generate Client ID and Client Secret for the Client Application

1. **Get the Client ID**:
   - Copy the **Application (client) ID** of the client application. This is your Client ID.

2. **Create a Client Secret**:
   - At the top right corner, click into **Certificates & secrets**.
   - Under **Client secrets**, click on **New client secret**.
   - Add a description and set an expiration period.
   - Click **Add**.
   - Copy the generated **Value** immediately. This is your Client Secret. (You won't be able to see it again once you leave this page.)

### Step 6: Use the Credentials with the Audience ID GUID in the Scope

You can now use the Client ID, Client Secret, and scope with the audience ID GUID in your application to authenticate and authorize against Azure AD.

### Example Usage in Code

Here's an example of how you might use these credentials in a Python application using the `requests` library to authenticate:

```python
import requests

tenant_id = 'your-tenant-id'
client_id = 'your-client-id'
client_secret = 'your-client-secret'
audience_id_guid = 'your-audience-id-guid'  # This is the Application (client) ID of your API

# Construct the scope
scope = f'{audience_id_guid}/.default'

# OAuth2 token endpoint
token_url = f'https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token'

# Prepare the request payload
payload = {
    'grant_type': 'client_credentials',
    'client_id': client_id,
    'client_secret': client_secret,
    'scope': scope
}

# Request the token
response = requests.post(token_url, data=payload)
response.raise_for_status()
token = response.json().get('access_token')

print('Access token:', token)
```

Replace the placeholders with your actual `tenant_id`, `client_id`, `client_secret`, and `audience_id_guid`.

