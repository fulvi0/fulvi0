---
layout: post
title: How to populate a Rails 5 Application with sample Data
excerpt: "This small guide will is to help you to populate your application with sample data that may useful for test during the development."
tags: [post, code, ruby, ruby on rails, fake.gem, populate.gem]
---

# How to populate a Rails 5 Application with sample Data.

*This small guide will is to help you to populate your application with sample data that may useful for test during the development.*

I'll be using the following scaffold for the explanations purposes. 
`$ rails g scaffold house family:string members:integer number:integer address:string`

So, beginning, as usual, we Need to add in the **Gemfile** the [`gem 'populator'`](https://github.com/ryanb/populator) and [`gem 'faker'`](https://github.com/stympy/faker) in `group :development` and/or ` group :test`, in case you need for the test environment.

After `$ bundle install`, let open `seed.rb` file located under `db/` then will add a few lines.

```ruby
# The gem will populate 10 times in the model House and will include sample data provided by faker gem.
House.populate 10 do |m|
    m.family = Faker::Name.last_name
    m.members = 3..6
    m.number = 1..10
    m.address = Faker::Address.street_address
end
```

After saving the file we will run in the terminal `$ rails db:seed`

```bash
[3] pry(main)> House.all
  House Load (2.0ms)  SELECT "houses".* FROM "houses"
=> [#<House:0x000000035f3bd0 id: 1, family: "Schowalter", members: 3, number: 7, address: "3279 Joey Spring", created_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00, updated_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00>,
 #<House:0x000000035e2330 id: 2, family: "Hagenes", members: 4, number: 4, address: "145 Sipes Islands", created_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00, updated_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00>,
 #<House:0x000000035e1f20 id: 3, family: "Dach", members: 5, number: 6, address: "32723 Alize Groves", created_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00, updated_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00>,
 #<House:0x000000035e1ca0 id: 4, family: "McCullough", members: 6, number: 10, address: "787 Marvin Dale", created_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00, updated_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00>,
 #<House:0x000000035e1958 id: 5, family: "Ebert", members: 4, number: 10, address: "8976 Cartwright Spur", created_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00, updated_at: Mon, 02 Oct 2017 17:01:24 UTC +00:00>,
 ...
[4] pry(main)> 
```
As you can see the application populate our developemnt database with 10 records and filled with sample data.

----
Note: in case you got an error after ran the seed and the error is about:

```ruby
rails aborted!
NoMethodError: undefined method `sanitize' for #<Class:0x007fb8f28082e0>
```
The error have been reported in the [repo in github](https://github.com/ryanb/populator/issues/30#issuecomment-328362117) and there is a workaround for it where you have to replace a line in the file `populator/lib/populator/factory.rb`

```ruby
    def rows_sql_arr
        @records.map do |record|
 -        quoted_attributes = record.attribute_values.map { |v| @model_class.sanitize(v) } # replace this line
 +        quoted_attributes = record.attribute_values.map { |v| @model_class.connection.quote(v) } # with this one
          "(#{quoted_attributes.join(', ')})"
        end
      end
```

As well if you need to implement this project in different places, I recommend to fork the populator gem, make the previous modification, `push` to your repository in GitHub and use this line in your `Gemfile` with `gem 'populator', :github => 'fulvi0/populator'`.
