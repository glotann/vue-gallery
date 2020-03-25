<template>
  <div class="container">
    <div class="row">

      <div class="col-md-12">
        <h2>Vue Mini Gallery</h2>

        <template>
            <div>
              <b-carousel
                id="carousel-1"
                v-model="slide"
                :interval="10000"
                controls
                indicators
                background="#ababab"
                img-width="1024"
                img-height="720"
                style="text-shadow: 1px 1px 2px #333;"
                @sliding-start="onSlideStart"
                @sliding-end="onSlideEnd"
              >
                  <b-carousel-slide
                  v-for="item in items" v-bind:key="item.image"
                  :caption="item.title"
                  :text="item.desc"
                  :img-src="item.image"
                  ></b-carousel-slide>

              </b-carousel>
            </div>
            <div><p></p></div>
            <div>
              <b-button v-b-modal.modal-prevent-closing>Add Gallery</b-button>

              <b-modal 
                id="modal-prevent-closing"
                ref="modal"
                title="Add Image to Gallery"
                @show="resetModal"
                @hidden="resetModal"
                @ok="handleOk"
              >
                  <form ref="form" @submit.stop.prevent="handleSubmit">

                    <b-form-group
                      :state="titleState"
                      label="Title"
                      label-for="title-input"
                      invalid-feedback="Title is required"
                    >
                      <b-form-input
                        id="title-input"
                        v-model="title"
                        :state="titleState"
                        required
                      ></b-form-input>
                    </b-form-group>

                    <b-form-group
                      :state="imageState"
                      label="Image URL"
                      label-for="image-input"
                      invalid-feedback="Image URL is required"
                    >
                      <b-form-input
                        id="image-input"
                        v-model="image"
                        :state="imageState"
                        required
                      ></b-form-input>
                    </b-form-group>

                    <b-form-group
                      :state="descState"
                      label="Description"
                      label-for="desc-input"
                      invalid-feedback="Description is required"
                    >
                      <b-form-input
                        id="desc-input"
                        v-model="desc"
                        :state="descState"
                        required
                      ></b-form-input>
                    </b-form-group>

                  </form>
              </b-modal>

            </div>
          </template>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
      return {
        items : 
        [
          {title: 'Amsterdam', image: 'https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/KeizersgrachtReguliersgrachtAmsterdam.jpg/1200px-KeizersgrachtReguliersgrachtAmsterdam.jpg', desc: 'Amsterdam is the Netherlands’ capital, known for its artistic heritage, elaborate canal system and narrow houses with gabled facades, legacies of the city’s 17th-century Golden Age.'},

          {title: 'Venice', image: 'https://i.dmarge.com/2020/03/venice-image-feature.jpg', desc:'Venice, the capital of northern Italy’s Veneto region, is built on more than 100 small islands in a lagoon in the Adriatic Sea.'},

          {title: 'London', image: 'https://lp-cms-production.imgix.net/2019-06/55425108.jpg?fit=crop&q=40&sharp=10&vib=20&auto=format&ixlib=react-8.6.4', desc:'London, the capital of England and the United Kingdom, is a 21st-century city with history stretching back to Roman times.'},

          {title: 'Paris', image: 'https://www.fodors.com/wp-content/uploads/2018/10/HERO_UltimateParis_Heroshutterstock_112137761.jpg', desc: 'Paris, France capital, is a major European city and a global center for art, fashion, gastronomy and culture. '},

          {title: 'Prague', image: 'https://cdn.anadventurousworld.com/wp-content/uploads/2019/01/where-to-stay-in-prague.jpg', desc: 'Prague, capital city of the Czech Republic, is bisected by the Vltava River.'}
        ],
        slide: 0,
        sliding: null,
        title: '',
        titleState: null,
        
        desc: '',
        descState: null,
        
        image: '',
        imageState: null,
        
      }
  },

  methods: {
      onSlideStart() {
        this.sliding = true
      },
      onSlideEnd() {
        this.sliding = false
      },

      checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        
        this.titleState = valid
        this.descState = valid
        this.imageState = valid
        return valid
      },
      resetModal() {
        
        this.title = ''
        this.desc = ''
        this.image = ''
        
        this.titleState = null
        this.descState = null
        this.imageState = null
      },
      handleOk(bvModalEvt) {
        // Prevent modal from closing
        bvModalEvt.preventDefault()
        // Trigger submit handler
        this.handleSubmit()
      },
      handleSubmit() {
        // Exit when the form isn't valid
        if (!this.checkFormValidity()) {
          return
        }

        // Push the name to submitted names
        this.items.push({title: this.title, image: this.image, desc: this.desc})

        // Hide the modal manually
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
      }
  },
}
</script>
