<template>
<v-container>
    <View_Header></View_Header>
    <v-tabs fixed-tabs>
        <v-tab v-for="(menuItem, index) in menuItems" :key="index" @click="menuType=index">
            {{menuItem.name}}
        </v-tab>
    </v-tabs>

    <!-- need to process menuType when click each category cần xử lý menuType khi click vào từng category -->
    
    <div>
        <div v-if="menuType == 0">
            <menuMenu />
        </div>
        <div v-else-if="menuType == 1">
            <menuOthers :menuType=19 />
        </div>
        <div v-else-if="menuType == 2">
            <menuOthers :menuType=20 />
        </div>
    </div>
   
    <div class="text-center">
        <!--thêm sản phẩm-->
        <v-dialog v-model="dialog" width=450 style="border-radius: 8px;">
            <template v-slot:activator="{ on, attrs }">

                <a class="add_product" v-bind="attrs" v-on="on" v-b-tooltip.hover.top title="Thêm sản phẩm">
                    <v-icon large color="black" style="width: 60px;height: 0;" >mdi-plus-box-outline</v-icon>
                </a>
                

            </template>
            <v-card>
                <v-card-title>
                    <span class="text-h5">Thêm sản phẩm</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-row>
                            <v-col cols="12">
                                <v-select :items="categories" label="Chọn Category" v-model="selected" dense></v-select>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="image_url" required v-model="addProduct.addimage_url"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="Tên sản phẩm" required v-model="addProduct.addName"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="Giá" required v-model="addProduct.addPrice"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-textarea label="Mô tả" required v-model="addProduct.addDescription"></v-textarea>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false">
                        Đóng
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="handleAddProduct">
                        Thêm sản phẩm
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>


        <!-- thêm category -->
        <v-dialog v-model="dialog1" width=450 style="border-radius: 8px;">
            <template v-slot:activator="{ on, attrs }">

                <a class="add_category" v-bind="attrs" v-on="on" v-b-tooltip.hover.top title="Thêm danh mục">
                    <v-icon large color="black" style="width: 60px;height: 0;">mdi-plus-box-outline</v-icon>
                </a>
                

            </template>
            <v-card>
                <v-card-title>
                    <span class="text-h5">Thêm danh mục</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-row>
                            <v-col cols="12">
                                <v-select :items="categories1" label="Chọn parentCategory" v-model="selected1" dense></v-select>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="image_url" required v-model="addCategory.addimage_url"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="Tên danh mục" required v-model="addCategory.addName"></v-text-field>
                            </v-col>
                            
                        </v-row>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click=" dialog1 = false">
                        Đóng
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="handleAddCategory">
                        Thêm danh mục
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="handleDeleteCategory">
                        Xóa danh mục
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
    
    <Footer></Footer>

</v-container>
</template>

