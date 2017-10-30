---
layout: post
title: How to seed in rails with CSV
excerpt: "This will helpful if you need to load bunch of data as example or data that you may use on production apps."
tags: [post, code, csv, ruby, ruby on rails]
---

#### Seed your Rails app using a csv files.

If you are building a webapp with Ruby on Rails, you will maybe have the need to
load data, so here is a good way to insert data in the database, using a csv file
and couple lines of code.

Open the `project/db/seed.rd` file of your project and adding this few lines.

```ruby
require 'csv'

CSV.foreach(Rails.root.join('path/to/file.csv'), headers: true) do |row|
    # code...
end
```
> **CSV** This class provides a complete interface to CSV files and data.
> It offers tools to enable you to read and write to and from Strings or IO objects, as needed.
> [Source](http://goo.gl/PuiYw7).

Where `Rails.root.join` represents a pathname which locates a file in a filesystem,
`headers: true` we are specifying that the source file have header, will ignore
the first row, in case we don't have header just turn to `false` the option.

Then lets indicate the corresponding field of our table where will insert with.

```ruby
Model.create! do |model|
    model.file1 = row[0]
    model.file2 = row[1]
    model.file3 = row[2]
    model.file4 = row[3]
end
```

For complete with the seed, run in the terminal `$ bundle exec rake db:seed`

----
The final code should look like:
```ruby
require 'csv'

CSV.foreach(Rails.root.join('path/to/file.csv'), headers: true) do |row|
  Model.create! do |model|
    mode.fiel1 = row[0]
    mode.fiel2 = row[1]
    mode.fiel3 = row[2]
    mode.fiel4 = row[3]
  end
end
```

