Concept Questions

1. **Contexts**. The *NonceGeneration* concept ensures that the short strings it generates will be unique and not result in conflicts. What are the contexts for, and what will a context end up being in the URL shortening app?
   
   **An URL Shortening concept can benefit from multiple Contexts because each Nonce String only has to be unique in its Context, not globally. This allows users to use the same Nonce in different Contexts without conflicts.**
   
   **I believe each shortUrlBase corresponds to one Context, treating Contexts similar to Domains so each Nonce String is unique within its shortUrlBase.**
   
2. **Storing used strings**. Why must the *NonceGeneration* store sets of used strings? One simple way to implement the *NonceGeneration* is to maintain a counter for each context and increment it every time the generate action is called. In this case, how is the set of used strings in the specification related to the counter in the implementation? (In abstract data type lingo, this is asking you to describe an *abstraction function*.)
 
   **The NonceGeneration must store sets of used Strings to keep track of Strings already used in that Context**. **The set of used Strings would be mapped to the counter in the implementation. The abstraction function maps the counter “k” to the abstract set of used strings from 0 to k-1 incrementing by one every time NonceGeneration is called.**  

3. **Words as nonces**. One option for nonce generation is to use common dictionary words (in the style of [yellkey.com](http://yellkey.com/), for example) resulting in more easily remembered shortenings. What is one advantage and one disadvantage of this scheme, both from the perspective of the user? How would you modify the *NonceGeneration* concept to realize this idea?\

   **One advantage for users is that dictionary words are more memorable, however this could lead to more collisions as there are less common dictionary words than randomly generated combinations of letters and numbers. Therefore, the user could run out of unique words to use in their context. Random generations of common dictionary words could also lead to offensive pairings of Nonces and URLs that is avoided with random nonsense generations of letters and numbers.**  

   **I would modify the Purpose, Principle, State, and generate action in the NonceGeneration concept to replace the word “string(s)” with “word(s)”. I would also clarify that generate would draw on a public dictionary to produce words that are not already in the Context’s used set of words.**  
