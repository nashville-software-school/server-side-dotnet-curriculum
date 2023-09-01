# Handling Walker Cities in Deshawn's Dog Walking
In DeShawn's Dog Walking, a city can have many walkers in it, and a walker can walk in many cities. Therefore, the relationship between walker and city is _many-to-many_. You have covered this idea before in the first iteration of Deshawn's, but the .NET version will cause a few new wrinkles, which we will cover here. 

## Defining the data types
Of course, you will have a `Walker` and a `City` class. To connect a many-to-many relationship, you will also need a joining entity (you'll here these called join tables, bridge tables, or junction tables), which we'll call `WalkerCity` that has a `CityId` and a `WalkerId`.  The `Walker` class should have a `Cities` property where we can store cities for a walker before sending the data back to the client (think `_embed` in JSON Server). Similarly, the `City` class could have a `Walkers` property. But how do we query the database to populate those properties?

## Getting Cities for a Walker
Assume that we have `walkers` `cities`, and `walkerCities` collections holding our data. Getting a single walker looks like this:

``` csharp
int id = 1;
Walker walker = walkers.FirstOrDefault(w => w.Id == id);
```
If we want to get the walker's cities, we need to do that in two steps:
```csharp
int id = 1;
List<WalkerCity> walkerCitiesForWalker1 = walkerCities.Where(wc => wc.WalkerId == 1).ToList();

List<City> citiesFor1 = walkerCitiesForWalker1.Select(wc => cities.First(c => c.Id = wc.CityId)).ToList();
```
The second step returns a city that matches the `CityId` of each `WalkerCity` in the `List` stored in `walkerCitiesForWalker1` collection. 

Putting it all together, we can finally add the cities to the `Walker` object stored in the `walker` variable:
``` csharp
walker.Cities = citiesFor1; 
```

## Updating cities for a walker
Updating cities associated with a walker are a bit more complicated. Let's say the client send a `walker` object to the server to do a `PUT` operation, with a list of `Cities` that represent the correct list of cities that the walker walks in. 

Our first task is to remove the current `WalkerCity` items associated with the walker:
``` csharp
walkersCities =  walkerCities.Where(wc => wc.WalkerId != walker.Id).ToList();
```
Then, we add new `WalkerCity` items for each of the cities in the `Walker` object sent to the server from the client:
``` csharp
foreach (City city in walker.Cities)
{
    WalkerCity newWC = new WalkerCity
    {
        WalkerId = walker.Id,
        CityId = city.Id
    };
    newWC.Id = walkerCities.Count > 0 ?walkerCities.Max(wc => wc.Id) + 1 : 1;
    walkerCities.Add(newWC);
}
```
The endpoint that implements this logic functions as the Create, Update, and Delete functionality for walker cities, because the client should send the entire list of correct cities every time the walker is updated (which will either be larger, smaller, or the same size as the previous list of cities for the walker).