Mappit API
------------

Australian address validation and reverse geocoding made easy.


Authentication
--------------

After registering for an account, login to the `Dashboard`_ and generate an API key.
You can then use the generated API key in the ``X-Api-Key`` HTTP header on your `GraphQL`_ requests.

.. sourcecode:: http

  POST /graphql HTTP/2
  Host: api.mappit.app
  X-Api-Key: <your-api-key>
  Content-Type: application/json

Usage
-----

Search address
~~~~~~~~~~~~~~

You can search for an address using this query. This is built as a search-as-you-type query, and is also typo tolerant.

.. graphiql::
  :query:
    {
      searchAddress(query: "500 col"){
        gnafId
        displayAddress
        location{
          lat
          lon
        }
        components{
          buildingName
          number
          street
          suburb
          state
          postcode
        }
        score
      }
    }
  :response:
    {
      "data": {
        "searchAddress": [
          {
            "gnafId": "GAVIC424236450",
            "displayAddress": "500 Collins Street, Melbourne VIC 3000",
            "location": {
              "lat": -37.81784378,
              "lon": 144.95761462
            },
            "components": {
              "buildingName": "",
              "number": "500",
              "street": "Collins Street",
              "suburb": "Melbourne",
              "state": "VIC",
              "postcode": 3000
            },
            "score": 1
          }
        ]
      }
    }

You can also optionally filter the search results to a specific state by providing a value for the ``state`` parameter.

.. graphiql::
  :query:
    {
      searchAddress(query: "500 col", state: NSW){
        gnafId
        displayAddress
        location{
          lat
          lon
        }
        components{
          buildingName
          number
          street
          suburb
          state
          postcode
        }
        score
      }
    }
  :response:
    {
      "data": {
        "searchAddress": [
          {
            "gnafId": "GANSW716730214",
            "displayAddress": "500 Colemans Lane, Milvale NSW 2594",
            "location": {
              "lat": -34.25705103,
              "lon": 147.86797566
            },
            "components": {
              "buildingName": "",
              "number": "500",
              "street": "Colemans Lane",
              "suburb": "Milvale",
              "state": "NSW",
              "postcode": 2594
            },
            "score": 1
          }
        ]
      }
    }

The ``state`` parameter supports the following values: ``TAS, VIC, NSW, ACT, QLD, SA, NT, WA, OT``

Retrieve property
~~~~~~~~~~~~~~~~~

If you already know the gnaf ID of a property, you can use it to retrieve the details of that property.

.. graphiql::
  :query:
    {
      retrieveProperty(gnafId: "GAVIC424236450"){
        displayAddress
        location{
          lat
          lon
        }
      }
    }
  :response:
    {
      "data": {
        "retrieveProperty": {
          "displayAddress": "500 Collins Street, Melbourne VIC 3000",
          "location": {
            "lat": -37.81784378,
            "lon": 144.95761462
          }
        }
      }
    }

Reverse geocode
~~~~~~~~~~~~~~~

Given a latitude and longitude, returns the nearest addresses, sorted by distance.

.. graphiql::
  :query:
    {
      reverseGeocode(lat: -37.81784378, lon: 144.95761462){
        gnafId
        displayAddress
        location{
          lat
          lon
        }
        components{
          buildingName
          number
          street
          suburb
          state
          postcode
        }
        score
      }
    }
  :response:
    {
      "data": {
        "reverseGeocode": [
          {
            "gnafId": "GAVIC424875741",
            "displayAddress": "5RA/500 Collins Street, Melbourne VIC 3000",
            "location": {
              "lat": -37.81784378,
              "lon": 144.95761462
            },
            "components": {
              "buildingName": "",
              "number": "5RA/500",
              "street": "Collins Street",
              "suburb": "Melbourne",
              "state": "VIC",
              "postcode": 3000
            },
            "score": 1
          },
          {
            "gnafId": "GAVIC424236450",
            "displayAddress": "500 Collins Street, Melbourne VIC 3000",
            "location": {
              "lat": -37.81784378,
              "lon": 144.95761462
            },
            "components": {
              "buildingName": "",
              "number": "500",
              "street": "Collins Street",
              "suburb": "Melbourne",
              "state": "VIC",
              "postcode": 3000
            },
            "score": 1
          },
          {
            "gnafId": "GAVIC424770682",
            "displayAddress": "2/500 Collins Street, Melbourne VIC 3000",
            "location": {
              "lat": -37.81784378,
              "lon": 144.95761462
            },
            "components": {
              "buildingName": "",
              "number": "2/500",
              "street": "Collins Street",
              "suburb": "Melbourne",
              "state": "VIC",
              "postcode": 3000
            },
            "score": 1
          },
          {
            "gnafId": "GAVIC719244691",
            "displayAddress": "4/500 Collins Street, Melbourne VIC 3000",
            "location": {
              "lat": -37.81784378,
              "lon": 144.95761462
            },
            "components": {
              "buildingName": "",
              "number": "4/500",
              "street": "Collins Street",
              "suburb": "Melbourne",
              "state": "VIC",
              "postcode": 3000
            },
            "score": 1
          }
        ]
      }
    }

You can also optionally adjust the tolerance of the search and filter the results further using a term with optional ``tolerance`` and ``query`` parameters on the query.

- Using the ``tolerance`` parameter will be useful to reduce the number of properties when you're searching in a dense area. Tolerance defaults to ``100`` when not provided.
- Using the ``query`` parameter will be useful for searching against properties which might have the same latitude and longitude (Eg. apartments in a apartment building)

.. graphiql::
  :query:
    {
      reverseGeocode(lat: -37.81784378, lon: 144.95761462, query: "5RA", tolerance: 10){
        gnafId
        displayAddress
        location{
          lat
          lon
        }
        components{
          buildingName
          number
          street
          suburb
          state
          postcode
        }
        score
      }
    }
  :response:
    {
      "data": {
        "reverseGeocode": [
          {
            "gnafId": "GAVIC424875741",
            "displayAddress": "5RA/500 Collins Street, Melbourne VIC 3000",
            "location": {
              "lat": -37.81784378,
              "lon": 144.95761462
            },
            "components": {
              "buildingName": "",
              "number": "5RA/500",
              "street": "Collins Street",
              "suburb": "Melbourne",
              "state": "VIC",
              "postcode": 3000
            },
            "score": 1
          }
        ]
      }
    }
