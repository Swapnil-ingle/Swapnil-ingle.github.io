# JavaScript learning notes

These are the JavaScript learning notes, that I would most probably forget so I've kept them on the blog for future reference.

## Code 

```html
<!DOCTYPE html>
<html>
    <script>
        "use strict";

        var ourDog = {
            "name": "Tom",
            "legs": 4,
            "tails": 1,
            "likes": ["everything"]
        };

        var myDog = {
            "name": "Doggo",
            "legs": 3,
            "tails": 2,
            "friends": [],
            "who let this dog out": false
        };

        console.log(myDog.name);
        myDog.name = "Happy Doggo!";
        console.log(myDog["name"]);

        // Add new property to an object

        console.log("Before adding bark: ");
        console.log(myDog);
        myDog.bark = "bow-wow";
        console.log("After adding bark: ");
        console.log(myDog);

        delete myDog.name;
        console.log(myDog);

        // Check if an object has a property
        console.log(myDog.hasOwnProperty("name"));

        // let or const vs var
        // 1. let does not let a variable declare twiced.
        // 2. const is used to declare read-only variables.
        // 3. However we can mutate an array defined as const.
        
        // The scope of let is limited to block in which it was declared.
        // The scope of var is global

        // "use strict"
        // This will enable strict mode where JS will catch
        // common-coding mistake

        // To prevent data-mutation do OBJECT.freeze()

        const MATH_CONSTANTS = {
            PI: 3.14
        };

        Object.freeze(MATH_CONSTANTS);

        try {
            MATH_CONSTANTS.PI = 99;
        } catch (ex) {
            console.log("In exception block...")
            console.log(ex);
        }

        // Anonymous function (Arrow function)
        // Good use case: When a function takes another function as argument
        console.log("=========== Anonymous Function =============");

        const magic = function() {
            return new Date();
        };

        console.log(magic);

        const magic2 = () => {
            return new Date();
        };
        console.log(magic2);

        const magic3 = () => new Date();
        console.log(magic3);

        // Example of anonymous function;
        const myConcatFunction = (arr1, arr2) => arr1.concat(arr2);
        console.log(myConcatFunction([1,2], [3,4,5,6]));

        // Ex#2: Make square of every positive number from an array
        const inputArr1 = [3, 4.5, -2, 3, -3, 5, 8.7];

        const squaredList = (arr) => {
            const squaredIntegers = arr
                .filter(num => Number.isInteger(num) && num > 0)
                .map(x => x * x);
            return squaredIntegers;
        };

        console.log(squaredList(inputArr1));

        // Higher order arrow functions
        // To use a default value for parament make that "parameter = value"
        // in the declaration.
        const increment = (num, val = 1) => {
            return num + val;
        }

        console.log(increment(2));
        console.log(increment(4,5));

        // Rest operator
        console.log("Rest Operator")
        const sum = (...args) => {
            return args.reduce((a, b) => a + b, 0);
        };

        console.log(sum(1,2,3,4));

        // Spread Operator [...arrName]
        // Spreads out an array in it's individual parts 
        console.log("Spread Operator")

        // Without spread operator
        // When we declare oneArr = otherArr
        // Both references would point to the same arr.
        let oneArr = ["India", "New Zealand", "Dubai"]
        let otherArr = oneArr;

        oneArr[0] = "Pakistan";
        console.log("Other arr picked up the changes from oneArr: " + otherArr);

        // With spread operator
        let otherArrNew = [...oneArr]

        oneArr[0] = "Kazakistan";
        console.log("Other arr new did NOT pick up the changes from oneArr: " + otherArrNew);
        
        // Destructing Assignment syntax => Reassigning values from object to variables
        // with ease.

        const AVG_TEMP = {
            today: 35.12,
            tomorrow: 27.43
        }

        // Syntax: 
        // const { <variableFromObject> : <variableToPutValInto> } = <objectName>
        const { tomorrow : tomorrowTemp } = AVG_TEMP;
        console.log(tomorrowTemp);

        // Destruction Assignment with nested objects
        // Syntax:
        // const { <variableFromObject> : {<nestedVariable> : <variableToPutValInto>}} = <objectName>
        const FORECAST = {
            today: { min: 35.12, max: 45.23 },
            tomorrow: { min: 27.43, max: 30.88 }
        }

        // Getting tomorrows max temp using destruction assignment
        const {tomorrow : {max : tommMax}} = FORECAST;
        console.log(tommMax);

        // Destructing assignment to assign variables from Arrays
        // Assigning variables from array elements
        const[a,b,c,,d] = [1,2,3,4,5];
        // a --> 1; b --> 2; c --> 3; d --> 5
        console.log(a + " " + b + " " + c + " " + d);

        // Destructing to swap out elements
        var a1 = 1, b1 = 2;
        [a1, b1] = [b1, a1];

        // Destructing to use with Rest Operator (...)
        var newArr = [1,2,3,4];
        const [, ...arr] = newArr;

        console.log(arr);
        // This will skip the first element.
        // I don't when we would need something like this.

        // Destructing to pass an object as function param
        const stats = {
            max: 12,
            standardDeviation: 4.35,
            median: 2,
            min: 0.2,
            avg: 15
        }

        const half = ({min, max}) => {
            return (min + max) / 2;
        }

        console.log(stats);
        console.log(half(stats)); // This destruct the object and pass only min/max value from within

        // Create String with template literal
        const person = {
            name: "the Zodiac Killer",
            age: 1112
        }

        const greeting = 
            `Hello, my name is ${person.name}!
            I am ${person.age} years old.`;

        console.log(greeting);

        // Concise objects using simple fields
        const createPerson = (name, age, gender) => ({name, age, gender});
        console.log(createPerson("Samuel", 24, "Male"));

        // Putting a function in an object

        const bicycle = {
            gear: 2,
            setGear(newGear) {
                "use strict";
                this.gear = newGear;
            }
        };

        bicycle.setGear(5);
        console.log(bicycle);

        // Constructor of an object
        // Older method
        var SpaceShuttle = function(targetPlanet) {
            this.targetPlanet = targetPlanet;
        }
        var zeus = new SpaceShuttle('Jupiter');
        console.log(zeus.targetPlanet);

        // New method
        class SpaceShuttle02 {
            constructor(targetPlanet) {
                this.targetPlanet = targetPlanet;
            }
        }

        var python = new SpaceShuttle02('Venus');
        console.log(python.targetPlanet);

        // Using getters/setters to control access to an object
        class Book {
            constructor(author) {
                this._author = author;
            }

            get author() {
                return this._author;
            }

            set author(newAuthor) {
                this._author = newAuthor;
            }
        }

        const book = new Book("Samuel");
        console.log(book.author);
    </script>
</html>
```
