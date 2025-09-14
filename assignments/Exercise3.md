<strong>Concept</strong> Personal Access Token
  - <strong>Purpose</strong> provides a secure, scoped, and revocable access to GitHub’s services without requiring users to provide their password
  - <strong>Principle</strong>
    A User creates a randomly generated Personal Access Token with an optional expiration date and specified permissions, limited to the permissions of their GitHub account. Users can then use their Personal Access Token to authenticate themselves and access GitHub’s services. They can use their Token until it expires, or the owner deletes it at which time the owner can create a new Token or regenerate their deactivated Token. 
  - <strong>State</strong>
    + A set of Tokens String with
    + A permissions String
	  + An expiration Time (optional)
	  + An isActive Flag
	  + An owner User
  - <strong>Actions</strong>
    + Create (owner: User, permissions: String, (optional) expiration: Time): (Token: String)
	    * <strong>Requires</strong> a valid user and permissions
      * <strong>Effects</strong> creates a unique randomly generated Token String and sets isActive to True and associates the Token with the owner
    + Authenticate (Token: String)
      * <strong>Requires</strong> valid Token that matches with the owner. IsActive must be True and access being requested must be within permissions
      * <strong>Effects</strong> authenticated user gains access to requested service and is treated as that owner with the specified permissions
    + Regenerate (Token: String, owner: User, (optional) expiration: Time): (Token: String)
	    * <strong>Requires</strong> valid user who is the owner and a valid Token
      * <strong>Effects</strong> creates new randomly generated and unique Token with optional expiration time and with the same permissions as the entered Token and deletes entered Token. Associates new Token with Owner
    + Delete (Token: String, owner: User,)
	     * <strong>Requires</strong> a valid Token and a valid user who is the owner
       * <strong>Effects</strong> deletes Token
