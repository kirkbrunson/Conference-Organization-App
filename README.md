App Engine application for the Udacity training course.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup & Run Instructions
1. Clone [https://github.com/kirkbrunson/FSND-P4.git][8]
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.

Alternatively, see api explorer live [here][9].


## Design Decisions

### Sessions & Speakers:
Both Sessions and speakers are implemented as NDB kinds using the ndb.Model class. The attributes such as title, time, location, name, bio, and so on are represented as entities. This is done by defining the appropriate ndb property such as ndb.IntegerProperty() for the respective entity.   

Access to an entities, or instances of the kinds, is provided by using forms that are comprised of the messages.messageType  class. These forms provide an interface for inbound and outbound communication with the entities in the DB. 

By modeling information in this way, one can define the structure and relationships that their data has once and can reliably reuse these definitions across many instances.

### Wishlists:
Wishlists are implemented as an entity under the profile kind as a list of  sessionKeys. This implementation is similar to how conference registration is handled. Creating a separate kind for the wishlist is unnecessary and would waste space. 

## Queries:

Google App Engine handles queries by creating indexes for the kinds and entities defined. This is done in in the config file index.yaml. Basic usage is querying by kind, but one can also apply ancestor, property and inequality filters to the search.

### Additional Queries:
As I implemented session kind, it was only natural to add and implement queries for the sessions. Three specific ones that I implemented are as follows: query by session, filter by speakerName and typeOfSession;  query by session, filter by startTime and location; and query by session, filter by startTime and duration. A few use cases would be if a user needs to find sessions that are starting soon near them, or will fit into their schedule.

### Problem Query: [Problem && solution]
The problem query provided was to query for all non-workshop sessions before 7 pm. The problem with this is that there inequality filters applied to two different properties (typeOfSession & startTime) which is [prohibited by Datastore][7]. 

The solution I implemented was to split the filter into two queries and then merge there results. First, I query filtering by typeOfSession, and then I query filtering by startTime. Finally, I merge the two with the intersection of their results which is then returned to the client.



[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
[7]: https://cloud.google.com/appengine/docs/python/datastore/queries#Python_Restrictions_on_queries
[8]: https://github.com/kirkbrunson/FSND-P4.git
[9]: https://xw-boreal-airway-t.appspot.com/_ah/api/explorer
