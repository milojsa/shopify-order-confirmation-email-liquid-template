# Shopify Order Confirmation Email Template

A professional, responsive order confirmation email template for Shopify, featuring advanced Liquid templating logic for automotive parts e-commerce.

## 📋 Overview

This template provides a comprehensive order confirmation email with custom features tailored for automotive parts retailers, including vehicle information display, intelligent lead time calculations, shipping protection integration, and grouped payment summaries.

## ✨ Key Features

### 🎨 Design & Layout
- **Responsive Design**: Optimized for desktop and mobile email clients
- **600px Width**: Standard email-safe width with proper mobile breakpoints
- **Table-Based Layout**: Ensures compatibility across all major email clients (Gmail, Outlook, Apple Mail, etc.)
- **Professional Typography**: Helvetica Neue font stack with proper fallbacks
- **Grouped Payment Summary**: Organized sections for Discounts, Shipping, and Taxes

### 🚗 Automotive-Specific Features
- **Vehicle Information Display**: Shows customer vehicle details from order metafields
- **Intelligent Lead Time Logic**:
  - Standard products display their individual lead times
  - MOUNTBALANCE packages automatically calculate and display the longest lead time from other products in the order
  - When MOUNTBALANCE exists, lead times are hidden on all other products
- **Product Notes Integration**: Displays product-specific notes from Magento metafields

### 💰 Payment & Pricing
- **Detailed Payment Summary** with separate sections for:
  - Item Total with quantity count
  - Discounts (Product & Shipping)
  - Subtotal after discounts
  - Shipping costs with FREE shipping display
  - Tax breakdown by jurisdiction
  - Grand Total
- **Strikethrough Pricing**: Shows original prices when discounts are applied
- **Navidium Shipping Protection**: Automatic detection and display

### 📦 Order Information
- **Order Number & Date**: Formatted with proper timezone handling
- **Customer Name**: Personalized greeting
- **Product Details**:
  - Product images (200px width optimization)
  - Product title with variant information
  - SKU display
  - Quantity and pricing
  - Lead time calculations
  - Product-specific notes
- **Shipping & Billing Addresses**: Full address formatting with conditional fields

## 🔧 Technical Specifications

### Shopify Liquid Variables Used

#### Order Variables
```liquid
{{ name }}                    # Order number
{{ created_at | date: "%B %d, %Y" }}  # Order date
{{ customer.first_name }}     # Customer name
{{ item_count }}              # Total item count
{{ line_items }}              # Product items loop
{{ discounts }}               # Discounts loop
{{ tax_lines }}               # Tax breakdown loop
{{ shipping_lines }}          # Shipping methods loop
```

#### Pricing Variables
```liquid
{{ line_items_subtotal_price | money }}
{{ subtotal_price | money }}
{{ total_price | money }}
{{ line_item.final_price | money }}
{{ line_item.final_line_price | money }}
{{ discount.savings | money }}
{{ tax.price | money }}
{{ shipping_line.price | money }}
```

#### Address Variables
```liquid
{{ shipping_address.name }}
{{ shipping_address.company }}
{{ shipping_address.address1 }}
{{ shipping_address.address2 }}
{{ shipping_address.city }}
{{ shipping_address.province }}
{{ shipping_address.zip }}
{{ shipping_address.country }}
```

### Custom Metafields

#### Order Metafields
- `order.metafields.custom.vehicle` - Vehicle information (Year, Make, Model)
- `order.metafields.custom.inventory_info` - Order notes/inventory information

#### Product Metafields (Magento Integration)
- `product.metafields.magento.lead_time` - Manufacturing/shipping lead time (in days)
- `product.metafields.magento.notes` - Product-specific notes and warnings

### Conditional Logic Highlights

#### MOUNTBALANCE Package Logic
```liquid
{% if line_item.sku contains 'MOUNTBALANCE' %}
  # Calculate longest lead time from all other products
  # Display aggregate lead time for the package
{% elsif has_mountbalance == false %}
  # Show individual product lead time
  # Only if no MOUNTBALANCE exists in order
{% endif %}
```

#### Discount Type Detection
```liquid
{% for discount in discounts %}
  {% if discount.type == 'shipping' %}
    # Shipping discount
  {% else %}
    # Product discount
  {% endif %}
{% endfor %}
```

#### Navidium Shipping Protection Detection
```liquid
{% if line.product.handle contains 'navidium' or 
     line.title contains 'Navidium' or 
     line.title contains 'Shipping Protection' %}
  # Display as shipping protection item
{% endif %}
```

## 📁 File Structure

```
new_template/
├── index.html                      # Main production template
├── index_pre_merge_backup.html     # Backup before major changes
├── updated-v4/                     # Design reference files
│   └── index.html                  # Static design mockup
├── images/                         # Email template images
│   ├── 1_Shape_Background.png
│   ├── 2_Header_Wide.png
│   ├── 3_1A1A1A.png
│   ├── 4_Processing_Blue_1.png
│   └── 5_Dark.png
├── screenshots/                    # Visual documentation
└── README.md                       # This file
```

## 🚀 Installation & Deployment

