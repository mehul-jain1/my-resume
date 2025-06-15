---
layout: post
title:  "Authentication Notes"
date:   2024-07-17 16:02:06 +0530
categories: auth security
---
### Legacy Auth System Needs

A custom-built authentication system often needs to be revisited, even if it's only for short-term use. It can become insecure over time, due to spaghetti code and potentially flawed custom crypto/auth implementations.

The first thing to work on is often refactoring things like password and email reset token flows. Often, there might be a single token model handling multiple duties, but modern systems create individual models for each task (e.g., a `PasswordReset` model, an `EmailChange` model). This separation of concerns improves security and maintainability.

I once worked on a branch to do this kind of refactoring. The key was to introduce distinct models for token and password reset logic.

Also, it's crucial to continuously audit any authentication service with resources like the [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html) to ensure it meets current security standards.

### Authentication System Replacements

#### Full systems

##### Auth0
[auth0.com](https://auth0.com/)

- Full SaaS
- Can be expensive for millions of users

##### FusionAuth
[fusionauth.io](https://fusionauth.io/)

- Full SaaS option
- On-premises option
- On-premises version can be very cost-effective compared to Auth0

##### Keycloak
[www.keycloak.org](https://www.keycloak.org/)

- Self-hosted
- Open-source

#### Libraries

There are a lot of off-the-shelf open-source libraries for implementing auth systems. If you're going to roll your own in-house solution, it should involve grabbing one of these and gluing it together in a simple application where you don't write any of the actual auth logic or system yourself—just build the necessary views and administrative interfaces on top.

##### Doorkeeper + Devise

[github.com/doorkeeper-gem/doorkeeper](https://github.com/doorkeeper-gem/doorkeeper)

[github.com/heartcombo/devise](https://github.com/heartcombo/devise)

These are battle-tested and well-supported libraries for Ruby on Rails. This approach is great for adding OAuth to an application.

##### Rodauth

[rodauth.jeremyevans.net](http://rodauth.jeremyevans.net/)

This is a very well-written but lesser-known library. The architecture is very clean and easy to understand, whereas Devise can feel a little messy even though it's better supported.

It's worth looking into. I started a small project to try it out once and was impressed. 