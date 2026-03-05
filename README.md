# Evershop API Testing with Postman

This repository contains an automated API testing suite for the [Evershop Demo](https://demo.evershop.io/) site, built using Postman. The collection validates core e-commerce functionalities such as searching for products, interacting with the shopping cart, and dynamically chained API operations.

This project is tailored to demonstrate advanced QA and API automation skills, including dynamic data generation, variable extraction, request chaining, and comprehensive assertions.

## 🚀 Features & Testing Techniques Demonstrated

*   **Dynamic Data Generation:** Utilizes pre-request scripts to inject dynamic content (e.g., picking a random search keyword from an array, generating random product quantities). This ensures test robustness across different combinations.
*   **Request Chaining:** Seamless workflows where responses from one request are extracted and utilized in subsequent requests (e.g., fetching a product's SKU from the search response and passing it to the "Add to Cart" API).
*   **Environment & Collection Variables:** Effective management of state during the test execution flow using Postman Collection Variables (`search_text`, `product_sku`, `product_qty`, `remove_api`, etc.).
*   **Detailed Validations (Assertions):** Tests are not limited only to status codes (e.g., HTTP 200 OK); they deeply validate JSON responses. Tests include checking for array lengths, boolean flags, numeric string conversions, and ensuring correct mathematical totals inside the cart.

## 🗂 API Endpoints Covered

The Postman Collection (`evershop_collection_postman.json`) consists of five linked requests simulating an end-to-end user shopping behavior:

1.  **`search` (GET)**
    *   *Pre-request Script:* Picks a random keyword (`Cup`, `Desk`, `Black`, `White`) and sets it as `search_text`.
    *   *Action:* Queries the search API for the keyword.
    *   *Tests:* Validates status code 200, verifies success flag, ensures cart item length is greater than 0, checks if returned product names include the search term, and extracts `product_name` and `product_sku` for the next request.
2.  **`pre_viewCart` (GET)**
    *   *Action:* Fetches the current state of the cart before adding items.
    *   *Tests:* Validates status code 200, handles empty cart exceptions using `try-catch` block, and captures the current cart quantity (`pre_total_qty`).
3.  **`addToCart` (POST)**
    *   *Pre-request Script:* Generates a random number between 1 to 5 to be used as `product_qty`.
    *   *Action:* Submits the previously extracted `sku` and randomly generated `qty` in the JSON request body.
    *   *Tests:* Validates status code 200. Automatically calculates expected cart quantity combining `pre_total_qty` + new `product_qty` and verifies it matches the server's returned cart quantity.
4.  **`post_viewCart` (GET)**
    *   *Action:* Views the cart after items are added.
    *   *Tests:* Validates status code 200. Re-verifies the cart total. Dynamically extracts the RESTful API Endpoint to cleanly remove the newly added product (`remove_api`).
5.  **`removeCart` (DELETE)**
    *   *Action:* Sends a dynamically built DELETE request using the previously extracted `remove_api` endpoint.
    *   *Tests:* Verifies status code 200 to ensure successful cleanup.

## 🛠️ Prerequisites

*   **[Postman](https://www.postman.com/downloads/)**: You must have the Postman desktop application or web agent installed to import and run this collection.

## 📥 How to Run the Tests

1.  Clone this repository or download the `evershop_collection_postman.json` file.
2.  Open **Postman**.
3.  Click on the **Import** button in the top left corner of the Postman UI.
4.  Select and upload the `evershop_collection_postman.json` file.
5.  Once imported, the collection will appear as "My Collection" under your Collections tab.
6.  Click on the collection name to open the overview.
7.  Click the **Run** button (Collection Runner) on the top right.
8.  Ensure all requests are checked.
9.  Click **Run My Collection**.
10. Review the test results and detailed assertions under the Collection Runner dashboard.

## 🔧 Extracted Variables Reference

Throughout the lifecycle of the collection runner, these variables are actively managed and passed:
*   `search_text`: The keyword being searched.
*   `product_name`: Stored product name from the search response.
*   `product_sku`: Stored SKU to be added to the cart later.
*   `product_qty`: Number of items to add to the cart.
*   `pre_total_qty`: How many items existed in the cart prior to modification.
*   `total_qty`: The active calculated total length of the cart.
*   `remove_api`: Dynamically captured endpoint route for test teardown.
