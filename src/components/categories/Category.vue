<template>
  <div class="form-elements">
    <div class="row category">
      <div class="col-sm-12">
        <vuestic-widget :headerText="'Categories'">
          <div class="row">
            <div class="col-md-6">
              <form v-if="!editForm" @submit.prevent="saveCategory">
                <div class="row">
                  <div class="col-md-5">
                    <fieldset>
                      <div class="form-group">
                        <div class="input-group">
                          <input v-model="newCategory.name" id="simple-input" required>
                          <label class="control-label" for="simple-input">Add new category</label>
                          <i class="bar"></i>
                        </div>
                      </div>
                      <!-- <div class="form-group with-icon-right"
                       :class="{'has-error': errors.has('successfulEmail'), 'valid': isSuccessfulEmailValid}">
                    <div class="input-group">
                      <input
                        id="successfulEmail"
                        name="successfulEmail"
                        v-model="successfulEmail"
                        v-validate="'required|email'"
                        required/>
                      <i
                        class="fa fa-exclamation-triangle error-icon icon-right input-icon"></i>
                      <i
                        class="fa fa-check valid-icon icon-right input-icon"></i>
                      <label class="control-label" for="successfulEmail">{{'forms.inputs.emailValidatedSuccess'
                        | translate}} </label><i
                      class="bar"></i>
                      <small v-show="errors.has('successfulEmail')"
                             class="help text-danger">
                        {{ errors.first('successfulEmail') }}
                      </small>
                    </div>
                  </div>
                  <div class="form-group with-icon-right"
                       :class="{'has-error': errors.has('wrongEmail')}">
                    <div class="input-group">
                      <input
                        id="wrongEmail"
                        name="wrongEmail"
                        v-model="wrongEmail"
                        v-validate="'required|email'"
                        required/>
                      <i
                        class="fa fa-exclamation-triangle icon-right input-icon"
                        v-show="errors.has('wrongEmail')"></i>
                      <label class="control-label" for="wrongEmail">{{'forms.inputs.emailValidated'
                        | translate}}</label><i class="bar"></i>
                      <small v-show="errors.has('wrongEmail')"
                             class="help text-danger">{{
                        errors.first('wrongEmail')
                        }}
                      </small>
                    </div>
                      </div>-->
                      <button
                        type="submit"
                        class="btn btn-primary btn-micro font-weight-bold"
                      >Save Category</button>
                    </fieldset>
                  </div>
                </div>
              </form>

              <form v-else @submit.prevent="editCategory">
                <div class="row">
                  <div class="col-md-4">
                    <fieldset>
                      <div class="form-group">
                        <div class="input-group">
                          <input v-model="editedCategory.name" id="simple-input" required>
                          <label class="control-label" for="simple-input">Edit category</label>
                          <i class="bar"></i>
                        </div>
                      </div>
                      <button
                        type="submit"
                        class="btn btn-primary btn-micro font-weight-bold"
                      >Edit Category</button>
                    </fieldset>
                  </div>
                </div>
              </form>
            </div>

            <div class="col-xs-6 col-md-6">
              <div class="table-responsive">
                <table class="table table-striped first-td-padding loading">
                  <thead>
                    <tr>
                      <td>SL</td>
                      <td>NAME</td>
                      <td>ACTION</td>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(category, index) in categories" :key="index">
                      <td>{{ category.id }}</td>
                      <td>{{ category.name }}</td>
                      <td>
                        <span class="edit" @click="editFormProcess(index, category)">
                          <i data-v-b0748c0c class="fa fa-edit"></i>
                        </span> &nbsp; &nbsp;
                        <span class="delete">
                          <i data-v-b0748c0c class="fa fa-trash"></i>
                        </span>
                      </td>
                    </tr>
                  </tbody>
                </table>
                <!-- <div  class="loader">
                    <spring-spinner
                      class="animation"
                      slot="loading"
                      v-if="loader"
                      :animation-duration="2500"
                      :size="50"
                      color="#4ae387"
                    />
                </div>-->
              </div>
            </div>
          </div>
        </vuestic-widget>
      </div>
    </div>
  </div>
</template>

<script>
import CountriesList from "data/CountriesList";
import axios from "axios";

import { SpringSpinner } from "epic-spinners";

// Vue.component('badge-column', BadgeColumn)

export default {
  name: "categories",
  components: {
    SpringSpinner
  },
  computed: {
    datePickerDisabled: () => [date => !(date.getDate() % 5)],
    isSuccessfulEmailValid() {
      let isValid = false;
      if (this.formFields.successfulEmail) {
        isValid =
          this.formFields.successfulEmail.validated &&
          this.formFields.successfulEmail.valid;
      }
      return isValid;
    }
  },
  data() {
    return {
      loader: true,
      editForm: false,
      successfulEmail: "andrei@dreamsupport.io",
      wrongEmail: "andrei@dreamsupport",
      categories: [],
      newCategory: {
        name: ""
      },
      editedCategory: {},
      token: localStorage.getItem("token"),
      headers: {
        "Content-Type": "application/json",
        Authorization: "Bearer " + this.token
      }
    };
  },
  methods: {
    clear(field) {
      this[field] = "";
    },
    saveCategory() {
      var self = this;
      axios
        .post("http://192.168.10.10/api/categories", self.newCategory, {
          headers: self.headers
        })
        .then(response => {
          this.getCategories();
        })
        .catch(error => {
          console.log(error.response);
        });
      // this.newCategory.name = '';
    },

    getCategories() {
      this.loader = true;
      var self = this;
      axios
        .get("http://192.168.10.10/api/categories", { headers: self.headers })
        .then(response => {
          self.categories = response.data;
          console.log(self.categories);
        })
        .catch(error => {});
    },

    editFormProcess(index, category) {
      this.editForm = true;
      this.editedCategory = category;
    },

    editCategory() {
    //   this.editedCategory = category;

      var self = this;
      axios
        .put("http://192.168.10.10/api/categories/" + self.editedCategory.id, self.editedCategory, {
          headers: self.headers
        })
        .then(response => {
            console.log(response.data)
          this.getCategories();
        })
        .catch(error => {
          console.log(error.response);
        });

        this.editForm = false;
    }

  },
  created() {
    this.$nextTick(() => {
      this.$validator.validateAll();
    });

    this.getCategories();
  },
  mounted() {}
};
</script>

<style lang="scss" scoped>
.vuestic-widget {
  min-height: 400px;
}
.table-responsive {
  position: relative;
  min-height: 300px;
  .loader {
    background-color: rgba(255, 255, 255, 0.9);
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    position: absolute;
    .animation {
      position: absolute;
      top: 50%;
      left: 50%;
    }
  }
  td {
    span {
      cursor: pointer;
    }
    .edit {
      color: #4ae387;
    }
    .delete {
      color: #ff3737;
    }
    i {
      font-size: 20px;

    }
  }
}
</style>