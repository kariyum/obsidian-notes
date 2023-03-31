Was like this
JWT authentikat doesn't work with scala 2.12, no matter what version i use
using this instead
[JWT Scala: Native (jwt-scala.github.io)](https://jwt-scala.github.io/jwt-scala/jwt-core-jwt.html)

add ID for the user
encrypt the passord with secret key
verify from cookie

alright;
# future updates
now we are just verifying if the token is expired or not but in the real world case it's more logical and robust to hash the user_id + encrypted_password verify that is valid + expiration date