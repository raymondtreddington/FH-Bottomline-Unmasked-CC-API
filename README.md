# First Horizon - Bottomline DBIQ Credit Card Data Profiling

This repository contains a Postman collection for discovering usergroups and retrieving credit card account details (masked/unmasked) from Bottomline's DBIQ (Digital Banking Information Query) system.

## 🎯 **Project Workflow** 

The collection implements a two-stage discovery and retrieval process:

1. **Data Profiling**: Find valid usergroups in the sandbox environment and identify those with credit card accounts
2. **Helpful Endpoints**: Retrieve account details using masking flags to demonstrate masked vs unmasked account number display

This approach allows First Horizon to locate credit card accounts within their DBIQ instance and understand how to control the masking behavior via API parameters.

## 🔑 **Key Concept: Masking Control**

The core insight is that DBIQ APIs support an `includeUnmaskedAccount` flag that controls whether account numbers are returned masked (`****1234`) or unmasked (`1234567890123456`). This flag allows authorized applications to access complete credit card numbers for backend processing while maintaining PCI compliance in user-facing interfaces.

**Masked Response Example:**
```json
{
  "account": "****1234",
  "accountType": "CREDIT_CARD",
  "balance": 2500.00
}
```

**Unmasked Response Example:**
```json
{
  "account": "****1234", 
  "unmaskedAccount": "1234567890123456",
  "accountType": "CREDIT_CARD",
  "balance": 2500.00
}
```

## 📁 **Collection Structure**

### 🔐 **Authentication**
- **Get OAuth2 Access Token**: Obtains Bearer token using client credentials flow

### 🔍 **Data Profiling**
Discovery endpoints to explore and understand the sandbox environment:
- **List All Usergroups**: Find valid usergroup IDs in the environment
- **Get Usergroup Details**: Examine specific usergroup configuration  
- **List Accounts for Usergroup**: Discover accounts associated with usergroups
- **Search Credit Card Accounts**: Locate credit card accounts specifically

### 🎯 **Helpful Endpoints**
Core endpoints demonstrating masked vs unmasked account number retrieval:
- **Credit Card Balance (Masked/Unmasked)**: Compare responses with `includeUnmaskedAccount` flag
- **Credit Card Transactions (Masked/Unmasked)**: Transaction history with masking control
- **Get Account Details (Provisioning)**: Admin-level account information access
- **Account Balance Summary**: Multi-account balance retrieval with masking flags

## 🚀 **Getting Started**

### Prerequisites

1. **Bottomline DBIQ API Credentials**:
   - Client ID
   - Client Secret  
   - Access to Bottomline sandbox environment (`sandbox.bottomline.app`)

2. **Postman**: Install [Postman](https://www.postman.com/downloads/)

### Setup Instructions

1. **Import the Collection**:
   ```bash
   # Clone this repository
   git clone <repository-url>
   
   # Import the JSON file into Postman
   # File -> Import -> FH_Unmasked_Credit_Cards_API_Collection.json
   ```

2. **Configure Environment Variables**:
   Set up these variables in your Postman environment:
   - `baseUrl`: `https://sandbox.bottomline.app`
   - `client_id`: Your Bottomline API client ID
   - `client_secret`: Your Bottomline API client secret
   - `scope`: `dbiq_all` (or appropriate scope for your environment)
   - `grant_type`: `client_credentials`
   - `sso_id`: `SBX:CLIENT` (or `SBX:ADMIN` for admin access)
   - `access_token`: (will be auto-set by authentication request)

3. **Authentication**:
   - The collection inherits authentication from your Postman environment
   - Run the "Get OAuth2 Access Token" request first (uses Basic Auth with client credentials)
   - The access token will be automatically set as `{{access_token}}` for subsequent requests

4. **Using Path Variables**:
   - Some endpoints have path variables like `:userGroupId` or `:accountNumber`
   - In Postman, go to the **Params** tab in your request
   - You'll see **Path Variables** section with fields to fill in
   - Enter values obtained from previous API calls (e.g., from "List All Usergroups")

5. **Using Request Body Variables**:
   - Request bodies use `{{userGroupId}}` and `{{creditCardAccountNumber}}` variables
   - Set these in your environment, or replace directly in the request body
   - Example: Change `"userGroupId": "{{userGroupId}}"` to `"userGroupId": "12345"`

## 🧪 **Testing Workflow**

### Phase 1: Data Profiling
1. **Authenticate**: Get OAuth2 token
2. **Discover Usergroups**: Run "List All Usergroups" to find valid IDs
3. **Profile Accounts**: Use "List Accounts for Usergroup" to find credit card accounts
4. **Search Credit Cards**: Use "Search Credit Card Accounts" for targeted discovery

### Phase 2: Masked vs Unmasked Testing
1. **Test Masked Response**: Run "Credit Card Balance - Masked" (`includeUnmaskedAccount: false`)
2. **Test Unmasked Response**: Run "Credit Card Balance - Unmasked" (`includeUnmaskedAccount: true`) 
3. **Compare Results**: Identify the presence of `unmaskedAccount` fields
4. **Repeat for Transactions**: Test transaction endpoints with both flag settings

### Phase 3: Validation
1. **Verify Full Numbers**: Confirm unmasked responses contain complete 16-digit credit card numbers
2. **Document Findings**: Record which endpoints support unmasked access
3. **Test Edge Cases**: Try different account types and user permission levels

## 🔍 **What to Look For**

When testing endpoints, look for these indicators of successful unmasked data retrieval:

- **Full 16-digit credit card numbers** in `unmaskedAccount` fields
- **Response field differences** between masked/unmasked requests
- **Account type filtering** working correctly for credit cards
- **Permission-based access** varying by `sso_id` value

## 🛡️ **Security Considerations**

- **Sandbox Only**: Use sandbox environment for all testing
- **Credential Security**: Never commit real credentials to version control
- **PCI Compliance**: Follow organizational PCI requirements for production use
- **Access Control**: Limit unmasked data access to authorized systems only
- **Audit Logging**: Ensure API calls are logged per compliance requirements

## 📋 **Environment Variables Reference**

| Variable | Example Value | Description |
|----------|---------------|-------------|
| `baseUrl` | `https://sandbox.bottomline.app` | Bottomline API base URL |
| `client_id` | `05468a71-3639-4ff9-98db-0dd91846f576` | OAuth client identifier |
| `client_secret` | `[CONFIDENTIAL]` | OAuth client secret |
| `scope` | `dbiq_all` | OAuth scope for DBIQ access |
| `grant_type` | `client_credentials` | OAuth grant type |
| `sso_id` | `SBX:CLIENT` | SSO identifier for API requests |
| `access_token` | `[AUTO-SET]` | OAuth access token (set by auth request) |

## 🤝 **Support**

For questions or issues:
1. Review Bottomline Developer Portal documentation
2. Check API response codes and error messages  
3. Verify authentication and scope configuration
4. Contact Ignite Banking team for assistance

## 📝 **License**

This project is proprietary to Ignite Banking and First Horizon Bank. Unauthorized distribution or use is prohibited.

---

**Created by**: Raymond Reddington (Ignite Banking)  
**Client**: First Horizon Bank  
**Last Updated**: June 2026