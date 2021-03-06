## Description

[This package](https://www.npmjs.com/package/objer) is used to interact with objects

## Basic Usage

*install*

`npm install --save objer`

*import*

`import { get, set, has } from 'objer';`

## Api Reference

### get(object, path, defaultValue)

**Important note, null path is treated abnormally, if you need something more traditional use yank**

*parameters:*

* `object`: The object to get a value from
* `path`: The key path to pull the value from
    * This can be a string 'a.b.c' or an array ['a', 'b', 'c'] or null, if the key is null `object` is returned
* `defaultValue`: If there is no value at that location, (if that value comes out as `undefined`), get will return this value. If this value is not specified get will return undefined

*response:*

The value at that key

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        }
    };

    get(person, 'address.city'); // returns 'faketown'
    get(person, ['address', 'city']); // returns 'faketown'
    get(person, 'key.that.doesnot.exist'); // returns undefined
    get(person, 'key.that.doesnot.exist', 'hola'); // returns 'hola'
    get(person, null, 'hola'); // returns whole person object

### yank(object, path, defaultValue)

*parameters:*

* `object`: The object to get a value from
* `path`: The key path to pull the value from
    * This can be a string 'a.b.c' or an array ['a', 'b', 'c'] or a number, if the path is any other type the default value will be returned
* `defaultValue`: If there is no value at that location, (if that value comes out as `undefined`), get will return this value. If this value is not specified get will return undefined

*response:*

The value at that key

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        }
    };

    yank(person, 'address.city'); // returns 'faketown'
    yank(person, ['address', 'city']); // returns 'faketown'
    yank(person, 'key.that.doesnot.exist'); // returns undefined
    yank(person, 'key.that.doesnot.exist', 'hola'); // returns 'hola'
    yank(person, null, 'hola'); // returns 'hola'

### firstValue(object)

*parameters:*

* `object`: The object or array to get a value from

*response:*

The first value in the object or array

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        },
        things: [5, 9, 0],
    };

    firstValue(person); // returns 'jeffery'
    firstValue(person.things); // returns 5

### firstKey(object)

*parameters:*

* `object`: The object or array to get a key from

*response:*

The first key in the object or array

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        },
        things: [5, 9, 0],
    };

    firstKey(person); // returns 'name'
    firstKey(person.things); // returns 0

### has(object, path)

*parameters:*

* `object`: The object to get a value from
* `path`: The key path to check for existence
    * This can be a string 'a.b.c' or an array ['a', 'b', 'c'] or null, if the key is null `object` is returned

*response:*

`true`|`false`

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        }
    };

    has(person, 'address'); // returns true
    has(person, 'address.city'); // returns true
    has(person, ['address', 'city']); // returns true
    has(person, 'key.that.doesnot.exist'); // returns false
    has(person, 'key.that.really.doesnot.exist'); // returns false

### set(object, path, value)

*parameters:*

* `object`: The object to get a value from
* `path`: The key path to pull the value from
    * This can be a string 'a.b.c' or an array ['a', 'b', 'c'] or null, if the key is null `object` is returned
* `value`: Set the value of subkey inside object to value

*response:*

`object`: the original object passed into set

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        }
    };

    set(person, ['address', 'country'], 'United States'); // returns person, person.address.country is now equal to 'United States'
    set(person, ['address', 'city'], 'realtown'); // returns person, person.address.realtown is changed to 'realtown'

### keys(object)

*parameters:*

* `object`: The object to get a list of keys from, passing an array returns an array of indexes with each index a string

*response:*

array of keys

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        }
    };

    keys(person); // returns ['name', 'address']
    keys(person.address); // returns ['street', 'city', 'state', 'zip']

### values(object)

*parameters:*

* `object`: The object to get a list of values from

*response:*

array of values

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        }
    };

    values(person); // returns ['jeffery', { street: '123 fake st', city: 'faketown', state: 'FS', zip: '90909' }]
    values(person.address); // returns ['123 fake st', 'faketown', 'FS', '90909']

### pick(object, whitelistedKeys)

*parameters:*

* `object`: The object to get a list of values from
* `whitelistedKeys`: An array of keys to pick from the original object, can be deep values such as a.b.c.e[5]

