<template>
<div class="container">
  <div class="modal fade" 
  id="newAccountModal" 
  tabindex="-1" 
  aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" 
          id="exampleModalLabel">Criar Conta</h5>
          <button 
          type="button" 
          class="btn-close" 
          data-bs-dismiss="modal" 
          aria-label="Close">
          </button>
        </div>
        <div class="modal-body">
        <!-- FORM with fields NOME, EMAIL, SENHA -->
          <newuser-form 
          id="registerform" 
          :validation-schema="schema" 
          v-slot="{ errors }">
              <div class="mb-3">
                  <label class="form-label">Nome</label>
                  <newuser-field 
                  type="text" 
                  class="form-control" 
                  name="name" 
                  v-model="user.name" 
                  placeholder="ex.: Marcelo Coelho"/>
                  <span class="text-danger" 
                  v-text="errors.name" 
                  v-show="errors.name">
                  </span>
              </div>
              <div class="mb-3">
                  <label class="form-label">E-mail</label>
                  <newuser-field 
                  type="email" 
                  class="form-control" 
                  name="email" 
                  v-model="user.email" 
                  placeholder="ex.: mcoelho@email.com" />
                  <span 
                  class="text-danger" 
                  v-text="errors.email" 
                  v-show="errors.email">
                  </span>
              </div>
              <div class="mb-3">
                  <label class="form-label">Senha</label>
                  <newuser-field 
                  type="password" 
                  class="form-control" 
                  name="password" 
                  v-model="user.password" 
                  placeholder="ex.: 1#3A5k78"/>
                  <span 
                  class="text-danger" 
                  v-text="errors.password" 
                  v-show="showError" >
                  </span>
              </div>
          </newuser-form>
        </div>
        <!-- BUTTONS -->
        <div class="modal-footer">
          <button 
          type="button" 
          class="btn btn-outline-info" 
          data-bs-dismiss="modal">
          Sair
          </button>
          <button 
          type="button" 
          class="btn btn-info" 
          data-bs-dismiss="modal" 
          @click="setAccount">
          Enviar
          </button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>
<script>
import { Form, Field } from 'vee-validate'
import rules from '../validations/validateusers'

rules

export default {
    components: {
    "newuser-form": Form,
    "newuser-field": Field,
    },
    data() {
        return {

            schema: {
              name: 'required',
              email: 'required|emailcheck',
              password: 'required|password',
            },
            user: {} // Recebe os inputs
        }
    },
    methods: {

      // Cria nova conta
      setAccount() {
        // Envia novo usu??rio para store
        this.$store.commit('users/setAccount', {...this.user})
        let form = document.getElementById('registerform')
        form.reset()
        // Se houver successo na cria????o da conta
        let success = this.$store.state.users.success
        if (success) {
          this.$toast.info('Conta criada com sucesso!', {position: 'top-right'})
        }
        if (!success) {
          this.$toast.warning('E-mail j?? cadastrado.', {position: 'top-right'})
        }
      }
    },
    computed: {

      // Se, 0 < caracteres no input < 8, apresenta msg de erro
      showError() {
        if (this.user.password) {
          let show = this.user.password.length
          // 
          if (show > 0 && show < 8) {
            return true
          }
        }
        return false
      }
    }
}
</script>
<style scoped>

::placeholder {
  font-size: 12pt;
  color: rgb(204, 201, 201);
  font-style: italic;
  font-weight: 100;
}
</style>
