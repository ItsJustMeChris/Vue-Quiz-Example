<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>
        <b>Fancy Quiz App</b>
      </b-nav-item>
      <b-nav-item disabled>Counter: {{cntCorrect}}/{{cntAnswered}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["cntCorrect", "cntAnswered"]
};
</script>
