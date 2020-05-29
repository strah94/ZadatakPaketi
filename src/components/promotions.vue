<template>

<!-- template za promo deo 24 meseca, koji se ubacuje u slotove kod paket komponenti -->
<div class="promocijaKomp" style="height: 187.2px;">

    <img class="tvPromoSlika" :src="imgUrl">

    <p class="tekstPromocije" v-html="promotionText" ></p>

</div>


</template>



<script>
export default {
  data() {

    return {
      imgUrl:'',
      promotionText:''

    }

  },
  
  mounted() {

    fetch('http://www.mocky.io/v2/5e8465c23000008400cf4395', {
        method: 'GET'
      }).then(response => response.json())
      .then(json => {
// izvlacenje podataka za tekst i sliku iz Json objekta
           this.imgUrl=json.items[0].promotions[0].promotion_image;
           this.promotionText=json.items[0].promotions[0].promo_text;

      })
  }

}
</script>
