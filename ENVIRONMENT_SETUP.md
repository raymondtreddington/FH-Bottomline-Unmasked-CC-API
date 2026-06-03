# Bottomline DBIQ API Environment Setup

This guide covers setting up the proper environment variables and authentication for the First Horizon DBIQ Credit Card Data Profiling collection.

## 🔧 Required Environment Variables

Configure these variables in your Postman environment:

| Variable | Value | Description |
|----------|--------|-------------|
| `baseUrl` | `https://sandbox.bottomline.app` | Bottomline API base URL |
| `client_id` | `[YOUR_CLIENT_ID]` | OAuth client ID from Bottomline |
| `client_secret` | `[YOUR_CLIENT_SECRET]` | OAuth client secret from Bottomline |
| `scope` | `dbiq_all` | OAuth scope for DBIQ access |
| `grant_type` | `client_credentials` | OAuth grant type |
| `sso_id` | `SBX:CLIENT` | SSO identifier for API requests |
| `accessToken` | `[AUTO-SET]` | OAuth token (automatically set by auth request) |

## 🔑 Authentication Flow

### Step 1: Configure Credentials
1. Open Postman Environment settings
2. Set `client_id` and `client_secret` from your Bottomline developer account
3. Ensure `scope` is set to `dbiq_all` (or appropriate scope for your environment)

### Step 2: Obtain Access Token
1. Run the "Get OAuth2 Access Token" request in the Authentication folder
2. The request will automatically set the `accessToken` variable on success
3. All subsequent requests will use this token via `{{accessToken}}`

### Step 3: Verify Authentication
```http
POST {{baseUrl}}/oauth/v1/token
Content-Type: application/x-www-form-urlencoded

client_id={{client_id}}&client_secret={{client_secret}}&grant_type={{grant_type}}&scope={{scope}}
```

Expected response:
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "Bearer", 
  "expires_in": 3600
}
```

## 🏷️ Header Configuration

All API requests (except authentication) use these headers:

| Header | Value | Purpose |
|--------|--------|---------|
| `HTTP_nameid` | `{{sso_id}}` | User identification |
| `Authorization` | `{{accessToken}}` | Bearer token authentication |
| `Content-Type` | `application/json` | Request body format (for POST requests) |

## 🌐 Environment-Specific Settings

### Client Services Environment
- **Client ID**: `05468a71-3639-4ff9-98db-0dd91846f576`
- **SSO ID**: `SBX:CLIENT`
- **Use Cases**: Account balances, transaction history, reporting

### Administrative Environment  
- **Client ID**: `a5826cd0-7a33-4b23-9974-bc0780c6d009`
- **SSO ID**: `SBX:ADMIN` 
- **Use Cases**: User group provisioning, account management

## 🔍 Testing Placeholder Values

When testing endpoints, replace these placeholder values:

| Placeholder | Example | How to Obtain |
|-------------|---------|---------------|
| `{userGroupId}` | `12345` | Run "List All Usergroups" |
| `{accountNumber}` | `1234567890123456` | Run "List Accounts for Usergroup" |
| `{creditCardAccountNumber}` | `4111111111111111` | Search for accounts with type "CREDIT_CARD" |

## 🛠️ Troubleshooting

### Common Issues

**401 Unauthorized**
- Verify client_id and client_secret are correct
- Check that access token is not expired (expires_in: 3600 seconds)
- Re-run authentication request

**403 Forbidden** 
- Verify `sso_id` matches your environment permissions
- Try switching between `SBX:CLIENT` and `SBX:ADMIN`
- Ensure scope includes necessary permissions

**Invalid Scope Error**
- Contact Bottomline support for valid scope values
- Verify your client is authorized for requested scopes
- Check environment-specific scope requirements

### Debug Steps

1. **Verify Connectivity**: Test `GET {{baseUrl}}/health` (if available)
2. **Check Authentication**: Ensure token request returns 200 OK
3. **Test Simple Endpoint**: Try "List All Usergroups" first
4. **Validate Headers**: Confirm HTTP_nameid and Authorization headers are set
5. **Review Logs**: Check Postman console for detailed error messages

## 📞 Support Contacts

- **Bottomline Developer Portal**: [portal.bottomline.app](https://portal.bottomline.app)
- **API Support Email**: DBIQE-APIDevPortal-ProductSupport@bottomline.com
- **Ignite Banking Team**: Contact for internal questions

---

**Note**: Keep credentials secure and never commit them to version control. Use Postman environment variables for sensitive data.