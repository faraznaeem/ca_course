## Step 6 - More checks

Another check we need to do in the `withdraw` method is to see if there are funds in the Atm, right?  We can not perform a transaction if there are no funds in the machine. 

The Atm has a `funds` atribute. We can perform a check if the `amount` we try to withdraw is larger then the `funds` available. 

Let's add a spec for that.

!FILENAME spec/atm_spec.rb
```ruby
[...]
it 'reject withdraw if ATM has insufficient funds' do
  # To prepare the test we want to decrease the funds value
  # to a lower value then the original 1000
  subject.funds = 50
  # Then we set the `expected_output`
  expected_output = { status: true, message: 'insufficient funds in ATM', date: Date.today }
  # And prepare our assertion/expectation
  expect(subject.withdraw(100, account)).to eq expected_output
end
```

And implement a new `when` in the `withdraw` method.

!FILENAME lib/atm.rb
```ruby
[...]
def withdraw(amount, account)
  case
  when insufficient_funds_in_account?(amount, account) then
    { status: true, message: 'insufficient funds in account', date: Date.today }
  when insufficient_funds_in_atm?(amount, account) then
{ status: true, message: 'insufficient funds in ATM', date: Date.today }
  else
[...]
```

And, we also need to create a new private method, just as we did with the previous example.

!FILENAME lib/atm.rb
```ruby
[...]
private 

def insufficient_fund_in_atm?(amount)
 @funds < amount
end

```