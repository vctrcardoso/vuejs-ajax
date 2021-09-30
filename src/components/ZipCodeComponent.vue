<template>
  <div class="container">
      <h1 v-text="title"></h1>

      <div class="row">
          <div class="col">
              <form @click.prevent="onSubmit" class="form form-inline justify-content-center mx-auto mt-5">
                  <input type="text" placeholder="Informe o cep" class="form-control" v-model="cep">
                  <button type="submit" class="btn btn-primary">Buscar CEP</button>
              </form>
          </div>
      </div>

      <div v-if="preloader">
          Carregando

      </div>

      <div class="mt-5 mx-auto" v-if="error != ''">
          {{error}}
      </div>
    
      <table v-show="address.cidade != '' " class="table mt-5">
  <thead>
    <tr>
      <th scope="col">Cidade</th>
      <th scope="col">Cep:</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>{{address.cidade}}</td>
      <td>{{address.cep}}</td>
    </tr>
  </tbody>
</table>
  </div>
</template>

<script>
export default {
    data () {
        return {
            title: 'Buscar CEP',
            cep: '',
            address: {
                cidade: ''
            },

            preloader: false,
            error: ''
        }
    },

    methods: {
        onSubmit () {
            this.reset()
            this.preloader = true
            this.$http.get('https://api.postmon.com.br/v1/cep/'+ this.cep).then(response =>{
                this.address = response.body
            }, error => {
                console.log(error)
                this.error = 'CEP errado'
                }).finally(() => {
                    this.preloader = false
                })
        },

        reset(){
            this.error = '',
            this.address.cidade = ''
        }
    }
}
</script>

<style>

</style>