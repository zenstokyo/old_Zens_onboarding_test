# Zens onboarding programming exercises

1. Fork this repo and make a branch for your changes.
2. Please complete _all_ of the following problems.
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
