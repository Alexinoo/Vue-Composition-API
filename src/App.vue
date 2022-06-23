
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




<template>
  <section class="container">
    <h2>{{ user.name }}</h2>
    <h3>{{ user.age }}</h3>
  </section>
</template>

<script>
import { ref } from 'vue';

export default {

  // data() {
  //   return {
  //     userName: 'Maximilian',
  //   };
  // },

  setup(){

    // const userName = ref('Maximilian')
    // const userAge = ref(31)

    const user = ref({
        name : 'Maximilian',
        age : 31
    })

    setTimeout(()=>{

      user.value.name = 'Max'
      user.value.age = 32

    },2000 )

    return { username : user.value.name ,  age : user.value.age , user };
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