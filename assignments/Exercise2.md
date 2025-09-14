concept PasswordAuthentication
  - purpose limit access to known users
  - principle after a user registers with a username and a password,
    they can authenticate with that same username and password
    and be treated each time as the same user
  - state
    + a set of Users with
    + an email address String
    + a username String
    + a password String
    + a confirmed flag
    + a token String

  - actions
    + register (username: String, password: String, email: String): (user: User) (token: String)
      * <strong>requires</strong> a unique username
      * <strong>effect</strong>s creates a User with the associated username and password. Creates a new
              token to email the user via a synchronous state and verify their email address. Confirmed is set to false
    + confirm (username: String, token: String)
      * <strong>effects</strong> if the token matches the usernames’ associated User, the flag confirmed changes to true else confirmed stays false
    + authenticate (username: String, password: String)
      * <strong>requires</strong> User’s confirmed flag must be true and username and password must match internal record for user
      * <strong>effects</strong> if the user is authenticated, they will be treated each time as the same user
              associated with that username and password. If they are not, they will not proceed
Questions
1.	Complete the definition of the concept state.
2.	Write a requires/effects specification for each of the two actions. (Hints: The register action creates and returns a new user. The authenticate action is primarily a guard, and doesn’t mutate the state.)
3.	What essential invariant must hold on the state? How is it preserved?\
<strong>No two users can have the same username, this is preserved by the register action that requires a unique username.</strong>
4.	One widely used extension of this concept requires that registration be confirmed by email. Extend the concept to include this functionality. (Hints: you should add (1) an extra result variable to the register action that returns a secret token that (via a sync) will be emailed to the user; (2) a new confirm action that takes a username and a secret token and completes the registration; (3) whatever additional state is needed to support this behavior.)
