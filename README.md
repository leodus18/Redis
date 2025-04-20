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






Code:
package org.example;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.params.ScanParams;
import redis.clients.jedis.resps.ScanResult;

import java.io.BufferedReader;
import java.io.FileReader;
import java.util.UUID;

public class Main {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("robust-cat-52911.upstash.io", 6379, true);
        jedis.auth("Ac6vAAIjcDE0MDBkNzA2NWIxNTc0ZDc3YjllYWRkNTE4NDc5Mzk4YXAxMA");

        try (BufferedReader br = new BufferedReader(new FileReader(""))) {
            String line;
            br.readLine(); // skips header (column titles) first line

            int count = 0;

            while ((line = br.readLine()) != null) {  // loop through each line
                String[] parts = line.split(",");

                if (parts.length >= 4) {
                    String firstName = parts[0];
                    String lastName = parts[1];
                    String age = parts[2];
                    String country = parts[3];

                    String uuid = UUID.randomUUID().toString();

                    String json = String.format(
                            "{\"firstName\":\"%s\", \"lastName\":\"%s\", \"age\":%s, \"country\":\"%s\"}",
                            firstName, lastName, age, country
                    );

                    jedis.set(uuid, json);
                    count++;
                }
            }

            System.out.println(count + " records written to Redis.");
        } catch (Exception e) {
            e.printStackTrace();
        }

        String searchKey = "00158964-05cb-48dc-8a09-c808954f3943";

        long startTime = System.nanoTime();
        String result = jedis.get(searchKey);
        long endTime = System.nanoTime();
        long durationInMs = (endTime - startTime) / 1_000_000;

        if (result != null) {
            System.out.println("Data found for key " + searchKey + ": " + result);
        } else {
            System.out.println("No data found for key: " + searchKey);
        }

        System.out.println("Search time " + durationInMs + " ms.");



        String targetFirstName = "Osgood";
        System.out.println("\n First name = \"" + targetFirstName + "\"...");

        long start = System.nanoTime();
        int matchCount = 0;

        String cursor = ScanParams.SCAN_POINTER_START; // (0) beginning of the key list
        ScanParams scanParams = new ScanParams().match("*").count(100); // all keys, 100 per scan

        //fetches some keys from Redis.
        do {
            ScanResult<String> scanResult = jedis.scan(cursor, scanParams);

            // For every key, get the value
            for (String key : scanResult.getResult()) {
                String value = jedis.get(key);
                if (value != null && value.contains("\"firstName\":\"" + targetFirstName + "\"")) {
                    System.out.println("Match found. Key: " + key + " = " + value);
                    matchCount++;
                }
            }

            // Loop continues until the cursor says “0" (end)
            cursor = scanResult.getCursor();
        } while (!cursor.equals("0"));


        long end = System.nanoTime();
        long duration = (end - start) / 1_000_000;

        System.out.println("Found " + matchCount + " matching data.");
        System.out.println("Search took " + duration + " ms.");

    }
}




