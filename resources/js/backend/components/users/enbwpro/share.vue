<template>
    <div>
        <main class="background" style="padding: 0 15px !important;">
            <div class="boxs">
                <div class="b-title">
                    <span>আমন্ত্রণ প্রক্রিয়া</span>
                </div>
                <div class="d-flex justify-content-between muen px-3 my-2">
                    <div class="m-item text-center">
                        <div class="">
                            <i class="icon fa-solid fa-link"></i>
                        </div>
                        <div class="m-item-text">
                            <span>আপনার বন্ধুদের সাথে লিংকটি শেয়ার করুন</span>
                        </div>
                    </div>
                    <div class="m-item text-center">
                        <div class="">
                            <i class="icon fa-solid fa-link"></i>
                        </div>
                        <div class="m-item-text">
                            <span>নিবন্ধন সফল</span>
                        </div>
                    </div>
                    <div class="m-item text-center">
                        <div class="">
                            <i class="icon fa-solid fa-link"></i>
                        </div>
                        <div class="m-item-text">
                            <span>রিওয়ার্ড অর্জন করুন</span>
                        </div>
                    </div>
                </div>
                <div class="m-btn copyBtn" style="cursor: pointer;" :data-clipboard-text="ref">বন্ধুদের আমন্ত্রণ জানাতে লিংকটি কপি করতে ক্লিক করুন</div>
            </div>

            <div class="boxs mt-4">
                <div class="b-title">
                    <span>বিস্তারিত বর্ণনা</span>
                </div>

                
<p><span class="detailed-description my-3 text-center">রেফার করে অর্থ উপার্জন। করুন </span> রেফারেল লিংক শেয়ার করে আপনি আপনার বন্ধু বা পরিচিতদের আপনার প্ল্যাটফর্মে যোগ দিতে আমন্ত্রণ জানাতে পারেন। প্রতিটি সফল রেফারেলের জন্য আপনি নির্দিষ্ট পুরস্কার বা কমিশন পেতে পারেন। যত বেশি মানুষকে রেফার করবেন, তত বেশি উপার্জন করতে পারবেন!</p>


                
            </div>
        </main>

        <Message :Isactive="Messageactive" :Message="Message" />
    </div>
</template>

<script>
import ClipboardJS from 'clipboard';
export default {
    created() {
        const clipboard = new ClipboardJS('.copyBtn');
        clipboard.on('success', (e) => {
            this.notifiy('রেফার লিংক সফলভাবে কপি করা হয়েছে');
        });
        clipboard.on('error', (e) => {
            this.notifiy('ক্লিপবোর্ডে কপি করতে ব্যর্থ!');
        });
    },
    data() {
        return {
            ref: '',
            Messageactive: false,
            Message: '',
            rows: {}
        };
    },
    methods: {
        notifiy(mess) {
            this.Messageactive = true;
            this.Message = mess;
            setTimeout(() => {
                this.Messageactive = false;
            }, 1000);
        },
        async getData() {
            var id = localStorage.getItem('userid');
            var res = await this.callApi('get', `/api/admin/user/${id}`, []);
            this.ref = window.location.origin + '/register?ref=' + res.data.user.username;
            this.rows = res.data;
        },
    },
    mounted() {
        this.getData();
    },
}
</script>
