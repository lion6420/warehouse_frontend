<template>
  <div :class="$style.material">
    <div :class="$style.row">
      <div :class="$style.material_list">
        <material-list-element catagory="A"></material-list-element>
      </div>
      <div :class="$style.material_list">
        <material-list-element catagory="B"></material-list-element>
      </div>
      <div :class="$style.material_list">
        <material-list-element catagory="C"></material-list-element>
      </div>
      <div :class="$style.material_list">
        <material-list-element catagory="Expensive"></material-list-element>
      </div>
    </div>
  </div>
</template>

<script>
import materialListElement from './material-list-element'
export default {
  components: {
    materialListElement
  },
  data() {
    return {

    }
  }
}
</script>

<style lang="scss" module>
@import '@/styles/general.scss';
.material {
  @include block(100%);
  margin-bottom: 50px;
  .row {
    @include block(100%);
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
    .material_list {
      @include block(48%, 480px);
      margin-top:20px;
    }
  }
}

@media screen and (max-width: 1140px) {
  .material {
    .row {
      @include block(100%);
      display: flex;
      flex-wrap: wrap;
      justify-content: space-evenly;
      .material_list {
        @include block(98%, 480px);
        margin: auto;
        margin-top:20px;
      }
    }
  }
}

</style>