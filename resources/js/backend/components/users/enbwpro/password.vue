<template>
    <div>
        <header class="header">
            <p class="header-text">পাসওয়ার্ড পরিবর্তন</p>
        </header>

        <form @submit.stop.prevent="accountPassChange">
            <div class="bg-white mb-2 mx-2 p-2">
                <div class="input-group mb-3">
                    <span class="input-group-text" id="basic-addon1"><i class="fa-solid fa-lock"></i></span>
                    <input type="password" class="form-control" placeholder="পুরানো পাসওয়ার্ড প্রবেশ করুন" v-model="accountPass.old" aria-label="Username" aria-describedby="basic-addon1">
                </div>
                <div class="input-group mb-3">
                    <span class="input-group-text" id="basic-addon1"><i class="fa-solid fa-lock"></i></span>
                    <input type="password" class="form-control" placeholder="নতুন পাসওয়ার্ড প্রবেশ করুন" v-model="accountPass.newpass" aria-label="Username" aria-describedby="basic-addon1">
                </div>
                <div class="input-group mb-3">
                    <span class="input-group-text" id="basic-addon1"><i class="fa-solid fa-lock"></i></span>
                    <input type="password" class="form-control" placeholder="নতুন পাসওয়ার্ড নিশ্চিত করুন" v-model="accountPass.confirm" aria-label="Username" aria-describedby="basic-addon1">
                </div>

                <button type="submit" class="btn btn-danger mt-3 mx-1 w-100 defaltColorBg">রিসেট</button>
            </div>
        </form>

        <Preload :Isactive="isActive" />
        <Message :Isactive="Messageactive" :Message="Message" />
    </div>
</template>

<script>
export default {
    data() {
        return {
            isActive: false,
            Messageactive: false,
            Message: "",

            accountPass: {
                old: '',
                newpass: '',
                confirm: '',
            },
        };
    },
    methods: {
        async accountPassChange() {
            this.isActive = true;
            if (this.accountPass.newpass === this.accountPass.confirm) {
                var id = localStorage.getItem('userid');
                var res = await this.callApi('post', `/api/admin/user/password/change?id=${id}&type=account`, this.accountPass);
                this.isActive = false;
                if (res.data == 0) {
                    this.notifiyGlobal(`পুরানো পাসওয়ার্ড মেলেনি`);
                } else {
                    this.notifiyGlobal(`পাসওয়ার্ড আপডেট হয়েছে`);
                    this.$router.push({ name: 'information' });
                }
            } else {
                this.isActive = false;
                this.notifiyGlobal(`নতুন পাসওয়ার্ড এবং নিশ্চিত পাসওয়ার্ড মেলেনি`);
            }
        },
    },
    mounted() {},
};
</script>
