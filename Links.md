[Список хелперов](https://metacpan.org/release/DBIx-Class-Helpers)

[В примерах](http://pragmaticperl.com/issues/06/pragmaticperl-06-dbixclass-%D0%B2-%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%D0%B0%D1%85.html)

[Сборник рецептов](http://pragmaticperl.com/issues/22/pragmaticperl-22-dbixclass.-%D1%81%D0%B1%D0%BE%D1%80%D0%BD%D0%B8%D0%BA-%D1%80%D0%B5%D1%86%D0%B5%D0%BF%D1%82%D0%BE%D0%B2.html)

[Поиск (Searching)](https://metacpan.org/pod/distribution/DBIx-Class/lib/DBIx/Class/Manual/Cookbook.pod#SEARCHING)

### Manuals

* DBIx::Class::Manual::DocMap - What documentation do we have?
* DBIx::Class::Manual - User's Manual overview.
* DBIx::Class::Manual::QuickStart - Up and running with DBIC in 10 minutes.
* DBIx::Class::Manual::Intro - More detailed introduction to setting up and using DBIx::Class.
* DBIx::Class::Manual::SQLHackers - How to use DBIx::Class if you know SQL (external, available on CPAN)
* DBIx::Class::Manual::Joining - Joining tables with DBIx::Class.
* DBIx::Class::Manual::Features - A boatload of DBIx::Class features with links to respective documentation.
* DBIx::Class::Manual::Glossary - What do all those terms mean?
* DBIx::Class::Manual::Cookbook - Various short recipes on how to do things.
* DBIx::Class::Manual::FAQ - Frequently Asked Questions, gathered from IRC and the mailing list.
* DBIx::Class::Manual::Troubleshooting - What to do if things go wrong (diagnostics of known error messages).

### Some essential reference documentation. The first two list items are the most important.

The first two list items are the most important.

* "search" in DBIx::Class::ResultSet - Selecting and manipulating sets.

The DSL (mini-language) for query composition is only partially explained there, see "WHERE CLAUSES" in SQL::Abstract for the complete details.

* $schema::Result::$resultclass - Classes representing a single result (row) from a DB query.

Such classes normally subclass DBIx::Class::Core, the methods inherited from DBIx::Class::Row and DBIx::Class::Relationship::Base are used most often.

* DBIx::Class::ResultSetColumn - Perform operations on a single column of a ResultSet.
* DBIx::Class::ResultSource - Source/Table definition functions.
* DBIx::Class::Schema - Overall sources, and connection container.
* DBIx::Class::Relationship - Simple relationship declarations.
* DBIx::Class::Relationship::Base - Relationship declaration details.
* DBIx::Class::InflateColumn - Making objects out of your column values.
