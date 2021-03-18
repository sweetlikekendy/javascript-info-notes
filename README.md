# Javascript.info Notes

My notes from the [javascript.info](https://javascript.info) website.

**Table of Contents**

- [Javascript.info Notes](#javascriptinfo-notes)
  - [Objects](#objects)
    - [Object references and copying](#object-references-and-copying)
    - [Garbage Collection](#garbage-collection)

## Objects

### [Object references and copying](https://javascript.info/object-copy)

- Objects are assigned and copied by reference
  ![Object reference example](4-objects/images/references/example.png)
- Object.assign for the so-called “shallow copy”
- Nested objs have to be deeply cloned. Use [`_.cloneDeep(obj)`](https://lodash.com/docs/4.17.15#cloneDeep) from [lodash](https://lodash.com/docs/) lib

### [Garbage Collection](https://javascript.info/garbage-collection)

- Garbage collection is performed automatically. We cannot force or prevent it.

- Objects are retained in memory while they are reachable.

```js
function marry(man, woman) {
  woman.husband = man
  man.wife = woman

  return {
    father: man,
    mother: woman,
  }
}

let family = marry(
  {
    name: "John",
  },
  {
    name: "Ann",
  }
)

delete family.father
delete family.mother.husband
```

Deleted reference example

![Deleted references example](4-objects/images/garbage-collection/delete-references-example.png)

Garbage collected

![Garbage collected example](4-objects/images/garbage-collection/garbage-collected-example.png)

- Being referenced is not the same as being reachable (from a root): a pack of interlinked objects can become unreachable as a whole.
