<template>
<div id="app">

  <h4 class="titl">{{title}}</h4>

<!-- selekt opcija za duzinu paketa -->
  <select  class="selekcija "v-model="selected">

    <option class="opcija" v-for="contract in contractOptions">
      {{ contract }}
    </option>

  </select>

<!-- stilizovanje glavnog prikaza -->
  <section class="glavniPrikaz">

        <PaketBox>

<!-- popunjavanje slota iz komponente -->
          <div slot="promoPaketBox" class="single-product">

<!-- ubacivanje promo slike ako je izabrana opcija 24 meseca -->
            <promotions v-if="selected=='Ugovor 24 meseca'"></promotions>

            <h4 class="sveCene">
              <div class="sveCeneZajedno" >

<!-- prikaz cena u zavisnosti od duzine trajanja ugovora -->
                <div class="glavneCene" v-if="selected=='Ugovor 24 meseca'">
                <div class="staraCena"><span class="selected-price">{{Math.round(Number(jsonObjekat.items[0].prices.old_price_recurring['Ugovor 24 meseca']))}}</span><span class="monthly"> rsd/mes.</span></div>
                <div class="trenutnaCena"><span class="selected-price">{{Math.round(Number(jsonObjekat.items[0].prices.price_recurring['Ugovor 24 meseca']))}}</span><span class="monthly"> rsd/mes.</span></div>
                </div>
                <div v-if="selected=='Ugovor 24 meseca'" v-html="jsonObjekat.items[0].prices.old_price_promo_text" class="sestMeseci"></div>
                <div v-if="selected=='Ugovor 12 meseci'" class="trenutnaCena"><span class="selected-price">{{jsonObjekat.items[0].prices.price_recurring['Ugovor 12 meseci']}}</span><span class="monthly"> rsd/mes.</span></div>
                <div v-if="selected=='Bez ugovorne obaveze'" class="trenutnaCena"><span class="selected-price">{{jsonObjekat.items[0].prices.price_recurring['Bez ugovorne obaveze']}}</span><span class="monthly"> rsd/mes.</span></div>

              </div>
            </h4>

          </div>

        </PaketBox>

        <BoxDupli>

<!-- popunjavanje slota iz komponente -->
          <div slot="promoBoxDupli" class="single-product">

<!-- ubacivanje promo slike ako je izabrana opcija 24 meseca -->
            <promotions v-if="selected=='Ugovor 24 meseca'"></promotions>

            <h4 class="sveCene">
              <div class="sveCeneZajedno">

<!-- prikaz cena u zavisnosti od duzine trajanja ugovora -->
                <div class="glavneCene" v-if="selected=='Ugovor 24 meseca'">
                <div class="staraCena"><span class="selected-price">{{Math.round(Number(jsonObjekat.items[1].prices.old_price_recurring['Ugovor 24 meseca']))}}</span><span class="monthly"> rsd/mes.</span></div>
                <div class="trenutnaCena"><span class="selected-price">{{Math.round(Number(jsonObjekat.items[1].prices.price_recurring['Ugovor 24 meseca']))}}</span><span class="monthly"> rsd/mes.</span></div>
                </div>
                <div v-if="selected=='Ugovor 24 meseca'" v-html="jsonObjekat.items[1].prices.old_price_promo_text" class="sestMeseci"></div>
                <div v-if="selected=='Ugovor 12 meseci'" class="trenutnaCena"><span class="selected-price">{{jsonObjekat.items[1].prices.price_recurring['Ugovor 12 meseci']}}</span><span class="monthly"> rsd/mes.</span></div>
                <div v-if="selected=='Bez ugovorne obaveze'" class="trenutnaCena"><span class="selected-price">{{jsonObjekat.items[1].prices.price_recurring['Bez ugovorne obaveze']}}</span><span class="monthly"> rsd/mes.</span></div>

              </div>
            </h4>

          </div>

        </BoxDupli>

        <BoxMax>

<!-- popunjavanje slota iz komponente -->
          <div slot="promoBoxMax" class="single-product">

<!-- ubacivanje promo slike ako je izabrana opcija 24 meseca -->
            <promotions v-if="selected=='Ugovor 24 meseca'"></promotions>

            <h4 class="sveCene">
              <div class="sveCeneZajedno">

<!-- prikaz cena u zavisnosti od duzine trajanja ugovora -->
                <div class="glavneCene" v-if="selected=='Ugovor 24 meseca'">
                  <div class="staraCena"><span class="selected-price">{{Math.round(Number(jsonObjekat.items[2].prices.old_price_recurring['Ugovor 24 meseca']))}}</span><span class="monthly"> rsd/mes.</span></div>
                  <div class="trenutnaCena"><span class="selected-price">{{Math.round(Number(jsonObjekat.items[2].prices.price_recurring['Ugovor 24 meseca']))}} </span><span class="monthly"> rsd/mes.</span></div>
                </div>
                  <div v-if="selected=='Ugovor 24 meseca'" v-html="jsonObjekat.items[2].prices.old_price_promo_text" class="sestMeseci"></div>
                 <div v-if="selected=='Ugovor 12 meseci'" class="trenutnaCena"><span class="selected-price">{{jsonObjekat.items[2].prices.price_recurring['Ugovor 12 meseci']}}</span><span class="monthly"> rsd/mes.</span></div>
                 <div v-if="selected=='Bez ugovorne obaveze'" class="trenutnaCena"><span class="selected-price">{{jsonObjekat.items[2].prices.price_recurring['Bez ugovorne obaveze']}}</span><span class="monthly"> rsd/mes.</span></div>

              </div>
            </h4>

          </div>

        </BoxMax>

  </section>

</div>
</template>



<script>

// importovanje prethodno napravljenih komponenti

import PaketBox from './components/PaketBox.vue'
import BoxDupli from './components/BoxDupli.vue'
import BoxMax from './components/BoxMax.vue'
import promotions from './components/promotions.vue'

export default {
  name: 'app',

  data() {
    return {
      selected: '',
      listaSadrzajaPaketBox: [],
      contractOptions: [],
      jsonObjekat: null,
      title: 'Izaberite paket',

    }
  },

// izvlacenje potrebnih podataka iz JSON-a za potrebne varijable
  mounted() {

    fetch('http://www.mocky.io/v2/5e8465c23000008400cf4395', {
        method: 'GET'
      }).then(response => response.json())
      .then(json => {

        json.contract_length.contract_length_options.forEach((item) => {
          this.contractOptions.push(item)
        });

        this.selected = json.contract_length.preselected_contract_length;

        this.jsonObjekat = json;

        json.items[0].included.forEach((item) => {
          this.listaSadrzajaPaketBox.push(item.long_name);
        });

      })
  },

  components: {
    PaketBox,
    BoxDupli,
    BoxMax,
    promotions

  }

}
</script>
