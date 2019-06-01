[Список хелперов](https://metacpan.org/release/DBIx-Class-Helpers)

[В примерах](http://pragmaticperl.com/issues/06/pragmaticperl-06-dbixclass-%D0%B2-%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%D0%B0%D1%85.html)

[Сборник рецептов](http://pragmaticperl.com/issues/22/pragmaticperl-22-dbixclass.-%D1%81%D0%B1%D0%BE%D1%80%D0%BD%D0%B8%D0%BA-%D1%80%D0%B5%D1%86%D0%B5%D0%BF%D1%82%D0%BE%D0%B2.html)

=head1 NAME

DBIx::Class::Manual::DocMap - What documentation do we have?

=head1 Manuals

=over 4

=item L<DBIx::Class::Manual> - User's Manual overview.

=item L<DBIx::Class::Manual::QuickStart> - Up and running with DBIC in 10 minutes.

=item L<DBIx::Class::Manual::Intro> - More detailed introduction to setting up and using DBIx::Class.

=item L<DBIx::Class::Manual::SQLHackers> - How to use DBIx::Class if you know SQL (external, available on CPAN)

=item L<DBIx::Class::Manual::Joining> - Joining tables with DBIx::Class.

=item L<DBIx::Class::Manual::Features> - A boatload of DBIx::Class features with links to respective documentation.

=item L<DBIx::Class::Manual::Glossary> - What do all those terms mean?

=item L<DBIx::Class::Manual::Cookbook> - Various short recipes on how to do things.

=item L<DBIx::Class::Manual::FAQ> - Frequently Asked Questions, gathered from IRC and the mailing list.

=item L<DBIx::Class::Manual::Troubleshooting> - What to do if things go wrong (diagnostics of known error messages).

=back

=head1 Some essential reference documentation

The first two list items are the most important.

=over 4

=item L<DBIx::Class::ResultSet/search> - Selecting and manipulating sets.

The DSL (mini-language) for query composition is only partially explained there,
see L<SQL::Abstract/WHERE CLAUSES> for the complete details.

=item L<C<$schema>::Result::C<$resultclass>|DBIx::Class::Manual::ResultClass>
- Classes representing a single result (row) from a DB query.

Such classes normally subclass L<DBIx::Class::Core>, the methods inherited
from L<DBIx::Class::Row|DBIx::Class::Row/METHODS> and
L<DBIx::Class::Relationship::Base|DBIx::Class::Relationship::Base/METHODS>
are used most often.

=item L<DBIx::Class::ResultSetColumn> - Perform operations on a single column of a ResultSet.

=item L<DBIx::Class::ResultSource> - Source/Table definition functions.

=item L<DBIx::Class::Schema> - Overall sources, and connection container.

=item L<DBIx::Class::Relationship> - Simple relationship declarations.

=item L<DBIx::Class::Relationship::Base> - Relationship declaration details.

=item L<DBIx::Class::InflateColumn> - Making objects out of your column values.

=back

=head1 FURTHER QUESTIONS?

Check the list of L<additional DBIC resources|DBIx::Class/GETTING HELP/SUPPORT>.

=head1 COPYRIGHT AND LICENSE

This module is free software L<copyright|DBIx::Class/COPYRIGHT AND LICENSE>
by the L<DBIx::Class (DBIC) authors|DBIx::Class/AUTHORS>. You can
redistribute it and/or modify it under the same terms as the
L<DBIx::Class library|DBIx::Class/COPYRIGHT AND LICENSE>.
