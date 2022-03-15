<template>
  <div class="toast">
    toast- <slot></slot>
  </div>
</template>

<script>
export default {
  name: "toast",
  methods: {
    open(opt) {
      console.log("this is open console")
    }
  }
}
</script>


<style lang="scss" scoped>
.toast {
  color: #ccc;
}
</style>
