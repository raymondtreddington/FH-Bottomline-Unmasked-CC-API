# Environment Setup for First Horizon - Bottomline DBIQ API Testing

## Required Environment Variables

Set these variables in your Postman environment:

```json
{
  "name": "First Horizon Bottomline DBIQ",
  "values": [
    {
      "key": "baseUrl",
      "value": "https://sandbox.bottomline.app",
      "enabled": true
    },
    {
      "key": "client_id", 
      "value": "YOUR_CLIENT_ID_HERE",
      "enabled": true
    },
    {
      "key": "client_secret",
      "value": "YOUR_CLIENT_SECRET_HERE", 
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

1. **Get Bottomline API Credentials**:
   - Contact Bottomline support for sandbox API credentials
   - Ensure you have client_id and client_secret for the sandbox environment

2. **Import Environment**:
   - In Postman, go to Environments
   - Click "Import" and select this environment file
   - Update the `client_id` and `client_secret` with your actual credentials

3. **Test Authentication**:
   - Run the "Get OAuth2 Access Token" request
   - Verify that the `access_token` variable is automatically set

4. **Find User Group ID**:
   - Run the "Search User Groups" request to find available user groups
   - Update the `userGroupId` variable with a valid ID from the response

## Authentication Flow

The collection uses OAuth 2.0 Client Credentials flow:

1. POST to `/oauth2/token` with client credentials
2. Receive access_token in response
3. Use Bearer token for all subsequent API calls
4. Include `HTTP_UserIdentifier: SBX:ADMIN` header for administrative access

## Testing Notes

- All endpoints are configured to work with the sandbox environment
- The `SBX:ADMIN` SSOID provides maximum administrative permissions
- Look for `unmaskedAccount` or similar fields in API responses
- Start with HIGH PRIORITY endpoints for best chance of success