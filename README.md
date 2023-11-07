# ENSF380-FINAL-PROJECT
# 380-Final Project

This is the repo that houses the project for the ENSF-380 final project that creates a application for the
Wildlife Rescue that schedules tasks based on a SQLDatabase and inferred tasks based on the amimals kept
and provides a GUI that allows the user to create .txt file of the schedule for the day.

## Table of Contents

- [Installation](#installation)
- [Usage](#Usage)
- [Documentation](#Documentation)
- [Contributing](#Contributing)
- [License](#License)

## Installation

### Setup
- move into the desired folder ```cd dir```
- run ```git clone [git link]```
- refer to [usage](#usage) to use the code

### Dependencies

- mysql-connector for connecting to a hosted MySQL database [Download Link](http://www.java2s.com/Code/Jar/m/Downloadmysqlconnectorjar.htm)
- JUnit for testing [Download Link](https://sourceforge.net/projects/junit/files/junit/4.10/)
- Hamcrest for running testing [Download Link](https://mvnrepository.com/artifact/org.hamcrest/hamcrest-core/1.3)

## Usage

The following sections explains how to use the code as a developer and a user

### Using the Program as a User

- 1. Clone & cd into the repo in terminal (e.g. cmd for windows) 
- 2. Run ```javac -cp .;lib/mysql-connector-java-8.0.23.jar edu/ucalgary/oop/GUI.java```
- 3. Run ```java -cp .;lib/mysql-connector-java-8.0.23.jar edu.ucalgary.oop.GUI```

### Runnning Tests in Command Line

- 1. Clone & cd into the repo in terminal (e.g. cmd for windows)
- 2. Add the dependencies listed in [dependencies](#Dependencies) to the lib folder
- 3. Run the compliation: ```javac -cp .;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar;lib/mysql-connector-java-8.0.23.jar edu/ucalgary/oop/ExampleTest.java```
- 4. Run the test file: ```java -cp .;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar;lib/mysql-connector-java-8.0.23.jar org.junit.runner.JUnitCore edu.ucalgary.oop.ExampleTest```

Note ensure you replace 'ExampleTest' with whatever test file you want to run.

### Runnning files in Command Line

The same steps should be followed as in running tests, but the the junit and hamcrest libraries are optional, as they are not needed.

## Documentation

### Classes:
#### Animal, AnimalType, Beaver, Coyote, Fox, Raccoon, FeedingType:
   - These classes all relate to the animals extracted from the database. The Animal class is an abstract parent class to the Beaver, Coyote, Fox, and Raccoon, which each contain information specific to the type of animal they are. These animal type are validated using the AnimalType enumeration class. The FeedingType class is an enumeration containing all of the available feeding type that an animal may be. It contains an abstract method that will return the feeding start time for each type.
   
#### SQLDatabase, GUI, Scheduler:
   - The Scheduler class is the class that the GUI and SQLDatabase interact with. It is instantiated in GUI and contains all of the methods that the GUI can call. One of these methods is getFromSQL, this method instanciates a new SQLDatabase object, and passes the constructor of SQLDatabase the ArrayLists that hold tasks, treatments, and animals, that the SQLDatabase constructor will then populate from the "EWR" sql databse. The scheduler also houses the method that allows the user to modify the start times of treatments in the database through the GUI. Lastly, the GUI is also able to call a method that calculates a schedule, or displays an error message if it is not possible.
   
#### Task, Treatment:
   - These classes hold the data for a given task or treatment. Task will hold the taskID, description, duration, and maximum window. Treatment will hold the treatment ID, task ID, animal ID, and start hour.
   
#### ScheduleItem, DailySchedule:
   - ScheduleItem is a class that holds a combination of data from Tasks, Treatments, and the Animal subclasses. A schedule item object holds the animal name, or names, the task description, the start hour, the duration, the quanitity of a given task, the max window, and the prep time for a task. The DailySchedule class is what is generated when the scheduler calls calulateSchedule(). This calculation is done in the constructor by first creating an ArrayList of scheduleItems that contain all of the scheduleitems from the treatments provided in the sql and also adding all of the tasks that are inferred to this ArrayList. Tasks that are more efficient to be done together (i.e. have a prep time) are then grouped into single scheduleItems in this arraylist. This ArrayList is then iterated through. Items are scheduled in order of the lowest max window to the largest. The class will validate each attmepted addition, and figure out if the items are able to be scheduled in under 60 minutes at a given start hour. If not, it will determine if they can be scheduled with an extra volunteer (under 120 minutes total). If it may still not be scheduled, the constructor throws a custom ImpossibleScheduleException that will provide a descriptive message that notifies the user of all of the tasks being scheduled at the time that are not possible. If the item being scheduled is one of the ones that was grouped to be more efficient, and the item is unable to be scheduled, it will divide this task into two that are able to be scheduled at different hours. It will attempt to schedule without a bonus volunteer, and split accross two hours before scheduling with a bonus volunteer. This schedule is then put into a hash map with a key peratining to a given start hour, and the value for that key being an arrayList of the scheduleitems that are scheduled at that hour. This schedule is then printed into a text file. 
