# Shopify Order Confirmation Email Liquid Template

A professional and responsive email template for Shopify order confirmation emails. This template provides a clean, modern design that works across all email clients and devices.

## Features

- ✅ **Responsive Design**: Mobile-friendly layout that adapts to any screen size
- ✅ **Collection/Pickup Support**: Automatic detection and special layout for pickup orders
- ✅ **Complete Order Information**: Order number, date, and customer details
- ✅ **Itemized Product List**: Products with images, variants, quantities, and prices
- ✅ **Comprehensive Pricing**: Subtotal, shipping, taxes, discounts, and total
- ✅ **Address Display**: Both shipping and billing addresses (or collection location for pickup)
- ✅ **Payment Information**: Payment method and transaction details
- ✅ **Branding Support**: Shop logo, name, and contact information
- ✅ **Customer Support Section**: Easy access to help and contact information
- ✅ **Professional Styling**: Clean, modern design with proper spacing and typography

## Template Structure

### Sections Included:

1. **Header Section**
   - Shop logo or name
   - Order confirmation status (changes for pickup/collection orders)

2. **Order Information**
   - Order number
   - Order date
   - Customer email

3. **Order Summary**
   - Product listing with images
   - Variant details
   - Custom product properties
   - Quantities and prices

4. **Price Breakdown**
   - Subtotal
   - Shipping/Collection costs
   - Discounts (if applicable)
   - Taxes
   - Total amount with currency

5. **Delivery & Billing Information**
   - Shipping address (for delivery orders)
   - Collection location (for pickup orders - shows store address)
   - Billing address
   - Contact phone numbers

6. **Payment Method**
   - Payment gateway information
   - Transaction ID

7. **Customer Support**
   - Shop contact information
   - Email and phone support
   - Website link

8. **Footer**
   - Shop address
   - Legal disclaimers

## Installation

1. Log in to your Shopify admin panel
2. Navigate to **Settings** → **Notifications**
3. Find **Order confirmation** in the list
4. Click on **Order confirmation** to edit
5. Copy the contents of `order-confirmation.liquid`
6. Paste into the email template editor
7. Click **Save**

## Pickup/Collection Orders

The template automatically detects when a customer has selected local pickup or collection and adjusts the email accordingly:

### Automatic Detection
The template checks if the shipping method contains any of these terms (case-insensitive):
- "pickup"
- "collection"
- "collect"

### Changes for Pickup Orders
When a pickup order is detected, the template automatically:

1. **Status Message**: Changes from "✓ Order Confirmed" (green) to "✓ Order Ready for Collection" (blue)
2. **Greeting Text**: Updates to mention collection instead of shipping
3. **Address Section**: Shows "Collection Location" with your store address instead of the customer's shipping address
4. **Collection Notice**: Displays a highlighted notice with pickup instructions
5. **Price Label**: Changes "Shipping" to "Collection" in the totals

### Configure Store Address
Make sure your store address is set correctly in Shopify:
1. Go to **Settings** → **General**
2. Scroll to **Store address**
3. Fill in your complete business address

This address will be displayed as the collection location for pickup orders.

## Customization

### Colors

You can customize the color scheme by modifying the CSS variables in the `<style>` section:

- `#3498db` - Primary button color (blue)
- `#27ae60` - Success/confirmation color (green)
- `#2c3e50` - Primary heading color (dark blue)
- `#f8f9fa` - Light background color
- `#7f8c8d` - Muted text color

### Typography

The template uses system fonts for optimal rendering:
```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
```

### Layout

- Maximum email width: 600px
- Responsive breakpoint: 600px
- Addresses display in 2 columns on desktop, 1 column on mobile

## Liquid Variables Used

The template uses standard Shopify Liquid variables:

- `shop.*` - Shop information (name, logo, email, phone, url, address)
- `order.*` - Order details (name, created_at, customer)
- `customer.*` - Customer information (first_name, email)
- `line_items` - Product line items
- `shipping_address.*` - Shipping address details
- `billing_address.*` - Billing address details
- `shipping_lines` - Shipping method and cost
- `discounts` - Applied discount codes
- `tax_lines` - Tax information
- `transactions` - Payment transaction details
- `subtotal_price` - Order subtotal
- `total_price` - Order total
- `currency` - Currency code

## Browser & Email Client Support

This template has been designed to work well in:
- Gmail (Desktop & Mobile)
- Apple Mail (iOS & macOS)
- Outlook (Windows & Office 365)
- Yahoo Mail
- AOL Mail
- Samsung Email
- Thunderbird

## Testing

Before deploying to production:

1. Send a test order to yourself
2. Check the email on multiple devices:
   - Desktop email clients
   - Mobile devices (iOS and Android)
   - Web-based email clients
3. Verify all dynamic content renders correctly
4. Test with orders that have:
   - Multiple products
   - Product variants
   - Discounts applied
   - Different shipping addresses

## Best Practices

- Keep images optimized and under 200KB
- Test with actual order data before going live
- Ensure your shop logo is high quality
- Verify all contact information is correct
- Consider adding your return policy link
- Make sure colors meet accessibility standards (WCAG 2.1)

## Troubleshooting

### Images not displaying
- Ensure images are hosted on a secure (HTTPS) server
- Check that image URLs are accessible publicly
- Verify image dimensions are reasonable

### Layout issues in Outlook
- The template uses tables for compatibility with Outlook
- Avoid complex CSS that Outlook doesn't support
- Test specifically in Outlook if many customers use it

### Variables not rendering
- Check that you're using the correct Shopify Liquid variable names
- Ensure variables are properly closed with `{% endif %}` or `{% endfor %}`
- Preview in Shopify's notification editor before saving

## License

This template is provided as-is for use with Shopify stores.

## Contributing

Feel free to submit issues or pull requests to improve this template.

## Support

For questions or issues with this template:
1. Check the Shopify Liquid documentation: https://shopify.dev/docs/api/liquid
2. Review Shopify notification documentation: https://help.shopify.com/en/manual/orders/notifications
3. Open an issue in this repository