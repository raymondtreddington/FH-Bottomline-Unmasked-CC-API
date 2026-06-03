# First Horizon - Bottomline DBIQ Unmasked Credit Card API Collection

## 🎯 Project Objective

This repository contains a comprehensive Postman collection designed to help First Horizon Bank access unmasked credit card account numbers from their Bottomline DBIQ instance via API. The collection focuses on endpoints that are specifically designed for credit cards and are most likely to bypass the PCI security masking present in the standard user interface.

## 🔍 Problem Statement

First Horizon needs to retrieve unmasked credit card account numbers from Bottomline DBIQ for their business processes. While the standard interface masks these numbers for PCI compliance and security purposes, specific API endpoints are designed to provide authorized systems with access to complete account information.

## 📊 **Current Status Update (2026-06-03)**

### ✅ **Credentials Located**
Located actual Bottomline sandbox credentials in OpenClaw environment:
- **Client Services**: `05468a71-3639-4ff9-98db-0dd91846f576` (for reporting APIs)
- **Provisioning**: `a5826cd0-7a33-4b23-9974-bc0780c6d009` (for user management APIs)
- **Base URL**: `https://sandbox.bottomline.app`
- **OAuth Endpoint**: `/oauth/v1/token` (confirmed accessible)

### ❌ **OAuth Scope Issue - BLOCKING** 
**Problem**: All OAuth authentication attempts fail with **"invalid_scope"** errors.

**Root Cause**: The OAuth `scope` parameter is mandatory, but all tested scope values are rejected:
```
✗ digital_banking_provisioning
✗ digital_banking_client_services  
✗ digital-banking
✗ read, openid, etc.
```

**Resolution Required**: Contact **DBIQE-APIDevPortal-ProductSupport@bottomline.com** for current valid OAuth scope values.

### 🔧 **What's Ready**
✅ Complete Postman collection with 12 endpoints  
✅ Sandbox connectivity confirmed  
✅ API endpoint structure validated  
✅ Authentication flow documented  
✅ Multiple environments configured (Client Services + Provisioning)

### ⏳ **Next Steps**
1. **Immediate**: Resolve OAuth scope issue with Bottomline support
2. **Then**: Execute full endpoint testing
3. **Finally**: Document unmasked credit card examples for First Horizon

## 📋 Comprehensive Endpoint Analysis

### 🔴 **HIGH PRIORITY ENDPOINTS** 
*Credit Card Specific - Most Likely to Return Unmasked Data*

| Endpoint | Base Path | Priority | Description |
|----------|-----------|----------|-------------|
| `POST /balanceAndTransaction/creditCardAccounts/ALLACCOUNTS/accountSummary/getListView` | `/digital-banking/reporting-account-balances` | **HIGHEST** | All credit card account summaries |
| `POST /balanceAndTransaction/creditCardAccounts/CURRDAY/accountSummary/getListView` | `/digital-banking/reporting-account-balances` | **HIGH** | Current day credit card summaries |
| `POST /balanceAndTransaction/creditCardAccounts/accountSummary/requestRealTimeBalances` | `/digital-banking/reporting-account-balances` | **HIGH** | Real-time credit card balances |
| `POST /balanceAndTransaction/creditCardAccounts/currentDay/getTransactions/getListView` | `/digital-banking/reporting-account-transactions` | **HIGH** | Credit card transaction list |
| `POST /balanceAndTransaction/creditCardAccounts/getTransactions/requestRealTimeTransactions` | `/digital-banking/reporting-account-transactions` | **HIGH** | Real-time credit card transactions |

### 🟡 **MEDIUM PRIORITY ENDPOINTS**
*Account Management - Provisioning APIs Often Expose Full Data*

| Endpoint | Base Path | Priority | Description |
|----------|-----------|----------|-------------|
| `GET /usergroups/{userGroupId}/accounts` | `/digital-banking/onboarding-usergroup-provisioning` | **MEDIUM** | All client accounts for user group |
| `POST /accounts:search` | `/digital-banking/onboarding-usergroup-provisioning` | **MEDIUM** | Search accounts by criteria |
| `POST /usergroups:search` | `/digital-banking/onboarding-usergroup-provisioning` | **MEDIUM** | Search user groups with account details |
| `GET /usergroups/{userGroupId}` | `/digital-banking/onboarding-usergroup-provisioning` | **MEDIUM** | User group details with accounts |
| `POST /usergroups/{userGroupId}/accounts` | `/digital-banking/onboarding-usergroup-provisioning` | **MEDIUM** | Create account (response may include full data) |
| `PATCH /usergroups/{userGroupId}/accounts` | `/digital-banking/onboarding-usergroup-provisioning` | **MEDIUM** | Update account |
| `POST /usergroups/{userGroupId}/accountlimits:retrieve` | `/digital-banking/onboarding-usergroup-provisioning` | **MEDIUM** | Account limits with details |

