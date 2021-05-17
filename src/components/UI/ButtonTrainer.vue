<template>
  <button v-on="$listeners" class="btn-trainer"><slot></slot></button>
</template>

<style lang="scss">
.btn-trainer {
  width: 196px;
  height: 50px;
  background-color: #fff;
  border: 2px solid black;
  text-transform: uppercase;
  transition: 0.5s all;
  &:hover {
    background-color: black;
    color: white;
  }
}
</style>