# Scope Homework: Who Dunnit

### Learning Objectives

- Understand function scope
- Know the difference in between the let and const keywords

## Brief

Using your knowledge about scope and variable declarations in JavaScript, look at the following code snippets and predict what the output or error will be and why. Copy the following episodes into a JavaScript file and add comments under each one detailing the reason for your predicted output.

### MVP

#### Episode 1

```js
///////////////////////////////
// THIS IS THE SHARED SCENARIO//
///////////////////////////////
const scenario = {
  murderer: 'Miss Scarlet',
  room: 'Library',
  weapon: 'Rope'
};
///////////////////////////////
// ALL FUNCTIONS BELOW REFERENCE THE SCENARIO - unless a function has another one specified//
///////////////////////////////

const declareMurderer = function() {
  return `The murderer is ${scenario.murderer}.`;
}

const verdict = declareMurderer();
console.log(verdict);
// 'Miss Scarlet', because the const variable veredict takes the declareMurderer function, and that one returns the value murderer : 'Miss Scarlet'.
```

#### Episode 2

```js
const murderer = 'Professor Plum';

const changeMurderer = function() {
  murderer = 'Mrs. Peacock';
}

const declareMurderer = function() {
  return `The murderer is ${murderer}.`;
}

changeMurderer();
const verdict = declareMurderer();
console.log(verdict);

// first I thought the veredict would be 'Professor Plum' because we locked the murderer value as a const thus the next functions wouldn't be able to re-assign it, and when in the end we would call for the veredict using the annonymous function assigned to the variable declareMurderer, the murderer would be the const value we were unable to re-assign.
//HOWEVER, as further testing soon disproved, there is only one possible outcome to this murder. And that is...
//  >> that THIS WON'T WORK! <<
// it took me a while to guess why, but then I realized that if we turn an object as a const, the values within the object can still be re-assigned;
// however, in this case we are declaring that its value "murderer" is a const;
// which means that when the program tries to run the following function aka ChangeMurderer, it will throw an error because it won't be able to re-assign it, and the process will stop.
// which would be a pitty, because if it was able to keep running, mr detective aka veredict would declareMurderer Professor Plum...he's still sus.
```

#### Episode 3

```js
let murderer = 'Professor Plum';

const declareMurderer = function() {
  let murderer = 'Mrs. Peacock';
  return `The murderer is ${murderer}.`;
}

const firstVerdict = declareMurderer();
console.log('First Verdict: ', firstVerdict);

const secondVerdict = `The murderer is ${murderer}.`;
console.log('Second Verdict: ', secondVerdict);

// for this episode we are throwing two console.logs aka two veredicts. There are many suspects, but after further consideration...
// for firstVeredict - as a const variable that holds an annonymous function which has safely declareMurderer 'Mrs Peacock' as a local variable within its own declareMurderer cont scope - we can determine that the veredict will be Mrs. Peacock' !!
// for secondVeredict - as a const refering to the initial local variable murderer outside the scope of declareMurderer - the murderer is Professor Plum!!
```

#### Episode 4

```js
let suspectOne = 'Miss Scarlet';
let suspectTwo = 'Professor Plum';
let suspectThree = 'Mrs. Peacock';

const declareAllSuspects = function() {
  let suspectThree = 'Colonel Mustard';
  return `The suspects are ${suspectOne}, ${suspectTwo}, ${suspectThree}.`;
}

const suspects = declareAllSuspects();
console.log(suspects);
console.log(`Suspect three is ${suspectThree}.`);

// interestingly enough, for the cont variable declareAllSuspects, Colonel Mustard has become the primer suspectThree while still keeping suspectOne and suspectTwo as they were. Lucky you, Mrs. peacock. But don't get too excited yet because this is not the end of the story... in fact, this is NOT the real veredict. There are TWO outcomes again!
// console.log("audience gasps")
// for the FIRST RESULT: console.log(suspects) the suspects variable is defined as a const, which is OK because we are NOT trying to modify the other cont variable that it has assigned (aka declareAllSuspects). in other words, as suspects is not trying to modify declareAllSuspects, it is an admisible operation. And as we said before, declareAllSuspects returns three vales, one per each suspect: "Miss Scarlet" as suspectOne, "Processfor Plum" as suspectTwo AND "Colonel Mustart" as SuspectThree because he became a let variable within the declareAllSuspects function scope, and it is an allowed re-assignment since the original variable suspectThree is a let.
// for the SECOND RESULT: we are NOT calling the suspects variable anymore, neither declareAllSuspects by extension. The log is only interseted in the global suspectThree variable which is.... Correct! it is you Mrs. Peacock! unfortunately, you are still sus.

```

