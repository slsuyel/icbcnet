<template>
    <div>
        <header style="background: var(--defaltColor); padding: 7px 12px;">
            <h1
                class="text-white"
                style="
                    height: 44px;
                    font-size: 16px;
                    padding-left: 15px;
                    padding-right: 15px;
                    font-weight: 700;
                    display: flex;
                    align-items: center;
                "
            >
                রিচার্জ
            </h1>
        </header>

        <form @submit.stop.prevent="onSubmit">
    <div
        style="
            padding: 0 12px;
            background: linear-gradient(1turn, rgb(241, 243, 248), rgb(151 127 247) 58%, #B000FF);
        "
    >
        <hr class="text-bg-dark" />
        <div class="bg-white py-3 rounded-4 shadow-sm">
            <p class="mb-0 text-center" style="color: #ff883a;">অনুগ্রহ করে পরিমাণ নির্বাচন করুন</p>
            <div class="ammount-select">
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 200 }"
                    @click="form.amount = 200"
                >
                    200
                </p>
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 500 }"
                    @click="form.amount = 500"
                >
                    500
                </p>
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 1000 }"
                    @click="form.amount = 1000"
                >
                    1,000
                </p>
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 2000 }"
                    @click="form.amount = 2000"
                >
                    2,000
                </p>
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 5000 }"
                    @click="form.amount = 5000"
                >
                    5,000
                </p>
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 10000 }"
                    @click="form.amount = 10000"
                >
                    10,000
                </p>
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 25000 }"
                    @click="form.amount = 25000"
                >
                    25,000
                </p>
                <p
                    class="amount"
                    :class="{ 'active': form.amount == 45000 }"
                    @click="form.amount = 45000"
                >
                    45,000
                </p>
            </div>
            <div
                class="align-items-center border border-danger d-flex justify-content-between money-input mx-2 my-3 p-2 rounded-3 shadow-sm"
            >
                <input
                    class="border-0"
                    maxlength="140"
                    v-model="form.amount"
                    step="0.01"
                    enterkeyhint="done"
                    pattern="[0-9]*"
                    autocomplete="off"
                    type="number"
                    placeholder="অনুগ্রহ করে পরিমাণ নির্বাচন করুন"
                />
                <button type="submit" class="money-btn">রিচার্জ</button>
            </div>
        </div>
    </div>
</form>


        <footer class="bg-white mx-2 my-4 rounded-3 shadow-sm">
            <p
                class="text-start"
                style="
                    font-size: 18px;
                    font-weight: 500;
                    color: #31323e;
                "
            >
                রিচার্জ রেকর্ড
            </p>

            <div
                class="px-4 py-2"
                v-for="rech in lists"
                :key="'rech' + rech.id"
            >
                <div class="d-flex justify-content-between">
                    <p><i class="fa-mobile-screen-button fa-solid me-1"></i> {{ rech.status }}</p>
                    <p>{{ rech.amount }} BDT</p>
                </div>
                <p class="fs-6 mb-0 time">{{ dateformatglobal(rech.created_at)[6] }}</p>
            </div>
        </footer>
    </div>
</template>

<script>
export default {

    created() {

        // if(User.dateformat()[9]<=23){
        //     if(User.dateformat()[9]>=9){
        //     }else{
        //         this.notifiyGlobal('Deposit Time Everyday 9am To 12pm.')
        //         this.$router.push({ name: 'Authuser' });
        //     }
        // }else{
        //     this.notifiyGlobal('Deposit Time Everyday 9am To 12pm.')
        //     this.$router.push({ name: 'Authuser' });
        // }

    },

    data() {
        return {
               popup:false,
            form: {
                amount: '',
            },


            payMethods: '',
            paymentType: 'onepay',
            lists: {},
            amount: 0,
            step: 1,
            copyMessage:'',
            rates:'',
            con:false,
        }
    },
    methods: {

        PymamentSelect(type){
            this.paymentType = type
        },

         randomLetter(length) {
                let result = 'S'+this.dateformatglobal()[10]+this.dateformatglobal()[11]+this.dateformatglobal()[12]+Math.floor(Math.random() * (999999999999 - 11111111111));
                return result;
            },

            async getData() {
            var id = localStorage.getItem('userid');
            var res = await this.callApi('get', `/api/admin/deposit?id=${id}`, []);
            this.lists = res.data;
            this.isActive = false
        },

        async onSubmit() {

            if (this.settings.min_deposit > Number(this.form.amount)) {
                this.notifiyGlobal(`Minimum deposit amount ${this.settings.min_deposit}`);
            } else {
                this.form['orderid'] = this.randomLetter(20);
                localStorage.setItem('rechargeData',JSON.stringify(this.form))
                var regTimer = new Date(new Date().getTime() + 300000);
                localStorage.setItem('regTimer',regTimer);

                if(this.paymentType=='onepay'){
                    this.$router.push({ name: 'rechargepage' });

                }else if(this.paymentType=='USDT'){
                    this.$router.push({ name: 'rechargepageUsd' });

                }

            }


        },



    },
    mounted() {
        this.getData();

    },
}
</script>
<style scoped>


body {
            background-color: #f1f1f1;
        }

        hr {
            margin-top: -12px;
        }

        i.fa-solid.fa-mobile-screen-button {
            background: #f78b21;
            color: white;
            padding: 2px;
            border-radius: 3px;
        }

        .time {
            padding: 6px 0 16px;
            font-size: 13px;
            font-family: PingFangSC-Regular, PingFang SC;
            font-weight: 400;
            color: #9899a6;
        }

        .balance {
            font-size: 16px;
            font-family: PingFangSC-Regular, PingFang SC;
            font-weight: 400;
            color: hsla(0, 0%, 100%, .7);
        }

        .ammount-select {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            text-align: center;
            gap: 15px;
        }

        .amount {
            font-size: 17px;
            font-family: PingFangSC-Medium, PingFang SC;
            font-weight: 500;
            color: #31323e;
            line-height: 66px;
            text-align: center;
            background: #f7f8fa;
            border-radius: 8px;
        }

        button.money-btn {
            width: 166px;
            margin: 0;
            padding: 0 13px;
            line-height: 39px;
            background: var(--defaltColor);
            border-radius: 8px;
            font-size: 16px;
            font-family: PingFangSC-Regular, PingFang SC;
            font-weight: 400;
            color: #fff;
            border: 0;
        }


.mainitem {
    display: flex !important;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-between;
}
.money-style {
    text-align: center;
    width: 31%;
    border-radius: 5px;
    background-color: #eceded;
    color: #000;
    font-size: 15px;
    margin: 7px 1%;
    height: 50px;
    box-sizing: border-box;
    line-height: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
}
.active {
    color: #fff;
    background: var(--defaltColor) !important;
}
button.btn.btn-primary {
    background: var(--defaltColor) !important;
    border: var(--defaltColor) !important;
    height: 38px;
    width:100%;
}

.grid-container {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-gap: 5px;
      }



</style>
