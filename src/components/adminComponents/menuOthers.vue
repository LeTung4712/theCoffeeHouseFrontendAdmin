<template>
<v-container>
    <v-row class="menu_bar">
        <v-col cols="12" xs="12" sm="6" md="3" lg="3" v-for="item in items" :key="item.id" style="margin: 24px 0">
            <Item :image_url="item.image_url" :name="item.name" :description="item.description" :price="item.price" :id="item.id" :category_id="item.category_id" :price_sale="item.price_sale" :active="item.active" :created_at="item.created_at" :updated_at="item.updated_at" />
        </v-col>
    </v-row>
</v-container>
</template>

<script>
import axios from 'axios'
export default {
    name: "menuCategory_withoutTree",
    props: {
        menuType: Number,
    },

    //  set up data base on menuType from backend
    // this data below is manually set up, neet to get from backend when have a api
    data() {
        return {
            items: []
        };
    },
    components: {
        Item: () => import("@/components/Item"),
    },

    created() {
        if(localStorage.getItem("AdminLoggedIn") == "false"){
            this.$router.push("/pages/login")
        }
        else{
        this.getItemsByCategoryId()
        }
    },

    watch: {
        menuType() {
            this.getItemsByCategoryId()
        }
    },

    methods: {
        getItemsByCategoryId() {
            console.log(this.menuType);
            axios
                .post("http://127.0.0.1:8000/api/admin/product/indexByCategoryId", {
                    category_id: this.menuType
                })
                .then((response) => {
                    this.items = response.data.products;
                    console.log(response.data.products);
                })
                .catch((error) => {
                    console.log("Start\n");
                    console.log(error.response)
                    console.log("END\n");
                });
            localStorage.setItem("items", JSON.stringify(this.items))
        },
    }

};
</script>

<style>
.menu_bar::after {
    content: "";
    position: absolute;
    width: 2px;
    top: 12px;
    background: #00000026;
}
</style>
