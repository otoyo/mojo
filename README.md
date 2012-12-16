
# Mojolicious [![Build Status](https://secure.travis-ci.org/kraih/mojo.png)](http://travis-ci.org/kraih/mojo)

  Back in the early days of the web, many people learned Perl because of a
  wonderful Perl library called CGI. It was simple enough to get started
  without knowing much about the language and powerful enough to keep you
  going, learning by doing was much fun. While most of the techniques used are
  outdated now, the idea behind it is not. Mojolicious is a new attempt at
  implementing this idea using state of the art technology.

## Features

  * An amazing real-time web framework, allowing you to easily grow single
    file [Mojolicious::Lite](http://mojolicio.us/perldoc/Mojolicious/Lite)
    prototypes into well structured web applications.
    * Powerful out of the box with RESTful routes, plugins, commands, Perl-ish
      templates, content negotiation, session management, testing framework,
      static file server, first class Unicode support and much more for you to
      discover.
  * Very clean, portable and Object Oriented pure-Perl API without any hidden
    magic and no requirements besides Perl 5.10.1 (although 5.14+ is
    recommended, and optional CPAN modules will be used to provide advanced
    functionality if they are installed).
  * Full stack HTTP and WebSocket client/server implementation with IPv6, TLS,
    SNI, IDNA, Comet (long polling) and gzip compression support.
  * Built-in non-blocking I/O web server, supporting multiple event loops and
    hot deployment, perfect for embedding.
  * Automatic CGI and PSGI detection.
  * JSON and HTML/XML parser with CSS selector support.
  * Fresh code based upon years of experience developing Catalyst.

## Installation

  All you need is a oneliner, it takes less than a minute.

    $ curl get.mojolicio.us | sh

  We recommend the use of a [Perlbrew](http://perlbrew.pl) environment.

## Getting Started

  These three lines are a whole web application.

    use Mojolicious::Lite;

    get '/' => {text => 'Hello World!'};

    app->start;

  To run this example with the built-in development web server just put the
  code into a file and start it with `morbo`.

    $ morbo hello.pl
    Server available at http://127.0.0.1:3000.

    $ curl http://127.0.0.1:3000/
    Hello World!

## Duct tape for the HTML5 web

  Web development for humans, making hard things possible and everything fun.

    use Mojolicious::Lite;

    # Simple plain text response
    get '/' => {text => 'Hello World!'};

    # Route associating "/time" with template in DATA section
    get '/time' => 'clock';

    # RESTful web service with JSON and text representation
    get '/list/:offset' => sub {
      my $self    = shift;
      my $numbers = [0 .. $self->param('offset')];
      $self->respond_to(
        json => {json => $numbers},
        txt  => {text => join(',', @$numbers)}
      );
    };

    # Scrape information from remote sites
    post '/title' => sub {
      my $self = shift;
      my $url  = $self->param('url') || 'http://mojolicio.us';
      $self->render_text(
        $self->ua->get($url)->res->dom->html->head->title->text);
    };

    # WebSocket echo service
    websocket '/echo' => sub {
      my $self = shift;
      $self->on(message => sub {
        my ($self, $msg) = @_;
        $self->send("echo: $msg");
      });
    };

    app->start;
    __DATA__

    @@ clock.html.ep
    % use Time::Piece;
    % my $now = localtime;
    The time is <%= $now->hms %>.

  Single file prototypes like this one can easily grow into well-structured
  applications.

## Want to know more?

  Take a look at our excellent [documentation](http://mojolicio.us/perldoc>)!