---
layout: page
weight: 0
title: phpBB
group: open-source
navigation:
  show: true
---

phpBB supports sending email over SMTP. To have phpBB relay email through SendGrid go to *Client Communication \> E-mail settings* in the admin panel and change:

-   **Use SMTP server for e-mail** - Yes
-   **SMTP server address:** - smtp.sendgrid.net
-   **SMTP server port** - 587
-   **Authentication method for SMTP** - PLAIN
-   **SMTP username** - sendgrid_username
-   **SMTP api_key** - sendgrid_api_key
