Synchronization Questions

1. **Partial matching**. In the first sync (called *generate*), the *Request.shortenUrl* action in the when clause includes the shortUrlBase argument but not the targetUrl argument. In the second sync (called register) both appear. Why is this?  

   **TargetURL is not included in the first sync because that information is not relevant to generate, the action only syncs with shortURLBase.**
   
2. **Omitting names**. The convention that allows names to be omitted when argument or result names are the same as their variable names is convenient and allows for a more succinct specification. Why isn’t this convention used in every case?  

    **This convention isn’t used when the argument/result names are different than their variable names because it creates ambiguity. Explicit naming helps identify how arguments are bound to variables.**  

3. **Inclusion of request**. Why is the *request* action included in the first two syncs but not the third one?  

    **It is not included in the third sync because the setExpiry action is triggered by the system.  In contrast, the first two syncs are requested through user interaction.**  

4. **Fixed domain**. Suppose the application did not support alternative domain names, and always used a fixed one such as “bit.ly.” How would you change the synchronizations to implement this?  

    **I would change the register and generate syncs to not take shortUrlBase as there is just one domain under bit.ly. The register and generate syncs would also just pass one context that all shortUrlSuffix’s would be associated with.**  

5. **Adding a sync**. These synchronizations are not complete; in particular, they don’t do anything when a resource expires. Write a sync for this case, using appropriate actions from the *ExpiringResource* and *URLShortening* concepts.  


**sync** generate
  - **when** Request.shortenUrl (shortUrlBase)
  - **then** NonceGeneration.generate (context: shortUrlBase)  

**sync** register
  - **when**  
      Request.shortenUrl (targetUrl, shortUrlBase)  
      NonceGeneration.generate (): (nonce)
  - **then** UrlShortening.register (shortUrlSuffix: nonce, shortUrlBase, targetUrl)

**sync** setExpiry  
  - **when** UrlShortening.register (): (shortUrl)
  - **then** ExpiringResource.setExpiry (resource: shortUrl, seconds: 3600\)

**sync expireResource**
  - **when** ExpiringResource.system ()
  - **then**
      ExpiringResource.expireResource (resource): (resource: shortUrl)
      UrlShortening.delete (shortUrl: resource)

