Asynchronous Twitter client API for node.js
===========================================

[node-twitter](https://github.com/jdub/node-twitter) aims to provide a complete,
asynchronous client library for Twitter (and other compliant endpoints),
including REST, stream and search APIs. It was inspired by, and uses some code
from, technoweenie's [twitter-node](https://github.com/technoweenie/twitter-node).


## Requirements

You can install node-twitter and its dependencies with npm: `npm install twitter`.

- [node](http://nodejs.org/) v0.2+
- [node-oauth](https://github.com/ciaranj/node-oauth)
- [cookie-node](https://github.com/jed/cookie-node)


## Getting started

It's early days for node-twitter, so I'm going to assume a fair amount of
knowledge for the moment. Better documentation to come as we head towards a
stable release.


### Setup API (stable)

	var sys = require('sys'),
	    Twitter = require('twitter');

	var twitter = new Twitter({
		consumer_key: 'CONSUMER_KEY',
		consumer_secret: 'CONSUMER_SECRET',
		access_token_key: 'ACCESS_TOKEN_KEY',
		access_token_secret: 'ACCESS_TOKEN_SECRET'
	});


### Basic OAuth-enticated GET/POST API (stable)

The convenience APIs aren't finished, but you can get started with the basics:

	twitter.get('/statuses/show/27593302936.json', {include_entities:true},
	    function(data) {
		    sys.puts(sys.inspect(data));
	    }
	);


### REST API (unstable, may change)

Note that all functions may be chained:

	twitter
        .verifyCredentials(
            function (data) {
			    sys.puts(sys.inspect(data));
		    }
		)
		.updateStatus('Test tweet from node-twitter/' + twitter.VERSION,
			function (data) {
				sys.puts(sys.inspect(data));
			}
		);


### Search API (unstable, may change)

	twitter.search('nodejs OR #node', function(data) {
		sys.puts(sys.inspect(data));
	});


### Streaming API (stable)

The stream() callback receives a Stream-like EventEmitter:

	twitter.stream('statuses/sample', function(stream) {
		stream.on('data', function (data) {
			sys.puts(sys.inspect(data));
		});
	});

node-twitter also supports user and site streams:

	twitter.stream('user', {track:'nodejs'}, function(stream) {
		stream.on('data', function (data) {
			sys.puts(sys.inspect(data));
		});
		// Disconnect stream after five seconds
		setTimeout(stream.destroy, 5000);
	});


## Contributors

- [Jeff Waugh](http://github.com/jdub) (author)
- [rick](http://github.com/technoweenie) (parser.js and, of course, twitter-node!)
- [Daniel Gasienica](https://github.com/gasi) (Twitter.createFriendship)


## TODO

- Complete the convenience functions, preferably generated
- Support [recommended reconnection behaviour](http://dev.twitter.com/pages/user_streams_suggestions) for the streaming APIs
- Should probably implement basic auth for non-Twitter endpoints
