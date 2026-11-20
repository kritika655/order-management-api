# Order Management API

A structured Express REST API demonstrating CRUD, Zod validation, centralized errors,
pagination/filtering, OpenAPI documentation, webhook simulation, and consistent responses.

## Requirements
- Node.js 20 or newer
- npm

## Run on Windows PowerShell
```powershell
npm install
Copy-Item .env.example .env
npm run dev
```
If `Copy-Item` says the file exists, keep your existing `.env`.
The API runs at `http://localhost:3000`.
Swagger UI: `http://localhost:3000/api-docs`
Health: `http://localhost:3000/health`

## Main endpoints
- `POST /api/v1/orders` create an order
- `GET /api/v1/orders` list orders (`page`, `limit`, `status`, `customerEmail`)
- `GET /api/v1/orders/:id` get one order
- `PATCH /api/v1/orders/:id` update order fields
- `DELETE /api/v1/orders/:id` delete an order
- `POST /api/v1/webhooks/simulate` send a sample event to `WEBHOOK_TARGET_URL`
- `POST /api/v1/webhooks/receiver` local demo receiver

## Example create request
```json
{
  "customerName": "Asha Sharma",
  "customerEmail": "asha@example.com",
  "items": [
    { "sku": "BOOK-101", "name": "REST API Guide", "quantity": 2, "unitPrice": 499 }
  ]
}
```

## Response format
Success:
```json
{
  "success": true,
  "message": "Order created",
  "data": { "id": "...", "status": "pending" },
  "meta": null
}
```
Error:
```json
{
  "success": false,
  "message": "Validation failed",
  "error": { "code": "VALIDATION_ERROR", "details": [] },
  "meta": null
}
```

## Important production note
This starter persists JSON data locally to keep setup simple. JSON-file storage is not
suitable for concurrent production workloads, multiple server instances, or strong
transaction guarantees. Before production, replace `src/repositories/orderRepository.js`
with PostgreSQL (or another database), add authentication/authorization, rate limiting,
request size limits, audit logging, secrets management, automated tests, and a real
webhook signature/retry strategy. The webhook simulator is for local learning only.
