# TeamVirtual Custom Contact Logger

A lightweight, secure WordPress plugin to log contact form submissions into a custom post type (`contact_log`). Built with PSR-12 standards, nonce verification, and ACF integration.

## Features
- Custom Post Type: `contact_log`
- Nonce-based security for form submission
- Sanitized input fields (name, email, message)
- ACF integration for structured data storage
- No global variables, namespaced code

## Usage
1. Activate the plugin.
2. Ensure ACF fields (`field_name`, `field_email`, `field_message`) are set for `contact_log`.
3. Add a contact form on a page named `contact` with fields: `name`, `email`, `message`.
4. Plugin auto-inserts nonce and handles submission securely.

## Security
- Nonce verification
- Input sanitization
- No global variables
- PSR-12 compliant code

## Author
Sakib Hossain  
Founder, WPProDesk | Team Virtual International LTD  
