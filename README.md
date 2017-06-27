# Fish
Project F.I.S.H: The Fictional Infection Simulation Host

## Getting Started

Compile the all the fish java files and generate some seeds for reproducible randomness.
``` 
$ javac /PATH/TO/FISH/DIRECTORY/*.java

$ java fish.Helper seed FILENAME.txt COUNT

Ex, while at the directory containing the fish folder
$ java fish.Helper seed seed.txt 10000
```

## Documentation

**Java refersher**<br>

Static/Class methods - Can be called anywhere given you have the variables for its parameters
    
Instance/Object Methods - Called from an instance of an object using dot notaiton


**Table of Contents**<br>

**[AgeGroup](#agegroup)**<br>
**[Person](#person)**<br>
**[City](#city)**<br>
**[Location](#location)** - Extendable <br>
**[Routine](#routine)** - Extendable <br>
**[Pathogen](#pathogen)** - Extendable <br>

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

// TODO/Questions
//  - Need more info?
//  - Why is State a nested enum and AgeGroup not nested?
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
* parameters    - 
*/
public void doTurn(City city)

/*
* parameters    -
*/
public void doInfect(Pathogen pathogen)

/*
* returns       - 
*/
public boolean feelsSick()

/*
* returns       - 
*/
public boolean isContagious()

/*
* returns       - 
*/
public boolean isVulnerable()

/*
* returns       - 
*/
public double getSusceptibility()

/*
* returns       - 
*/
public boolean isIncubated()

/*
* returns       - 
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
*   
*/
public void doTurn()

/*
* parameters    -
*/
public void addLocation(Location loc)

/*
* parameters    -
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


Create various of locations with their own features by extending this class, this allows you to create custom infection behavior based on location.
For example, a school location might have a higher chance of getting infected than a hospital location, a school might shutdown if there are too many infected but a hospital might give vaccinations.


``` java
package fish;

public class HospitalLocation extends Location {
    
    /* ...Instantiation and other methods... */
    
    @Override
    public void doInteractions(List<Person> people) {}
}

```

### Routine

Define routines that people can follow by extending the Routine class

``` java

```

### Pathogen

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