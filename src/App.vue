<template>
  <div id="app">
<!--    <MyComponent />-->
<!--    <PhotoRedactor />-->
    <UserForm />
  </div>
</template>

<script>
  // import MyComponent from "./components/MyComponent";
  // import PhotoRedactor from "./components/PhotoRedactor";
  import UserForm from "./components/UserForm";

export default {
  name: 'App',
  components: {
    // PhotoRedactor,
    UserForm
  }
}
</script>

<style lang="scss" scoped>
    div {
        height: 100%;
        background-color: rgba(34, 34, 34, .9);
        display: flex;
        justify-content: center;
    }
</style>
