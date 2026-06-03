# First Horizon Unmasked Credit Card API Collection - Implementation Summary

## Problem Statement
First Horizon Bank needed API access to unmasked credit card account numbers from their Bottomline DBIQ instance, bypassing the UI masking functionality for security/PCI purposes.

## Solution Overview
Created a comprehensive Postman collection implementing the correct Bottomline DBIQ accounts search endpoint with the `unmaskColumns` parameter to access unmasked credit card data.

## Key Implementation Details

### Correct Endpoint Structure
- **Endpoint**: `POST /digital-banking/onboarding-usergroup-provisioning/accounts:search`
- **Schema**: Uses ListViewRequest schema with `unmaskColumns` parameter
- **Method**: POST (not GET as initially attempted)

### Request Structure
```json
{
  "criteria": [
    {
      "fieldName": "accountType",
      "fieldValue": ["CC"]
    },
    {
      "fieldName": "userGroupId",
      "fieldValue": ["12345"]
    }
  ],
  "unmaskColumns": true,
  "operationalData": true,
  "limit": 25,
  "page": 1
}
```

### Critical Headers
- `HTTP_UserIdentifier: SBX:ADMIN` (for sandbox access)
- `Authorization: {{access_token}}` (OAuth Bearer token)
- `Content-Type: application/json`
- `Accept: application/json`

### Authentication Flow
- **OAuth Endpoint**: `POST /oauth/v1/token`
- **Auth Type**: Basic Authentication (client_id/client_secret)
- **Scope**: `digital-banking` (confirmed working scope)
- **Grant Type**: `client_credentials`
- **Body Format**: URL-encoded (`grant_type` and `scope` parameters)

## Collection Structure

### 1. Authentication Folder
- **Get OAuth Token**: Manual token acquisition with test script for token capture
- **Auto-OAuth Script**: Collection-level pre-request script for automatic token management

### 2. Data Profiling Folder  
- **Get All Usergroups**: Discovery of available user groups
- **Search User Groups**: Advanced filtering and search capabilities
- **Get User Group Details**: Detailed information about specific user groups
- **Get All Accounts for User Group**: Account discovery within user groups

### 3. Usergroup Account Access - Unmasked Credit Cards Folder
- **Search Credit Card Accounts (UNMASKED)**: Core endpoint with `unmaskColumns: true`
- **Search Credit Card Accounts (MASKED)**: Comparison endpoint with `unmaskColumns: false`
- **Search All Account Types (UNMASKED)**: Broader account discovery without type filtering

## Environment Variables
- `baseUrl`: API base URL
- `client_id`: OAuth client identifier
- `client_secret`: OAuth client secret  
- `scope`: OAuth scope (`digital-banking`)
- `grant_type`: OAuth grant type (`client_credentials`)
- `sso_id`: SSO identifier for context headers
- `access_token`: Auto-managed OAuth token

## Key Implementation Corrections

### From Initial Wrong Approach
- **❌ Wrong**: GET endpoints for individual account management
- **❌ Wrong**: `/usergroups/{id}/accounts?includeUnmaskedAccount=true` query parameter approach
- **❌ Wrong**: Account balance/transaction endpoints requiring admin privileges

### To Correct Implementation  
- **✅ Correct**: POST accounts:search endpoint with ListViewRequest schema
- **✅ Correct**: `unmaskColumns` parameter in request body for masking control
- **✅ Correct**: Criteria-based filtering with `accountType: CC` and `userGroupId`
- **✅ Correct**: `HTTP_UserIdentifier: SBX:ADMIN` for sandbox access
- **✅ Correct**: `operationalData: true` for complete account information

## Documentation References
Based on Jason's provided references:
1. Google Drive documents containing official Bottomline DBIQ API documentation
2. Example curl command demonstrating proper request structure
3. ListViewRequest schema documentation showing `unmaskColumns` parameter usage
4. Account type filtering patterns (`CC` for credit cards)

## Testing Validation
- OAuth authentication flow tested and working with `digital-banking` scope
- Collection structure validated for Postman import compatibility  
- Request body structure matches official API documentation exactly
- Headers configured according to sandbox requirements

## Repository
- **GitHub**: https://github.com/raymondtreddington/FH-Bottomline-Unmasked-CC-API
- **Latest Commit**: d08e17f - Added comprehensive descriptions to search endpoints
- **Collection File**: FH_Unmasked_Credit_Cards_API_Collection.json

## Next Steps for First Horizon
1. Import collection into Postman
2. Configure environment variables with actual credentials
3. Test OAuth authentication flow
4. Use Data Profiling endpoints to discover available user groups
5. Execute unmasked credit card search with real user group IDs
6. Compare masked vs unmasked results to verify functionality

## Critical Success Factors
- ✅ Uses officially documented endpoint structure
- ✅ Implements correct `unmaskColumns` parameter for unmasking
- ✅ Follows Bottomline DBIQ ListViewRequest schema exactly  
- ✅ Includes proper OAuth 2.0 client credentials flow
- ✅ Provides both discovery and execution workflows
- ✅ Demonstrates masked vs unmasked comparison capability