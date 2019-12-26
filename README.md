# C# MongoDB Cheatsheet

## MongoDB
```c#
var client = new MongoClient();
var database = client.GetDatabase("Database");
var collection = database.GetCollection<Customer>("Customer");
```

## Filter
```c#
var dateFilter = FilterDefinition<Customer>.Empty;
dateFilter &= Builders<Customer>.Filter.Lte("CreateDate", DateTime.Now);
dateFilter &= Builders<Customer>.Filter.Gte("CreateDate", DateTime.Now.AddDays(-1));

var stateFilter = FilterDefinition<Customer>.Empty;
stateFilter |=  Builders<Customer>.Filter.Eq("State", "NY");
stateFilter |= Builders<Customer>.Filter.Eq("State", "AZ");

var filter = FilterDefinition<Customer>.Empty;
filter &= stateFilter;
filter &= dateFilter;
filter &= Builders<Customer>.Filter.Regex("Phone", "^[2-9]\\d{2}-\\d{3}-\\d{4}$");        
```

## Query Sync
```c#
var top20 = collection.Find(filter).Skip(0).Limit(20).ToList();
var count = collection.Find(filter).CountDocuments();
```

## Query Async
```c#
var top20 = await collection.Find(filter).Skip(0).Limit(20).ToListAsync();
var count = await collection.Find(filter).CountDocumentsAsync();
```
