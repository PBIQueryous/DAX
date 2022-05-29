```ruby
# Customers - SUMX + Expanded Table :=
CALCULATE (
    SUMX (
        VALUES ( Customer[LastName] ),
        1
    ),
    Sales
)
```