*response:*

new object filled with matching `whitelistedKeys` keys from `object`

*example:*

    const person = {
        name: 'jeffery',
        address: {
            street: '123 fake st',
            city: 'faketown',
            state: 'FS',
            zip: '90909',
        }
    };

    pick(person, ['address.street']); // returns { address: { street: '123 fake st' } }
    pick(person, ['address.street', 'address.state']); // returns { address: { street: '123 fake st', state: 'FS' } }
    pick(person, ['name']); // returns { name: 'jeffery' }

### getObjectPath(path)

*parameters:*

* `path`: The path to retrieve a path array from

*response:*

array of keys representing path

*example:*

    getObjectPath('a.b.c'); // returns ['a', 'b', 'c']
    getObjectPath(['a', 'b', 'c']); // returns ['a', 'b', 'c']
    getObjectPath(['a', 'b.c']); // returns ['a', 'b.c']
    getObjectPath('a[1].b'); // returns ['a', 1, 'b']
    getObjectPath('a.1.b'); // returns ['a', '1', 'b']

### getStringPathForArray(pathArray)

*parameters:*

* `pathArray`: The path array to retrieve a path string from

*response:*

string representing the path

*example:*

    getObjectPath(['a', 'b', 'c']); // returns 'a.b.c'
    getObjectPath(['a', 1, 'c']); // returns 'a[1].c'
    getObjectPath('a[1].c'); // returns 'a[1].c'

### getTypeString(entity)

*parameters:*

* `entity`: Anything

*response:*

string representing the type

*example:*

    getTypeString(''); // returns 'string'
    getTypeString(1); // returns 'number'
    getTypeString(1.5); // returns 'number'
    getTypeString({}); // returns 'object'
    getTypeString([]); // returns 'array'
    getTypeString((() => {})); // returns 'function'
    getTypeString(true); // returns 'boolean'
    getTypeString(null); // returns 'null'
    getTypeString(undefined); // returns 'undefined'
    getTypeString(NaN); // returns 'number'
    getTypeString(Infinity); // returns 'number'
    getTypeString(new Date()); // returns 'date'
    getTypeString(/hello/g); // returns 'regexp'
    class test {}
    getTypeString(new test()); // returns 'object'
    getTypeString(Symbol(55)); // returns 'symbol'
    getTypeString(new Promise((() => {}))); // returns 'promise'
    getTypeString(new String('')); // returns 'string'

### deepEq(item1, item2)

*parameters:*

* `item1`: Anything
* `item2`: Anything

*response:*

Checks for direct equality, and in the case of objects and arrays checks for subkey equality
_note: does not handle class instance equality_

*example:*

    deepEq([{ first: 'bobabob', second: 'gogagog' }], [{ second: 'gogagog', first: 'bobabob' }]); // returns true
    deepEq({ first: 1 }, { first: 2 }); // returns false


### partition(list, paritionFunctionObject, defaultKey)

*parameters:*

* `list`: Original list to partition, array
* `partitionFunctionObject`: Takes an object where the key is the partition key, and the value is a function that returns a boolean indicating whether or not a given line item passed the condition to be added to that partition
* `defaultKey`: If the list item does not pass any of the partitionFunctionObject function tests, it will be assigned to the default key, if the default key is null the line item is ignored

*response:*

An object that contains the same keys the partitionFunctionObject + the default key, the values are the list of line items that passed the conditions in the partition functions

*example:*

    const original = [1, 2, 3, 4, 5];

    partition(original, { even: item => item % 2 === 0 }, 'odd'); // returns { even: [2, 4], odd: [1, 3, 5] }


### mapObject(object, modificationFunction)

*parameters:*

* `object`: Original object to modify
* `modificationFunction`: Function to execute on every value of object, the results of this function will be assigned as the new value of the resultant object for the original key
    * (value, key, keyIndex) => any

*response:*

A new object with all the same keys as the original object with every value the result of the modification function

*example:*

    const original = { a: 1, b: 2, c: 3 };

    mapObject(original, item => item + 3); // returns { a: 4, b: 5, c: 6 }