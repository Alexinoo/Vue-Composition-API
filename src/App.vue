
<!--  1) Replacing "data" with "setup"
======================================

-Done by adding the setup() {}

-If we had components registered locally or props , that remains the same

-We have 2 new features offered by Vue which helps us manage our data which we need to import from vue

  1.) ref 
        -is a function which we can call inside setup(){}

        -creates a reference not to some DOM element ; but a value which we can use in our template

        -creates a value but not just a value but a reactive value where Vue will find out when we change it , watch it and update the template 

        -And since it creates a value, we need to store it in some constant as a variable

        - We might wanna store a Number / String which we can therefore pass to ref()

        -And this now creates a reactive string you could say

        -Under the hood , it creates an obj and the string is stored in that object;

        -Vue will watch this value and updates it everytime it changes

        -However , const userName is still not available in the template

        -We need to return an object - ALWAYS RETURN AN OBJECT - AND IN THIS OBJECT WE RETURN ONLY THE THINGS WE NEED TO EXPOSE TO THE TEMPLATE,

        -We return a key : value pair ; WHERE key is the one used in the template and value is the value returned 

          const uName = 'Maximillian'

      -IF THE ABOVE STILL THIS WORKS , WHY DO WE NEED ref THEN?     -It's because it creates a reactive value 

      EXAMPLE
      ===========

       setup(){

          const uName = ref('Maximilian')
          
          setTimeout(()=>{

            uName.value = 'Max';

          },2000 )

          return {
              userName : uName
          };
    }

    -Sets a timeout that updates name to Max after 2 seconds
-->



<!--

 2.)    Replacing the setup() Method
 =========================================================================
-Adding a setup() method to the exported object (as shown in the previous) section can get annoying if you're using the composition API in many components.

 -Especially since you also need to add export default { ... } and return any values that should be available in the <template> of the component.

Vue.js offers an alternative syntax: You can use <script setup> instead of manually adding a setup() method.

The following code:

<script>
import { ref } from 'vue';
 
export default {
  setup() {
    const uName = ref('Maximilian');
 
    setTimeout(function() {
      uName.value = 'Max';
    }, 2000);
 
    return { uName };
  }
}
</script>


could be replaced potentially with:



<script setup>
import { ref } from 'vue';

const uName = ref('Maximilian');
 
setTimeout(function() {
  uName.value = 'Max';
}, 2000);
</script>


You can learn more about <script setup> here.

Both syntaxes can be used and you can simply go with your personal favorite. 

This course was recorded before <script setup> was added, hence I use the setup() method throughout this course. 
All Vue and Composition API concepts apply either way though. 

Therefore, what you learn in this course still works in the same way, no matter if you're using <script setup> or the setup() method.
 -->



 <!-- Building Reactive Objects
 ========================================================================

 -Using ref with objects works but we can also work with reactive that is a function we import from vue

      import { reactive } from 'vue'

-reactive is specifically made for objects

  const user = reactive({
          name : 'Maximilian',
          age : 31
      })
-Unlike ref which works with Strings / Number , You must only pass an object to reactive

-reactive does one  thing - Wraps the object passed into a proxy

-Therefore we can access property values by ommitiing the .value
      
      user.name = 'Max'
      user.age = 32


-ref - Wraps the object into an object with an extra value : _value : Proxy object

-And therefore we have to drill down when accessing their values by:

      user.value.name = 'Max'
      user.value.age = 32

    -->


  <!--A Deep Dive into Reactivity
  ==================================================================

  -Exposing the values directly to the template won't work ;-

  -And changes made by setTimeout aren't picked up

  e.g

    <h2>{{ name }}</h2>
    <h3>{{ age }}</h3>

  return { name: user.name, age: user.age};

  -And the reason for that is that the entire object is reactive but the properties are not and if we expose their values to the template , Vue thinks that only the values matter and not the entire object

  -That's why we had to do it differently and expose the object instead

    return { user : user };

- If we console log the 2 ; ref and reactive

      console.log(userAge.value); // 31

      console.log(user.name, user.age); // name: 'Maximilian', age: 31

-We see that they are only values that won't change and therefore will be rendered as they are

-Vue offers 2 Helper methods that we can use to check if these values are reactive or not

    1.) isRef
    2.) isReactive

-And therefore we can import them from vue if we want to use them

      import { ref , reactive , isRef , isReactive } from 'vue';     

-And once we use to test the reactivity 

    console.log( isRef(userAge.value) ); // false

    console.log(isReactive(user.name), user.age); //false 31

-They all return false

-On the other hand , if we test the userAge and the entire user {}

    console.log( isRef(userAge) ); //true

    console.log(isReactive(user) ) ; //true

-We get true on them 

-There is a function we can use to make the values reactive toRefs()

-We pass the user object toRefs({}) and thus the properties will now be reactive

    const userRefs = toRefs(user)
  
    return { name : userRefs.name , age : userRefs.age };

-And this now works ; Updates from setTimeout() function are automatically updated in the template



 -->



<template>
  <section class="container">
    <h2>{{ user.name }}</h2>
    <h3>{{ user.age }}</h3>
    <button @click="setAge">Change Age</button>
  </section>
</template>

<script>
import {  reactive } from 'vue';

export default {

  //OPTIONS API

 /* 
  data() {

    return {
      userName: 'Maximilian',
      age : 31

    }
  },

  methods : {
    setNewAge(){
      this.age = 32
    }
  }
  */

//COMPOSITIONS API

  setup(){

    // const userAge = ref(31)

    const user = reactive({
        name : 'Maximilian',
        age : 31
    })
  
  function setNewAge(){
     user.age = 32
  }

    // setTimeout(()=>{
    //   user.name = 'Max'
    //   user.age = 32
    // },2000 )

    return { user, setAge: setNewAge  };
  }
};

</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

.container {
  margin: 3rem auto;
  max-width: 30rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  text-align: center;
}
</style>