# salesforce_bulk

## Overview

Salesforce bulk is a simple ruby gem for connecting to and using the [Salesforce Bulk API](http://www.salesforce.com/us/developer/docs/api_asynch/index.htm). There are already some gems out there that provide connectivity to the Salesforce SOAP and Rest APIs, if your needs are simple, I recommend using those, specifically raygao's [asf-rest-adapter](https://github.com/raygao/asf-rest-adapter) or [databasedotcom](https://rubygems.org/gems/databasedotcom).

## Installation

~~~ sh
$ sudo gem install salesforce_bulk
~~~

## How to use

Using this gem is simple and straight forward.

To initialize:

~~~ ruby
require 'salesforce_bulk'
salesforce = SalesforceBulk::Api.new("YOUR_SALESFORCE_USERNAME", "YOUR_SALESFORCE_PASSWORD+YOUR_SALESFORCE_TOKEN")
~~~

To use sandbox:

~~~ ruby
salesforce = SalesforceBulk::Api.new("YOUR_SALESFORCE_SANDBOX_USERNAME", "YOUR_SALESFORCE_PASSWORD+YOUR_SALESFORCE_SANDBOX_TOKEN", true)
~~~

Note: the second parameter is a combination of your Salesforce token and password. So if your password is xxxx and your token is yyyy, the second parameter will be xxxxyyyy

Sample operations:

~~~ ruby
# Insert
new_account = Hash["name" => "Test Account", "type" => "Other"] # Add as many fields per record as needed.
records_to_insert = Array.new
records_to_insert.push(new_account) # You can add as many records as you want here, just keep in mind that Salesforce has governor limits.
result = salesforce.insert("Account", records_to_insert)
puts "result is: #{result.inspect}"
~~~

~~~ ruby
# Update
updated_account = Hash["name" => "Test Account -- Updated", "id" => "a00A0001009zA2m"] # Nearly identical to an insert, but we need to pass the salesforce id.
records_to_update = Array.new
records_to_update.push(updated_account)
salesforce.update("Account", records_to_update)
~~~

~~~ ruby
# Upsert
upserted_account = Hash["name" => "Test Account -- Upserted", "External_Field_Name" => "123456"] # Fields to be updated. External field must be included
records_to_upsert = Array.new
records_to_upsert.push(upserted_account)
salesforce.upsert("Account", records_to_upsert, "External_Field_Name") # Note that upsert accepts an extra parameter for the external field name
~~~

OR

~~~ ruby
# Delete
deleted_account = Hash["id" => "a00A0001009zA2m"] # We only specify the id of the records to delete
records_to_delete = Array.new
records_to_delete.push(deleted_account)
salesforce.delete("Account", records_to_delete)
~~~

~~~ ruby
# Query
res = salesforce.query("Account", "select id, name, createddate from Account limit 3") # We just need to pass the sobject name and the query string
puts res.result.records.inspect
~~~

#### TODO document API for finnishing job
#### TODO document API for parsing results

## Copyright

Copyright (c) 2012 Jorge Valdivia.
Copyright (c) 2013 Leif Gensert, [Propertybase GmbH](http://propertybase.com)