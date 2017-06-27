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

## API

**[AgeGroup](#AgeGroup)**<br>
**[Person](#Person)**<br>
**[City](#City)**<br>
**[Location](#Location)**<br>
**[Routine](#Routine)**<br>
**[Pathogen](#Pathogen)**<br>

### AgeGroup

Age group are used to define a Person class

``` java
AgeGroup.CHILD
AgeGroup.TEEN
AgeGroup.ADULT
AgeGroup.ELDER
```

### Person


### City


### Location

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

Define routines that people can follow 



### Pathogen

Design your own custom pathogen by extending this class, for example, you can create a bacteria that targets children by using the AgeGroup enum. 
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