#### Episode 5

```js
const scenario = {
  murderer: 'Miss Scarlet',
  room: 'Kitchen',
  weapon: 'Candle Stick'
};

const changeWeapon = function(newWeapon) {
  scenario.weapon = newWeapon;
}

const declareWeapon = function() {
  return `The weapon is the ${scenario.weapon}.`;
}

changeWeapon('Revolver');
const verdict = declareWeapon();
console.log(verdict);

// This was a very tricky one. Because, totally overseeing the  "scenario.weapon = newWeapon;" line, i wrongly assumed the weapon was the candle stick. 
// But no worries! further testing confirmed that my first suspicion was wrong and that IN FACT! the weapon that Miss Scarlet used to commit the murder was a Revolver!!
// console.log("someone in the audience dramatically faints")
// (it also makes more sense... how would anyone manage to kill with a candle stick? ... well, let's ctrl + c our imagination here and focus again on the result).
// It was the revolver! WHY? Because console.log has determined a const and immutable veredict. This veredict as a const variable has the function declareweapon() assigned as a value, and when we track this declareweapon() function, we discover that it runs an annonymous function which returns the senario.weapon. 
// NOW. HERE is where things get intersting. Here is where i first wrongly addumed this scenario.weapon was referencing the global variable, HOWEVER, even though my intuition was correct,
// upon further inspection I discovered that the global weapon value had been modified by a previous function while nobody was looking!!!!!
// indeed, the const changeWeapon variable with an annonymous function assiged (which takes one parameter), had changed the global weapon value to Revolver AND THIS IS an ALLOWED operation because the global variable weapon had. never. been. locked in a const. 
// well played, changeWeapon... well played.
```

#### Episode 6

```js

const scenario = {
  murderer: 'Miss Scarlet',
  room: 'Library',
  weapon: 'Rope'
};

let murderer = 'Colonel Mustard';

const changeMurderer = function() {
  murderer = 'Mr. Green';

  const plotTwist = function() {
    murderer = 'Mrs. White';
  }

  plotTwist();
}

const declareMurderer = function () {
  return `The murderer is ${murderer}.`;
}

changeMurderer();
const verdict = declareMurderer();
console.log(verdict);
// This murder case is not for the weak hearted. It is also a tricky one, so let's roll up our sleeves and go granular on it.
// The SCENARIO object is set as a const, but that is not very relevant in regards to its values such as murderer : "Miss Scarlet", as we all know they can still be re-assigned.
// let murderer: here is the first time our global variable murderer gets re-assigned, it gets re-assigned to the string value 'Colonel Mustard'.
// NEXT! the const variable changeMurder comes into play. It has assigned an annonymous function which re-assigns the let variable murderer ('Colonel Mustard') to 'Mr Green'. BUT behooold the PLOTTWIST!!! NOOOBODY expected the const changeMurder variable/function to be hosting another const variable for murderer within its scope, which is in fact, re-assigning the let variable 'Mr Green' to an unmutable const variable named "Mrs. White" This is an allowed operation because we have yet not left the scope of the const changeMurderer variable, so any changes ocurring within will be carried until they are returned. Plus, "Colonel mustart" was a let variable and not a const, so yes, allowed.
// Now. moving on. 
// there is another const variable named declareMurderer which has another annonymous function assigned. This new function simply returns the global value of murderer.
// And here is the catch.
// What has happened with the global variable scenario.murderer? The changeMurderer function CHANGED IT, remember?
// in consecuence, when we throw the veredict aka console.log(veredict) which is a const variable with the declareMurderer function assigned - a function which returns the global value of murderer WHICH changemurderer function had previously modified - we end up with a SINGLE AND UNEQUIVOCAL RESULT. 
// that MRS. WHITE IS THE MURDERER!!!!!
```

#### Episode 7

