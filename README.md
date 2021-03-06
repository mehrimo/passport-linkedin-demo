# passport-linkedin-demo

Passport-LinkedIn
Passport strategy for authenticating with LinkedIn using the OAuth 1.0a API.

This module lets you authenticate using LinkedIn in your Node.js applications. By plugging into Passport, LinkedIn authentication can be easily and unobtrusively integrated into any application or framework that supports Connect-style middleware, including Express.

Install

$ npm install --save passport-linkedin passport express cookie-session dotenv


Usage

Configure Strategy

The LinkedIn authentication strategy authenticates users using a LinkedIn account and OAuth tokens. The strategy requires a verify callback, which accepts these credentials and calls done providing a user, as well as options specifying a consumer key, consumer secret, and callback URL.

passport.use(new LinkedInStrategy({
    consumerKey: LINKEDIN_API_KEY,
    consumerSecret: LINKEDIN_SECRET_KEY,
    callbackURL: "http://127.0.0.1:3000/auth/linkedin/callback"
  },
  function(token, tokenSecret, profile, done) {
    User.findOrCreate({ linkedinId: profile.id }, function (err, user) {
      return done(err, user);
    });
  }
));

PUT LINKEDIN_KEYS in .env file
Add .env and node_modules to gitignore

Authenticate Requests

Use passport.authenticate(), specifying the 'linkedin' strategy, to authenticate requests.

For example, as route middleware in an Express application:

app.get('/auth/linkedin',
  passport.authenticate('linkedin'));

app.get('/auth/linkedin/callback',
  passport.authenticate('linkedin', { failureRedirect: '/login' }),
  function(req, res) {
    // Successful authentication, redirect home.
    res.redirect('/');
  });
Extended Permissions

If you need extended permissions from the user, the permissions can be requested via the scope option to passport.authenticate().

For example, this authorization requests permission to the user's basic profile and email address:

app.get('/auth/linkedin',
  passport.authenticate('linkedin', { scope: ['r_basicprofile', 'r_emailaddress'] }));
Profile Fields

The LinkedIn profile is very rich, and may contain a lot of information. The strategy can be configured with a profileFields parameter which specifies a list of fields your application needs. For example, to fetch the user's ID, name, email address, and headline, configure strategy like this.

passport.use(new LinkedInStrategy({
    // clientID, clientSecret and callbackURL
    profileFields: ['id', 'first-name', 'last-name', 'email-address', 'headline']
  },
  // verify callback
));
Examples

For a complete, working example, refer to the login example.

Tests

$ npm install --dev
$ make test
Build Status

Credits

Jared Hanson
License

The MIT License

Copyright (c) 2011-2013 Jared Hanson <http://jaredhanson.net/>
