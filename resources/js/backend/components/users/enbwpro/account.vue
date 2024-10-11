<template>
    <div>

        <header class="header-bg">
            <div class="align-items-center d-flex pt-5 gap-3">
                <img :src="row.user.flags" alt="" style="width: 60px; height: 60px; border-radius: 50px;">
                <div>
                    <p class="mb-0" style="font-size: 19px; font-family: ui-rounded;">ID: {{ row.user.username }}</p>
                    <p class="description mb-0">অ্যাকাউন্ট: {{ row.user.mobile }}</p>
                </div>
            </div>

            <div class="mx-2 mb-5 mt-3 d-flex accountDW">
                <router-link :to="{name:'Recharge'}" class="text-dark mt-3"><p>রিচার্জ</p></router-link>
                <router-link :to="{name:'withdraw'}" class="text-dark mt-3"><p>উত্তোলন</p></router-link>
            </div>
        </header>

        <main class="bg-white">
            <section class="px-4 d-flex justify-content-between mx-4 py-4 text-center section-div" style="flex-wrap: wrap;">
                <router-link :to="{name:'pullmoney'}" class="text-dark mt-3 accountMenu">
                    <i class="fa-solid fa-building-columns" style="font-size: 25px;"></i>
                    <p>আমার ব্যাংক</p>
                </router-link>
                <router-link :to="{name:'team'}" class="text-dark mt-3 accountMenu">
                    <i class="fa-solid fa-people-group" style="font-size: 25px;"></i>
                    <p>আমার টিম</p>
                </router-link>
                <router-link :to="{name:'records'}" class="text-dark mt-3 accountMenu">
                    <i class="fa-solid fa-table-list" style="font-size: 25px;"></i>
                    <p>রেকর্ডস</p>
                </router-link>
                <router-link :to="{name:'password'}" class="text-dark mt-3 accountMenu">
                    <i class="fa-solid fa-lock" style="font-size: 25px;"></i>
                    <p>পাসওয়ার্ড পরিবর্তন করুন</p>
                </router-link>
            </section>

            <router-link :to="{name:'logout'}" class="d-block text-center logoutBtn">লগআউট</router-link>
        </main>
    </div>
</template>



<script>


export default {

    data(){
        return {
            levelTeam: {
                level1: {},
                level2: {},
                level3: {},
            },
        }
    },
    methods: {
        async getData(){
            var id = localStorage.getItem('userid');
            var res = await this.callApi('get', `/api/user/multi/refer?id=${id}`, []);
            this.levelTeam = res.data;
        },
    },
    mounted() {
        this.getData();
    },
}
</script>

<style scoped>
.accountMenu{
    width: 48%;
    border: 1px solid var(--defaltColor);
    padding: 13px 0px;
}
section.px-4.d-flex.justify-content-between.mx-4.py-4.text-center.section-div {
    box-shadow: 0px 0px 2px black;
    background: #93939326;
    margin: 25px 4px;
}
.logoutBtn {
    color: #fff6f6;
    margin: 11px 0px;
    padding: 11px 0;
    background: var(--defaltColor);
}
.accountDW a {
    width: 50%;
    text-align: center;
    background: orange;
    padding: 7px 5px;
    border-right: 1px solid white;
    color: white !important;
}
</style>
