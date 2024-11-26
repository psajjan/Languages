# Arrays

### Functions

#### pop
It removes and returns the last element from the array. If the array is empty, it return `undef`.
```perl
my @names = ('Foo', 'Bar', 'Baz');
my $last_one = pop @names;
 
print "$last_one\n";  # Baz
print "@names\n";     # Foo Bar
```

#### push
It adds the given element at the end of the array. It can even add an entire array.
```perl
my @names = ('Foo', 'Bar');
push @names, 'Moo';
print "@names\n";     # Foo Bar Moo
 
my @others = ('Darth', 'Vader');
push @names, @others;
print "@names\n";     # Foo Bar Moo Darth Vader
```

#### shift
Similar to `pop` but removes and returns the first element.
```perl
my @names = ('Foo', 'Bar', 'Moo');
my $first = shift @names;

print "$first\n";     # Foo
print "@names\n";     # Bar Moo
```

#### unshift
Similar to `push` but adds elements to the beginning of the array.
```perl
my @names = ('Foo', 'Bar');
unshift @names, 'Moo';
print "@names\n";     # Moo Foo Bar 
 
my @others = ('Darth', 'Vader');
unshift @names, @others;
print "@names\n";     # Darth Vader Moo Foo Bar
```
