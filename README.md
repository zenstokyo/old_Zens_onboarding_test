# Zens onboarding programming exercises

1. Fork this repo and make a branch for your changes.
2. Please complete the following Ruby/Rails coding problems.
3. We expect to see evidence of good development processes. Write tests, and commit often.
4. When finished, do **not** submit a pull request to this repo. Instead, send us a link to your forked project.

## Problem 1 - Bugfix

We have a `ListingTransaction` ActiveRecord model which is a financial transaction for a Airbnb listing.
A `Listing` model can have many `ListingTransction` models, and an `Expense` model can have either 0 or 1 `ListingTransaction` models.

We have a bug where many `ListingTransaction` models can incorrectly have the same `Expense` model, which results in and an `Expense` model having many `ListingTransaction` models.

What changes need to be made to ensure that only 1 `ListingTransaction` has a relationship with the same `Expense`?

This is the Ruby on Rails database migration, and the necessary parts of each model class.

```
class CreateListingTransactions < ActiveRecord::Migration[5.0]
  def change
    create_table :listing_transactions do |t|
      t.integer :listing_id, index: true, null: false
      t.integer :expense_id, index: true
      t.date :date, null: false
      t.integer :amount, null: false
      t.string :comment, null: false

      t.timestamps
    end
  end
end
```

```
# == Database Schema
#
# Table name: listing_transactions
#
#  id               :integer          not null, primary key
#  listing_id       :integer          not null
#  date             :date             not null
#  amount           :integer          not null
#  comment          :string           not null
#  expense_id       :integer
#  created_at       :datetime         not null
#  updated_at       :datetime         not null
#

class ListingTransaction < ActiveRecord::Base
  belongs_to :expense
  belongs_to :listing
end

class Listing < ActiveRecord::Base
  has_many :listing_transactions
end

class Expense < ActiveRecord::Base
  has_one :listing_transaction
end
```

## Problem 2 - String manipulation

Write a `StringDemystifier` class that - when its `demystify` method is called - applies the
following rules in order of precedence, and until they can no longer be applied to any string it
is given in its constructor:

* If a character has the same character to its left and right, it should be replaced with that other
  character (i.e. AWA becomes AAA) unless the surrounding character is a space
* Any sequence of six characters should be replaced with a single character, i.e. AAAAAA becomes A
* Any instance of the exclamation mark (!) character causes the string to be reversed, and the
  exclamation mark character removed

When you're done, try running it on the following string and see what result you get. When you've gotten the problem right, you will know!

```
'!YTIRCO!IQIIQIDEMGMMIM F!O YMJMMSM!RA !EGEEJEHT FPFWFFO TUFUTUUO PAEJEEBEL TN!AIKIITIG ENVNNMNO ,RE!ENRNNNNIGGAGIGNE SNEOEEQ!EZ A RJRRDROF PETOTTJTS LLZLLEL!AMSXSSMS ENODOOSO'
```
