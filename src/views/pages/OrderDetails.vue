<template>
<v-container fluid class="down-top-padding">

    <View_Header></View_Header>
    <v-row style="background-color: white">
        <v-col cols="12" sm="12" md="12">
            <h2 style="justify-content: center; display: flex;left: 50%; padding: 16px 0">CHI TIẾT ĐƠN HÀNG </h2>
        </v-col>

        <v-col cols="12" sm="12" md="6">
            <v-row>
                <v-col cols="12" md="4" style=" align-items: center; display: flex;">
                    <h4 class="black--text text--darken-2 font-weight-medium text-subtitle-1  align-center" style="font-size: 1rem"> Tên khách hàng: </h4>
                </v-col>
                <v-col cols="12" md="8">
                    <v-text-field filled readonly dense hide-details type="text" :value=userOrderInfor.order_username class="v-text-field " solo flat style="justify-content: center">
                    </v-text-field>
                </v-col>
            </v-row>
        </v-col>

        <v-col cols="12" sm="12" md="6">
            <v-row>
                <v-col cols="12" md="4" style=" align-items: center; display: flex;">
                    <h4 class="black--text text--darken-2 font-weight-medium text-subtitle-1  align-center" style="font-size: 1rem"> Số điện thoại: </h4>
                </v-col>
                <v-col cols="12" md="8">
                    <v-text-field filled readonly dense hide-details type="text" :value=userOrderInfor.order_userphone class="v-text-field " solo flat style="justify-content: center">
                    </v-text-field>
                </v-col>
            </v-row>
        </v-col>

        <v-col cols="12" sm="12" md="12">
            <v-row>
                <v-col cols="12" md="2" style=" align-items: center; display: flex;">
                    <h4 class="black--text text--darken-2 font-weight-medium text-subtitle-1  align-center" style="font-size: 1rem"> Địa chỉ: </h4>
                </v-col>
                <v-col cols="12" md="10">
                    <v-text-field filled readonly dense hide-details type="text" :value=userOrderInfor.order_useraddress class="v-text-field " solo flat style="justify-content: center">
                    </v-text-field>
                </v-col>
            </v-row>
        </v-col>

        <v-col cols="12" sm="12" md="12">
            <v-row>
                <v-col cols="12" md="2" style=" align-items: center; display: flex;">
                    <h4 class="black--text text--darken-2 font-weight-medium text-subtitle-1  align-center" style="font-size: 1rem"> Ghi chú: </h4>
                </v-col>
                <v-col cols="12" md="10">
                    <v-text-field filled readonly dense hide-details type="text" :value=userOrderInfor.order_note class="v-text-field " solo flat style="justify-content: center">
                    </v-text-field>
                </v-col>
            </v-row>
        </v-col>

        <v-col cols="12" md="12" lg="12">
            <v-row>
                <div class="offset-md-5 mb-1">
                    <v-card-actions>
                        <v-spacer></v-spacer>
                        <strong>Thông tin sản phẩm</strong>
                    </v-card-actions>
                </div>
                <v-data-table dense disable-sort :header-props="{ sortIcon: null }" :headers="headers" :items="products_infors" hide-default-footer class="elevation-1" style="width: 100%"></v-data-table>
            </v-row>
        </v-col>

        <v-col cols="12" md="12" lg="12">
            <v-row>
                <div class="offset-md-5  mt-3">
                    <v-card-actions>
                        <v-spacer></v-spacer>
                        <strong>Thanh toán</strong>
                    </v-card-actions>
                </div>
                <v-data-table disable-sort :header-props="{ sortIcon: null }" :headers="payment_headers" :items="payment_infor" hide-default-footer class="elevation-1" style="width: 100%"></v-data-table>
            </v-row>
        </v-col>

        <v-row align="center" justify="space-around">
            <v-btn color="success" dark large class="mt-9 mb-9" @click="handleGiaoHang">
                BẮT ĐẦU GIAO HÀNG
            </v-btn>

            <v-btn color="error" dark large class="mt-9 mb-9" @click="handleCancel">
                HỦY
            </v-btn>
        </v-row>
    </v-row>

    <Footer></Footer>
</v-container>
</template>

