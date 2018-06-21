# Credential comparison rules

After a set of credentials is inserted, the SSO implementation in AM allows for a second set of credentials to be inserted on top of the first one, provided that the second credentials are different. That means that at most two separate sets of credentials can be stored at the same time (that is, only a single level of restore is supported). The credentials are cached in SSO in a stack fashion, with only the topmost set of credentials accessible for authentication.

When inserting a set of credentials into SSO, it is compared to those already available in the container (if any).
 If the comparison is positive, the new credentials are ignored and not cached.
 If the comparison is negative, the new credentials are stored on top of the currently stored credentials (if the second slot is available).

The rule enforced to compare domain credentials is as follows: Two sets of domain credentials are considered matching if username and domain are equal (the password is ignored in the comparison).

The rule enforced to compare smart card credentials is as follows: PINs never match (they are always considered different because they might be associated with different smart cards).

## Domain credentials examples

* `usernameA/domainA/passwordA == usernameA/domainA/passwordA` <br>Credentials match: all three fields match.
* `usernameA/domainA/passwordA != usernameB/domainA/passwordA` <br>Credentials do not match: the username is different.
* `usernameA/domainA/passwordA != usernameA/domainB/passwordA` <br>Credentials do not match: the domain is different.
* `usernameA/domainA/passwordA == usernameA/domainA/passwordB` <br>Credentials match: only the password is different.

## Smart card credential examples

* `PIN 1234 != PIN 5678` <br>PINs are different.
* `PIN 1234 != PIN 1234` <br>PINs are considered different even if they are actually identical.


