# Environment Setup for First Horizon - Bottomline DBIQ API Testing

## ⚠️ Current OAuth Issues (2026-06-03)

**Status:** OAuth authentication is currently failing with "invalid_scope" errors.

The credentials below are from the OpenClaw .env file and were confirmed working on 2026-04-18, but the Bottomline sandbox OAuth configuration appears to have changed.

**Contact for OAuth Support:**
- Email: DBIQE-APIDevPortal-ProductSupport@bottomline.com
- Issue: Need current valid OAuth scope values for sandbox environment

## Required Environment Variables

### Client Services Environment (for Account Balances & Transactions)

```json
{
  "name": "Bottomline DBIQ - Client Services",
  "values": [
    {
      "key": "baseUrl",
      "value": "https://sandbox.bottomline.app",
      "enabled": true
    },
    {
      "key": "client_id", 
      "value": "05468a71-3639-4ff9-98db-0dd91846f576",
      "enabled": true
    },
    {
      "key": "client_secret",
      "value": "DDK2mvH0jpYqhvMhS8BrllUPPtfMg7ge", 
      "enabled": true
    },
    {
      "key": "auth_token_url",
      "value": "https://sandbox.bottomline.app/oauth/v1/token",
      "enabled": true
    },
    {
      "key": "ssoid",
      "value": "SBX:CLIENT",
      "enabled": true
    },
    {
      "key": "access_token",
      "value": "",
      "enabled": true
    }
  ]
}
```

### Provisioning Environment (for User Group & Account Provisioning)

```json
{
  "name": "Bottomline DBIQ - Provisioning",  
  "values": [
    {
      "key": "baseUrl",
      "value": "https://sandbox.bottomline.app",
      "enabled": true
    },
    {
      "key": "client_id",
      "value": "a5826cd0-7a33-4b23-9974-bc0780c6d009", 
      "enabled": true
    },
    {
      "key": "client_secret",
      "value": "fpdSq5IgP0nr5ul4BlArqYUihkc4V2y9",
      "enabled": true
    },
    {
      "key": "auth_token_url",
      "value": "https://sandbox.bottomline.app/oauth/v1/token",
      "enabled": true
    },
    {
      "key": "ssoid",
      "value": "SBX:ADMIN",
      "enabled": true
    },
    {
      "key": "access_token",
      "value": "",
      "enabled": true
    },
    {
      "key": "userGroupId",
      "value": "SAMPLE_USER_GROUP_ID",
      "enabled": true
    }
  ]
}
```

## Setup Steps

1. **OAuth Scope Resolution (REQUIRED FIRST)**:
   - Contact DBIQE-APIDevPortal-ProductSupport@bottomline.com
   - Request current valid OAuth scope values for client_credentials grant
   - Specifically need scopes for:
     - digital-banking reporting APIs (account balances, transactions)
     - digital-banking provisioning APIs (user management, account maintenance)

2. **Import Environment**:
   - In Postman, go to Environments
   - Click "Import" and create two environments with the JSON above
   - Use Client Services environment for reporting endpoints
   - Use Provisioning environment for user/account management endpoints

3. **Test Authentication (once scopes resolved)**:
   - Run the "Get OAuth2 Access Token" request
   - Verify that the `access_token` variable is automatically set
   - Test with both environments

4. **Find User Group ID**:
   - Run the "Search User Groups" request to find available user groups
   - Update the `userGroupId` variable with a valid ID from the response

## Authentication Flow

The collection uses OAuth 2.0 Client Credentials flow:

1. POST to `/oauth/v1/token` with client credentials + valid scope
2. Receive access_token in response  
3. Use Bearer token for all subsequent API calls
4. Include appropriate SSOID header:
   - `HTTP_UserIdentifier: SBX:CLIENT` for reporting APIs
   - `HTTP_UserIdentifier: SBX:ADMIN` for provisioning APIs

## Current Testing Status

✅ **Confirmed Working:**
- Sandbox connectivity (sandbox.bottomline.app responds)
- OAuth endpoint exists at `/oauth/v1/token`
- API endpoints exist (return proper 403/400 errors vs 404)
- Credentials are valid (no "invalid_client" errors)

❌ **Blocked:**
- OAuth scope parameter - all tested values return "invalid_scope"
- Cannot obtain access tokens without valid scope
- API calls fail with "Authorization field missing" errors

## Next Steps

1. **Immediate:** Contact Bottomline support for current OAuth scopes
2. **Once resolved:** Test all endpoints for masked vs unmasked account data
3. **Document:** Update this file with working scope values
4. **Validate:** Confirm unmasked credit card account number access

## Tested OAuth Scopes (All Failed - 2026-06-03)

- `digital_banking_provisioning`
- `digital_banking_client_services`
- `digital-banking`
- `digital_banking`
- `provisioning`
- `reporting`
- `read`
- `client_services`
- `openid`

Error: "The API scope URL provided is invalid" or "Missing Mandatory Parameter: Scope"