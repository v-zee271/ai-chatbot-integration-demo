# PayBridge API V2 Reference Snippet

The PayBridge API allows developers to programmatically process cross-border payments. This reference guide provides the technical specifications for initiating a transaction.
<br/>

## 1. Authentication
To ensure security, all requests must be authenticated using a Bearer Token in the HTTP header. Unauthorized requests will return a `401` error.

**Header Requirement:**
`Authorization: Bearer YOUR_API_KEY`
<br/>

## 2. Endpoints: Transactions
Transactions are the core object of the PayBridge ecosystem. This endpoint allows for the creation of new outbound payments.

### Create a Transaction
`POST /transactions`

**Base URL:** `https://api.paybridge.com/v2`

#### Request Body Parameters
Providing a clear glossary of fields is essential for reducing integration friction.

| Field | Type | Mandatory | Description |
| :--- | :--- | :--- | :--- |
| `amount` | Integer | Yes | The total value in cents (e.g., 1500 = $15.00). |
| `currency` | String | Yes | 3-letter ISO currency code (e.g., "USD"). |
| `recipient_id` | String | Yes | The unique identifier for the verified recipient. |
| `metadata` | Object | No | Key-value pairs for internal tracking and order IDs. |

#### Example Request (JSON)
```json
{
  "amount": 1500,
  "currency": "USD",
  "recipient_id": "rcp_98765",
  "metadata": {
    "order_id": "A102"
  }
}
```

#### Example Response (201 Created)
A successful request returns a transaction object with a `pending` status while the funds are cleared.

```json
{
  "transaction_id": "tx_abc123",
  "status": "pending",
  "created_at": "2026-04-07T10:00:00Z"
}
```
<br/>

## 3. Error Handling
The API uses standard HTTP response codes to indicate the success or failure of an API request.

| Status Code | Error Type | Description |
| :--- | :--- | :--- |
| **401** | Unauthorized | Missing or invalid API key in the header. |
| **422** | Unprocessable Entity | The recipient ID does not exist or is currently inactive. |
| **500** | Server Error | An unexpected error occurred on our end. |


