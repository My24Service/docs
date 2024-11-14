# My24Service docs

* TOC
{:toc}

## Statuscode template fields

### Orders

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
`assigned_user_id` | The ID of the that is currently assigned to the order | 2664
`all_assigned_user_ids` | When only this value is there, send a push notification to all assigned users | [ 1, 5, 10, 25 ]
`all_assigned_usernames` | This can also be used in a condition, for example test if a username is part of the assigned users | [ Peter01, Mark ]
`companycode` | The companycode of the member of the order | stormy
`quotation_name` | The company name where a quotion has been created attached to the order | Stormy BV.
`quotation_address` | The address of the company where a quotion has been created attached to the order | Metaalweg 4
`quotation_postal` | The postal code of the company where a quotion has been created attached to the order | 3751LS
`quotation_city` | The city of the company where a quotion has been created attached to the order | Bunschoten-Spakenburg
`quotation_country_code` | The country code of the company where a quotion has been created attached to the order | NL
`workorder_link` | A link to the online version of the workorder | https://demo.my24service.com/#/orders/orders/workorder/a3932030-ef11-43e1-aa3f-52a895f12123
`customer_email` | Email address(es) of the customer the order belongs to | info@customer.com
`org_order_id` | Original order ID of the copied order | 14652
`org_member_companycode` | Company code of the member of the original order of the copied order | ritehite
`order_detail_link` | Detail link to order | https://demo.my24service.com/#/orders/orders/detail/a3932030-ef11-43e1-aa3f-52a895f12123

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

#### Examples

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

De volgende monteurs zijn toegewezen:
{% for username in all_assigned_usernames %}
   *  username
{% endfor %}

Met vriendelijke groet,

Stormy

