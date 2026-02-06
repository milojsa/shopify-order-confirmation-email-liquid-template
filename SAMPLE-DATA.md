# Sample Data Example

This file demonstrates the data structure that Shopify provides to the order confirmation email template. Use this to understand what data is available and test your customizations.

## Sample Order Object

```liquid
{
  "order": {
    "name": "#1234",
    "created_at": "2026-02-06T18:00:00Z",
    "customer": {
      "first_name": "John",
      "last_name": "Doe",
      "email": "john.doe@example.com"
    }
  },
  
  "customer": {
    "first_name": "John",
    "last_name": "Doe",
    "email": "john.doe@example.com"
  },
  
  "line_items": [
    {
      "title": "Classic Cotton T-Shirt",
      "variant": {
        "title": "Blue / Large"
      },
      "quantity": 2,
      "price": 2999,
      "final_line_price": 5998,
      "image": {
        "src": "https://cdn.shopify.com/s/files/1/0000/0000/products/tshirt.jpg"
      },
      "properties": [
        {
          "first": "Gift Message",
          "last": "Happy Birthday!"
        }
      ],
      "product": {
        "handle": "classic-cotton-tshirt",
        "type": "Apparel"
      }
    },
    {
      "title": "Denim Jeans",
      "variant": {
        "title": "Dark Wash / 32"
      },
      "quantity": 1,
      "price": 7999,
      "final_line_price": 7999,
      "image": {
        "src": "https://cdn.shopify.com/s/files/1/0000/0000/products/jeans.jpg"
      },
      "properties": [],
      "product": {
        "handle": "denim-jeans",
        "type": "Apparel"
      }
    }
  ],
  
  "shipping_address": {
    "name": "John Doe",
    "address1": "123 Main Street",
    "address2": "Apt 4B",
    "city": "New York",
    "province_code": "NY",
    "zip": "10001",
    "country": "United States",
    "phone": "+1 (555) 123-4567"
  },
  
  "billing_address": {
    "name": "John Doe",
    "address1": "123 Main Street",
    "address2": "Apt 4B",
    "city": "New York",
    "province_code": "NY",
    "zip": "10001",
    "country": "United States",
    "phone": "+1 (555) 123-4567"
  },
  
  "shipping_lines": [
    {
      "title": "Standard Shipping",
      "price": 795
    }
  ],
  
  "discounts": [
    {
      "code": "SUMMER10",
      "amount": 1400
    }
  ],
  
  "tax_lines": [
    {
      "title": "Sales Tax",
      "rate_percentage": 8.5,
      "price": 1189
    }
  ],
  
  "transactions": [
    {
      "id": "txn_abc123xyz789",
      "status": "success",
      "gateway": "shopify_payments",
      "amount": 14582
    }
  ],
  
  "subtotal_price": 13997,
  "total_price": 14582,
  "currency": "USD",
  
  "shop": {
    "name": "Your Store Name",
    "logo": "https://cdn.shopify.com/s/files/1/0000/0000/files/logo.png",
    "email": "support@yourstore.com",
    "phone": "+1 (555) 987-6543",
    "url": "https://yourstore.myshopify.com",
    "address": {
      "address1": "456 Commerce Blvd",
      "city": "San Francisco",
      "province_code": "CA",
      "zip": "94102",
      "country": "United States"
    }
  }
}
```

## Expected Email Output

When the template is rendered with the above data, it will display:

### Header
- Store logo (if available) or store name
- "✓ Order Confirmed" message

### Greeting
```
Hi John,
Thank you for your order! We're getting your order ready to be shipped...
```

### Order Information
```
Order Number: #1234
Order Date: February 06, 2026
Customer Email: john.doe@example.com
```

### Order Summary Table
```
Product                                    Quantity    Price
-------------------------------------------------------
[Image] Classic Cotton T-Shirt                2      $59.98
        Blue / Large
        Gift Message: Happy Birthday!

[Image] Denim Jeans                           1      $79.99
        Dark Wash / 32
```

### Price Breakdown
```
Subtotal:                           $139.97
Shipping (Standard Shipping):       $7.95
Discount (SUMMER10):               -$14.00
Sales Tax (8.5%):                   $11.89
----------------------------------------------
Total:                              $145.82 USD
```