<script>
import axios from "axios"
export default {
    name: "Icons",

    data() {
        return {
            dialog: false,
            dialog1: false,
            selected: "",
            selected1: "",
            menuType: 0,
            menuItems: [{
                    name: 'Menu'
                },
                {
                    name: 'Chocolate'
                },
                {
                    name: 'Trà'
                },
            ],
            categories: [],
            categories1: [],
            addProduct: {
                addimage_url: "",
                addName: "",
                addPrice: "",
                addDescription: "",
            },
            addCategory: {
                addimage_url: "",
                addName: "",
            }
        }
    },
    created() {
        if(localStorage.getItem("AdminLoggedIn") == "false"){
            this.$router.push("/pages/login")
        }
        else{
            this.getCategories()
            this.getCategoriesTo()
        }
    },
    methods: {
        handleAddProduct() {
            this.dialog = false
            axios
                .post("http://localhost:8000/api/admin/product/create", {
                    name: this.addProduct.addName,
                    category_id: this.selected,
                    description: this.addProduct.addDescription,
                    price: this.addProduct.addPrice,
                    price_sale: 0,
                    image_url: this.addProduct.addimage_url,
                    active: 1
                })
                .then((response) => {
                    console.log(response)
                    alert("Thêm sản phẩm thành công")
                    window.location.reload()
                })
                .catch((error) => {
                    console.log(error.response);
                    alert("Thêm sản phẩm thất bại")
                    window.location.reload()
                });
        },
        handleAddCategory() {
            this.dialog1 = false
            axios
                .post("http://localhost:8000/api/admin/category/create", {
                    name: this.addCategory.addName,
                    parent_id: this.selected1,
                    image_url: this.addCategory.addimage_url,
                    active: 1
                })
                .then((response) => {
                    console.log(response)
                    alert("Thêm danh mục thành công")
                    window.location.reload()
                })
                .catch((error) => {
                    console.log(error.response);
                    alert("Thêm danh mục thất bại")
                    window.location.reload()
                });
        },
        handleDeleteCategory() {
            this.dialog1 = false
            axios
                .post("http://localhost:8000/api/admin/category/delete", {
                    id: this.selected1,
                })
                .then((response) => {
                    console.log(response)
                    alert("Xóa danh mục thành công")
                    window.location.reload()
                })
                .catch((error) => {
                    console.log(error.response);
                    alert("Xóa danh mục thất bại")
                    window.location.reload()
                });
        },
        //lấy danh mục cho create product
        getCategories() {
            axios
                .get("http://127.0.0.1:8000/api/admin/category/index")
                .then((response) => {
                    // console.log("START get categories\n");
                    // // console.log("END\n");
                    let categories_list = response.data.Categories;
                    for (let i in categories_list) {
                        let category = categories_list[i]
                        this.categories.push({
                            text: category.name,
                            value: category.id
                        })
                    }

                    for (let i in categories_list) {
                        let category = categories_list[i]
                        for (let index in this.categories) {
                            let category_tmp = this.categories[index]
                            if (category.parent_id == category_tmp.value) {
                                this.categories.splice(index, 1)
                            }
                        }
                    }

                    // console.log(this.categories)
                    // console.log("DONE CATEGORIES with length:", this.categories.length);
                    // console.log("END\n");
                })
                .catch((error) => {
                    console.log(error.response);
                });
        },

        //lấy danh mục cho create category
        getCategoriesTo() {
            axios
                .get("http://127.0.0.1:8000/api/admin/category/index")
                .then((response) => {
                    // console.log("START get categories\n");
                    // // console.log("END\n");
                    this.categories1.push({
                        text: "New Category parent",
                        value: 0
                    })
                    let categories_list = response.data.Categories;
                    for (let i in categories_list) {
                        let category = categories_list[i]
                        if (category.parent_id == 0) {
                            this.categories1.push({
                                text: category.name + " (parent)",
                                value: category.id
                            })
                        }else{
                            this.categories1.push({
                                text: category.name,
                                value: category.id
                            })
                        }
                    }
                })
                .catch((error) => {
                    console.log(error.response);
                });
        },

        removeItemOnce(arr, value) {
            var index = arr.indexOf(value);
            if (index > -1) {
                arr.splice(index, 1);
            }
            return arr;
        },
    },
    components: {
        menuMenu: () => import('@/components/adminComponents/menuMenu'),
        menuOthers: () => import('@/components/adminComponents/menuOthers'),
        View_Header: () => import('@/layouts/header/View_Header'),
        Footer: () => import('@/layouts/footer/Footer'),
    }
}
</script>

<style scoped>
a /deep/ .mdi-plus-box-outline::before {
    height: 51px;
}

.add_product {
    width: 60px;
    height: 60px;
    position: fixed;
    right: 24px;
    bottom: 150px;
    background: rgb(235, 72, 72);
    border-radius: 50%;
    text-align: center;
    line-height: 76px;
    box-shadow: 0 8px 36px rgb(0 0 0 / 17%);
    z-index: 10;
    padding: 0 !important;
}
.add_category {
    width: 60px;
    height: 60px;
    position: fixed;
    right: 24px;
    bottom: 80px;
    background: rgb(233, 230, 52);
    border-radius: 50%;
    text-align: center;
    line-height: 76px;
    box-shadow: 0 8px 36px rgb(0 0 0 / 17%);
    z-index: 10;
    padding: 0 !important;
}
</style>
