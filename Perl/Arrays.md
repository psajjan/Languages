# Arrays

## Basics

```perl
# Define an array
my @fruits = ('apple', 'kiwi', 'grapes');

# Print entire array
print "@fruits\n";

# Index to get scalar elements
print "0: $fruits[0]\n";
print "1: $fruits[1]\n";
print "2: $fruits[2]\n";

# Elements can also be indexed backwards using negative integers instead of positive numbers.
print "0: $fruits[0]\n";
print "Last: $fruits[-1]\n";
print "Last but one: $fruits[-2]\n";
```

### Size
Following are different ways to get the size of the array.
```perl
# If the array is used in scalar context.
my $size1 = scalar @fruits;
my $size2 = @fruits;

# Get the index of last element and add 1
my $size3 = $#fruits + 1;
```

### Sequential Arrays
PERL offers a shortcut for sequential numbers and letters.
```perl
# Shortcut saves time
@10 = (1 .. 10);
@100 = (1 .. 100);
@1000 = (100 .. 1000);
@abc = (a .. z);
```

## Functions

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