### Prerequisites
- Shopify store with admin access
- Required metafields configured:
  - `order.custom.vehicle` (Single line text)
  - `order.custom.inventory_info` (Multi-line text)
  - `product.magento.lead_time` (Integer)
  - `product.magento.notes` (Multi-line text)

### Deployment Steps

1. **Upload Images**
   ```
   Upload images from /images/ folder to:
   Shopify Admin → Settings → Files
   ```

2. **Update Image URLs**
   Replace image URLs in the template with your Shopify CDN URLs:
   ```liquid
   src="https://cdn.shopify.com/s/files/1/YOUR_STORE/..." 
   ```

3. **Install Template**
   ```
   Shopify Admin → Settings → Notifications → 
   Order confirmation → Edit code → 
   Paste the contents of index.html
   ```

4. **Configure Metafields** (if not already set up)
   ```
   Shopify Admin → Settings → Custom data → Orders/Products
   Add the required metafield definitions
   ```

5. **Test the Email**
   - Place a test order
   - Verify all sections render correctly
   - Test with different scenarios:
     - Orders with/without MOUNTBALANCE
     - Orders with discounts
     - Orders with FREE shipping
     - Orders with multiple tax jurisdictions

## 🔍 Customization Guide

### Modifying Colors
Look for inline style attributes:
```html
background-color: #f5f5f5;  /* Light grey sections */
border: 1px solid #e3e4e6;  /* Border color */
color: #101112;              /* Text color */
```

### Adjusting Layout Widths
Column widths are percentage-based:
```html
width="66.66666666666667%"  /* 2/3 column */
width="33.333333333333336%" /* 1/3 column */
width="50%"                 /* Half column */
```

### Adding New Metafields
1. Add metafield check:
```liquid
{% if product.metafields.namespace.key != blank %}
  {{ product.metafields.namespace.key }}
{% endif %}
```

2. Add corresponding HTML table structure following existing patterns

## 📱 Email Client Compatibility

Tested and verified on:
- ✅ Gmail (Desktop & Mobile)
- ✅ Apple Mail (macOS & iOS)
- ✅ Outlook 2016/2019/365 (Windows)
- ✅ Outlook.com
- ✅ Yahoo Mail
- ✅ Samsung Email
- ✅ Thunderbird

### Known Limitations
- Outlook 2007-2013 uses Word rendering engine (limited CSS support)
- Some mobile clients may adjust font sizes automatically
- Background images may not display in all clients

## 🛠️ Development Notes

### Liquid Logic Highlights

**Lead Time Calculation**: The template intelligently calculates lead times based on whether MOUNTBALANCE packages are present in the order, ensuring customers receive accurate shipping expectations.

**Dynamic Discount Display**: Separates product and shipping discounts into organized sections with conditional rendering to avoid empty sections.

**Conditional Content**: All sections use Liquid conditionals to hide elements when data is unavailable, ensuring clean email rendering.

### MSO Conditionals
Microsoft Office-specific code is wrapped in:
```html
<!--[if mso]>
  <table>...</table>
<![endif]-->
```

### Mobile Optimization
Uses `@media (max-width:620px)` queries with important overrides to ensure proper mobile rendering.

## 📊 Performance Considerations

- **Image Optimization**: Product images resized to 200px width via Shopify's `image_url` filter
- **Inline CSS**: All styles are inline for maximum email client compatibility
- **Minimal External Resources**: Only product images are external; no web fonts or external stylesheets
- **Table-Based Layout**: Ensures consistent rendering across all email clients

## 🐛 Troubleshooting

### Lead Times Not Displaying
- Verify `product.metafields.magento.lead_time` is populated
- Check that the metafield is an integer value
- Ensure metafield namespace and key match exactly

### MOUNTBALANCE Logic Not Working
- Confirm SKU contains the exact string "MOUNTBALANCE"
- Check that other products have lead_time metafields set
- Verify the `has_mountbalance` assignment logic runs before the line items loop

### Images Not Loading
- Ensure image URLs are using HTTPS
- Verify images are uploaded to Shopify Files
- Check image URLs don't have authentication requirements

### Addresses Not Showing
- Confirm address fields are filled during checkout
- Check for conditional logic (`!= blank`) preventing empty field display
- Verify shipping/billing address variables are available

## 📄 License & Credits

**Project**: Shopify Order Confirmation Email Template  
**Created for**: David Newton  
**Repository**: [milojsa/shopify-order-confirmation-email-liquid-template](https://github.com/milojsa/shopify-order-confirmation-email-liquid-template)

### Technology Stack
- Shopify Liquid Templating Language
- HTML5 Email Standards
- Inline CSS
- Table-Based Responsive Layout

## 🤝 Contributing

For modifications or improvements:
1. Always create a backup before making changes
2. Test thoroughly across multiple email clients
3. Document any new metafields or custom logic
4. Update this README with significant changes

## 📞 Support

For issues specific to:
- **Shopify Liquid**: [Shopify Liquid Documentation](https://shopify.dev/docs/api/liquid)
- **Email HTML Best Practices**: [Email on Acid](https://www.emailonacid.com/blog/)
- **Responsive Email Design**: [Litmus Resources](https://www.litmus.com/resources/)

---

**Version**: 2.0  
**Last Updated**: February 2026  
**Shopify API Version**: Compatible with latest stable release
