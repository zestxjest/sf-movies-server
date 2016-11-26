# sf-movies-server
This is only for the server side and the client side repository is [here](https://github.com/zestxjest/sf-movies).

I don't have much real large application experience of nodejs from my work in HPE. I learn nodejs by some open source projects and I also have some open source projects for learnning and practicing.

## Requirement
Create a service that shows on a map where movies have been filmed in San Francisco. The
user should be able to filter the view using autocompletion search.

## Business Analysis
1. Need to add the pagination so that not all of the data will be shown up on the map.
2. The user can query the data by title, case-insensitive.
3. The client side can get the record by the specific id.
4. The client side can delete the record by the specific id.
5. The client side can create a record.
6. The client side can update a record.

## Technical Design
### Programming Language
* Javascript

### Environment
* Ubuntu

### Backend Tech 
* Node
* Express
* Restful API

### Database
* Rethinkdb 

### Unit Test 
* Mocha
* Chai

### SIT
* POSTMAN

### Project Structure
**DAL/movie.js**: Data access for the Movie store, it supports the CRUD.

**routes/movie.js**: Restful API for the Movie store, it support GET, POST, PUT, DELETE.

**test/movie.DAL.js**: The test specification for the DAL/movie.js.

**others**: A little modification or generated by the express generator.

### Run and Test
#### Run:
```
// Import the data to rethinkdb:
➜  sf-movies-server git:(master) ✗ rethinkdb import -f data-export.csv --table test.Movie --pkey id --format csv

// Start the rethinkdb.
➜  sf-movies-server git:(master) ✗ rethinkdb

// Install all the dependencies.
sf-movies-server git:(master) ✗ npm install

// Then start the node server, it will listen to port 3001.
➜  sf-movies-server git:(master) ✗ npm start
```

#### Test
```
➜  sf-movies-server git:(master) ✗ npm test 
> sf-movies-server@0.0.0 test /home/dean/CodeRepos/sf-movies-server
> mocha


Creating a pool connected to localhost:28015
Creating a pool connected to localhost:28015

// Result:
  DAL/movie.js
    #list()
      ✓ should get the first 50 records
      ✓ should get 50 records from the 3rd page
      ✓ should get the records which the title start with Need
    #get()
      ✓ should get one record that id is 04bce074-eba3-4ef1-ab9a-c13871a87e0f
    #create()
      ✓ should create one record with the specific title
    #update()
      ✓ should update one record with the specific value
    #delete()
      ✓ should delete one record with the specific id


  7 passing (266ms)
```

## Issues during development even rethinkdb was installed
### Cannot import the csv data to rethinkdb
> Solution: Need to install the python rethinkdb driver.
```
➜  sf-movies-server git:(master) ✗ pip3 install rethinkdb
```

### The original data doesn't have the coordinate information
> Solution: Call Google API after get the movies from the database and then return them back to the client. Note: Google API has the request limitation.