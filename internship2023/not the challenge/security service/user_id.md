Thoughts
If we generate a UUID for each user
	- we need to add ALLOW FILTERING to
	- tell if the user exists or not 
	- login a user

I suggest we use a hashing / encryption algorithm for the email+constantINT combination and use it as the user_id
OR even better base64 encode the email and set it as the ID of the user that way we can just decode it and get the email back

In this case we could login a user if the hash exists in the database which does not require a ALLOW FILTERING option since we are going to set it as a primary/partition key.
- Same email different password could be registered