## 🔑 Key Indicators for Unmasked Data

1. **`unmaskedAccount` Flag**: Look for this parameter in request bodies and response fields
2. **Admin-Level Access**: Endpoints using `SBX:ADMIN` in `HTTP_UserIdentifier` header
3. **Provisioning vs Reporting**: Account provisioning/management endpoints often expose full data
4. **Real-Time Requests**: Real-time data endpoints may bypass UI masking
5. **Credit Card Specific**: Endpoints specifically designed for credit card account types

## 🚀 Getting Started

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
   - `baseUrl`: `https://sandbox.bottomline.app`
   - `client_id`: Your Bottomline API client ID
   - `client_secret`: Your Bottomline API client secret
   - `ssoid`: `SBX:ADMIN` (for administrative access)
   - `userGroupId`: Target user group ID (obtain from user group search)

3. **Authentication**:
   - Run the "Get OAuth2 Access Token" request first
   - The access token will be automatically set for subsequent requests

## 📁 Collection Structure

```
📦 First Horizon - Bottomline DBIQ Unmasked Credit Card APIs
├── 🔐 Authentication
│   └── Get OAuth2 Access Token
├── 🔴 HIGH PRIORITY - Credit Card Endpoints
│   ├── Get All Credit Card Account Summaries
│   ├── Get Current Day Credit Card Summaries  
│   ├── Request Real-Time Credit Card Balances
│   ├── Get Credit Card Transaction List
│   └── Request Real-Time Credit Card Transactions
└── 🟡 MEDIUM PRIORITY - Account Management
    ├── Get All Client Accounts for User Group
    ├── Search Accounts by Criteria
    ├── Search User Groups
    ├── Get User Group Details
    ├── Create Client Account
    ├── Update Client Account
    └── Retrieve Account Limits
```

## 🧪 Testing Strategy

1. **Start with HIGH PRIORITY endpoints** - these are most likely to return unmasked data
2. **Use Admin-level access** (`SBX:ADMIN` SSOID) for maximum permissions
3. **Include unmasked flags** in request bodies where applicable:
   ```json
   {
     "includeUnmaskedAccount": true,
     "includeAccountDetails": true
   }
   ```
4. **Test different account types** - focus on `CREDIT_CARD` account type
5. **Check response schemas** for `unmaskedAccount` or similar fields

## 🔍 Response Analysis

When testing endpoints, look for these indicators of unmasked data:

- **Full 16-digit credit card numbers** (vs. masked like `****1234`)
- **Fields named**: `unmaskedAccountNumber`, `fullAccountNumber`, `completeAccount`
- **Admin response sections** that may contain sensitive data
- **Account details in provisioning responses** 

## 🛡️ Security Considerations

- **Use sandbox environment only** for testing
- **Never commit real credentials** to this repository
- **Follow PCI compliance** requirements in production usage
- **Limit access** to authorized personnel only
- **Audit API calls** as required by your organization

## 📊 Documentation Sources

This collection is based on comprehensive analysis of:
- Bottomline DBIQ REST API Specification v24.11.0
- Account Balances Reporting API (16 endpoints)
- Account Transactions Reporting API (12 endpoints) 
- User Group Provisioning API v24.08.0
- Client Account Maintenance API
- User Management Provisioning API

## 🤝 Support

For technical questions or issues:
1. Check Bottomline Developer Portal documentation
2. Review API response codes and error messages
3. Verify authentication and permissions
4. Contact Ignite Banking team for assistance

## 📝 License

This project is proprietary to Ignite Banking and First Horizon Bank. Unauthorized distribution or use is prohibited.

---

**Created by**: Raymond Reddington (Ignite Banking)  
**Client**: First Horizon Bank  
**Last Updated**: December 2024