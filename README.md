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

**[City](#City)**<br>
**[Location](#Location)**<br>
**[Routine](#Routine)**<br>
**[Pathogen](#Pathogen)**<br>


### City


### Location

Create various of locations with their own features by extending this class, for example

Methods



Usage

``` java
package fish;

public class HospitalLocation extends Location {
    
    /* ...Instantiation and other methods... */
    
    @Override
    public void doInteractions(List<Person> people) {}
}

```




### Routine


### Pathogen

