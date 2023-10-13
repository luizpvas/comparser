# Comparser

* [`integer`](#integer)
* [`chomp_if`](#chomp_if)
* [`chomp_while`](#chomp_while)


#### `integer`

Chomps an integer from source and pushes the result to the state.

```ruby
parser = succeed.and_then(:integer)

parser.call(Comparser::State.new('2023')).tap do |state|
  state.good?   # => true
  state.offset  # => 4
  state.chomped # => ''
  state.result  # => Result::Good[value: 2023]
end
```

#### `chomp_if`

Chomps a single character from source if predicate returns true. Otherwise, a bad result is pushed to state.

```ruby
parser = succeed.and_then(:chomp_if, ->(ch) { ch == 'a' })

parser.call(Comparser::State.new('aaabb')).tap do |state|
  state.good?   # => true
  state.offset  # => 1
  state.chomped # => 'a'
end

parser.call(Comparser::State.new('bbbcc')).tap do |state|
  state.bad?    # => true
  state.offset  # => 0
  state.chomped # => ''
end
```

#### `chomp_while`

Chomps characters from source as long as predicate returns true. This function always leaves the state in a good
state, even if predicate returns false on the first run. It works as a zero-or-more loop.

```ruby
parser = succeed.and_then(:chomp_while, ->(ch) { ch == 'a' })

parser.call(Comparser::State.new('aaabb')).tap do |state|
  state.good?   # => true
  state.offset  # => 3
  state.chomped # => 'aaa'
end

parser.call(Comparser::State.new('bbbcc')).tap do |state|
  state.good?   # => true
  state.offset  # => 0
  state.chomped # => ''
end
``````