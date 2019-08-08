<template>
  <div class="footer">
    <div class="columns container center">
      <div class="column has-text-justify">
        <h6 class="title">Address</h6>
        <p>
          <span class="icon">
            <i class="fa fa-map-marker"></i>
          </span>
        </p>
        <p>UDOCALIZG GLOBAL ENTERPRISE</p>
        <p>
          No 10, Hon. Uche Anyanwu Crescent,
          <br />Road 1 Imo Housing extension,
          <br />Owerri, Imo State. Nigeria
        </p>
        <br />
        <a href="#">
<<<<<<< HEAD
          <p>MailArts ❤ From Arts Love World | &trade;</p>
=======
          <p>With ❤ From Arts Love World | &trade;</p>
>>>>>>> e24c0913ed5498d0513f3f28fbd8dae67792809d
        </a>
        <br />
        <a href="#">
          <p>&copy; Copyright 2019, All Rights Reserve.</p>
        </a>
      </div>

      <div class="column has-text-center">
        <div>
<<<<<<< HEAD
          <h6 class="title">MailArts 24/7 Support</h6>
=======
          <h6 class="title">24/7 Support</h6>
>>>>>>> e24c0913ed5498d0513f3f28fbd8dae67792809d
          <nuxt-link :to="{ name: 'contact' }">
            <h3 class="subtitle">
              <span class="icon">
                <i class="fa fa-envelope-o"></i>
              </span> support@artsloveworld.store
            </h3>
          </nuxt-link>
        </div>
        <br />
        <div>
          <p>
            <span class="icon">
              <i class="fa fa-phone"></i>
            </span> +2349017318250
            <br />
            <span class="icon">
              <i class="fa fa-phone"></i>
            </span> +2348038789872
            <br />
          </p>
        </div>

        <br />
        <div>
          <h3 class="title">Satisfaction Guaranteed</h3>
          <p class="font-weight-light">
            We are incredibly proud of our craftsmanship and stand by our product 100%,
            no matter what, no questions asked, no holds barred, no ifs, no ands, no buts.
            If you have a problem, we will solve it. Refund it. Send you new Paintings.
            Whatever it takes. Just reach out to our support team.
          </p>
        </div>
      </div>

      <div class="column has-text-justify">
        <h6 class="title">Links</h6>
        <p>
          <nuxt-link :to="{ name: 'index' }">
            <h3>
              <span class="icon">
                <i class="fa fa-home"></i>
              </span>Home
            </h3>
          </nuxt-link>
        </p>
        <br />
        <p>
          <nuxt-link :to="{ name: 'policy' }">
            <h3>
              <span class="icon">
                <i class="fa fa-credit-card"></i>
              </span>Sales Policy
            </h3>
          </nuxt-link>
        </p>
        <br />

        <p>
          <nuxt-link :to="{ name: 'contact' }">
            <h3>
              <span class="icon">
                <i class="fa fa-inbox"></i>
              </span>Contact Us
            </h3>
          </nuxt-link>
        </p>
        <br />
        <p>
          <nuxt-link :to="{ name: 'terms' }">
            <h3>
              <span class="icon">
                <i class="fa fa-file-pdf-o"></i>
              </span>Terms And Conditions
            </h3>
          </nuxt-link>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VmFooter"
};
</script>

<style lang="scss" scoped>
.footer {
  background: #b9e2fc;
  color: black;
}
</style>
