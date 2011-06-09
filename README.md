SpreeSalecycle
==============

Salecycle provides a pre-configured implementation of the SaleCycle JavaScript complete with variable population and custom method definition for the easiest possible implementation of SaleCycle's services.

### SaleCycle ###

SaleCycle is a __shopping cart recovery__ service.  If a user stops shopping with items in their shopping cart, or abandons the checkout phase without confirming their order, then any information that SaleCycle was able to record throughout the user's window-shopping can be used to re-engage the customer in their shopping or checkout to complete the order.

Much more information is available here: http://www.salecycle.com/

Installation
============

Add the following to your Gemfile: `gem 'spree_salecycle', :git => 'git://github.com/minustehbare/spree-salecycle.git'`.

Run `bundle install`

And you should be all set.  Finish things up by navigating to the Spree Administration interface, clicking the 'Configuration' tab, selecting 'SaleCycle Settings' from the list, and input your Client ID and application name that were defined/provided from signing up with SaleCycle.

Overview
========

### Hooks ###

The Salecycle extension uses hooks defined by Spree to insert the SaleCycle JavaScript that records user information.  In order to track the user's order before, during, and after checkout the following hooks were used:

+ `insert_after :outside_cart_form, 'shared/salecycle'`
+ `insert_after :checkout_summary_box, 'shared/salecycle'`
+ `insert_after :order_details_total, 'shared/salecycle'`

For inquiring minds, here is a list of the hook locations:

+ \*\*/core/app/views/orders/edit.html.erb
+ \*\*/core/app/views/checkout/edit.html.erb
+ \*\*/core/app/views/shared/_order_details.html.erb

### Salecycle Javascript ###

The _salecycle.html.erb partial provides the SaleCycle JavaScript Array of variables and attempts to set them based on the current state of the order and any settings defined through the Spree Administration interface.  It also includes a final JavaScript tag to submit the data to SaleCycle based on the name of your application.


### Configuration Settings ###

There are a handful of configuration settings that can be set through the Spree Administration interface.  To access these settings, simply navigate to the 'Configuration' tab of the Administration interface and select the 'SaleCycle Settings' link from the list.

##### Mandatory #####

In order for SaleCycle to function correctly, two settings must be defined: `:salecycle_client_id` and `:salecycle_app_name`.  Both of these are defined or provided as a result of signing up with SaleCycle and must be configured before SaleCycle can receive any information being recorded.

##### Optional #####

A client can optionally supply a currency code to be recorded with every purchase.  A value of 'GBP' supplied in the settings interface would tell SaleCycle that all orders are valued in British pounds.

##### Custom #####

SaleCycle also provides 3 variables which are not already handled by attributes on Products, Variants, or Users in a default Spree environment: `:salecycle_custom_field_1`, `:salecycle_custom_field_2` and `:salecycle_user_opt_out_method`.

Each of these settings is configurable through the settings interface and has a required format of `<receiver>#<method>`.  For example, if the User model in your application has an attribute of `:opt_out` then the `:salecycle_user_opt_out_method` would result in `User#opt_out` or `user#opt_out`.  Deviations from this format might prove unsavory.

The custom fields provided by SaleCycle traditionally track additional information on Products or Variants.  To populate these fields simply define a `<receiver>#<method>` setting.  When evaluating the LineItems of the Order will each issue a `line_item.product.send(method)` or `line_item.variant.send(method)`, with Products taking precedence over Variants.  More information on these fields can be found in the SaleCycle documentation.

Copyright (c) 2011 minustehbare, released under the New BSD License
