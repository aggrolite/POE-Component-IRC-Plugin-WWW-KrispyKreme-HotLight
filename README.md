# NAME

POE::Component::IRC::Plugin::Donuts - IRC Plugin
to announce when there are fresh donuts in the area!

# SYNOPSIS

    use POE::Component::IRC::Plugin::Donuts;

    use strict;
    use warnings;

    use POE qw(
      Component::IRC
      Component::IRC::Plugin::Donuts
    );

    my $nick    = 'donut_bot';
    my $ircname = 'the donut bot';
    my $server  = 'irc.foobar';

    my @channels = ('#coffee');

    my $irc = POE::Component::IRC->spawn(
        nick    => $nick,
        ircname => $ircname,
        server  => $server
    ) or die "oops... $!";

    POE::Session->create(
        package_states => [main => [qw(_start irc_001)],],
        heap           => {irc  => $irc},
    );

    $poe_kernel->run;

    sub _start {
        my $heap = $_[HEAP];

      my $irc = $heap->{irc};
      $irc->yield(register => 'all');

      $irc->plugin_add(
          Donuts => POE::Component::IRC::Plugin::Donuts->new(    #
              geo => [34.101509, -118.32691]
          )
      );

        $irc->yield(connect  => {});
        return;
    }

    sub irc_001 {
        $irc->yield(join => $_) for @channels;
        return;
    }

# CONSTRUCTOR

    $irc->plugin_add(
        Donuts => POE::Component::IRC::Plugin::Donuts->new(    #
            geo => [34.101509, -118.32691]
        )
    );

The geo attribute is REQUIRED.  See [WWW::KrispyKreme::HotLight](https://metacpan.org/pod/WWW::KrispyKreme::HotLight) for more info

# DESCRIPTION

POE::Component::IRC::Plugin::Donuts is an IRC
plugin that announces when there are fresh Krispy Kreme donuts near
the given location

# SEE ALSO

[WWW::KrispyKreme::HotLight](https://metacpan.org/pod/WWW::KrispyKreme::HotLight)

[POE::Component::IRC::Plugin](https://metacpan.org/pod/POE::Component::IRC::Plugin)

# AUTHOR

Curtis Brandt <curtis@cpan.org>

# COPYRIGHT

Copyright 2013- Curtis Brandt

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
