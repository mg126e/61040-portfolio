Extending The Designs Questions

URL shortening services often provide analytics (see, for example, the offerings from [Bitly](https://bitly.com/pages/products/analytics) and [TinyURL](https://tinyurl.com/app/features/link-analytics)) in addition to the basic service, which allows users to see how short links are used. Your task is to extend our design to include this feature. Your analytics need only provide counts of the number of accesses (and not other metadata such as location). Analytics should not be public but should be viewable only by the user who registered the shortening.

1\.     Design a couple of additional concepts to realize this extension, and write them out in full (but including only the essential actions and state). It should *not* be necessary to make any changes to the existing concepts.

2\.     Specify three essential synchronizations with your new concepts: one that happens when shortenings are created; one when shortenings are translated to targets; and one when a user examines analytics.

3\.     As a way to assess the modularity of your solution, consider each of the following feature requests, to be included along with analytics. For each one, outline how it might be realized (eg, by changing or adding a concept or a sync), or argue that the feature would be undesirable and should not be included:

  1. Allowing users to choose their own short URLs;

     **To realize this, I would edit UrlShortening to give users the option of inputting their own suffix in the register action. I would indicate in the sync if users chose their own suffix or opted for a randomly generated Nonce. So, if the user chose their own suffix the NonceGeneration would not generate a new suffix and UrlShortening would instead use the users inputed suffix.**

  2. Using the “word as nonce” strategy to generate more memorable short URLs;

     **To realize this feature, I would edit NonceGeneration’s action generate. Generate would search a public dictionary and randomly choose a word that is not already used in that context.**
  
  4. Including the target URL in analytics, so that lookups of different short URLs can be grouped together when they refer to the same target URL;

     **I would replace shortUrl with targetUrl in my application of AnalyticsLogging. Therefore, in the registerAccess sync, recordAccess would take the targetUrl and incriminate the count associated with the targetUrl and not the ShortUrl. In this way all lookups with the same targetUrl will be grouped together.**

  5. Generate short URLs that are not easily guessed;

     **I believe this feature would be undesirable because the benefit of shortened Urls is that they are easier to remember and reference. The original Url is usually harder to guess than its shortened version because they are longer. Additionally because longer URLs are more secure than shorter URLs, I don’t believe this feature would align with the UrlShortening concept.**

6. Supporting reporting of analytics to creators of short URLs who have not registered as user.

   **I believe this feature would not be desirable because users could potentially access the analytics of other’s short URLs if user registration is not required. If it is open access there is a major security concern. On the other hand, if the user is recognized by their IP address, they would not be able to see their analytics if they switched devices. The best way to maintain security and user ease is to require users to register to view their short URLs analytics report.**

**sync registerUser**
  - **when** Request.registerUser ()
  - **then** PasswordAuthentication.register ()

**sync requestShortUrl**
  - **when**
    Request.shortenUrl ()
    PasswordAuthentication.authenticate () : (User)
  - **then**
    UrlShortening.register () : (shortUrl)
    AnalyticsLogging.initializeCount (Url: shortUrl, creator: User)
    
**sync registerAccess**
  - **when** UrlShortening.lookup (shortUrl) : ()
  - **then** AnalyticsLogging.recordAccess (shortUrl: Url)

**sync reportAnalytics**
  - **when**
    Request.analytics (shortUrl)
    PasswordAuthentication.authenticate () : (User)
  - **then**
    AnalyticsLogging.report (Url : shortUrl, Creator: User) : (count)


**Concept** AnalyticsLogging \[AccessLogs\]\
**Purpose** record and report the number of accesses to Urls\
**Principle** The count is  initialized, after which each successful lookup of the Url increments count by one. The creator of the Url can request its count.

**State**
  - a set of AccessLogs with
    - a count Number
    - a Url String
    - a creator User
  
**Actions**
  - report (Url: String, creator: User) : (count: Number)
    - **requires** Url exists in AccessLogs and user is the one associated with the AccessLog
    - **effect** returns count associated with the Url
  - recordAccess (Url: String)
    - **requires** Url exists in AccessLogs
    - **effect** increments Url’s count by one
  - initializeCount (Url: String, creator: User) : (AccessLog)
    - **require** Url not already in AccessLogs
    - **effect** create new AccessLog, initialize count to 0, and associates entered user as the creator


**Concept** PasswordAuthentication \[Users\]\
**Purpose** limit access to known Users\
**Principle** The user registers their password and username. Each username is associated with exactly one password. Authentication succeeds only when the provided username and password match the stored pair.

**State**
  - A set of Users with
    - A password String
    - An username String

**Actions**
  - register (password: String, username : String)  : (u: User)
    - **requires** a unique username
    - **effect** creates a User with the associated username and password
  - authenticate (password: String, username: String) : (u: User)
    - **requires** username and password must match internal record for user
    - **effects** if the user is authenticated, they will be treated each time as the same user associated with that username and password. Otherwise, fail


My implementation of AnalysticsLogging keeps track of the successful lookups of the shortUrl and not the original URL. To ensure the analytics of a shortUrl is only accessed by its creator, the user must authenticate themselves when they request the analytics.
