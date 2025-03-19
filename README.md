# Voucher QC Tool

An automated quality control tool for testing e-commerce voucher codes. This tool helps you verify that voucher codes are working correctly on your e-commerce store by automating the testing process.

## Features

- **Automated Testing**: Automatically test voucher codes by simulating the customer journey from product page to checkout.
- **Multiple Voucher Types**: Support for various voucher types including percentage discounts, fixed amount discounts, free shipping, BOGO (Buy One Get One), tiered discounts, and combinations.
- **Parallel Testing**: Run multiple tests concurrently to save time.
- **Detailed Reports**: Generate comprehensive reports in various formats (HTML, JSON, CSV, Excel).
- **Screenshots**: Capture screenshots at each step of the testing process for visual verification.
- **Configurable**: Easily configure the tool to work with different e-commerce platforms and store themes.
- **UTM Support**: Test vouchers with UTM parameters to verify marketing campaign tracking.

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/voucher-qc.git
cd voucher-qc

# Install dependencies
npm install

# Build the tool
npm run build
```

## Quick Start

1. Initialize the configuration:

```bash
npm run init
```

This will create the necessary directories and sample configuration files.

2. Update the store configuration in `config/stores.json` with your store details.

3. Create your test data or update the sample tests in `config/tests.json`.

4. Run the tests:

```bash
npm run run -- --file config/tests.json
```

## Usage

### Command Line Interface

```
Usage: voucher-qc [options] [command]

Voucher QC Tool - Automated testing for e-commerce voucher codes

Options:
  -V, --version              output the version number
  -h, --help                 display help for command

Commands:
  run [options]              Run voucher tests
  init [options]             Initialize configuration
  help [command]             display help for command
```

### Run Command

```
Usage: voucher-qc run [options]

Run voucher tests

Options:
  -f, --file <path>          Path to test data file (JSON, CSV, or Excel)
  -u, --url <url>            URL to test data
  -s, --store <id>           Store ID to test
  -c, --concurrency <number> Number of concurrent tests (default: "3")
  -t, --timeout <ms>         Test timeout in milliseconds (default: "60000")
  -r, --report <format>      Report format (json, csv, excel, html) (default: "html")
  -o, --output <path>        Output directory for reports (default: "./reports")
  -h, --headless <mode>      Headless mode (true, false, new) (default: "new")
  --config <path>            Path to config file (default: "./config/stores.json")
  --limit <number>           Limit number of tests to run
  --help                     display help for command
```

### Init Command

```
Usage: voucher-qc init [options]

Initialize configuration

Options:
  -d, --dir <path>  Directory to initialize (default: ".")
  -h, --help        display help for command
```

## Test Data Format

The test data should be in JSON, CSV, or Excel format. Here's an example of the JSON format:

```json
[
  {
    "id": "test1",
    "storeId": "default",
    "productUrl": "https://example.myshopify.com/products/sample-product",
    "voucherCode": "WELCOME10",
    "expectedDiscount": 10,
    "voucherType": "PERCENTAGE"
  },
  {
    "id": "test2",
    "storeId": "default",
    "productUrl": "https://example.myshopify.com/products/sample-product",
    "voucherCode": "FREESHIP",
    "voucherType": "FREE_SHIPPING"
  }
]
```

### Test Case Properties

| Property | Type | Description |
|----------|------|-------------|
| id | string | Unique identifier for the test |
| storeId | string | Store ID to use for the test (must match a store in the configuration) |
| productUrl | string | URL of the product to test |
| voucherCode | string | Voucher code to test |
| expectedDiscount | number | Expected discount amount (optional) |
| voucherType | string | Type of voucher (PERCENTAGE, FIXED_AMOUNT, FREE_SHIPPING, BOGO, TIERED, COMBINATION) (optional) |
| skip | boolean | Whether to skip this test (optional) |
| skipReason | string | Reason for skipping (optional) |
| fillShipping | boolean | Whether to fill shipping information (optional, default: true) |
| shippingAddress | object | Custom shipping address to use (optional) |
| fallbackProducts | array | Array of fallback products to try if the main product is out of stock (optional) |
| secondProductUrl | string | URL of a second product to add to cart (for BOGO tests) (optional) |
| targetSpend | number | Target spend amount (for TIERED tests) (optional) |
| combinationTypes | array | Array of voucher types for combination vouchers (optional) |

## Store Configuration

The store configuration is stored in `config/stores.json`. Here's an example:

```json
{
  "default": {
    "domain": "example.myshopify.com",
    "name": "Default Store",
    "selectors": {
      "addToCart": ".add-to-cart-button, .add_to_cart, button[name=\"add\"]",
      "cartIcon": ".cart-icon, .cart-link, a[href=\"/cart\"]",
      "checkoutButton": ".checkout-button, [name=\"checkout\"]",
      "voucherField": "#discount-code, #checkout_reduction_code, [name=\"checkout[reduction_code]\"]",
      "voucherApplyButton": ".apply-discount, .btn--apply-code, [name=\"checkout[submit]\"]",
      "successMessage": ".reduction-code__success, .discount-applied, .applied-reduction-code__information",
      "subtotalSelector": ".cart-subtotal__price, .subtotal .money, .cart__subtotal",
      "shippingSelector": ".shipping-cost .money, .shipping__price, .shipping-price",
      "discountSelector": ".discount .money, .cart-discount__price, .order-discount__amount",
      "totalSelector": ".payment-due__price, .cart__total, .total-price .money"
    },
    "defaultShippingAddress": {
      "firstName": "Test",
      "lastName": "User",
      "address1": "123 Test St",
      "city": "Test City",
      "zip": "12345",
      "country": "United States",
      "state": "New York",
      "phone": "1234567890"
    }
  }
}
```

## UTM Parameter Testing

To test vouchers with UTM parameters, simply include the UTM parameters in the product URL:

```json
{
  "id": "test-utm",
  "storeId": "default",
  "productUrl": "https://example.myshopify.com/products/sample-product?utm_source=test&utm_medium=email&utm_campaign=welcome",
  "voucherCode": "WELCOME10",
  "expectedDiscount": 10,
  "voucherType": "PERCENTAGE"
}
```

## License

MIT
