##  Iterate to foloops

```typescript

const user = {
  name: "Daniel",
  age: 26,
};
 
const keys = Object.keys(user) as Array<keyof typeof user>;
 
keys.forEach((key) => {
              
(parameter) key: "name" | "age"
  // No more error!
  console.log(user[key]);
});

```
