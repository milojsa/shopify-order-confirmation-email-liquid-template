# Customization Guide

This guide will help you customize the order confirmation email template to match your brand and requirements.

## Quick Start Customizations

### 1. Change Color Scheme

Replace the colors in the `<style>` section to match your brand:

```css
/* Primary brand color (buttons, links) */
background-color: #3498db; /* Change to your brand color */

/* Success/confirmation color */
color: #27ae60; /* Change to your preference */

/* Heading colors */
color: #2c3e50; /* Dark text color */

/* Background colors */
background-color: #f8f9fa; /* Light gray for sections */
```

### 2. Modify Button Style

Find the `.button` class and customize:

```css
.button {
    padding: 12px 30px;           /* Adjust padding */
    background-color: #3498db;    /* Button background */
    color: #ffffff;               /* Button text color */
    border-radius: 5px;           /* Rounded corners */
    font-weight: 600;             /* Text weight */
}
```

### 3. Update Typography

Change the font family:

```css
body {
    font-family: 'Your Font', -apple-system, sans-serif;
}
```

### 4. Add Company Branding

#### Logo
Make sure your shop logo is set in Shopify Settings. The template automatically uses it:
```liquid
{% if shop.logo %}
<img src="{{ shop.logo }}" alt="{{ shop.name }}" class="logo">
{% endif %}
```

#### Custom Header
Replace the header section with custom HTML:
```liquid
<div class="header">
    <img src="https://yourstore.com/logo.png" alt="Store Name" class="logo">
    <p style="color: #27ae60; font-size: 18px;">✓ Order Confirmed</p>
</div>
```

### 5. Customize Success Message

Find the confirmation text and change it:
```liquid
<p style="color: #27ae60; font-size: 18px; margin-top: 15px;">
    ✓ Order Confirmed
</p>
```

Change to:
```liquid
<p style="color: #27ae60; font-size: 18px; margin-top: 15px;">
    🎉 Thank You! Your Order is Confirmed
</p>
```

## Advanced Customizations

### 1. Customize Pickup Detection Keywords

If your store uses different terms for pickup, modify the detection logic:

```liquid
{% assign is_pickup = false %}
{% if shipping_lines %}
    {% assign shipping_method = shipping_lines.first.title | downcase %}
    {% if shipping_method contains 'pickup' 
       or shipping_method contains 'collection' 
       or shipping_method contains 'collect'
       or shipping_method contains 'store pickup'
       or shipping_method contains 'click and collect' %}
        {% assign is_pickup = true %}
    {% endif %}
{% endif %}
```

### 2. Customize Collection Notice

Change the pickup notice message and style:

```liquid
<p style="margin-top: 10px; padding: 10px; background-color: #e3f2fd; border-radius: 4px; border-left: 4px solid #2196f3;">
    <strong>🏪 Store Pickup Information</strong><br>
    <small>Your order will be ready within 2-4 hours. We'll send you an email when it's ready!</small><br>
    <small>Please bring your order number and a valid ID.</small>
</p>
```

### 3. Add Opening Hours for Pickup

Add pickup hours to the collection location:

```liquid
{% if is_pickup %}
<h3>Collection Location</h3>
{% if shop.address %}
<p>
    <strong>{{ shop.name }}</strong><br>
    {{ shop.address.address1 }}<br>
    {{ shop.address.city }}, {{ shop.address.province_code }} {{ shop.address.zip }}
</p>
<p><strong>Opening Hours:</strong><br>
    Monday - Friday: 9:00 AM - 6:00 PM<br>
    Saturday: 10:00 AM - 4:00 PM<br>
    Sunday: Closed
</p>
{% endif %}
{% endif %}
```

### 4. Different Status Colors for Pickup

Customize the status color and icon for pickup orders:

```liquid
{% if is_pickup %}
<p style="color: #ff9800; font-size: 18px; margin-top: 15px;">📦 Ready for Pickup Soon</p>
{% else %}
<p style="color: #27ae60; font-size: 18px; margin-top: 15px;">✓ Order Confirmed</p>
{% endif %}
```

### 5. Add Tracking Information

Add this after the Order Information section:

```liquid
{% if fulfillments %}
<h2>Shipping Information</h2>
<div class="order-info">
    {% for fulfillment in fulfillments %}
    <p><strong>Tracking Number:</strong> {{ fulfillment.tracking_number }}</p>
    <p><strong>Tracking URL:</strong> 
        <a href="{{ fulfillment.tracking_url }}">Track your package</a>
    </p>
    <p><strong>Carrier:</strong> {{ fulfillment.tracking_company }}</p>
    {% endfor %}
</div>
{% endif %}
```

### 2. Add Return Policy Section

Add before the footer:

```liquid
<h2>Return Policy</h2>
<p>You have 30 days from the date of delivery to return items. 
   Please visit our <a href="{{ shop.url }}/pages/returns">return policy page</a> 
   for detailed instructions.</p>
```

### 3. Add Product Reviews Link

Modify the product table to include review links:

```liquid
<td>
    <strong>{{ line.title }}</strong>
    <br>
    <a href="{{ shop.url }}/products/{{ line.product.handle }}/reviews" 
       style="font-size: 12px;">Leave a review</a>
</td>
```

### 4. Add Discount Code for Next Purchase

Add after the totals section:

