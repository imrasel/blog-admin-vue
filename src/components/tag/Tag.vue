<template>
  <div class="form-elements">
    <div class="row category">
      <div class="col-sm-12">
        <vuestic-widget :headerText="'Tags'">
          <div class="row">
            <div class="col-md-6">
              <form v-if="!editForm" @submit.prevent="saveTag">
                <div class="row">
                  <div class="col-md-5">
                    <fieldset>
                      <div class="form-group">
                        <div class="input-group">
                          <input v-model="newTag.name" id="simple-input" required>
                          <label class="control-label" for="simple-input">Add new tag</label>
                          <i class="bar"></i>
                        </div>
                      </div>
                      <button
                        type="submit"
                        class="btn btn-primary btn-micro font-weight-bold"
                      >Save Tag</button>
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
                          <input v-model="editedTag.name" id="simple-input" required>
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
                    <tr v-for="(tag, index) in tags" :key="index">
                      <td>{{ tag.id }}</td>
                      <td>{{ tag.name }}</td>
                      <td>
                        <span class="edit" @click="editFormProcess(index, tag)">
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
  name: "tag",
  components: {
    SpringSpinner
  },
  computed: {

  },
  data() {
    return {
      loader: true,
      editForm: false,
      tags: [],
      newTag: {
        name: ""
      },
      editedTag: {},
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
    saveTag() {
      var self = this;
      axios
        .post("http://192.168.10.10/api/tags", self.newTag, {
          headers: self.headers
        })
        .then(response => {
          this.getTags();
        })
        .catch(error => {
          console.log(error.response);
        });
      // this.newCategory.name = '';
    },

    getTags() {
      this.loader = true;
      var self = this;
      axios
        .get("http://192.168.10.10/api/tags", { headers: self.headers })
        .then(response => {
          self.tags = response.data;
          console.log(self.tags);
        })
        .catch(error => {});
    },

    editFormProcess(index, tag) {
      this.editForm = true;
      this.editedTag = tag;
    },

    editTag() {
    //   this.editedCategory = category;

      var self = this;
      axios
        .put("http://192.168.10.10/api/tags/" + self.editedTag.id, self.editedTag, {
          headers: self.headers
        })
        .then(response => {
            console.log(response.data)
          this.getTags();
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

    this.getTags();
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