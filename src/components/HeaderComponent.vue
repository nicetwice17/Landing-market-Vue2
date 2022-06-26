<template>
  <b-container>
    <header>
      <b-jumbotron header="Акции и спецпредложения" lead="Магазины Магнум работают для вас в обычном режиме.">
      </b-jumbotron>
    </header>
  </b-container>
</template>

<script>
export default {
  name: 'HeaderComponent',
  props: {
    pageTitle: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
header {
  h1 {
    font-size: 33px;
    line-height: 33px;
    font-weight: 700;
  }

  .lead {
    margin-top: 20px;
    font-size: 14px;
    line-height: 20px;
    font-weight: 400;
  }
}

</style>