```js
const scenario = {
  murderer: 'Miss Scarlet',
  room: 'Library',
  weapon: 'Rope'
};

let murderer = 'Professor Plum';

const changeMurderer = function() {
  murderer = 'Mr. Green';

  const plotTwist = function() {
    let murderer = 'Colonel Mustard';

    const unexpectedOutcome = function() {
      murderer = 'Miss Scarlet';
    }

    unexpectedOutcome();
  }

  plotTwist(); 
}

const declareMurderer = function() {
  return `The murderer is ${murderer}.`; 
}

changeMurderer();
const verdict = declareMurderer();
console.log(verdict);

// I am afraid my detective senses betrayed me once again for I promptly assumed that miss scarlett was the murderer. However, after running the real results I have understood what drove me to that conclusion and WHO was the real culprit. 
// The line that totally tricked me was line 196 let murderer = 'Colonel Mustard' since - followint the road of the previous case -  I assumed the murderer got reassigned.
// HOWEVER!! let murderer = 'Colonel Mustard' is a LOCAL variable that ONLY EXISTS within the scope of the CONST variable changeMurderer BECAUSE if was defined with a LET type.
// Variables defined with the let type are known as local variables, they are only accessible within the block of code in which they are defined.
// Now. Even though the const unnexpectedOutcome variable within the plotTwist variable is changing the value of the murderer from 'Colonel mustard' to 'Miss Scarlett', THIS change ONLY exists whithin the scope of the plotTwist block of code and won't be able to modify "Mr. Green" as a result from the changeMurderer function.
// We also have a declareMurderer function that returns the global value of murderer WHICH changeMurdered had succesfuly re-assigned to Mr. Green.
// SO. in CONCLUSION. There is NO DOUBT that Mr. Green was the murderer!!


```

#### Episode 8

```js
const scenario = {
  murderer: 'Mrs. Peacock',
  room: 'Conservatory',
  weapon: 'Lead Pipe'
};

const changeScenario = function() {
  scenario.murderer = 'Mrs. Peacock';
  scenario.room = 'Dining Room';  

  const plotTwist = function(room) {
    if (scenario.room === room) {
      scenario.murderer = 'Colonel Mustard';  
    }

    const unexpectedOutcome = function(murderer) {
      if (scenario.murderer === murderer) {
        scenario.weapon = 'Candle Stick';
      }
    }

    unexpectedOutcome('Colonel Mustard'); // candle stick
  }

  plotTwist('Dining Room'); //colonel mustard
}

const declareWeapon = function() {
  return `The weapon is ${scenario.weapon}.`  //candle stick
}

changeScenario();
const verdict = declareWeapon();
console.log(verdict);

// Remember the time when we faced that scenario where we wondered how could someone ever kill anyone with a candlestick? Well, long a behold, it has happend. RIGHT NOW!
// Because the weapon of this crime is indeed a candle stick!
// even though it kind of deserves some praise and a star sticker for creativity, let's focus on WHY is it a candle stick and not a lead pipe.
// It is all thanks to the cont Changescenario variable that has an annonymous function assigned to it. When it runs, it follows two different conditionals at a deeper scope level.
// Unlike before, there are no LET variables defined within these conditionals inside the scopes, which means that the variables that are re-assigned CAN INDEED leak from the scope and influence their upper level. We just need to follow the premises they state, and how the murderer gets reassigned to Colonel Mustard, which means that the weapon will be re-assigned to a candle-stick too.
// This is why, when we call the function declareWeapon(referencing the changeScenario function) using the variable veredict and we log the veredict, the result is CandleStick.
```

#### Episode 9

```js

const scenario = {
  murderer: 'Mrs. Peacock',
  room: 'Conservatory',
  weapon: 'Lead Pipe'
};

let murderer = 'Professor Plum';

if (murderer === 'Professor Plum') {
  let murderer = 'Mrs. Peacock'; 
}

const declareMurderer = function() {
  return `The murderer is ${murderer}.`;
}

const verdict = declareMurderer();
console.log(verdict);

// this case felt quite misleading in the sense that the local variable in line 286 "let murderer = 'Mrs. Peacock' was not defined within another variable like it has been until now.
// so it took me a bit to realise that a local variable can not only exist on the block scope of the variable hosting it, but it can also be the case it is hosted within the block scope of another statement, in this case, a conditional.
// So. In this case, the fact that we are re-assigning "Mrs. Peacock" to be the murderer is completely inconsecuential because it's effects won't escape the if block and "Professor Plum" will be, in fact, the only culprit.

```

### Extensions

Make up your own episode!
