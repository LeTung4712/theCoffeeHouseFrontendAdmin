<template>
  <v-card class="mx-auto" max-width="344" color="transparent">
    <v-img :src="image_url" height="200px"></v-img>

    <v-card-title style="padding: 12px 0">
      {{ name }}
    </v-card-title>

    <v-card-subtitle style="padding: 12px 0">
      {{ separator(price) }}
    </v-card-subtitle>

    <v-card-actions>
      <v-row>
        <v-dialog v-model="dialog" persistent max-width="600px" right>
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark v-bind="attrs" v-on="on" mdi-edit icon>
              <!-- Open Dialog  -->
              <!-- <v-icon icon="fa:fas fa- edit"></v-icon> -->
              <v-icon large>mdi-pencil</v-icon>
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">Chỉnh sửa sản phẩm</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      label="image_url"
                      required
                      v-model="image_url"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      label="Name"
                      required
                      v-model="name"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      label="Gia"
                      required
                      v-model="price"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-textarea
                      label="Mo ta"
                      required
                      v-model="description"
                    ></v-textarea>
                  </v-col>
                </v-row>
              </v-container>
              <!-- <small>*indicates required field</small> -->
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialog = false">
                Đóng
              </v-btn>
              <v-btn color="blue darken-1" text @click="handleUpdate">
                Sửa
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>

      <div class="text-center">
        <v-dialog v-model="dialog1" width="450" style="border-radius: 8px">
          <template v-slot:activator="{ on, attrs }">
            <a class="hotline_button" v-bind="attrs" v-on="on">
              <img
                style="width: 30px; height: 30px; color: #191919"
                src="https://free-icons.net/wp-content/uploads/2021/03/symbol079.png"
                alt=""
              />
            </a>
          </template>
          <v-card>
            <div
              class="hotline lighten-2 text-center font-weight-thin"
              style="
                padding: 26px 0 22px 0;
                font-family: 'SF Pro Text', sans-serif;
              "
            >
              Xác nhận xóa sản phẩm ?
            </div>

            <div style="display: flex; justify-content: space-between">
              <v-btn
                color="orange"
                text
                @click="handleXoa"
                style="
                  padding-bottom: 28px;
                  padding-top: 28px;
                  align-items: center;
                  right: -15%;
                  width: 35%;
                "
                height="32px"
                class="hotline_call"
              >
                Xác nhận
              </v-btn>

              <v-btn
                color="black"
                text
                @click="dialog1 = false"
                style="
                  left: -15%;
                  padding-bottom: 28px;
                  padding-top: 28px;
                  align-items: center;
                  width: 35%;
                "
                height="32px"
              >
                Quay lại
              </v-btn>
            </div>
          </v-card>
        </v-dialog>
      </div>


      <v-btn icon @click="show = !show">
        <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
      </v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text>
          {{ description }}
        </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script>
import axios from "axios";
// import BtnEdit from './vuetifyComponents/BtnEdit.vue'
export default {
  props: {
    image_url: String,
    name: String,
    description: String,
    price: String,
    id: Number,
    category_id: String,
    price_sale: Number,
    active: Number,
    created_at: String,
    updated_at: String,
  },
  name: "Item",
  data: () => ({
    show: false,
    dialog: false,
    dialog1: false,
  }),

  methods: {
    handleXoa() {
      this.dialog1 = false;
      console.log(this.id);
      console.log(this.image_url);
      console.log(this.name);
      console.log(this.description);
      console.log(this.price);
      console.log(this.category_id);
      axios
        .post("http://127.0.0.1:8000/api/admin/product/destroy", {
          id: this.id,
        })
        .then((response) => {
          console.log(response);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    separator(numb) {
      var str = numb.toString().split(".");
      str[0] = str[0].replace(/\B(?=(\d{3})+(?!\d))/g, ".");
      return str.join(".");
    },
    handleUpdate() {
      this.dialog = false;
      console.log(this.id);
      console.log(this.image_url);
      console.log(this.name);
      console.log(this.description);
      console.log(this.price);
      console.log(this.category_id);
      axios
        .post("http://http://localhost:8000/api/admin/product/update", {
          id: this.id,
          image_url: this.image_url,
          name: this.name,
          description: this.description,
          price: this.price,
          category_id: this.category_id,
          //price_sale: this.price_sale,
          //active: this.active,
          created_at: this.created_at,
          updated_at: this.updated_at,
        })
        .then((response) => {
          console.log(response);
          alert("Sửa thành công");
        })
        .catch((error) => {
          console.log(error.response);
          alert("Sửa thất bại");
        });
    },
  },
  components: {
    // BtnEdit: () => import("@/components/vuetifyComponents/BtnEdit")
  },
};
</script>
