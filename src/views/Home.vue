<template>
  <div>
    <HeaderTemplate></HeaderTemplate>
    <v-container>
      <v-row justify="center">
        <v-btn
          class="ma-2"
          tile
          outlined
          color="primary"
          dark
          v-on:click="showcreateblog()"
        >Create Blog</v-btn>
      </v-row>

      <v-dialog v-model="dialog" persistent max-width="600px">
        <v-card>
          <v-card-title class="headline">New blog post</v-card-title>
          <v-card-text>
            <v-row>
              <v-col cols="12">
                <v-text-field label="Title" v-model="blog.name" :rules="titleRule" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-textarea
                  label="Description"
                  v-model="blog.description"
                  :rules="descriptionRule"
                  required
                ></v-textarea>
              </v-col>
            </v-row>
            <small>*Provide all relevant information</small>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false">Close</v-btn>
            <v-btn color="blue darken-1" text @click="createBlog">Save</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog v-model="editdialog" persistent max-width="600px">
        <v-card>
          <v-card-title class="headline">Update Post</v-card-title>
          <v-card-text>
            <v-row>
              <v-col cols="12">
                <v-text-field label="Title" v-model="blog.name" :rules="titleRule" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-textarea
                  label="Description"
                  v-model="blog.description"
                  :rules="descriptionRule"
                  required
                ></v-textarea>
              </v-col>
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="green darken-1" text @click="editdialog = false">Cancel</v-btn>
            <v-btn color="green darken-1" text @click="updateblog()">Update</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-row v-for="blog in blogs" v-bind:key="blog.id" class="pa-2">
        <v-card class="pa-2" min-width="100%" elevation="3" outlined tile>
          <v-card-title>
            <div>{{blog.name}}</div>
            <v-spacer></v-spacer>
            <v-btn
              class="ma-2"
              tile
              depressed
              small
              outlined
              color="warning"
              v-on:click="showeditblog(blog.id)"
            >
              <v-icon left>mdi-pencil</v-icon>Edit
            </v-btn>
            <v-btn
              class="ma-2"
              tile
              depressed
              small
              outlined
              color="red"
              v-on:click="deleteblog(blog.id)"
            >
              <v-icon left>mdi-delete</v-icon>Delete
            </v-btn>
          </v-card-title>
          <v-card-text>
            {{blog.description}}
            <div class="pt-2">
              <small>
                {{blog.createdDate | formatDate }}
                <v-spacer></v-spacer>
              </small>
            </div>
          </v-card-text>
        </v-card>
      </v-row>
    </v-container>
    <FooterTemplate></FooterTemplate>
  </div>
</template>


<script>
import HeaderTemplate from "../components/Header.vue";
import FooterTemplate from "../components/Footer.vue";
import axios from "axios";
export default {
  name: "Home",
  components: {
    HeaderTemplate,
    FooterTemplate,
  },
  data() {
    return {
      dialog: false,
      editdialog: false,
      blogs: [],
      blog: {
        id: "",
        name: "",
        description: "",
      },
      titleRule: [
        (v) => !!v || "Title is required",
        (v) => v.length > 5 || "Title must be greater than 5 characters",
      ],
      descriptionRule: [
        (v) => !!v || "Description is required",
        (v) =>
          v.length > 10 || "Description must be greater than 10 characters",
      ],
    };
  },
  methods: {
    createBlog: function () {
      this.dialog = false;
      console.log("Creating blog " + JSON.stringify(this.blog));
      axios
        .post("http://localhost:8080/blog", this.blog)
        .then((resp) => console.log(resp))
        .catch((err) => console.log(err));
      window.location.reload();
    },
    showcreateblog: function () {
      this.dialog = true;
    },
    showeditblog: function (id) {
      console.log("Modifying blog " + id);
      var bb = this.blogs.find((x) => x.id == id);
      console.log("Obtained blog: " + JSON.stringify(bb));
      this.blog.name = bb.name;
      this.blog.description = bb.description;
      this.blog.id = bb.id;
      this.editdialog = true;
    },
    deleteblog: function (id) {
      console.log("Calling deleteblog with id " + id);
      console.log("Deleting blog " + id);
      axios
        .delete("http://localhost:8080/blog/" + id)
        .then((res) => {
          console.log("Delete Response: " + JSON.stringify(res.data));
        })
        .catch((err) => console.log("Update response: " + err));
      window.location.reload();
    },
    updateblog: function () {
      console.log(
        "Update blog id: " + this.blog.id + " with " + JSON.stringify(this.blog)
      );
      axios
        .put("http://localhost:8080/blog/" + this.blog.id, {
          name: this.blog.name,
          description: this.blog.description,
        })
        .then((res) => {
          console.log(JSON.stringify(res.data));
        })
        .catch((err) => console.log("Update response: " + err));
      window.location.reload();
    },
  },
  created: function () {
    axios
      .get("http://localhost:8080/blog")
      .then((res) => (this.blogs = res.data.data))
      .catch((err) => console.log(err));
  },
  filters: {
    formatDate: function (val) {
      if (!val) return;
      var mydate = new Date(val);
      var options = {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric",
        hour: "numeric",
        minute: "numeric",
        second: "numeric",
      };
      return mydate.toLocaleDateString("en-US", options);
    },
  },
};
</script>

<style scoped>
</style>
