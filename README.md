#xolvio:inbox-stub

Overrides Meteor's `Email.send` and collects emails into a collection.

###Installation

`meteor add xolvio:inbox-stub`


###Usage

Run a through your app manually, or using integration / end-to-end tests and
emails that are sent from your app will be captured wherever `Email.send` is
used, such as accounts for verifying emails.


To see the sent emails from your console/test as a json object you run:

```javascript
Meteor.call('getEmailsFromInboxStub', function(e, emails) {
  console.log(emails);
});
```

Assuming two emails were sent, the code above would show:

```json
[
 {
   _id: "ACZqWmLejePo9zQQD",
   from: "Xolv.io <no-reply@xolv.io>",
   to: "user@example.com"
   subject: "Please verify your email address",
   text: "Hello Someone,↵↵To verify your account email, simply click the link below.↵↵http://localhost:3000/#/verify-email/m_3n4CbgeESDGaugJ656RoqJRj5PlCjk0Cm43PU3aEN↵↵Thanks.↵"
 },
 {
   _id: "2aY6FkWRcbcr8RxL7",
   from: "Xolv.io <no-reply@xolv.io>",
   to: "user@example.com"
   subject: "Another email",
   text: "with different content"
 }
]
```

In your code, you can then do assertions like:

```javascript
  emails[0].subject.should.be('Please verify your email address');
```

Another use for testing is to extract the verification link like this:

```javascript

  // grab the verification link
  var verificationLink = emails[0].text.match(/(http|https|www)\S+/)[0];

  // then use something like xolvio:webdriver to visit the URL like a user would
  browser.url(verificationLink)
```

This package is a `debugOnly` package and will only be used when developing
locally.
