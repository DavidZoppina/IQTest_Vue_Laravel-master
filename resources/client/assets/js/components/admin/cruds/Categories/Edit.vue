<template>
  <section class="content-wrapper" style="min-height: 960px;">
    <section class="content-header">
      <h1>Category</h1>
    </section>

    <section class="content">
      <div class="row">
        <div class="col-xs-12">
          <form @submit.prevent="submitForm">
            <div class="box">
              <div class="box-header with-border">
                <h3 class="box-title">Edit</h3>
              </div>

              <div class="box-body">
                <back-buttton></back-buttton>
              </div>

              <bootstrap-alert/>

              <div class="box-body">
                <div class="row">
                  <div class="form-group col-md-3">
                    <label for="sel1">Select Language:</label>
                    <select
                      class="form-control"
                      name="locale"
                      required
                      :value="locale"
                      @input="updateLocale"
                    >
                      <option
                        v-for="language in languages"
                        v-bind:key="language.id"
                        :value="language.locale"
                      >{{language.language}}</option>
                    </select>
                  </div>
                </div>
                <div class="row">
                  <div class="form-group col-md-3">
                    <label for="sel1">Select Test Type:</label>
                    <select
                      class="form-control"
                      name="type"
                      required
                      @input="updateType"
                      :value="item.test_type"
                    >
                      <option
                        v-for="type in types"
                        v-bind:key="type.id"
                        :value="type.id"
                      >{{type.name}}</option>
                    </select>
                  </div>
                </div>
                <div class="row" v-show="loading">
                  <div class="col-xs-4 col-xs-offset-4">
                    <div class="alert text-center">
                      <i class="fa fa-spin fa-refresh"></i> Loading
                    </div>
                  </div>
                </div>
                <div v-show="!loading">
                  <div class="form-group">
                    <label for="title">Title *</label>
                    <input
                      type="text"
                      class="form-control"
                      name="title"
                      placeholder="Enter Title *"
                      :value="item.title"
                      @input="updateTitle"
                      required
                    >
                  </div>
                  <div class="form-group">
                    <label for="description">Description</label>
                    <textarea
                      rows="3"
                      class="form-control"
                      name="description"
                      placeholder="Enter Description"
                      :value="item.description"
                      @input="updateDescription"
                    ></textarea>
                  </div>
                  <div class="form-group">
                    <label for="title">Question *</label>
                    <input
                      type="text"
                      class="form-control"
                      name="question"
                      placeholder="Enter Question *"
                      :value="item.question"
                      @input="updateQuestion"
                      required
                    >
                  </div>
                  <div class="form-group">
                    <label for="description">Short Description</label>
                    <textarea
                      rows="3"
                      class="form-control"
                      name="short_desc"
                      placeholder="Enter Short Description"
                      :value="item.short_desc"
                      @input="updateShortDescription"
                    ></textarea>
                  </div>
                </div>
              </div>

              <div class="box-footer">
                <vue-button-spinner
                  class="btn btn-primary btn-sm"
                  :isLoading="loading"
                  :disabled="loading"
                >Save</vue-button-spinner>
              </div>
            </div>
          </form>
        </div>
      </div>
    </section>
  </section>
</template>

<script>
import { mapGetters, mapActions } from "vuex";

export default {
  data() {
    return {
      // Code...
    };
  },
  computed: {
    ...mapGetters("CategoriesSingle", [
      "item",
      "loading",
      "languages",
      "locale",
      "types"
    ])
  },
  created() {
    this.fetchData(this.$route.params.id);
    this.fetchInitialData();
  },
  destroyed() {
    this.resetState();
  },
  watch: {
    "$route.params.id": function() {
      this.resetState();
      this.fetchData(this.$route.params.id);
      this.fetchInitialData();
    }
  },
  methods: {
    ...mapActions("CategoriesSingle", [
      "fetchData",
      "fetchInitialData",
      "updateData",
      "resetState",
      "setTitle",
      "setLocale",
      "setType",
      "setDescription",
      "setQuestion",
      "setShortDescription"
    ]),
    updateTitle(e) {
      this.setTitle(e.target.value);
    },
    updateQuestion(e) {
      this.setQuestion(e.target.value);
    },
    updateShortDescription(e) {
      this.setShortDescription(e.target.value);
    },
    updateDescription(e) {
      this.setDescription(e.target.value);
    },
    updateLocale(e) {
      this.setLocale(e.target.value);
      this.fetchData(this.$route.params.id);
    },
    updateType(e) {
      this.setType(e.target.value);
    },
    submitForm() {
      this.updateData()
        .then(() => {
          // this.$router.push({
          //   name: "categories.index"
          // });
          this.$eventHub.$emit("update-success");
        })
        .catch(error => {
          console.error(error);
        });
    }
  }
};
</script>

<style scoped>
</style>
