# fetchingdata

Second Project in Udemy react Net Ninja course

# Install Json sever package

- Open console
- npm install -g json-server
- create a json file to act as database
- create new folder called data
- new file called db.json
- Create json data  

```
{
  "trips": [
    {
      "title": "2 nights in Bognor Regis",
      "price": "£1,000"
    },
    {
      "title": "3 nights in Blackpool",
      "price": "£700"
    },
    {
      "title": "5 nights in Benidorm",
      "price": "£399"
    },
    {
      "title": "6 nights in Clacton",
      "price": "£99"
    }
  ]
}
```

- run Json Server
- cd into correct directory
- in console type: 
- json-server --watch ./data/db.json

This watches the file for changes

To fetch the data 

```
export default function TripList() {

fetch('http://localhost:3000/trips')
  .then(response => response.json())
  .then(json => console.log(json))

  return (
    <div>
      <h2>Trip List</h2>
    </div>
  )
}
```
## useEffect 

This will continuely fetch the data if you dont use useEffect, if you only want it to run once use an empty dependency []

```

export default function TripList() {
  //fetch data from the server
  const [trips, setTrips] = useState([]);

  useEffect(() => {
    fetch("http://localhost:3000/trips")
      .then((response) => response.json())
      .then((json) => setTrips(json));
  }, []);

  console.log(trips);

  return (
    <div className="trip-list">
      <h2>Trip List</h2>
      <ul>
        {trips.map((trip) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```
## Adding Dependencies

If you add a dependency, every time that changes it runs the function again and reloads the data on the screen. You can use this to filter information from the database so every time a user clicks a button, it changes the url and it fetches that data from the database and react will then show that information on the page

```
import { useState, useEffect } from "react";

// Styles
import "./TripList.css";

export default function TripList() {
  //fetch data from the server
  const [trips, setTrips] = useState([]);
  const [url, setUrl] = useState("http://localhost:3000/trips");

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((json) => setTrips(json));
  }, [url]);

  console.log(trips);

  return (
    <div className="trip-list">
      <h2>Trip List</h2>
      <ul>
        {trips.map((trip) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
      <div className="filters">
        <button onClick={() => setUrl("http://localhost:3000/trips")}>
          All Trips
        </button>
        <button
          onClick={() => setUrl("http://localhost:3000/trips?loc=europe")}
        >
          European Trips
        </button>
      </div>
    </div>
  );
}
```
## useCallback  

If you use a reference type like a function, array or objects as a dependency (when it changes) you need to useCallback hook for functions or useState for arrays and objects. 

```
import { useState, useEffect, useCallback } from "react";

// Styles
import "./TripList.css";

export default function TripList() {
  //fetch data from the server
  const [trips, setTrips] = useState([]);
  const [url, setUrl] = useState("http://localhost:3000/trips");

  const fetchTrips = useCallback(async () => {
    const response = await fetch(url);
    const json = await response.json();
    setTrips(json);
  }, [url]);

  useEffect(() => {
    fetchTrips();
  }, [fetchTrips]);

  console.log(trips);

  return (
    <div className="trip-list">
      <h2>Trip List</h2>
      <ul>
        {trips.map((trip) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
      <div className="filters">
        <button onClick={() => setUrl("http://localhost:3000/trips")}>
          All Trips
        </button>
        <button
          onClick={() => setUrl("http://localhost:3000/trips?loc=europe")}
        >
          European Trips
        </button>
      </div>
    </div>
  );
}
```
