<template>

<!-- stilizovanje Paket box komponente   -->
<div class="paketGlavni">
  <h4 class="imeGlavno" style="max-height:200px;margin-bottom:40px">{{title}}</h4>
  <div class="sadrzajPaketa" style="height: 440px;">
    <div class="tvSadrzaj" style="height: 250px;">

<!-- ikonica za tv deo -->
      <img class="ikonica" :src="tvUrl">

<!-- izvacenje tv karakteristika iz liste -->
      <div>
        <ul class="opcije" v-for="tvOpcija in tvOpcije">
          <li>
            <p> {{tvOpcija}}</p>
          </li>
        </ul>
      </div>

    </div>
    <div class="netSadrzaj" style="height:187.2px;">
<!-- ikonica za net deo -->
      <img class="ikonica" :src="netUrl">

<!-- izvacenje net karakteristika iz liste -->

      <div>
        <ul class="opcije" v-for="netOpcija in netOpcije">
          <li>
            <p class="text"> {{netOpcija}}</p>
          </li>
        </ul>
      </div>
    </div>

  </div>

<!-- slot na cije mesto ce se ubaciti komponenta promotions i cene -->
  <slot name="promoPaketBox"></slot>

  <button class="dugmeNarucite">Naručite</button>
</div>
</template>

<script>
export default {
  data() {

    return {
      title: '',
      tvUrl: '',
      netUrl: '',
      contract: {},
      tvOpcije: [],
      netOpcije: []
    }

  },

// izvlacenje potrebnih podataka iz JSON objekta

  mounted() {

    fetch('http://www.mocky.io/v2/5e8465c23000008400cf4395', {
        method: 'GET'
      }).then(response => response.json())
      .then(json => {
        json.items[0].included.forEach((item) => {
          if (item.product_category == "tv") {
            this.tvOpcije.push(item.long_name)
          } else {
            this.netOpcije.push(item.long_name);
          }
        });
        this.title = json.items[0].name;
        this.tvUrl = json.assets.tv_category;
        this.netUrl = json.assets.net_category;
      })
  }

}
</script>
