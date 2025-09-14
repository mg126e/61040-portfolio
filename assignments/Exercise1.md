
Questions
1.	Invariants. What are two invariants of the state? (Hint: one is about aggregation/counts of items, and one relates requests and purchases). Say which one is more important and why; identify the action whose design is most affected by it and say how it preserves it.
There are no duplicate requests on a Registry.
You can’t remove a request after it has been purchased.
<strong>I believe the second invariant is more important because it prevents requests from being removed after they have been purchased and associated with a set of Purchases.
In terms of the action that is most affected for the first invariant, the addItem preserves this invariant by checking if the item already exists, and if it does, just adding to the count instead of creating a duplicate new item.</strong>

2.	Fixing an action. Can you identify an action that potentially breaks this important invariant, and say how this might happen? How might this problem be fixed?
<strong>RemoveItem potentially breaks this important invariant because it could potentially remove an item that has already been purchased. This problem could be fixed by requiring RemoveItem to check if the item has been purchased and not allowing it to be removed if even the request has a count of two and only one has been purchased.</strong>

3.	Inferring behavior. The operational principle describes the typical scenario in which the registry is opened and eventually closed. But a concept specification often allows other scenarios. By reading the specs of the concept actions, say whether a registry can be opened and closed repeatedly. What is a reason to allow this?
<strong>A registry can be opened and closed repeatedly using the actions open and close. I think there are many scenarios where a celebration is repeated like a baby shower. The expecting-person could just open their previous registry again and add additional items they want now.</strong>

4.	Registry deletion. There is no action to delete a registry. Would this matter in practice?
<strong>No, I don’t believe this would matter in practice because you can close a registry and that effectively deletes it from the public view. This prevents also users from accidentally deleting their registry.</strong>

5.	Queries. What are two common queries likely to be executed against the concept state? (Hint: one is executed by a registry owner, and one by a giver of a gift.)
<strong>How many items on the registry have been purchased?
What items are still available to purchase?</strong>

6.	Hiding purchases. A common feature of gift registries is to allow the recipient to choose not to see purchases so that an element of surprise is retained. How would you augment the concept specification to support this?
<strong>I would add an additional flag the Registrys similar to active that allows the owner to hide if the requests have been purchased. I would have to add actions like hide and show that function similar to open and close that would hide the set of purchases from the owner.</strong>

7.	Generic types. The User and Item types are specified as generic parameters. The Item type might be populated by SKU codes, for example. Explain why this is preferable to representing items with their names, descriptions, prices, etc.
<strong>It is preferable to represent them as generic types because it does not clutter the structure and keeps the design focused on the greater concept of a gift registry.</strong>