```liquid
<h2>Special Offer</h2>
<div class="order-info" style="background-color: #fff3cd; border: 2px solid #ffc107;">
    <p style="text-align: center; margin: 0;">
        <strong>Get 10% off your next order!</strong><br>
        Use code: <strong style="font-size: 20px;">THANKYOU10</strong>
    </p>
</div>
```

### 5. Add Social Media Links

Add to the footer section:

```liquid
<div style="margin: 20px 0;">
    <p>Follow us on social media:</p>
    <a href="https://facebook.com/yourstore" style="margin: 0 10px;">Facebook</a>
    <a href="https://instagram.com/yourstore" style="margin: 0 10px;">Instagram</a>
    <a href="https://twitter.com/yourstore" style="margin: 0 10px;">Twitter</a>
</div>
```

### 6. Conditional Content Based on Order Value

Show special message for high-value orders:

```liquid
{% if total_price >= 10000 %}
<div class="order-info" style="background-color: #d4edda; border: 2px solid #28a745;">
    <p style="text-align: center; margin: 0;">
        <strong>🌟 VIP Order</strong><br>
        Thank you for your premium order! You qualify for priority shipping.
    </p>
</div>
{% endif %}
```

### 7. Add Estimated Delivery Date

Add to the Order Information section:

```liquid
{% if shipping_lines %}
<p><strong>Estimated Delivery:</strong> 
    {{ 'now' | date: '%s' | plus: 604800 | date: "%B %d, %Y" }}
</p>
{% endif %}
```

### 8. Personalized Greeting Based on Time

Replace the greeting:

```liquid
{% assign hour = 'now' | date: "%H" | plus: 0 %}
<p>
    {% if hour < 12 %}Good morning{% elsif hour < 18 %}Good afternoon{% else %}Good evening{% endif %}
    {% if customer.first_name %}{{ customer.first_name }}{% else %}{{ customer.email }}{% endif %},
</p>
```

## Layout Modifications

### 1. Single Column Layout for Addresses

Replace the addresses section with:

```liquid
<h2>Shipping Address</h2>
<div class="address-block">
    <!-- Shipping address content -->
</div>

<h2>Billing Address</h2>
<div class="address-block">
    <!-- Billing address content -->
</div>
```

### 2. Add Banner Image

Add after the header:

```liquid
<div style="margin: 20px 0;">
    <img src="https://yourstore.com/email-banner.jpg" 
         alt="Thank you for your order" 
         style="width: 100%; height: auto; border-radius: 8px;">
</div>
```

### 3. Change Product Table Layout

For a more compact layout:

```liquid
<table>
    <tbody>
        {% for line in line_items %}
        <tr>
            <td colspan="3">
                {% if line.image %}
                <img src="{{ line.image | img_url: 'small' }}" 
                     alt="{{ line.title }}" 
                     style="float: left; margin-right: 15px; max-width: 80px;">
                {% endif %}
                <div>
                    <strong>{{ line.title }}</strong><br>
                    <span>Qty: {{ line.quantity }}</span> × 
                    <span>{{ line.price | money }}</span> = 
                    <strong>{{ line.final_line_price | money }}</strong>
                </div>
            </td>
        </tr>
        {% endfor %}
    </tbody>
</table>
```

## Conditional Features

### 1. Hide Sections Based on Conditions

Hide billing address if same as shipping:

```liquid
{% unless billing_address == shipping_address %}
<div class="address-block">
    <h3>Billing Address</h3>
    <!-- Billing address content -->
</div>
{% endunless %}
```

### 2. Show Different Messages for Different Products

```liquid
{% for line in line_items %}
    {% if line.product.type == "Digital" %}
    <p style="color: #3498db;">
        📧 This item will be delivered via email within 24 hours.
    </p>
    {% endif %}
{% endfor %}
```

### 3. Country-Specific Content

```liquid
{% if shipping_address.country == "United States" %}
<p>🇺🇸 Free returns within the US!</p>
{% endif %}
```

## Testing Your Customizations

1. **Use Shopify's Preview Feature**
   - In the notification editor, click "Preview"
   - This shows how the email will look with sample data

2. **Send Test Orders**
   - Create test orders in your development store
   - Use different scenarios (multiple products, discounts, etc.)

3. **Test on Multiple Devices**
   - Check on desktop email clients
   - Test on mobile devices (iOS and Android)
   - Verify in different email services (Gmail, Outlook, etc.)

4. **Validate HTML**
   - Use an HTML email validator
   - Check for broken links
   - Verify all images load correctly

## Best Practices

1. **Keep It Simple**: Don't overload the email with too much information
2. **Mobile First**: Always test on mobile devices
3. **Brand Consistency**: Match colors and fonts to your website
4. **Clear Call-to-Action**: Make important buttons stand out
5. **Accessibility**: Ensure good color contrast for readability
6. **Loading Speed**: Optimize images for fast loading
7. **Legal Compliance**: Include necessary business information

## Common Pitfalls to Avoid

1. ❌ Don't use JavaScript (not supported in emails)
2. ❌ Don't use external CSS files (inline styles only)
3. ❌ Don't use complex CSS animations
4. ❌ Don't embed videos (use thumbnail with link instead)
5. ❌ Don't use background images (limited support)
6. ❌ Don't forget to test in Outlook (quirky rendering)

## Resources

- [Shopify Liquid Documentation](https://shopify.dev/docs/api/liquid)
- [Email on Acid](https://www.emailonacid.com/) - Email testing tool
- [Litmus](https://www.litmus.com/) - Email preview across clients
- [Can I Email](https://www.caniemail.com/) - CSS support in email clients
