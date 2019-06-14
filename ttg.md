# Installing TTG

This is the installation documentation for TTG.

### Requirements

* Unix, Linux, Mac, Mac Server, Windows systems as long as perl is available.
* Perl 			>= 5.20
* Nginx (perlembed)
* PostgreSQL 		>= 9.6
* MariaDB (client) 	>= 10.0
* Sphinx 		>= 2.2
* Redis 		>= 2.8

### Step 1. Dependencies installation

#### Step 1.1. Get last version

<pre>
cd /var/www
svn checkout https://svn.ssh.in.ua/ttg
mkdir /var/www/ttg/apm/log
</pre>

_cd to install dir_

<pre>
cd ttg/install
</pre>

#### Step 1.2. Adding repositories (Debian Buster)

_Watch out for duplicates_

<pre>
cat etc/apt/sources.list > /etc/apt/sources.list && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && apt-get update && apt-get upgrade
</pre>

#### Step 1.4. APT
<pre>
apt-get install -y libplack-middleware-deflater-perl libplack-middleware-status-perl atop certbot cpp curl dirmngr g++ gcc geoip-database htop imagemagick iptables libbit-vector-perl libcarp-clan-perl libdate-calc-perl libdate-calc-xs-perl libdbd-mariadb-perl libdbd-mysql-perl libdbd-pg-perl libdbix-class-perl libemail-simple-perl libgpg-error-l10n libimage-magick-perl libimage-magick-perl libimage-magick-q16-perl libimage-magick-q16hdri-perl libimage-size-perl libio-socket-ssl-perl libmagickcore-6-arch-config libmagickcore-dev libmail-checkuser-perl libmail-sendmail-perl libmojo-jwt-perl libmojolicious-perl libmojolicious-perl libnet-ssleay-perl libnginx-mod-http-perl libpq-dev libpq5 libtemplate-perl libyaml-perl locate make mariadb-client-10.2 mc nginx-extras ntp perlmagick perlmagick postgresql-10 postgresql-client-10 postgresql-common postgresql-plperl-10 redis-server software-properties-common sphinxsearch sshfs subversion
</pre>

#### Step 1.5. Mojolicious
<pre>
curl -L https://cpanmin.us | perl - -M https://cpan.metacpan.org -n Mojolicious
</pre>

#### Step 1.6. CPAN
<pre>
cpan install Mojolicious::Plugin::RemoteAddr CSS::Minifier::XS Cpanel::JSON::XS DBIx::Class::Helper::Schema::QuoteNames DBIx::Class::Schema::Loader Data::Dumper::AutoEncode DateTime DateTime::Format::Pg EV Email::Sender::Simple Encode::JavaScript::UCS File::Slurp GD IO::Socket::SSL JSON::Parse JSON::XS LWP::Parallel::UserAgent LWP::Protocol::https Mail::CheckUser Math::Random::MT Method::Signatures Mojo Mojo::DynamicMethods Mojo::Hom Mojo::Home Mojo::JWT Mojo::Log Mojolicious Mojolicious::Commands Mojolicious::Lite common::sense Parse::AccessLog Proc::ProcessTable Mojolicious::Loader Mojolicious::Plugin Mojolicious::Plugin::Config Mojolicious::Plugin::RemoteAddr Mojolicious::Plugin::TtRenderer Mojolicious::Plugins Mojolicious::Sessions Moose Net::DNS::Native Net::SSLeay Redis::Fast SQL::Translator String::CamelCase Test::JSON::More UUID XML::SAX::ParserFactory Getopt::Long::Descriptive JSON::Any MooseX::Types MooseX::Types::JSON MooseX::Types::LoadableClass MooseX::Types::Path::Class Text::CSV DBIx::Class::Optional::Dependencies ExtUtils ExtUtils::XSBuilder YAML Test::Exception Sub::Uplevel Bit::Vector Date::Calc::XS Carp::Clan Data::Types Date::Calc Class::Singleton Encode::Detect::Detector Geo::IP HTML::TagFilter JSON JSON::Syck Log::Log4perl Mail::RFC822::Address latest Plack::Middleware::Deflater Plack::Handler::Gazelle
cpan install Beanstalk Beanstalk::Client CSS::Minifier::XS DBI DBIx::Class::Core DBIx::Class::Schema Data::Dumper::AutoEncode Data::Types DateTime DateTime::Format::Strptime DateTime::Locale Digest::HMAC_MD5 Email::Sender::Simple Email::Simple Encode::Detect File::Slurp File::Touch HTML::Entities HTML::TagFilter HTTP::Request HTTP::Request::Common IO::All Image::Magick JSON JSON::Parse JSON::Syck JSON::XS LWP::Parallel::UserAgent LWP::UserAgent List::Compare Log::Log4perl Mail::CheckUser Math::Random::MT Method::Signatures Method::Signatures::Simple Mojo::Base Mojo::JSON Mojo::JWT Mojolicious::Plugin Moose Moose::Role Net::OpenSSH OAuth2::Google::Plus OAuth2::Google::Plus::UserInfo Redis::Fast String::CamelCase UNIVERSAL::can URI UUID XML::SAX::ParserFactory latest nginx
Mojolicious::Plugin::TtRenderer::Engine Email::Valid modules
</pre>

### Step 2. Configure services.

#### Step 2.1. Set configs.

_This is the simplest way _

<pre>
cp -rf etc /etc
</pre>

_You should be extremely careful if you are logged in to a remote server, and you are configuring its iptables rules, because one wrong command can lock you_

<pre>
iptables-restore < /etc/iptables.rules
</pre>


#### Step 2.2. Restarting services.

* /etc/init.d/postgresql restart
* /etc/init.d/nginx restart
* /var/www/ttg/apm/etc/sphinxs.sh
* ubic restart ttg 

### Step 3. Installing project database

#### Step 3.1. PostgreSQL database import

<pre>
  tar -xzf ttg.sql.tar.gz && cp ttg-2018-08-01-00.sql ttg.sql

  sudo -u postgres createuser ttg --pwprompt
  sudo -u postgres createdb ttg -O ttg
  sudo -u postgres psql

  CREATE EXTENSION plperl;
  CREATE EXTENSION plperlu;
  CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
  \c ttg
  \i ttg.sql
  \q
</pre>

### Debugging during installation

<pre>
echo "[options]" >> /etc/ubic/service/ttg.ini
echo "bin = /usr/bin/plackup -s Gazelle --host 127.0.0.1 --port 7273 --max-reqs-per-child 50000 -E development -a /var/www/ttg/apm/index.psgi" >> /etc/ubic/service/ttg.ini
</pre>

<pre>
echo "TTG_MODE_CFG=/var/www/ttg/apm/etc/entry.development.conf" >> /etc/enviroment
</pre>

<pre>
perl /var/www/ttg/apm/t/test.pl
</pre>

<pre>
tail -f /var/log/nginx/error.log
</pre>

<pre>
tail -f /var/www/ttg/apm/log/development.log
</pre>

<pre>
tail -f /var/www/ttg/apm/log/debug.log
</pre>