<script>
import axios from "axios"
export default {
    name: "OrderDetails",
    components: {
        View_Header: () => import('@/layouts/header/View_Header'),
        Footer: () => import('@/layouts/footer/Footer'),
    },
    data() {
        return {
            currentID: '',
            headers: [{
                    text: 'STT',
                    align: 'center',
                    value: 'number',
                    width: '10%',
                },
                {
                    text: 'TÊN SẢN PHẨM',
                    value: 'name',
                    align: 'center',
                    width: '30%'
                },
                {
                    text: 'Topping',
                    value: 'topping_name',
                    align: 'center',
                    width: '40%'
                },
                {
                    text: 'CỠ',
                    value: 'size',
                    align: "center",
                    width: '10%'
                },
                {
                    text: 'SỐ LƯỢNG',
                    value: 'amount',
                    align: 'center',
                    width: '10%'
                },
                {
                    text: 'THÀNH TIỀN',
                    value: 'cash',
                    align: 'center',
                    width: '15%'
                },
                {
                    text: 'GIẢM GIÁ',
                    value: 'discount',
                    align: 'center',
                    width: '15%'
                },

            ],
            products_infors: [],
            payment_headers: [{
                    text: 'PHƯƠNG THỨC THANH TOÁN',
                    align: 'center',
                    value: 'method',
                    width: '60%',
                },
                {
                    text: 'TỔNG TIỀN',
                    value: 'total',
                    align: 'center',
                    width: '40%'
                },
            ],
            payment_infor: [{
                method: 'Tiền Mặt',
                total: '525000',
            }, ],
            userOrderInfor: {
                order_id: "2",
                order_username: "Phạm Thành Chung",
                order_useraddress: "56a Ngõ 128c Đại La",
                order_userphone: "0971751698",
                order_note: "Không lấy đá"
            }
        }
    },
    created() {
        if(localStorage.getItem("AdminLoggedIn") == "false"){
            this.$router.push("/pages/login")
        }
        else{
        console.log(this.$route.path)
        this.currentID = this.$route.path.slice(14)
        this.getOrderInfo()

        console.log("current id: ", this.currentID)
        }
        //  get api by currentid
    },
    methods: {
        handleGiaoHang() {
            axios
                .post("http://127.0.0.1:8000/api/admin/order/acceptOrder", {
                    order_id: this.currentID
                })
                .then((response) => {
                    // this.items = response.data.products;
                    console.log(response.data);
                    alert("Xác nhận thành công")
                    this.$router.push('/pages/new-orders')
                })
                .catch((error) => {
                    console.log("Start\n");
                    console.log(error.response)
                    console.log("END\n");
                });
        },
        handleCancel() {
            axios
                .post("http://127.0.0.1:8000/api/admin/order/cancelOrder", {
                    order_id: this.currentID
                })
                .then((response) => {
                    // this.items = response.data.products;
                    console.log(response.data);
                    alert("Xác nhận hủy thành công")
                    this.$router.push('/pages/new-orders')
                })
                .catch((error) => {
                    console.log("Start\n");
                    console.log(error.response)
                    console.log("END\n");
                });
        },
        getOrderInfo() {
            axios
                .post("http://127.0.0.1:8000/api/admin/order/getOrderInfo", {
                    order_id: this.currentID
                })
                .then((response) => {
                    // this.items = response.data.products;
                    console.log(response.data);
                    // set up payment_info
                    this.payment_infor[0].method = response.data.order_info.payment_method
                    this.payment_infor[0].total = response.data.order_info.total_price

                    // set up userOrderInfor
                    this.userOrderInfor.order_id = response.data.order_info.order_id
                    this.userOrderInfor.order_username = response.data.order_info.user_name,
                        this.userOrderInfor.order_useraddress = response.data.order_info.address,
                        this.userOrderInfor.order_userphone = response.data.order_info.mobile_no,
                        this.userOrderInfor.order_note = response.data.order_info.note

                    // set up products_infors
                    for (let index in response.data.productsOfOrder) {
                        let price = 0
                        let product = response.data.productsOfOrder[index]
                        let toppings = []
                        for (let i in response.data.productsOfOrder[index].topping_id) {
                            let topping = response.data.productsOfOrder[index].topping_id[i]
                            console.log("wait: ", response.data.productsOfOrder[index].topping_count[i])
                            if (response.data.productsOfOrder[index].topping_count[i] > 0) {
                                console.log(topping)
                                toppings.push(topping.name + ' x ' + String(response.data.productsOfOrder[index].topping_count[i]))
                                price += topping.price * response.data.productsOfOrder[index].topping_count[i]
                            }
                        }
                        if (product.size == 'M') {
                            price += 6000
                        } else if (product.size == 'L') {
                            price += 10000
                        }
                        price += product.price * product.product_count
                        let topping_name = toppings.join(', ')
                        console.log("topping name: ", topping_name)
                        this.products_infors.push({
                            number: index,
                            name: product.product_id.name,
                            size: product.size,
                            amount: product.product_count,
                            cash: price,
                            discount: 0,
                            topping_name: topping_name
                        })
                    }
                })
                .catch((error) => {
                    console.log("Start\n");
                    console.log(error.response)
                    console.log("END\n");
                });
        }
    }
};
</script>

<style>
.v-text-field {
    width: 1000px;
}
</style>
