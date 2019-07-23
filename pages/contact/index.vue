<template>
  <div class="section">
    <div class="card is-clearfix columns">
      <div class="card-content column is-two-thirds">
        <div class="card-content__title">
          <div class="column card-content__text">
            <div class="container col-md">
              <h1 class="title">Contact</h1>
              <div class="content col-md">
                <form
                  name="contact"
                  action="/thank-you"
                  netlify-honeypot="bot-field"
                  method="post"
                  netlify
                >
                  <div class="form-group">
                    <label class="form-label" for="name">Name:</label>
                    <input class="form-field form-control col-md-6" name="name" id="name" />
                  </div>
                  <div class="form-group">
                    <label class="form-label" for="email">Email:</label>
                    <input
                      class="form-field form-control col-md-6 form-control"
                      name="email"
                      id="email"
                    />
                  </div>
                  <div class="form-group">
                    <label class="form-label" for="message">Message:</label>
                    <textarea
                      class="form-field col-md-6 form-control"
                      rows="5"
                      name="message"
                      id="message"
                    ></textarea>
                  </div>
                  <input
                    class="form-button btn btn-primary btn-md"
                    type="submit"
                    value="Send message"
                  />
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>



<style lang="scss" scoped>
.card-content {
  padding: 15px 10px 15px 0;

  &__text {
    margin: 15px 0;
  }
  &__reviews {
    display: inline-block;
    width: 100%;
    margin-bottom: 10px;
  }
}
</style>

