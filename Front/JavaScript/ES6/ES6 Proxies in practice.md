# ES6 Proxies in practice

- [JavaScript](https://habr.com/en/hub/javascript/)

- [From sandbox](https://habr.com/en/sandbox/)

With the appearance of ECMAScript 2015, an avalanche of features came; some of them make you mad, and others are pleasant surprises, like meeting an old friend after a long time.



Some features are related to metaprogramming. What is that? I'm not very eloquent, so let's turn to our friend, Wikipedia.



> Metaprogramming is a programming technique in which computer programs have the ability to treat other programs as their data. It means that a program can be designed to read, generate, analyze or transform other programs, and even modify itself while running. In some cases, this allows programmers to minimize the number of lines of code to express a solution, in turn reducing development time. It also allows programs greater flexibility to efficiently handle new situations without recompilation.

In a nutshell, metaprogramming allows a program to manipulate others or themselves at both compile and execution time. Metaprogramming in JavaScript is based on two features: Proxy and Reflect API. In this post, we will consider the first one.



## Proxy



Proxy is a new API that allows intercepting, modifying and extending objects at runtime. Using this API, you can:



- Profile and debug logs,
- Intercept calls to properties,
- Validate «on the fly»,
- etc.



Proxy is a constructor that accepts two parameters: source object and object that acts as a handler for the source object. The latter contains methods that are known as Traps.





A Trap is a method that modifies the behaviour of some part of the object. For example, the trap gets and set intercept the calls to properties to obtain and establish a value respectively, being able to place logic before and during this process.



To better understand the usefulness of proxies, let's do some small exercises.



### Example: logging/profiling



Just imagine, you are 17 years, about to turn 18 already. And you want your program to congratulate you automatically when you open it. For this, you can use Proxy.



```
et person = {
  name: "John Doe",
  age: 17
};

person = new Proxy(person, {
  set(target, property, value) {
    if (value === 18) {
      console.log("Congratulations! You are of legal age");
      Reflect.set(target, property, value);
      return true;
    }
  }
});

person.age = 18;

Not only can we do logs, as I said at the beginning, we can do as far as the language limits us. Here we could make validations for the age, for example, if it exceeds 100 that throw us an error:

if (value < 13 && value > 99) {
  throw new Error('The age should be between 13 and 99')
} else {
  Reflect.set(target, property, value)
}

Example: secure access to properties
let person = {
  name: "John Doe"
};

const Undefined = new Proxy(
  {},
  {
    get(target, name, receiver) {
      return Undefined;
    }
  }
);

const Safeish = obj => {
  return new Proxy(obj, {
    get(target, property, receiver) {
      if (target.hasOwnProperty(property)) {
        const isObject = target[property] instanceof Object;
        return isObject
          ? Safeish(target[property])
          : Reflect.get(target, property, receiver);
      }
      return Undefined;
    }
  });
};

person = Safeish(person);
console.log(person.name);
console.log(person.sister.name === Undefined);
```



### Example: query array



We have already seen an example, with the most used get and set traps. To reinforce, let's go a little further and use nested proxies. This exercise will try to convert a conventional array to a queryable array to use operators like the classic SQL groupBy.



For this, we will need two input parameters:



- collection: array of objects which we will extend.
- groupKeys: array of strings that represent the properties for which you are going to group (name, category, price, etc.)



```
const data = [
  {
    id: 1,
    category: 2,
    name: "Intel NUC Hades Canyon"
  },
  {
    id: 2,
    category: 1,
    name: "Logitech K380"
  },
  {
    id: 3,
    category: 1,
    name: "Genius ECO-8100"
  }
];

const groupable = (collection, groupKeys) => {
  // Check that the collection is an array
  if (!(collection instanceof Array)) {
    throw new TypeError("The input collection is not an Array");
  }
  const data = JSON.parse(JSON.stringify(collection));
  const clone = JSON.parse(JSON.stringify(collection));

  Object.defineProperty(clone, "groupBy", {
    configurable: true,
    enumerable: false,
    writable: false,
    value: groupKeys.reduce((acc, cur) => {
      acc[cur] = null;
      return acc;
    }, {})
  });
```



```
 return new Proxy(clone, {
    get(target, property, receiver) {
      if (property === "groupBy") {
        return new Proxy(target[property], {
          get(target, property, receiver) {
            // if the property to be grouped does not exist
            // log a warning and return []
            if (!target.hasOwnProperty(property)) {
              return [];
            }
            // Otherwise, group by property
            return data.reduce(function(acc, cur) {
              (acc[cur[property]] = acc[cur[property]] || []).push(cur);
              return acc;
            }, {});
          }
        });
      }
      return Reflect.get(target, property, receiver);
    }
  });
};

const datasource = groupable(data, ["category"]);
console.log(datasource.groupBy.category);
```



## Conclusions



The Proxy may not be the most used ES6 feature, but together with Reflect API, it is one of the most important and interesting. Its flexibility allows adopting it in a multitude of cases and, best of all, it is easy to implement.



## References


[https://es.wikipedia.org/wiki/Metaprogramaci%C3%B3n
](https://es.wikipedia.org/wiki/Metaprogramación)

- Tags:

  [ES6 Proxies](https://habr.com/en/search/?q=[ES6 Proxies]&target_type=posts)

- Hubs:

  [JavaScript](https://habr.com/en/hub/javascript/)

<iframe id="google_ads_iframe_/235032688/HH/HH01_ATF_Poster_0" title="3rd party ad content" name="google_ads_iframe_/235032688/HH/HH01_ATF_Poster_0" width="1" height="1" scrolling="no" marginwidth="0" marginheight="0" frameborder="0" srcdoc="" data-google-container-id="1" data-load-complete="true" style="border: 0px; vertical-align: bottom;"></iframe>