---
title:  "Service Portal Overview"
date: '2026-02-25T09:00:00+00:00'
tags:
- service portal
- overview
- key terms
- key concepts
---

It has been eight years since the previous post, and now, after working on several Service Portal projects, I’m going to start by explaining the fundamental concepts and subsequently delve deeper into them in future posts.

<!--break-->

# Key Terms

| Term | Description |
| ----------- | ----------- |
| Portal |  The starting point to creating or configuring a portal, allowing you to define the overall look and feel. |
| Theme | Defines the look and feel for a portal. Themes can be applied to multiple portals to maintain consistency. |
| Menu | Configures the items and appearance of the menu bar. The menu widget is dynamically included in the Theme’s header. |
| Page | A collection of widgets organised using containers, rows, and columns. Pages form the structure of the portal. |
| Widget | The individual building blocks that contain the actual content of the portal, such as forms, lists, and custom UI elements. |
| CSS | Style sheets used to control the visual aspects of the portal, attached to the Theme to maintain styling consistency. |
| Headers and Footers | Configurable sections that wrap the portal’s main content, providing branding and navigation elements. |
| Angular Providers | Reusable AngularJS components (Directives, Factories, Services) that add functionality to Widgets and Headers. |

# Portal
A portal is essentially an endpoint in the system where users interact with content. If you’re building out multiple portals, it’s essential to reuse components like Pages and Widgets to maintain efficiency and consistency. This approach reduces redundancy and minimises maintenance overhead.

When implementing portal-specific style values, use the *CSS Variables* field to set these overrides. These values will take precedence over the Theme styles.

For performance optimisation, logo and icon images should be compressed to small resolutions. The default ServiceNow header logo has the following max resolution guidelines:

- Width: 504 pixels
- Height: 64 pixels

# Theme
A Theme defines the overall look and feel of a portal. 

**It comprises the following components:**

- Header and Footer: Define the branding, navigation, and links around the core content.
- CSS Variables: Override default styles by specifying global values.
- CSS Styles: The actual style definitions that control colors, fonts, and layout.
- JavaScript Includes: Any JS functionality that needs to be loaded globally.

When creating multiple portals, it’s a good practice to apply the same theme where possible. This reduces technical debt and ensures consistent branding across portals.

A well-structured theme should also contain styles for commonly used Bootstrap and custom CSS classes. Be mindful of widget-specific styles to prevent unintentional overrides or conflicts.


# Menu
The Menu is a critical part of navigation in a portal. It provides users with links to different areas and functionalities within the portal. 

**There are several components used to build a menu bar:**

| Term | Description |
| ----------- | ----------- |
| Header | The Header is selected in the Theme record and acts as a container for the Menu Widget. It can also include branding elements such as the logo. |
| Menu | The Menu contains the items that will appear in the navigation bar. It is configured separately and passed into the Menu Widget for display. |
| Menu Widget | The widget responsible for rendering the Menu Items. It is dynamically included within the Header. |
| Menu Items | A list of links or options that are rendered in the Menu Widget, typically pointing to different Pages or URLs within the Service Portal. |

# Page
A Page is the structural element of a portal, bringing all content together. A page consists of containers, rows, and columns, which can hold multiple widgets.

**Hierarchy of a Page:**
1. Container: Defines the boundaries of the page content and contains rows.
1. Row: Rows help in organising the layout. Each row can be divided into columns.
1. Column: Each row contains columns, which can vary in width (based on Bootstrap’s grid system).
1. Widget Instance: Widgets are placed inside the columns to display dynamic content.

Pages can be used across multiple portals, making it easier to reuse and maintain content.

# Widget
Widgets are the building blocks of the Service Portal. Each widget can contain HTML, JavaScript, CSS, and AngularJS code to create dynamic, interactive content. Widgets are responsible for most of the visual and functional aspects of the portal.

**Key Components of a Widget:**
1. HTML Template: Defines the structure of the widget.
1. Client Script: The JavaScript that runs on the client-side to handle interactions and data manipulation.
1. Server Script: Runs on the server to fetch or manipulate data before rendering it on the client.
1. CSS: Custom styles applied to the widget.
1. Angular Providers: Reusable AngularJS components that can be injected into the widget for added functionality.

**Best Practices for Widget Development:**
- Reusability: Create widgets that can be reused across different pages or portals to avoid redundancy.
- Performance: Ensure widgets are optimised for performance, especially when loading data from external sources.
- Security: Sanitise and validate inputs, both in the client and server scripts, to prevent potential security vulnerabilities.

# CSS
CSS (Cascading Style Sheets) play an important role in customising the visual look and feel of your Service Portal. In ServiceNow’s Service Portal, the CSS for a portal is usually attached to the Theme, and it overrides the default styles provided by ServiceNow.

**Important Concepts:**
- CSS Variables: Set these on the Theme to define global values like colours, fonts, and spacing that can be applied across the portal.
- Scoped Styles: Widgets can have their own internal CSS, but it’s important to ensure styles are scoped properly to avoid affecting other widgets.
- Responsive Design: Service Portal uses Bootstrap, so always design CSS with responsiveness in mind. Make sure your styles look good across different devices and screen sizes.

**Best Practices:**
- Use class selectors instead of element selectors for better performance.
- Avoid using !important as much as possible—it can cause conflicts.
- Leverage ServiceNow’s out-of-the-box Bootstrap framework for layout and grid systems.

# Headers & Footers

Headers and Footers are key elements of a portal’s layout, providing consistency, branding, and navigation across pages.

- Header: Usually contains the portal logo, navigation menu, and user-related links (like profile, settings, or logout). It’s configured as part of the Theme and is shared across all pages in the portal.
- Footer: Typically includes information such as contact details, copyright notices, or links to additional resources. It is also part of the Theme configuration and appears on every page.

**Customisation:**
Headers and Footers can be customised by creating custom widgets. You can modify the structure, add dynamic content, or even apply role-based visibility rules.

# Angular Providers

Angular Providers are reusable components in ServiceNow’s AngularJS framework that enable modular development within widgets. 

**There are three types of providers:**
1. Directive: Custom HTML tags or attributes that manipulate the DOM.
1. Factory: A reusable object or function that returns values or methods that can be shared across different widgets.
1. Service: Similar to factories but instantiated using the new keyword. Services are singleton objects, providing a global instance that persists across the portal.

**Example Use Cases:**
- Directives: Used to create custom behavior in HTML elements, such as adding validation rules or custom input formats.
- Factories/Services: Commonly used to share data between different widgets, such as user session details or API integrations.

**Best Practices:**
- Use providers to encapsulate logic that can be reused across multiple widgets.
- Ensure providers follow the single-responsibility principle to avoid complex, monolithic components.

# Conclusion
The Service Portal is a powerful tool in ServiceNow, enabling the creation of user-friendly, branded portals. By understanding and mastering its core concepts—Portals, Themes, Menus, Pages, Widgets, and Angular Providers—you can build scalable and maintainable solutions that meet business needs.

In future posts, we will dive deeper into custom widget development, performance optimisation, and advanced AngularJS techniques to further enhance your Service Portal capabilities.