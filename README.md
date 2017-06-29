# Fish
Project F.I.S.H: The Fictional Infection Simulation Host

## Getting Started

Compile the all the fish java files and generate some seeds for reproducible randomness. 
Afterwards you should get an text file with the amount you entered of random doubles.
``` 
$ javac /PATH/TO/FISH/DIRECTORY/*.java

$ java fish.Helper seed FILENAME.txt COUNT

Ex, while at the directory containing the fish folder
$ java fish.Helper seed seed.txt 10000
```

To use the fish package, create your own java file(s) outside of the fish directory, check out the [Main.java](https://github.com/illinoistechesi/fish/blob/master/Main.java) in the fish directory as an example. 

``` java
package fish;

public class Main {
    
    public static void main(String[] args) {
        /* ...Your implementation of an bacteria simulation... */
    }
}
```

**Infection Timeline**
![Timeline](https://github.com/GeliClub/fish/raw/master/Gantt%20Chart.png)


## Documentation

**Java refersher**<br>

Static/Class methods - Can be called anywhere given you have inputs for its parameters
    
Instance/Object Methods - Called only if you have an instance of the object


**Table of Contents**<br>

**[AgeGroup](#agegroup)**<br>
**[Person](#person)**<br>
**[City](#city)**<br>
**[Location](#location)** - Extendable <br>
**[Routine](#routine)** - Extendable <br>
**[Pathogen](#pathogen)** - Extendable <br>
**[Helper](#helper)** <br>

### AgeGroup

Age group are used to define a Person class

``` java
// AgeGroup Enum
AgeGroup.CHILD
AgeGroup.TEEN
AgeGroup.ADULT
AgeGroup.ELDER
```

### Person

**Subclasses/Enum**
``` java
// State Enum
State.SUSCEPTIBLE
State.INFECTED
State.RESISTANT
```

**Static Methods**
``` java
/*
* parameters    - List of people
* returns       - List of contagious/infected people
*/
public static List<Person> getContagious(List<Person> people)

/*
* parameters    - List of people
* returns       - List of people that can be infected, non-resistance to bacteria
*/
public static List<Person> getVulnerable(List<Person> people)

/*
* parameters    - List of people
* returns       - HashMap with Key/Value entries, each key is a location with a list of people in that location as its value
*/
public static HashMap<Location, List<Person>> groupPeopleByLocation(List<Person> people)

/*
* parameters    - List of people
* returns       - HashMap with key/value entries, each key is a infection state with a list of people in that state as its value
*/
public static HashMap<State, List<Person>> groupPeopleByState(List<Person> people)
```

**Instance Methods**
``` java
/*
* Details        - Will exposured this person to the pathogen input, exposure can be used in interactions with infected persons (uses the seeded random for reproducible results)
*/
public void doExposure(Pathogen pathogen)

/*
* Details       - Similar to doExposure, but the possibility of getting infected by the given pathogen input is guaranteed
*/
public void doInfect(Pathogen pathogen)

/*
* details       - A person's state do not change to INFECTED until symptom are shown or feelsSick is true (similar to isIncubated, but used in interactions)
*/
public boolean feelsSick()

/*
* details       - When bacteria in the host has passed the latent stage (similar to isLatent, but used in interactions)
*/
public boolean isContagious()

/*
* details       - When a person is not infected and not resistant to bacteria (resistant is present only after going through the infection stages)
*/
public boolean isVulnerable()

/*
* details       - The possibility of getting infected when exposured to the bacteria (uses the seeded random for reproducible results)
*/
public double getSusceptibility()

/*
* details       - Once the bacteria is incubated the symptom will start to show and immune system response will increase (similar to feelsSick, but used to describe the immune system)
*/
public boolean isIncubated()

/*
* details       - When the bacteria goes exposured to the start of incubtion (similar to isContagious but used to describe the immune system)
*/
public boolean isLatent()

/*
* returns       - Integer time since exposured in hours, calculated value
*/
public int getTimeSinceExposure()
```

**Setters/Getters**
``` java
// Getters/Accessor Methods

public int getResponse()

public int getBacteria()

public Pathogen getPathogen()

public AgeGroup getAgeGroup()

public Routine getRoutine()

public Location getLocation()

public State getState()

public List<Record> getHistory()

// Setters/Mutator Methods

public void setRoutine(Routine routine)

public void setLocation(Location loc)

public void setState(State state)

```

### City

**Instance Methods**
``` java
/*
* Details       - Complete an move and increments a turn for everyone in the city
*/
public void doTurn()

/*
* parameters    - Takes a Location object and adds it to a list of locations
*/
public void addLocation(Location loc)

/*
* parameters    - Takes a Person object and adds it to a list of Persons
*/
public void addPerson(Person person)

/*
* returns       - calculated value of the time variables in day increments
*/
public int getDay()

/*
* returns       - calculated value of the time variable, gets the hour of the day
*/
public int getHour()
```

**Setters/Getters**
``` java
public String getName()

public int getTime()

public List<Location> getLocations()
```

### Location

**Static Methods**

``` java
/*
* parameters    - two Location objects
* returns       - distance between them
*/
public static double getDistance(Location l1, Location l2)

/*
* parameters    - reference location, and a list of locations
* returns       - list of location sorted by distance to the reference location
*/
public static List<Location> sortByDistanceFrom(Location ref, List<Location> locations)

/*
* parameters    - reference location, double, list of locations
* returns       - list of locations within the limit of the reference location
*/
public static List<Location> getWithin(Location ref, double limit, List<Location> locations)
```

**Setters/Getters**
``` java
public double getLat()

public double getLng()

public String getName()

public String getID()

public String toString()
```

**Extending**

Create various of locations with their own features by extending this class, this allows you to create custom infection behavior based on location.
For example, a school location might have a higher chance of getting infected than a hospital location, a school might shutdown if there are too many infected but a hospital might give vaccinations.


``` java
package fish;

public class HospitalLocation extends Location {
    
    public HospitalLocation(String id, double lat, double lng, String name) {
        super(id, lat, lng, name);
    }
    
    @Override
    public void doInteractions(List<Person> people) {
        List<Person> contagious = Person.getContagious(people);
        List<Person> vulnerable = Person.getVulnerable(people);
        for(Person personCon : contagious){
            Iterator<Person> iter = vulnerable.iterator();
            while(iter.hasNext()){
                Person personVul = iter.next();
                personVul.doExposure(personCon.getPathogen());
                if(personVul.getPathogen() != null){
                    iter.remove();
                }
            }
        }
        for (Person p : people) {
            if (p.feelsSick()) {
                p.setRoutine(new SleepRoutine(10.0));
            }
        }
    }
}

```

### Routine

**Method**
``` java
/*
* Detials       - Must implement method in the extending classes
*/
abstract public Location getNextLocation(Person person, City city)
```

**Extending**

Define routines that people can follow by extending the Routine class, this allows people to transition to different locations.
Routines can be added to different classes, for example location based routine, at a hospital there might be patients sleeping.

``` java
package fish;

public class SleepRoutine extends Routine {
    
    private double sleepCount;
    private double sleepLimit;
    
    public SleepRoutine(double limit) {
        this.sleepCount = 0.0;
        if (limit > sleepCount)
            this.sleepLimit = limit;
        else
            this.sleepCount = 7.0;
    }
    
    public SleepRoutine(double count, double limit) { ... }
    
    public Location getNextLocation(Perosn person, City city) {
        Location nextLoc = person.getLocation();
        while(sleepCount < sleepLimit) {
            sleepCount++;
            return nextLoc;
        }
        Location<Location> nearby = Location.getWithin();
        int rand = (int)(Math.abs(Helper.nextSeed() * nearby.size());
        nextLoc = nearby.get(rand);
        return nextLoc;
    }
}
```

### Pathogen

**Methods**
``` java
/*
* Details       - Must implement method in the extending classes
*/
abstract int expand(int bacteria)

/*
* parameters    - AgeGroup allows customize behavior based on age
* returns       - returns default values, if its not overrided in the extending class
*/
public double getInfectivity(AgeGroup ageGroup)
public double getToxigenicity(AgeGroup ageGroup)
public double getResistance(AgeGroup ageGroup)

/*
* Detials       - Prints pathogen information
*/
public void printSummary()
```

**Extending**

Design your own pathogen by extending this class, for example, you can create a bacteria that targets children by using the AgeGroup enum. 
When extending the Pathogen class, you must provide implementation for the expand method, which takes in the number of bacteria and returns a value that determines the bacteria's growth rate.

``` java
package fish;

public class BoogieMonsterBacteria extends Pathogen {
    
    @Override
    public String getName() {
        return "BoogieMonster";
    }
    
    @Override
    public double getInfectivity(AgeGroup ageGroup) {
        if (ageGroup == AgeGroup.CHILD)
            return 1.5;
        return 0.1;
    }
    
    @Override 
    public double getToxigenicity(AgeGroup ageGroup) {
        if (ageGroup == AgeGroup.CHILD) 
            return 1.0;
        return 0.5;
    }
    
    @Override 
    public double getResistance(AgeGroup ageGroup) {
        if (ageGroup == AgeGroup.CHILD)
            return 0.1;
        return 1.0;
    }
    
    // Must provide implementation for expand method when extending abstract class Pathogen
    public int expand(int bacteria) {
        // Input bacteria count, return a value that determines the bacteria's growth rate
        return 1;
    }
}
```

### Helper

**Static Methods**
``` java
/*
* Details       - Call this method to get seeded randomw with: // Helper.nextSeeed()
* returns       - randome double
*/
public static double nextSeed()

public static void writeFileLine(String filename, String content)

public static String readEntireFile(String filename)

public static void closeAllFiles()

public static List<String[]> readCoordsFromFile(String filename)

public static void printCityLine(String filename, City city)

public static void printLocationLine(String filename, City city)

public static void printPeopleData(String filename, City city)
```
