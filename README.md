# Indexing 101
## What is it ([ELI5](http://www.internetslang.com/ELI5-meaning-definition.asp))?
Indexing allows you to store data and documents in Haven OnDemand, and then perform search, find similar documents, or find related concepts throughout this index.

Piggybacking off of the search functionality, to give you a clearer picture of what this is and how it is useful - think of it like how Google works. Google "indexed" all of the internet, and now you can search through it. You can do the exact same thing with our APIs, only now you can use (i.e. index) whatever data you wish to.

This short how to guide briefly explains how to implement indexing with Haven OnDemand; so, don't think this is the tell all, end all of indexing!

## Steps
#### 1. What software will you be using?
We offer a client library for almost every major software language. Check out the list below for your language:
* [Ruby](https://github.com/HP-Haven-OnDemand/havenondemand-ruby)
* [Python](https://github.com/HP-Haven-OnDemand/havenondemand-python)
* [Node.js](https://github.com/HP-Haven-OnDemand/havenondemand-node)
* [PHP](https://github.com/HP-Haven-OnDemand/havenondemand-php)
* [Android](https://github.com/HP-Haven-OnDemand/havenondemand-android)
* [Windows Mobile](https://github.com/HP-Haven-OnDemand/havenondemand-windows-8.1-universal)
* [iOS Swift](https://github.com/HP-Haven-OnDemand/havenondemand-ios-swift)

#### 2. Create an index
*See [here](https://dev.havenondemand.com/apis/createtextindex#overview) for the Create Text Index API documentation and full overview.*

First, you need to create an index to be able to store all of your data in. Without this, where are you going to put everything!?

URL endpoint: `http://api.havenondemand.com/1/api/[sync|async]/createtextindex/v1`

This API has 2 required parameters:
*  The name of the index
* The flavor
  *   Typically, just use `standard`

Optionally, you can add:
  * A description
    * Makes it a bit easier to know which indexes are what. I'd recommend adding one

#### 3. Add data to the index
*See [here](https://dev.havenondemand.com/apis/addtotextindex#overview) for the Add Text to Index API documentation and full overview.*

Now that we have an index created, let's add some data to it, so afterwards, we can search through it and perform other cool features on it.

URL endpoint: `http://api.havenondemand.com/1/api/[sync|async]/addtotextindex/v1`

This API can take a few different inputs for the data:
* A document file
* JSON object
* Reference (i.e. something you've already stored in Haven OnDemand in the [Store Object API](https://dev.havenondemand.com/apis/storeobject#overview))
* URL (i.e. something from the internet)

Three of these are pretty easy to use and don't have much customization to them, so we're just going to focus on the JSON object input, although you're welcome to use whichever input type wish :P

This API takes 2 required parameters:
* JSON object (remember, you can use the other inputs listed above too!)
  * This comes in the following form:
  ```
  {
   "document" :
   [
      {
         "title" : "This is my document",
         "reference" : "mydoc1",
         "myfield" : ["a value"],
         "content" : "This content is dope."
      }, {
         "title" : "My Other document",
         "reference" : "mydoc2",
         "content" : "This content is sweet."
      }
   ]
}
```
    * `document` can take multiple documents at once (i.e it's an array of objects)
    * `content` is the ["meat and potatoes"](http://www.urbandictionary.com/define.php?term=Meat+and+Potatoes) of the data you're storing, so it's what will be searchable when you perform search on this index
    * `myfield` allows you to customize your search more by adding additional fields. See [here](https://dev.havenondemand.com/apis/addtotextindex#overview) for predefined field names you should use to best optimize our search functionality
* Index you're adding the data to (i.e. the one you created in step 2)

**Note: you can also scrap websites and have the data obtained stored in an index through our [Create Connector API](https://dev.havenondemand.com/apis/createconnector#overview) and [Start Connector API](https://dev.havenondemand.com/apis/startconnector#overview)**
#### 4. Search through it
*See [here](https://dev.havenondemand.com/apis/querytextindex#overview) for Query Text Index API documentation and full overview.*

We have all of our data in our index, so let's make some use of it by searching through it!

URL endpoint: `http://api.havenondemand.com/1/api/[sync|async]/querytextindex/v1`

This API can take a few different inputs for the data which are required to use it, but we're just going to focus on the `text` one which is like using a search engine.

There are also a lot of optional parameters which enhance the search feature, but we won't focus on those here.

Simply pass in the `text` you wish to search through in all of you documents you indexed, and out will come any documents matching the text you passed.
