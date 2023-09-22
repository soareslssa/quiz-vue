<template>
  <div>
    <h1>
      Pergunta 1
    </h1>

    <input type="radio" name="options" value="true">
    <label>True</label><br>

    <input type="radio" name="options" value="false">
    <label>False</label><br>

    <button class="send">Send</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {

  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto ;
  max-width: 960px;

  input[type=radio] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: forestgreen;
    border: 1px solid forestgreen;
    cursor: pointer;
  }
}
</style>
