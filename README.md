# Packing packages in trucks

[![CircleCI](https://circleci.com/gh/scottfrasso/packingtrucks/tree/master.svg?style=shield&circle-token=723c82399fce2303c1d0aff18a8e4d9ad0b0ca63](https://circleci.com/gh/scottfrasso/packingtrucks/tree/master)

This is a code test I was assigned. The test consists of putting packages
in trucks, using the minimum number of trucks, and assigning a price to the
manifest. This was done in express and exposes 1 public endpoint which takes
in a list of packages and returns a manifest with packages assigned to trucks.

## The original assignment
A logistics company wants to put packages from a customers warehouse onto trucks. The only way to measure the packages is by their mass in kilograms (maximum 500kg). A trucks maximum load is 1000kg.
Make an app that will say how many trucks are needed to move the clients cargo using as few trucks as possible, and calculate the price.

An example API Request:
````json
[
  { "id": "ID-1", "weight": 345 },
  { "id": "OTHER-ID-2", "weight": 500 },
  { "id": "CLIENT-ID-3", "weight": 300 },
]
````

The corresponding reply would be:
````json
{
    "price": 10.95,
    "trucks": [{
            "truckID": "unique truck ID",
            "load": [{
                    "id": "ID-1",
                    "weight": 345
                },
                {
                    "id": "OTHER-ID-2",
                    "weight": 500
                }
            ]
        },
        {
            "truckID": "other unique truck ID",
            "load": [{
                "id": "CLIENT-ID-3",
                "weight": 300
            }]
        }
    ]
}
````

The price list:

Up to 400kg: 0.01 x weight

Over 400kg: 2 + 0.005 x weight

## To run the server
I'll assume you have Node 10 and Homebrew installed, that's what I used to write this.

Install sqlite using [Homebrew](https://brew.sh/)
````
brew install sqlite3
````

Just run npm install and it'll start the server.
````
yarn install
````

Then start the server
````
yarn run start
````

Then make a request to the server
````
curl -H "Content-type: application/json" -d '[
  { "id": "ID-1", "weight": 345 },
  { "id": "OTHER-ID-2", "weight": 500 },
  { "id": "CLIENT-ID-3", "weight": 300 }
]' 'http://localhost:8080/api/package'
````

The result should look something like this
````json
{
  "price": 10.95,
  "trucks": [
    {
      "truckID": "90a676a4-77d7-4b9a-bcd1-3ae2863720ad",
      "load": [
        {
          "id": "OTHER-ID-2",
          "weight": 500
        },
        {
          "id": "ID-1",
          "weight": 345
        }
      ]
    },
    {
      "truckID": "790cbfe5-1da1-4845-8852-dcb544b7343c",
      "load": [
        {
          "id": "CLIENT-ID-3",
          "weight": 300
        }
      ]
    }
  ]
}
````

## To run the tests
Again you need Node 10 and Homebrew installed for this.

Install sqlite using [Homebrew](https://brew.sh/)
````
brew install sqlite3
````

Run the npm install
````
yarn install
````

Then just run the tests
````
yarn run test
````

## What's left to do?

If I spent more time on this I'd probably do some input validation
on the API. I'd also do something to stop SQL injection attacks by scrubbing
the input, the sequelize orm might already handle this. If this were
actually a product I'd write some more unit tests as well.
