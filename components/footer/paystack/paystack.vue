<template>
    <div id="app">
        <paystack
            :amount="amount"
            :email="email"
            :paystackkey="paystackkey"
            :reference="reference"
            :callback="callback"
            :close="close"
            :embed="false"
        >
            <i class="fas fa-money-bill-alt"></i>
            Make Payment
        </paystack>
    </div>

</template>

  <script>
    new Vue({
        el: '#app',
        components:{
          'paystack': VuePaystack.default
        },
        computed: {
        reference(){
          let text = "";
          let possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
          for( let i=0; i < 10; i++ )
            text += possible.charAt(Math.floor(Math.random() * possible.length));
          return text;
        }
        },
        methods: {
          callback: function(response){
            console.log(response)
          },
          close: function(){
            console.log("Payment closed")
          }
        },
        data: {
          paystackkey: "pk_test_xxxxxxxxxxxxxxxxxxxxxx",
          email: "foobar@example.com",
          amount: 1000000,
        }
    });
    </script>
<style>
    .App {
        text-align: center;
    }
    .App-intro {
        font-size: large;
    }
    .payButton{
        color: #61DAFB;
        width: 250px;
        height: 50px;
        font-weight: 800;
    }
</style>