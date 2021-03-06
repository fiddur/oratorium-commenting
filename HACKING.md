Contributing
============

* Functional.  Work with data, not method-carrying object.

* Comment on issue to let others know you started implementing something.
  Discuss data & code design on the issue.

* Use [gitflow](https://github.com/nvie/gitflow) branching model - make pull
  requests toward the `develop` branch.

* Make sure new code is tested both with relevant unit tests and integration
  tests.

* Consider parallellising of both comment service and storage in everything.

* Settings are EVIL!  ...make extensibility in clients.

* Clients dictates IDs, for posts and whatnots.

* NodeJS specifics:

  * Immutable.  Don't mutate state, always use `const`.  (Exception: replacing
    the complete datastore of a module...)

  * `require` should not have side effects.

  * Use manual currying to distinguish the pure factories from the impure
    executioners, like `myFunc = (general, params) => (execution, args) => {...}`


Architecture
------------

This is about comments, designed for a blog.

Extensibility lives in the client, don't complect the code!


### Endpoints

#### [POST /login](services/login_post/README.md)

Perform OpenID Connect login and exchange the `id_token` for a cookie (http
only, secure) `Authorization: access_token`.


#### GET /pages/:page/comments/

Response: `{ comments: [ ... ] }`


#### PUT /pages/:page/comments/:id

Header: `Authorization: Bearer <access_token>`
Payload: `{ page (uuid), body }`

#### DELETE /pages/:page/comments/:id

Header: `Authorization: Bearer <access_token>`


### Event streams and events

* Streams: `page-<page>`
  * Event `CommentAdded`: `{ body, page, user }`

  * page
  * user (sub@iss)
  * text

* page - The page ID Stream ID is `page@site`.
  * site
  * url ?

* subscription

* oidc_clients


### Things that belong elsewhere

* Rate limiting - should be done in a proxy layer.

* Hiding avatars - can be done with CSS.

* Changing comment display order - can be done with other client js.