### Addresses
```
Shipping Address                 Billing Address
-----------------                -----------------
John Doe                        John Doe
123 Main Street                 123 Main Street
Apt 4B                          Apt 4B
New York, NY 10001              New York, NY 10001
United States                   United States
Phone: +1 (555) 123-4567        Phone: +1 (555) 123-4567
```

### Payment Method
```
Payment Method: Shopify Payments
Transaction ID: txn_abc123xyz789
```

### Customer Support
```
Email: support@yourstore.com
Phone: +1 (555) 987-6543
Website: https://yourstore.myshopify.com
```

### Footer
```
Thank you for shopping with us!
Your Store Name
456 Commerce Blvd, San Francisco, CA 94102, United States
```

## Data Types and Formats

### Prices
- All prices in Shopify are stored in cents (integers)
- Use the `| money` filter to format them: `{{ price | money }}` → "$29.99"
- Example: `2999` → "$29.99"

### Dates
- Dates are ISO 8601 format strings
- Use date filters: `{{ date | date: "%B %d, %Y" }}` → "February 06, 2026"
- Available formats:
  - `%B` - Full month name (February)
  - `%b` - Abbreviated month (Feb)
  - `%d` - Day of month (06)
  - `%Y` - Four-digit year (2026)
  - `%y` - Two-digit year (26)

### Images
- Use the `img_url` filter with size parameter
- Available sizes: `pico`, `icon`, `thumb`, `small`, `compact`, `medium`, `large`, `grande`, `1024x1024`, `2048x2048`
- Example: `{{ line.image | img_url: 'small' }}`

### Conditional Fields
Some fields may be `null` or empty:
- `line.variant.title` - May be "Default Title" for products without variants
- `address2` - Often empty if no apartment/suite number
- `line.properties` - Empty array if no custom properties
- `discounts` - Empty if no discounts applied
- `shop.logo` - May be null if no logo uploaded

## Testing Scenarios

### Scenario 1: Simple Order
```liquid
- 1 product
- No variants
- No discounts
- Standard shipping
- Shipping address = Billing address
```

### Scenario 2: Complex Order
```liquid
- Multiple products with variants
- Custom properties (gift message, engraving)
- Discount code applied
- Different shipping and billing addresses
- Multiple tax lines
```

### Scenario 3: Digital Products
```liquid
- Digital/downloadable products
- No shipping address
- Instant delivery
- No shipping costs
```

### Scenario 4: International Order
```liquid
- Customer in different country
- Multiple currencies
- International shipping
- Different tax structure
```

### Scenario 5: Gift Order
```liquid
- Different billing and shipping addresses
- Gift message included
- Gift wrapping option
```

## Variable Quick Reference

### Order Variables
- `order.name` - Order number (e.g., "#1234")
- `order.created_at` - Order date/time
- `order.customer` - Customer object

### Customer Variables
- `customer.first_name` - First name
- `customer.last_name` - Last name
- `customer.email` - Email address

### Product Variables (in line_items loop)
- `line.title` - Product title
- `line.variant.title` - Variant name
- `line.quantity` - Quantity ordered
- `line.price` - Unit price
- `line.final_line_price` - Total line price
- `line.image` - Product image
- `line.properties` - Custom properties

### Address Variables
- `address.name` - Full name
- `address.address1` - Street address line 1
- `address.address2` - Street address line 2
- `address.city` - City
- `address.province_code` - State/Province code
- `address.zip` - Postal code
- `address.country` - Country name
- `address.phone` - Phone number

### Shop Variables
- `shop.name` - Store name
- `shop.logo` - Logo URL
- `shop.email` - Support email
- `shop.phone` - Support phone
- `shop.url` - Store URL
- `shop.address` - Store address object

### Financial Variables
- `subtotal_price` - Subtotal amount
- `total_price` - Total amount
- `currency` - Currency code (USD, EUR, etc.)

## Notes

- All monetary values are in cents (multiply by 100)
- Dates are in ISO 8601 format
- Use appropriate filters for formatting
- Always check for null/empty values with `{% if %}`
- Use `{% for %}` loops for arrays like `line_items`
