<template>
    <div>

        <section id="slider">
            <img width="100%" :src="slideimage" alt="">
        </section>

        <header class="defaltColorBg py-2 rounded-bottom-4 text-white" >
            <!-- <div class="align-items-center d-flex justify-content-between mx-2 pt-2 text-white">
                <p class="fs-3"></p>
                <a :href="$asseturl+''" class="app-download"><i class="fa-solid fa-download p-2"></i>অ্যাপ ডাউনলোড</a>
            </div> -->
            <div class=" mt-2">
                <div class="row mx-auto">
                    <div class="col-md-12">
                        <div class="align-items-center breaking-news d-flex justify-content-between mx-2 ps-3 rounded bg-trasparent">
                            <div class="d-flex flex-row flex-grow-1 flex-fill justify-content-center py-2 text-white px-1 news">
                                <span style="margin-left: -10px;" class="d-flex align-items-center">&nbsp;<i class="fa-solid fa-volume-high"></i></span>
                            </div>
                            <marquee class="news-scroll" behavior="scroll" direction="left" onmouseover="this.stop();" onmouseout="this.start();"> {{ settings.notice }}</marquee>
                        </div>
                    </div>
                </div>
            </div>

            <div class="d-flex d-sm-flex justify-content-between lh-1 mt-4 mx-2">
                <div>
                    <p class="balance-number">{{ parseFloat(row.user.balance).toFixed(2) }}</p>
                    <p class="balance">ব্যালেন্স</p>
                </div>
                <!-- <div>
                    <p class="balance-number">0.00</p>
                    <p class="balance">ইউবিএস ব্যালেন্স</p>
                </div> -->
                <div>
                    <p class="balance-number">0.00</p>
                    <p class="balance">সঞ্চিত আয়</p>
                </div>
            </div>
        </header>

        <main>
            <section class="d-flex justify-content-around mx-4 py-4 text-center section-div" style="grid-gap: 10px;">
                <router-link :to="{name:'Recharge'}" class="text-dark homePageIcon">
                    <i class="fa-solid fa-credit-card" style="color: #F78721;"></i>
                    <p>রিচার্জ</p>
                </router-link>
                <router-link :to="{name:'withdraw'}" class="text-dark homePageIcon">
                    <i class="fa-solid fa-briefcase" style="color: #9d00ff;"></i>
                    <p>উত্তোলন</p>
                </router-link>
                <span @click="dailyCheckIn" class="text-dark homePageIcon">
                    <i class="fa-solid fa-circle-check" style="color: #0066ff;"></i>
                    <p>চেক ইন</p>
                </span>
                <router-link :to="{name:'noreward'}" class="text-dark homePageIcon">
                    <i class="fa-solid fa-medal" style="color: #268300;"></i>
                    <p>বিনিয়োগ তথ্য</p>
                </router-link>
                <router-link :to="{name:'companyPro'}" class="text-dark homePageIcon">
                    <i class="fa-solid fa-address-card" style="color: #c33771;"></i>
                    <p>কোম্পানি প্রোফাইল</p>
                </router-link>
            </section>

            <section class="invite-section mx-2 mb-5">
                <h1 class="fw-bold invite-h1 text-white">বন্ধুকে আমন্ত্রণ জানান এবং পুরস্কার অর্জন করুন</h1>
                <router-link :to="{name:'share'}" class="invite text-decoration-none" href="">আমন্ত্রণ দিন</router-link>
            </section>

            <div class="parters">
                <h4>গ্যালারি</h4>
                <img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExaXdnZ2ZndXQyaXJ2bzNyZ2p6aDRuODN0eGx2OHlhaXJwZnp1eDNtayZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/allxMZ0wV4fZ4ljzR3/giphy.webp" class="w-100 mx-auto" alt="GIF Image">
            </div>
        </main>

        <div class="noticePopup" v-if="noticePop">
            <div class="popcontiner">
                <div class="PopHead"><span @click="noticePop=false">X</span></div>
                <div class="nPopBody" v-html="settings.popupNotice"></div>
            </div>
        </div>

        <Preload :Isactive="isActive"/>
        <Message :Isactive="Messageactive" :Message="Message"/>
    </div>
</template>

<script>
export default {
    created() {
        setTimeout(() => {
            if (this.settings.slide1) this.slider.push(this.settings.slide1);
            if (this.settings.slide2) this.slider.push(this.settings.slide2);
            if (this.settings.slide3) this.slider.push(this.settings.slide3);
            if (this.settings.slide4) this.slider.push(this.settings.slide4);

            var indexx = 1;
            this.slideimage = this.slider[0]
            setInterval(() => {
                if (indexx === this.slider.length) indexx = 0;
                this.slideimage = this.slider[indexx]
                indexx++
            }, 8000);
        }, 3000);
    },
    data() {
        return {
            noticePop: true,
            isActive: false,
            PackPurchase: false,
            Messageactive: false,
            Message: '',
            datas: {},
            PopupTitle: 'আপনি কি সত্যিই এই পণ্যটি কিনতে চান?',
            slideimage: '',
            slider: [],
        }
    },
    methods: {
        async dailyCheckIn() {
            this.isActive = true;
            var userid = localStorage.getItem('userid');
            var res = await this.callApi('post', `/api/daily/check/in?user_id=${userid}`, []);
            this.isActive = false;
            if (res.data == 1) {
                this.notifiyGlobal('ডেইলি চেক ইন সম্পন্ন হয়েছে')
            } else {
                this.notifiyGlobal('কালের অপেক্ষায় আসুন')
            }
        },
    },
    mounted() {},
}
</script>

<style>
/* Ensure to keep the existing styles */
.homeicons i {
    font-size: 35px;
}

.d-flex.marquee {
    border: 2px solid var(--defaltColor);
    margin: 18px 21px;
}

.d-flex.marquee span {
    background: var(--defaltColor);
    padding: 5px 14px;
    color: white;
    font-weight: 700;
}

.d-flex.marquee marquee {
    padding: 5px 14px;
    background: #d14d0ec7;
    color: white;
}

.parters {
    text-align: center !important;
}

.parters h4 {
    margin-bottom: 10px;
    border-bottom: 2px solid var(--defaltColor);
    color: var(--defaltColor);
}

.homePageIcon i {
    font-size: 35px;
    margin-bottom: 9px;
}

.homePageIcon {
    font-size: 12px;
}

.noticePopup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: #000000b5;
    height: 100%;
    z-index: 99;
}
.popcontiner {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #ffffff00;
    width: 300px;
    max-width: 300px;
}

.PopHead {
    color: white;
    text-align: right;
    padding: 0 11px;
    margin-bottom: 5px;
}

.nPopBody {
    background: white;
    padding: 11px 6px;
    border-radius: 11px;
}
.PopHead span {
    padding: 3px 6px;
    background: #f30707c2;
    border-radius: 50%;
    cursor: pointer;
}
</style>


