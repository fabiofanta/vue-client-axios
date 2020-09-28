<template>
    <div class="container mt-5">
        <form @submit.prevent= "sendToEndPoint()">
            <fieldset>
              <legend>Aggiungi Prodotti</legend>
              <div class="form-group">
                  <label for="productCode">Codice</label>
                  <input v-model="params.codice" type="text" class="form-control" id="productCode" aria-describedby="productCode" placeholder="Inserisci il codice prodotto"
                  required="required" pattern="(?=.*\d)(?=.*[a-zA-Z])(?=.*^[^\W_]+$).{1,}">
              </div>
              <div class="form-group">
                  <label for="descriptionTextarea">Descrizione</label>
                  <textarea v-model="params.descrizione" class="form-control" id="descriptionTextarea" rows="2" placeholder="Inserisci la descrizione prodotto" required="required"
                  ></textarea>
              </div>
              <div class="form-group">
                  <label class="control-label">Prezzo</label>
                  <div class="form-group">
                      <div class="input-group mb-3">
                          <div class="input-group-prepend">
                              <span class="input-group-text">€</span>
                          </div>
                          <input v-model="params.prezzo" type="text" class="form-control" aria-label="Amount (to the nearest euro)" required="required" pattern="[0-9]{1,}">
                          <div class="input-group-append">
                              <span class="input-group-text">.00</span>
                          </div>
                      </div>
                  </div>
              </div>
              <div class="form-group">
                  <label for="barCode">Barcode</label>
                  <input v-model="params.barcode" type="text" class="form-control" id="barCode" aria-describedby="barCode" placeholder="Inserisci il Barcode" required="required" pattern="(?=.*\d)(?=.*[a-zA-Z])(?=.*^[^\W_]+$).{1,}">
              </div>
              <div class="form-group">
                <label for="category">Categoria</label>
                <select v-model="params.categoria" class="form-control" id="category" required="required">
                  <option v-for="option in options" :disabled="option.disabled"  :key="option.text">
                    {{ option.text }}
                  </option>
                </select>
              </div>
              <div v-if= "params.categoria !== null" class="form-group">
                <label for="subcategory">Sottocategoria</label>
                <select v-model="params.sottocategoria" class="form-control" id="subcategory" required="required">
                  <option v-for="option in options.[index()].subs" v-bind:key="option">
                    {{ option }}
                  </option>
                </select>
              </div>
              <button type="submit" class="btn btn-primary">Submit</button>
            </fieldset>
        </form>
    </div>
    <div v-if= "status == 'ERROR'" class="alert alert-dismissible alert-danger">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        <strong>Attenzione!</strong> {{ message }}
    </div>
    <div v-if= "status == 'OK'" class="alert alert-dismissible alert-success">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        <strong>Ben fatto!</strong> {{ message }}
    </div>
</template>

<script>

import axios from 'axios';

export default {
    data() {
        return {
        params: {
            cf: 'GVGFGH44R12I804L',
            progetto: 'C',
            codice: null,
            descrizione: null,
            prezzo: null,
            barcode: null,
            categoria: null,
            sottocategoria: null,
        },
        options: [
            { text: 'Seleziona una opzione', disabled: true},
            { text: 'cat1', subs:['cat1.1','cat1.2']},
            { text: 'cat2', subs:['cat2.1','cat2.2']},
        ],
        status: '',
        message: ''
       }
   },
   methods: {
       index() {
            var self = this;
            return self.params.categoria.slice(3,4)
        },
        sendToEndPoint() {
            var self = this;
            let test = self.params;
            if (test.categoria.match(/[cat]./) && test.sottocategoria.match(/[cat]./)) {
                //
            } else {
                self.status = "ERROR";
                self.message = "Hai provato a modificare l'HTML";
                throw "modifica HTML"
            }
            if (test.descrizione.match(/^[^><]+$/)) {
                //
            } else {
                self.status = "ERROR";
                self.message = "Nel campo descrizione non sono ammissibili ><";
                throw "Anti script"
            }
            axios.get('https://dev.gestisconet.it/candidate/endpoint.php', {
                params: self.params,
			})
            .then(function (response) {
                self.status = response.data.status;
                self.message = response.data.message;
			});
       }
   }
}
</script>

<style scoped>
@import url(https://bootswatch.com/4/minty/bootstrap.min.css);
.alert {
    margin-top: 20px;
}
</style>