Afdeling planning
{% endraw %}
```

Order fields can also be used in the Address input field, for example:

```
info@my24service.com,{% raw %}{{ order_email }}{% endraw %}
```

And also in the Subject input field, for example:

```
{% raw %}new order has been entered: {{ order_name }}, {{ order_address }}, {{ order_city }}{% endraw %}
```

#### Conditions

Order fields can also be used in conditions. Lets say you only want to execute this
action for `customer_id` 1000, you can add a condition like this:


field | operator | value
--- | --- | ---
`{% raw %}{{ customer_id }}{% endraw %}` | = | 1000


Or if you only want to execute this action for customers in NL:

field | operator | value
--- | --- | ---
`{% raw %}{{ order_country_code }}{% endraw %}` | REGEXP | NL


Or if you only want to execute this action when user Peter01 is in the assigned users:

field | operator | value
--- | --- | ---
`{% raw %}{{ all_assigned_usernames }}{% endraw %}` | CONTAINS | Peter01

### Quotations

_TODO implement & document_

### Invoices

_TODO implement & document_

## Invoice template fields

### Invoice

Field | Description | Example value
--- | --- | ---
`invoice.invoice_id` | The invoice ID as created by my24service | 11004
`invoice.reference` | The invoice reference | ref-236891
`invoice.description` | The invoice description | Invoice for december
`invoice.term_of_payment_days` | The invoice payment days term | 30
`invoice.vat_type` | The invoice VAT type | 21%
`invoice.vat` | The invoice VAT | € 45.32
`invoice.total` | The invoice total | € 154.98
`invoice.created` | Invoice creation date | Date object (`order.start_date.strftime(‘%d-%m-%Y’)` => 5-12-2021, see https://strftime.org)
`invoice.definitive_date` | Date invoice made definitive | Date object (`order.start_date.strftime(‘%d-%m-%Y’)` => 5-12-2021, see https://strftime.org)
`invoice.invoicelines` | Invoice lines (see invoice line fields below) | `{% raw %}{%tr for line in invoice.invoicelines.all() %}{% endraw %}`

### Invoice line

Field | Description | Example value
--- | --- | ---
`line.description` | Invoice line description | Workhours Johan Cruyff
`line.amount` | Invoice line amount | 13:45
`line.vat_type` | Invoice line VAT type | 9%
`line.vat` | Invoice line VAT | € 45.32
`line.price` | Invoice line price | € 51.13
`line.total` | Invoice line total | € 154.98

### Order

Field | Description | Example value
--- | --- | ---
`order.customer_id` | The customer ID, often same as customer ID | 51199
`order.order_id` | The order ID as created by my24service | 1004
`order.order_name` | Order company name, often same as customer name | Ikea B.V.
`order.order_address` | Order company address, often same as customer address | Amstelveensestraat 12
`order.order_postal` | Order company postal, often same as customer postal | 1200AX
`order.order_city` | Order company city, often same as customer city | Amsterdam
`order.order_country_code` | Order company country code, often same as customer country code | NL
`order.order_email` | Order company email, often same as customer email | johan.cruyff@ikea.com.
`order.order_tel` | Order company telephone, often same as customer telephone | 020-1234567
`order.order_mobile` | Order company mobile, often same as customer mobile | 06-12345678
`order.order_contact` | Order company contact, often same as customer contact | Johan Cruyff
`order.order_reference` | Order reference | Ref-212777
`order.order_type` | Order type | Storing
`order.start_date` | Order start date | Date object (`order.start_date.strftime(‘%d-%m-%Y’)` => 5-12-2021, see https://strftime.org)
`order.start_time` | Order start time | Time object, nullable (`order.start_time.strftime(‘%H-%M’)` => "08:00", see https://strftime.org)
`order.end_date` | Order end date | Date object (`order.end_date.strftime(‘%d-%m-%Y’)` => 5-12-2021, see https://strftime.org)
`order.end_time` | Order end time | Time object, nullable (`order.start_time.strftime(‘%H-%M’)` => "08:00", see https://strftime.org)
`order.remarks` | Order remarks | Some remarks
`order.description` | Order description | Some description

### Customer

Field | Description | Example value
--- | --- | ---
`customer.customer_id` | The customer ID | 51199
`customer.name` | Customer name | Ikea B.V.
`customer.address` | Customer address | Amstelveensestraat 12
`customer.postal` | Customer postal | 1200AX
`customer.country_code` | Customer country code | NL
`customer.contact` | Customer contact | Johan Cruyff
`customer.city` | Customer city | Amsterdam
`customer.tel` | Customer telephone | 020-1234567
`customer.email` | Customer email | johan.cruyff@ikea.com
`customer.mobile` | Customer mobile | 06-12345678

## Quotation template fields

### Quotation

Field | Description | Example value
--- | --- | ---
`quotation.quotation_id` | The quotation ID as created by my24service | 11004
`quotation.name` | Name of the quotation | My quotation
`quotation.customer_id` | The customer ID, often same as customer ID | 51199
`quotation.quotation_name` | Quotation company name, often same as customer name | Ikea B.V.
`quotation.quotation_address` | Quotation company address, often same as customer address | Amstelveensestraat 12
`quotation.quotation_postal` | Quotation company postal, often same as customer postal | 1200AX
`quotation.quotation_city` | Quotation company city, often same as customer city | Amsterdam
`quotation.quotation_country_code` | Quotation company country code, often same as customer country code | NL
`quotation.quotation_email` | Quotation company email, often same as customer email | johan.cruyff@ikea.com.
`quotation.quotation_tel` | Quotation company telephone, often same as customer telephone | 020-1234567
`quotation.quotation_mobile` | Quotation company mobile, often same as customer mobile | 06-12345678
`quotation.quotation_contact` | Quotation company contact, often same as customer contact | Johan Cruyff
`quotation.quotation_reference` | Quotation reference | Ref-212777
`quotation.description` | Quotation description | Some description
`quotation.vat_type` | The quotation VAT type | 21%
`quotation.total` | The quotation total | € 154.98
`quotation.vat` | The quotation VAT | € 45.32
`quotation.quotation_expire_days` | Quotation expire days | 30
`quotation.created` | Quotation creation date |  | Date object (`order.start_date.strftime(‘%d-%m-%Y’)` => 5-12-2021, see https://strftime.org)
`quotation.definitive_date` | Date quotation made definitive | Date object (`order.start_date.strftime(‘%d-%m-%Y’)` => 5-12-2021, see https://strftime.org)
`quotation.chapters` | Quotation chapters (see quotation chapter fields below) | `{% raw %}{%tr for chapter in quotation.chapters.all() %}{% endraw %}`

### Quotation chapter

Field | Description | Example value
--- | --- | ---
`chapter.name` | Chapter name | General section
`chapter.description` | Chapter description | Some description
`chapter.lines` | Chapter quotation lines (see quotation line fields below) | `{% raw %}{%tr for line in chapter.lines.all() %}{% endraw %}`

### Quotation line

Field | Description | Example value
--- | --- | ---
`line.amount` | Invoice line amount | 13:45
`line.info` | Quotation line info | Workhours
`line.extra_description` | Quotation line extra description | Some optional extra text
`line.vat_type` | Quotation line VAT type | 9%
`line.price` | Quotation line price | € 51.13
`line.vat` | Quotation line VAT | € 45.32
`line.total` | Quotation line total | € 154.98

### Customer

Field | Description | Example value
--- | --- | ---
`customer.customer_id` | The customer ID | 51199
`customer.name` | Customer name | Ikea B.V.
`customer.address` | Customer address | Amstelveensestraat 12
`customer.postal` | Customer postal | 1200AX
`customer.country_code` | Customer country code | NL
`customer.contact` | Customer contact | Johan Cruyff
`customer.city` | Customer city | Amsterdam
`customer.tel` | Customer telephone | 020-1234567
`customer.email` | Customer email | johan.cruyff@ikea.com
`customer.mobile` | Customer mobile | 06-12345678
