# the social network - api
## `users`

User `id` is identified by (`username` or `email`) and `password`

Field | Type | Description
------|------|------------
`id` | `uuid` | user id
`username` | `string` | username
`password` | `string` | password
`email` | `email` | email address

Endpoint | Description
---------|------------
`GET /users` | Retrieves a list of users
`GET /users/:userId` | Retrieves a specific user
`POST /users` | Creates a new user
`PUT /users/:userId` | Updates a specific user
`PATCH /users/:userId` | Partially updates a specific user
`DELETE /users/:userId` | Deletes a specific user

### Edges

Endpoint | Description
---------|------------
`GET /users/:userId/photos` | alias to `/photo/?owner={userId}`
`GET /users/:userId/friends` | alias to `/connection/?type=friend&entity_contains={userId}`
`GET /users/:userId/groups` | get a users' groups
`GET /users/:userId/posts` | alias to `/post/?owner={userId}`

## `profile`

User `id` has the name `name`, was born on `birthday`, was employed by `employers`, studied at `education`, and is located at `location`

Field | Type | Description
------|------|------------
`id` | `user.id` | references `user.id`
`name` | `string` | display name (or real name)
`birthday` | `datetime` | birthday
`employers` | `list` | list of `employers.id`
`education` | `list` | list of `education.id`
`location` | `list` | list of `location.id` (current location, previous locations, birthplace, etc.)

Endpoint | Description
---------|------------
`POST /profile` | create a profile
`GET /profile/{USER-ID}` | get profile
`POST /profile/{USER-ID}` | update profile
`DELETE /profile/{USER-ID}` | delete profile

## `post`

Post `id` is of type `type`, containing `content`, posted by `owner` on `created`, last updated on `updated`, with the location `location`, and is visible to `visibility`

Field | Type | Description
------|------|------------
`id` | `uuid` | post id
`type` | `enum` | the post type (text, photo, etc.)
`content` | `string` | JSON representation of the data
`owner` | `user.id` | references `user.id`
`created` | `datetime` | creation time of the post
`updated` | `datetime` | when this post was last updated
`location` | `location.id` | references `location.id`
`visibility` | `list` | list of access for this post

Endpoint | Description
---------|------------
`POST /post` | create post
`GET /post/{POST-ID}` | read post
`POST /post/{POST-ID}` | update post
`DELETE /post/{POST-ID}` | delete post

### Endpoints

Endpoint | Description
---------|------------
`GET /post/{POST-ID}/comments` | get comments for a post
`GET /post/{POST-ID}/favs` | get favorites for a post

## `comment`

`owner` writes comment `content` on `type` (id `to`)

Field | Type | Description
------|------|------------
`id` | `uuid` | comment id
`type` | `enum` | comment type (`post`, `photo`, etc.)
`to` | `uuid` | `type.id` (`post.id`, `photo.id`, etc.)
`owner` | `user.id` | references `user.id`
`content` | `string` | comment

## `fav`

`owner` favorites `type` (id `to`)

Field | Type | Description
------|------|------------
`id` | `uuid` | favorite id
`type` | `enum` | fav type (`post`, `photo`, etc.)
`to` | `uuid` | `type.id` (`post.id`, `photo.id`, etc.)
`owner` | `user.id` | references `user.id`

## `message`

A message `id` was sent from `from` to `to` on `on` containing the message `content` and was `read`

Field | Type | Description
------|------|------------
`id` | `uuid` | message id
`from` | `user.id` | references `user.id`
`to` | `user.id` | references `user.id`
`type` | `enum` | message type (`post`, `photo`, etc.)
`id` | `uuid` | `type.id` (`post.id`, `photo.id`, etc.)
`on` | `datetime` | creation time
`read` | `boolean` | flag representing whether `to` has read this or not

## `connection`

All elements in `entity` are connected by `type`.

Field | Type | Description
------|------|------------
`entity` | `set` | entities involved with this connection
`type` | `enum` | what type of connection this is
`created` | `datetime` | creation time
`updated` | `datetime` | last updated

## `group`

`name` (`id`) is a group of type `type`, described to be `desc`

Field | Type | Description
------|------|------------
`id` | `uuid` | group id
`type` | `enum` | group type
`name` | `string` | group name
`desc` | `string` | group description

### Edges

Endpoint | Description
---------|------------
`GET /group/{GROUP-ID}/members` | A set of group members

## `photo`

Photo `id`, owned by `owner`, contains the title `title` and description `desc`, and was taken at `location`

Field | Type | Description
------|------|------------
`id` | `uuid` | photo id
`title` | `string` | photo title
`desc` | `string` | photo description
`album` | `album.id` | album id
`owner` | `user.id` | references `user.id`
`location` | `location.id` | references `location.id`

## `album`

Album `id`, owned by `owner`, contains the title `title` and description `desc`

Field | Type | Description
------|------|------------
`id` | `uuid` | album id
`title` | `string` | album title
`desc` | `string` | album description
`owner` | `user.id` | album owner

## `location`

Location `id` is located at `lat`, `lon`, with the name `name`

Field | Type | Description
------|------|------------
`id` | `uuid` | location id
`lat` | `double` | latitude
`lon` | `double` | longitude
`name` | `string` | location name

## `subscription`

`user` is subscribed to `type` `to` and receives notifications by `by`.

Field | Type | Description
------|------|------------
`id` | `uuid` | subscription id
`user` | `user.id` | references `user.id`
`type` | `enum` | subscription type (`profile`, `group`, etc.)
`to` | `uuid` | `type.id` (`profile.id`, `group.id`, etc.)
`by` | `enum` | `email`, `web`, `iOS`, etc.
