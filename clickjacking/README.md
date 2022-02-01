<!-- omit in toc -->
# Clickjacking

<!-- omit in toc -->
## Table of Contents

- [Basic clickjacking with CSRF token protection](#basic-clickjacking-with-csrf-token-protection)
- [Clickjacking with form input data prefilled from a URL parameter](#clickjacking-with-form-input-data-prefilled-from-a-url-parameter)
- [Clickjacking with a frame buster script](#clickjacking-with-a-frame-buster-script)

## Basic clickjacking with CSRF token protection
Reference: https://portswigger.net/web-security/clickjacking/lab-basic-csrf-protected

<!-- omit in toc -->
### Quick Solution
Just craft a webpage where there is a 'Click Me' button on top of the delete account. Look at the ``csrf_token_protection.html`` file in the ``exploits`` directory.

## Clickjacking with form input data prefilled from a URL parameter
Reference: https://portswigger.net/web-security/clickjacking/lab-prefilled-form-input

<!-- omit in toc -->
### Quick Solution
Similar to the previous lab, the only difference is that I had to prefill the input form data. Look at the ``input_data_prefilled.html`` file in the ``exploits`` directory.

## Clickjacking with a frame buster script

<!-- omit in toc -->
### Quick Solution
The exploit is **exactly** the same of the previous lab. The only difference is that the target website uses **frame busting**. This technique can easily be circumvented with the ``sandbox="allow-forms"`` attribute. Look at the ``frame_buster_script.html`` file in the ``exploits`` directory.