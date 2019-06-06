Source: https://metacpan.org/pod/distribution/DBIx-Class/lib/DBIx/Class/Manual/Cookbook.pod#SEARCHING

```perl
## SELECT artist.name FROM artist

my $rs = $schema->resultset('Artist')->search(
  undef,
  {
    columns => [qw/ name /]
  }
);

# SELECT name name, LENGTH( name ) FROM artist

my $rs = $schema->resultset('Artist')->search(
  {},
  {
    select => [ 'name', { LENGTH => 'name' } ],
    as     => [qw/ name name_length /],
  }
);

## WHERE artist LIKE ? AND title LIKE ?

my @albums = $schema->resultset('Album')->search({
  artist => { 'like', '%Lamb%' },
  title  => { 'like', '%Fear of Fours%' },
});


## WHERE ( artist LIKE '%Smashing Pumpkins%' AND title = 'Siamese Dream' ) OR artist = 'Starchildren'

my @albums = $schema->resultset('Album')->search({
  -or => [
    -and => [
      artist => { 'like', '%Smashing Pumpkins%' },
      title  => 'Siamese Dream',
    ],
    artist => 'Starchildren',
  ],
});


my $top_cd = $cd_rs->search({}, { order_by => 'rating' })->single;

```
