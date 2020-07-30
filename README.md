# my24-docs

* TOC
{:toc}

## Order fields

Here's a list of fields that can be used in email templates, or email addresses etc. Fields need to be encapsulated with "{% raw %}{{{% endraw %}" and "{% raw %}}}{% endraw %}".
For example `{% raw %}{{ order_id }}{% endraw %}`.

Field | Description | Example value
--- | --- | ---
`order_id` | The order ID created by my24service | 1004
`start_date` | The start date of the order | 06/07/2020
`start_time` | The start time of the order | 16:00
`end_date` | The end date of the order | 07/07/2020
`end_time` | The end time of the order | 20:00
`order_date` | A combined field using `start_date`, `start_time`, `end_date`, and `end_time` | 06/07/2020 16:00 - 07/07/2020 20:00
`order_name` | The name of the customer of the order | My24service.com
`order_address` | The address of the customer of the order | Metaalweg 4
`order_postal` |  The postal code of the customer of the order | 3751LS
`order_po_box` | The PO box of the customer of the order | 1234 AB
`order_city` | The city of the customer of the order | Bunschoten-Spakenburg
`order_country_code` | The country code of the customer of the order | NL
`order_email` | Email address(es) that are attached to the order (mostly the customer) | info@my24service.com
`order_tel` | Telephone number that is attached to the order (mostly the customer) | 033-2474020
`order_mobile` | Mobile number that is attached to the order (mostly the customer) | +31612345678
`order_contact` | Person to contact about the order (mostly the customer) | R. Buitenhuis
`service_number` | An additional service number that is connected to the order | 54646103
`order_reference` | Reference of the order | 132454-52
`order_type` | The order type | Klein orderhoud
`customer_remarks` | Remarks specific for this customer | Melden bij de poort
`user_email` | The email address of the logged in user (or None) | pieter@my24service.com
`username` | The username of the logged in user (or None) | Pieter Pietstra
`customer_id` | The customer ID of the customer of the order | 100658
`status_date` | The date of the last status of the order | 06/07/2020
`status_time` | The time of the last status of the order | 15:05
`active_user_username` | The username of the person that is currently working on the assigned order | pieter@my24service.com
`active_user_email` | The email of the person that is currently working on the assigned order | pieter@my24service.com
`active_user_name` | The full name of the person that is currently working on the assigned order | Pieter Pietstra
`assigned_user_username` | The username of the person that is currently assigned to the order | pieter@my24service.com
`assigned_user_email` | The email of the person that is currently assigned to the order | pieter@my24service.com
`assigned_user_name` | The full name of the person that is currently assigned to the order | pieter@my24service.com
`companycode` | The companycode of the member of the order | stormy
`quotation_name` | The company name where a quotion has been created attached to the order | Stormy BV.
`quotation_address` | The address of the company where a quotion has been created attached to the order | Metaalweg 4
`quotation_postal` | The postal code of the company where a quotion has been created attached to the order | 3751LS
`quotation_city` | The city of the company where a quotion has been created attached to the order | Bunschoten-Spakenburg
`quotation_country_code` | The country code of the company where a quotion has been created attached to the order | NL
`workorder_link` | A link to the online version of the workorder | http://demo.my24service.com/order/workorder-view/00yMDdBSlpOOVBTT0E9PQ==/

In addition to these fields, there are also the following fields that can be used inside the order lines loop:

Field | Description | Example value
--- | --- | ---
`product` | The product in this order line | Aanlasplaat hoek
`location` | The location of the product | Dock 14
`remarks` | Any remarks about this product | Gaat vaak stuk

Example how you can the use order lines inside the email template:

```python
{% raw %}
{% for orderline in orderlines %}
* product: {{ orderline.product }}
  locatie: {{ orderline.location }}
  opmerking: {{ orderline.remarks }}
{% endfor %}
{% endraw %}
```

## Examples

A complete example for an email template:

```
{% raw %}
Beste,

Er is een storing aangemaakt voor

{{ order_name }}
{{ order_address }}
{{ order_country_code }}-{{ order_postal }} {{ order_city }}

Het gaat om:

{% for orderline in orderlines %}
*  product: {{ orderline.product }}
   locatie: {{ orderline.location }}
   opmerking: {{ orderline.remarks }}
{% endfor %}

Referentie:{{ order_reference }}
Contactpersoon: {{ order_contact }}
Tel.: {{ order_tel }}
Datum: {{ order_date }}
Bijzonderheden klant: {{ customer_remarks }}

Met vriendelijke groet,

Stormy

Afdeling planning
{% endraw %}
```

Order fields can also be used in the Address input field, for example:

```
{% raw %}
info@my24service.com,{{ order_email }}
{% endraw %}
```

And also in the Subject input field, for example:

```
{% raw %}
new order has been entered: {{ order_name }}, {{ order_address }}, {{ order_city }}
{% endraw %}
```

## Conditions

Order fields can also be used in conditions. Lets say you only want to execute this
action for `customer_id` 1000, you can add a condition like this:


field | operator | value
--- | --- | ---
`{% raw %}{{ customer_id }}{% endraw %}` | = | 1000


Or if you only want to execute this action for customers in NL:

field | operator | value
--- | --- | ---
`{% raw %}{{ order_country_code }}{% endraw %}` | REGEXP | NL
