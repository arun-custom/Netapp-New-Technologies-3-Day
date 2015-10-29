# MongoDB

## Introduction to MongoDB
- MongoDB is a NoSQL storage solution that is meant to store unstructured, non-relational data.
- Use cases for this may include chat messages, task queues (think cron jobs), and application logging for analytics.
- In MongoDB, you don't create a database, add tables to it, define relationships, and then query it.
- Instead, data is broken down into Collections, which are similar to the concept of tables, and Documents, which are similar to a row in a relational database.
- Contrary to relational databases, documents within a collection don't have to have the same fields. You can add a field to some documents in a collection without adding that field to all documents in the collection.
- Schemas are also flexible, which means that the structure of your database is not fixed.

##### Let's think of a structure for storing information about users in our database. Let's say that every user generally has these attributes:
- Name
- Username
- Email

##### Here is how we could represent users via JSON:

```javascript
var users = [
	{
		name: "Arun Sood",
		username: "arsood",
		email: "arsood@gmail.com"
	},
	{
		name: "John Smith",
		username: "jsmith",
		email: "john@smith.com"
	},
	{
		name: "Susan Mills",
		username: "smills",
		email: "smills@yahoo.com",
		address: "225 Bush Street, San Francisco, CA",
		gender: "Female",
		age: 34
	}
];
```

- As you can see, some users can have different fields in this model. There is no fixed schema.

## MongoDB Query Interface
- MongoDB has a rich set of tools that allow us to query collections to add data or find data.

##### Open Mongo interface

```
mongo
```

##### Create database

```
use users_db
```

##### Add record to collection
- Note: Collections are generated upon the first insertion.

```javascript
db.users.insert({
	name: "Arun Sood",
	username: "arsood",
	email: "arsood@gmail.com"
})
```

##### Find record in collection

By specific field:

```javascript
db.users.find({
	name: "Arun Sood"
})
```

By automatically generated ID:

```javascript
db.users.find({
	"_id": ObjectId("4ef224bf0fec2806da6e9b29")
})
```

##### Update record in collection

```javascript
db.users.update({
	name: "Arun Sood"
}, {
	$set: {
		email: "arun.instructor@gmail.com"
	}
})
```

##### Delete record in collection

```javascript
db.users.remove({
	name: "Arun Sood"
})
```

## Mongo Lab 1
- In this lab we will be practicing setting up a collection and querying it.
- You will need to follow these steps:
	- Step 1: Set up a new collection called "inventory" with the following attributes:
		- Product Name
		- Description
		- Quantity
		- Price
	- Step 2: Insert a few documents into the collection.
	- Step 3: Try a few different methods to query for the documents.
	- Step 4: Update a record.
	- Step 5: Remove a record.

## Introduction to Mongoose
- Mongoose is a piece of software known as an Object Document Mapper (ODM).
- This is similar to ORMs used in relational databases.
- Mongoose enables us to set up our collections and documents in an object-oriented approach.
- Each Mongoose-enabled application will have a schema:

##### models/user.js

```javascript
//Import Mongoose module
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

mongoose.connect('mongodb://127.0.0.1/myappdatabase');

//Create the schema
var userSchema = new Schema({
	name: String,
	username: { type: String, required: true, unique: true },
	email: { type: String, required: true }
});

//Set up schema as model
var User = mongoose.model('User', userSchema);

//Optional: Export user schema as module
module.exports = User;
```

## Querying Using Mongoose
- Once we have set up a schema we can import it as a model and run queries with it.

##### app.js

```javascript
//Import user model
var User = require("./models/user");

var arun = new User({
	name: "Arun Sood",
	username: "arsood",
	email: "arsood@gmail.com"
});

arun.save(function(err) {
	console.log("User saved successfully");
});
```

##### Find all

```javascript
User.find({}, function(err, users) {
	console.log(users);
});
```

##### Find one

```javascript
User.find({ name: "Arun Sood" }, function(err, user) {
	console.log(user);
});
```

##### Find by ID

```javascript
User.findById(1, function(err, user) {
	console.log(user);
});
```

##### Update

```javascript
User.findById(1, function(err, user) {
	user.name = "George Simmons";
	
	user.save(function(err) {
		console.log("Update complete!");
	});
});
```

##### Delete

```javascript
User.findById(1, function(err, user) {

	user.remove(function(err) {
		console.log("Remove complete!");
	});
	
});
```

## Mongoose Lab
- In this lab we will be putting these concepts together with your learnings on HTML.
- We will be creating an inventory management system using MongoDB and Mongoose.
- Here are the steps you will have to follow:
	- Step 1: Set up a new node application that has three routes - show products, add a product, and edit product.
	- Step 2: Create a form that allows users to enter a new product.
	- Step 3: Set up a MongoDB database with Mongoose to save the product information into a collection.
	- Step 4: Create the functionality to update and delete a specific product.
	- **Bonus:** Use Bootstrap to make the UI look professional.