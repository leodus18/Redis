# Redis

## Tasks

- Leonis: Java + Redis connection

## Time Log

| Date       | Time            | Duration | Name     | Description                  |
|------------|-----------------|----------|----------|------------------------------|
| 2025-04-01 | 14:00 - 14:10   | 10 min   | Leonis   | Created Upstash account      |
| 2025-04-01 | 14:10 - 14:20   | 10 min   | Leonis   | Create Redis database        |
| 2025-04-01 | 14:40 - 15:20   | 45 min   | Leonis   | Connection with Java and DataGrip        |
| 2025-04-08 | 14:05 - 14:20   | 15 min   | Leonis   | Using Mockaroo for random Data Generation     |
| 2025-04-08 | 14:25 - 14:40   | 15 min   | Leonis   | Inserted all data in Redis     |
| 2025-04-08 | 14:40 - 14:55   | 15 min   | Leonis   | Search in Java to find the data record using the key(uuid)     |
| 2025-04-08 | 14:55 - 15:02   | 7 min   | Leonis   | Added: Measuring the time of the output how long it took to search     |
| 2025-04-08 | 15:02 - 15:30   | 28 min   | Leonis   | Search in Java to find the data record using FirstName     |
| 2025-04-14 | 10:25 - 10:30   | 5 min   | Leonis   | Added: Measuring the time of the output how long it took to search    |
| 2025-04-14 | 10:40 - 10:47   | 7 min   | Leonis   | Testing how long it took the search to finish    |
| 2025-04-14 | 10:50 - 11:00   | 10 min   | Leonis   | Documenting the Output    |



Final Output:
I ran 2 searches, one of them being with the uuid, and the other one being with the FirstName.
I also meassured the time of both of these searches to compare them at the end.

Search with uuid:

I put the uuid of a user in the java so it gets searched. It searched the uuid and it didnt take long to find as u can see here it took only 32 ms. Also knowing uuid is a special id for every user it found the excat user thats being search.
Data found for key 00158964-05cb-48dc-8a09-c808954f3943: {"firstName":"Aldus", "lastName":"Sandey", "age":71, "country":"China"}
Search time 31 ms.

Search with FirstName:

I put the users FirstName in the java so it gets searched. It searched the FirstName going 1 by 1 of each user till it found the matching name. Also knowing the 1 or more users can have the same it will give the output of all the people with that name.
As you can see here it took a long time more than the uuid search because it had to go thru all the users 1 by 1 checking thier FirstName to see if it matches the name that its searching for. It took 352402 ms and found 2 users with that FirstName.
First name = "Osgood"...
Match found. Key: 00190095-34d8-4ebe-be06-f195a4e5ea76 = {"firstName":"Osgood", "lastName":"Crowley", "age":58, "country":"China"}
Match found. Key: 25322c80-2376-40c9-844f-620d08311936 = {"firstName":"Osgood", "lastName":"Baunton", "age":37, "country":"Thailand"}
Found 2 matching data.
Search took 352402 ms.





