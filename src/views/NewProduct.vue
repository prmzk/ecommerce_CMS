<template>
  <div class="d-flex container pl-5 pr-5 pt-4">
      <div class="d-flex flex-column align-items-center flex-grow-1">
        <h2 class="display-5">Add a New Product</h2>
        <form class="mt-4 flex-grow-1 pb-4 d-flex flex-column">
          <div class="form-group">
            <input type="text" class="form-control pt-4 pb-4" placeholder="Name" v-model="newProduct.name">
          </div>
          <div class="form-row d-flex">
            <div class="d-flex flex-column col-6">
              <small class="ml-1">Price</small>
              <div class="col p-1">
                <input type="number" class="form-control pt-4 pb-4" placeholder="Price"  v-model="price">
              </div>
            </div>
            <div class="d-flex flex-column col-6">
              <small class="ml-1">Stocks</small>
              <div class="col p-1">
              <input type="number" class="form-control pt-4 pb-4" placeholder="Stock"  v-model="stock">
              </div>
            </div>
          </div>
          <div class="form-row img-row pb-3 pt-3">
            <div class="col-4 d-flex">
              <img :src="imgUrl" alt="" style="width: 12rem;">
            </div>
            <div class="custom-file col-4">
              <small>Image url (paste the img link here)</small>
              <input type="text" class="form-control pt-4 pb-4" v-model="imgUrlShow">
            </div>
            <div class="col-4 d-flex align-items-end justify-content-end flex-column">
              <small>Category:</small>
              <input class="pl-2" type="text" list="cats" v-model="newProduct.category" placeholder="Type to add new category"/>
              <datalist id="cats">
                <option v-for="(category, i) in categoryList.data" :key="i" :value="category" ></option>
              </datalist>
            </div>
          </div>
          <div class="form-group">
            <textarea class="form-control" id="exampleFormControlTextarea1" rows="3" placeholder="Description"  v-model="newProduct.description"></textarea>
          </div>
          <div class="form-check">
            <input type="checkbox" class="form-check-input" v-model="newProduct.highlighted">
            <label class="form-check-label" for="exampleCheck1">Highlight this product!</label><br>
            <small>Highlighted product will be shown on your banner.</small>
          </div>
          <div class="d-flex align-self-end justify-self-end mb-4" style="margin-top: auto">
            <button type="submit" class="btn btn-primary pt-1 pb-1 pr-4 pl-4" @click.prevent="addProduct">Add</button>
          </div>
        </form>
      </div>
      <loading-now v-if="nowLoading"></loading-now>
  </div>
</template>

<script>
import LoadingNow from '../components/LoadingNow'
export default {
  data () {
    return {
      imgUrl: 'https://2x5ito1uusjd19czwpsrbt7c-wpengine.netdna-ssl.com/wp-content/themes/total-child-theme/assets/images/common/image-placeholder.png'
    }
  },
  methods: {
    addProduct () {
      this.$store.dispatch('createProduct', this.newProduct)
    }
    // localUrl (e) {
    //   const files = e.target.files || e.dataTransfer.files
    //   const file = files[0]
    //   // https://api.cloudinary.com/v1_1/dsgjtva0h/image/upload?file=asd&upload_preset=khmjrywl
    //   var reader = new FileReader()
    //   reader.onloadend = () => {
    //     axios({
    //       url: 'https://api.cloudinary.com/v1_1/dsgjtva0h/image/upload',
    //       method: 'POST',
    //       params: {
    //         file: reader.result,
    //         upload_preset: 'bsxlq7xo'
    //       }
    //     })
    //       .then(result => {
    //         this.imgUrl = result.data.url
    //         this.$store.state.newProduct.img_url = result.data.url
    //       })
    //   }
    //   reader.readAsDataURL(file)
    // }
  },
  created () {
    this.newProduct.store_id = +localStorage.getItem('store_id')
    this.$store.dispatch('fetchCategories')
  },
  computed: {
    price: {
      set: function (price) {
        this.newProduct.price = +price
      },
      get: function () {
        return this.newProduct.price
      }
    },
    stock: {
      set: function (stock) {
        this.newProduct.stock = +stock
      },
      get: function () {
        return this.newProduct.stock
      }
    },
    nowLoading () {
      return this.$store.state.nowLoading
    },
    newProduct () {
      return this.$store.state.newProduct
    },
    categoryList () {
      return this.$store.state.categoryList
    },
    imgUrlShow: {
      get: function () {
        const placeholderImg = 'https://2x5ito1uusjd19czwpsrbt7c-wpengine.netdna-ssl.com/wp-content/themes/total-child-theme/assets/images/common/image-placeholder.png'
        if (this.imgUrl === placeholderImg) {
          return 'Default Picture'
        } else {
          return this.imgUrl
        }
      },
      set: function (newValue) {
        const placeholderImg = 'https://2x5ito1uusjd19czwpsrbt7c-wpengine.netdna-ssl.com/wp-content/themes/total-child-theme/assets/images/common/image-placeholder.png'
        if (!newValue) {
          this.$store.state.newProduct.img_url = placeholderImg
          this.imgUrl = placeholderImg
        } else {
          this.$store.state.newProduct.img_url = newValue
          this.imgUrl = newValue
        }
      }
    }
  },
  components: {
    LoadingNow
  }
}
</script>

<style>
.img-row {
  min-height: 12rem !important;
}
</style>
