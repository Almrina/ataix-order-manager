# ataix-order-manager
Here's a **README** template for your project on GitHub:

---

# Order Management System

This project provides a Python script for managing orders through an API, where orders are monitored, updated, and re-created if needed. The script communicates with the API of Ataix to fetch order statuses, cancel unfilled orders, and create new ones at adjusted prices.

## Requirements

- Python 3.x
- Requests library (`pip install requests`)

## Files

- `orders.json`: A file that stores the list of orders (both existing and new). This file is read and updated by the script.

## Script Overview

### Functions

1. **`load_orders()`**:  
   Loads the existing orders from the `orders.json` file.

2. **`save_orders(data)`**:  
   Saves the provided orders to `orders.json`. If the file does not exist, it is created. Existing orders are merged with the new ones based on their `orderID`.

3. **`send_request(endpoint, method="GET", data=None)`**:  
   Sends an API request to the Ataix API. This function supports `GET` and `POST` requests (and others if needed). It handles the API response and returns the result or logs errors if the request fails.

4. **`update_orders()`**:  
   This is the main function that:
   - Loads the orders from the file.
   - Checks the status of each order via the API.
   - Cancels unfilled orders.
   - Creates new orders at adjusted prices (1% higher than the original price).
   - Saves the updated list of orders to the `orders.json` file.

### API Endpoint

The script interacts with the Ataix API using the following base URL:

```
https://api.ataix.kz
```

- **GET** `/api/orders/{order_id}`: Fetches the status of a specific order.
- **DELETE** `/api/orders/{order_id}`: Cancels the order.
- **POST** `/api/orders`: Creates a new order.

### Configuration

- **API_KEY**: The API key required for authentication with the Ataix API.
- **ORDERS_FILE**: The file where order information is stored (`orders.json`).

### Example of a Request:

A `POST` request is made to the `/api/orders` endpoint to create a new order:

```json
{
  "symbol": "TRX/USDT",
  "price": "0.23",
  "side": "buy",
  "type": "limit",
  "quantity": 1
}
```

The `price` is adjusted by 1% higher for the re-created orders.

### Usage

1. Clone the repository:
    ```bash
    git clone https://github.com/yourusername/order-management.git
    cd order-management
    ```

2. Install dependencies:
    ```bash
    pip install requests
    ```

3. Run the script:
    ```bash
    python order_manager.py
    ```

This will:
- Fetch existing orders from `orders.json`.
- Check the status of each order.
- Cancel unfilled orders.
- Recreate cancelled orders with adjusted prices.

### Notes

- **Order File**: The file `orders.json` stores the current orders. Ensure that this file exists and has the proper format, or it will be created automatically.
- **API Rate Limiting**: Ensure you are aware of any rate limits imposed by the API to avoid making too many requests in a short period.
