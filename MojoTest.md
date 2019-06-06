```perl
#### Test request

use Test::More;
use Test::Mojo;
use Mojo::File qw( path );
use JSON;

my $t    = Test::Mojo->new( path( 'index.cgi' ) );

$t->get_ok( '/v3' )->status_is( 200 );

#### Get content 

$t->ua->get('/v3')->res->json;

#### Mojo::Test methods 
## app,
## tx,
## ua,
## has,
## new,
## or,
## success,
## message,
##
## status_is,
## status_isnt,
##
## header_is,
## header_isnt,
## header_like,
## header_unlike,
##
## 
## text_is,
## text_isnt,
## text_like,
## text_unlike,
## 
## content_is,
## content_isnt,
## content_like,
## content_type_is,
## content_type_isnt,
## content_type_like,
## content_type_unlike,
## content_unlike,
##
## json_has,
## json_hasnt,
## json_is,
## json_like,
## json_message_has,
## json_message_hasnt,
## json_message_is,
## json_message_like,
## json_message_unlike,
## json_unlike,
## 
## message_is,
## message_isnt,
## message_like,
## message_unlike,
##
## element_count_is,
## element_exists,
## element_exists_not,
##
## head_ok,
## send_ok,
## request_ok,
## message_ok,
## websocket_ok
## finished_ok,
## finish_ok,
## get_ok,
## options_ok,
## patch_ok,
## post_ok,
## put_ok,
## delete_ok,
##  
## reset_session,
```
