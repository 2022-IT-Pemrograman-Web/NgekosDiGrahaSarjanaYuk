<template>
    <div style="display: flex;"><button class="white" @click="$router.go(-1)">Kembali</button><button @click="$router.push('/')" class="green">Home</button></div>
    <div class="payment-container">
        <div class="payment-content">
            <div style="font-size: 28px; font-weight: 700; text-align: left;">Thank you so much for choosing</div>
            <div style="font-size: 32px; font-weight: 700; text-align: left;">Graha Sarjana</div>
            <div style="color: red;">Please proceed the payment before {{ getMaxDate() }} </div>
            <div style="font-size: 30px; font-weight: 700; margin: 10px 0px;">Rp {{ order.totalHarga }}</div>
            <div class="metode_pembayaran">{{ order.metodePembayaran }}</div>
            <div style="text-align: left; margin-top: 10px;">
                <label for="" style="font-size: 12px; font-weight: 700;">Upload your payment file<input type="file" name="file" id="file" required="true" ></label>
            </div>
            <div style="margin-top: 20px; display: flex;">
                <div><button class="green" @click="confirmCancel('confirm')">CONFIRM PAYMENT</button></div>
                <div><button class="red" @click="confirmCancel('cancel')">CANCEL BOOKING</button></div>
            </div>
        </div>
    </div>
    
</template>

<script>
import axios from 'axios'

export default{
    data(){
        return{
            order: ''
        }
    },
    methods:{
        getMaxDate(){
            const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thrusday', 'Friday', 'Saturday']
            var jsDate = new Date()
            var nextDate = new Date(jsDate)
            nextDate.setDate(jsDate.getDate() + 1)
            var date = nextDate.getDate()
            var month = nextDate.getMonth() + 1
            var year = nextDate.getFullYear()
            var day = nextDate.getDay()

            return '23.59 ' + days[day] + ', ' + date + '/' + month + '/' + year
        },
        getOrder(){
            var url = "http://localhost:1337/getCertainBooking/"
            axios.get(url,{
                params:{
                    id: this.$route.params.orderId
            }}).then(res => {
                this.order = res.data.order[0]
                this.order.totalHarga = this.toPrice(this.order.totalHarga)
            }).catch(err => {
                console.log(err)
            })
        },
        toPrice(price) {
            var new_price = price.toString()
            new_price = new_price.split('')
            var n_dots = Math.floor(new_price.length / 3)
            var it = 1
            for(it; it<n_dots+1; it++){
                var pos = new_price.length+1-it-3*it
                new_price.splice(pos,0,'.')
            }
            return new_price.join('')
        },
        async confirmCancel(action){
            var stat = 'Canceled'
            console.log(this.$route.params.orderId)
            if(action == 'confirm'){
                stat = 'waiting_verification'
                // file upload
                var formData = new FormData();
                var imagefile = document.querySelector('#file')
                formData.append("file", imagefile.files[0])
                console.log(formData)
                await axios.post('http://localhost:1337/uploadBuktiBayar', formData, {
                    headers: {
                    'Content-Type': 'multipart/form-data'
                    }
                    }).then(res =>{
                        console.log(res.data)
                    }).catch(err => {
                        console.log(err)
                    })
            }

            axios.patch("http://localhost:1337/updateBookingStatus", {
                id: this.$route.params.orderId,
                status: stat
            }).then(res => {
                console.log(res.data)
                this.$router.push('/history')
            }).catch(err => {
                console.log(err),
                this.$router.push('/history')
            })
        }
    },
    mounted(){
        this.getOrder()
    }
}
</script>

<style scoped>
.payment-container{
    padding: 20px 40px;
    width: 50%;
    margin: auto;
}
.payment-content{
    padding: 20px 20px;
    box-shadow: 0px 0px 5px 2px rgba(0, 0, 0, 0.094);
    margin-top: 10px;
    margin: auto;
    border-radius: 10px;
}
input{
    border: none;
}
.metode_pembayaran{
    padding: 5px 30px;
    background-color: #00863132;
    width: fit-content;
    border-radius: 20px;
    margin: auto;
}
</style>
