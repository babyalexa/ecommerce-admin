<template>
  <div class="card">
    <div class="card-header">
      <p class="card-header-title">Products</p>
    </div>
    <div class="card-content">
      <div class="content">
        <table class="table">
          <thead>
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Image URL</th>
              <th>Price</th>
              <th>Stock</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(product, index) in $store.state.products"
              :key="product.id"
            >
              <td>{{ index + 1 }}</td>
              <td>{{ product.name }}</td>
              <td><a @click="showImage(product.image_url)">show</a></td>
              <td>{{ product.price }}</td>
              <td>{{ product.stock }}</td>
              <td>
                <a @click="updateProduct(product.id)">
                  <i class="mdi mdi-file-edit" aria-hidden="true"></i>
                </a>
                |
                <a @click="deleteProduct(product.id)">
                  <i class="mdi mdi-trash-can-outline" aria-hidden="true"></i>
                </a>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <b-modal :active.sync="isImageModalActive">
      <p class="image is-4by3">
        <img :src="image_url" />
      </p>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'Products',
  data: function() {
    return {
      isImageModalActive: false,
      image_url: null
    }
  },
  created: function() {
    this.$store
      .dispatch('getProducts')
      .then(() => {})
      .catch(() => {
        this.$buefy.toast.open({
          message: 'Error loading data.',
          type: 'is-danger'
        })
      })
  },
  methods: {
    deleteProduct: function(id) {
      this.$buefy.dialog.confirm({
        title: 'Deleting Product',
        message:
          'Are you sure you want to <b>delete</b> this product? This action cannot be undone.',
        confirmText: 'Delete Product',
        type: 'is-danger',
        hasIcon: true,
        onConfirm: () => {
          // dispatch
          this.$store
            .dispatch('deleteProduct', id)
            .then(() => {
              this.$buefy.toast.open('Product deleted!')
            })
            .catch(() => {})
        }
      })
    },
    updateProduct: function(id) {
      this.$router.push(`/dashboard/update/${id}`)
    },
    showImage: function(image_url) {
      this.isImageModalActive = true
      this.image_url = image_url
    }
  }
}
</script>
