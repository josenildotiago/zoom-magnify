<template>
  <div>
    <div class="flex1-center">
      <div class="card1 flex1-center">
        <section ref="productImg" class="product-img">
          <img src="../dist/blue.jpg" alt="ajuste">
          <div ref="magnifierLens" class="magnifier-lens"></div>
        </section>
        <section class="product-details">
          <div ref="magnifiedImg" class="magnified-img"></div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ZoomMagnify',
  data() {
    return {
    }
  },
  mounted() {
    console.log("Component mounted Home. ");
    const lens = this.$refs.magnifierLens;
    const productImg = this.$refs.productImg;
    const magnifiedImg = this.$refs.magnifiedImg;
    this.magnify(lens, productImg, magnifiedImg);
  },
  methods: {
    magnify(lens, product_img) {
      lens.addEventListener('mousemove', this.moveLens);
      product_img.addEventListener('mousemove', this.moveLens);
      lens.addEventListener('mouseout', this.leaveLens);
    },
    moveLens(e) {
      // console.log("X: " + e.pageX + " Y:" + e.pageY);
      const lens = this.$refs.magnifierLens;
      const productImgRect = this.$refs.productImg.getBoundingClientRect();
      const productImg = this.$refs.productImg;
      const magnifiedImg = this.$refs.magnifiedImg;

      let x, y, cx, cy;
      x = e.pageX - productImgRect.left - lens.offsetWidth / 2;
      y = e.pageY - productImgRect.top - lens.offsetHeight / 2;
      // console.log(lens.offsetWidth)

      let max_xpos = productImgRect.width - lens.offsetWidth;
      let max_ypos = productImgRect.height - lens.offsetHeight;

      if (x > max_xpos) x = max_xpos;
      if (x < 0) x = 0;
      if (y > max_ypos) y = max_ypos;
      if (y < 0) y = 0;

      lens.style.cssText = `top: ${y}px; left: ${x}px`;

      cx = magnifiedImg.offsetWidth / lens.offsetWidth;
      cy = magnifiedImg.offsetHeight / lens.offsetHeight;

      magnifiedImg.style.backgroundImage = `url('${productImg.firstChild.src}')`;
      magnifiedImg.style.backgroundPosition = `-${x * cx}px -${y * cy}px`;
      magnifiedImg.style.backgroundSize = `
        ${productImgRect.width * cx}px
        ${productImgRect.height * cy}px
      `;
      magnifiedImg.style.backgroundRepeat = `no-repeat`;
      lens.classList.add('active');
      magnifiedImg.classList.add('active');
    },
    leaveLens() {
      const lens = this.$refs.magnifierLens;
      const magnifiedImg = this.$refs.magnifiedImg;
      lens.classList.remove('active');
      magnifiedImg.classList.remove('active');

    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.flex1-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.card1 {
  width: 95%;
  height: 60%;
  background-color: blue;
}

section.product-img {
  position: relative;
  width: 40%;
}

section.product-details {
  position: relative;
  width: 60%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

section.product-img img {
  width: 100%;
  height: auto;
}

.magnifier-lens {
  position: absolute;
  top: 0;
  left: 0;
  width: 150px;
  height: 100px;
  background-color: #ff980030;
  border: .1px solid orange;
  opacity: 0;
}

.magnifier-lens.active {
  opacity: 1;
}

.magnified-img {
  position: absolute;
  width: 500px;
  height: 380px;
  border: 2px solid orange;
  transform: scale(0.5);
  opacity: 0;
  transition: opacity .5s, transform .5s;
}

.magnified-img.active {
  opacity: 1;
  transform: scale(1);
}
</style>
