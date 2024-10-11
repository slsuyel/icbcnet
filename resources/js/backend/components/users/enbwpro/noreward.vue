<template>
    <div>
        <header
            class="header-text mb-5 defaltColorBg"
            style="margin-bottom: -16px !important"
        >
            <p class="py-2 text-white"></p>
        </header>
        <main class="mb-5 pb-5">
            <div>
                <div class="bg-gd p-3 text-white">
                    <p class="fs-4 fw-bold text-center">
                        আইসিবি নেটওয়ার্ক বিনিয়োগ প্রোগ্রাম
                    </p>
                    <div class="bg-gdd p-3 rounded-1">
                        <table style="width: 100%">
                            <thead>
                                <tr>
                                    <th class="text-white">পণ্য</th>
                                    <th class="text-white">মূল্য</th>
                                    <th class="text-white">প্রতিদিন</th>
                                    <th class="text-white">প্রতি মাসে</th>
                                    <th class="text-white">মোট আয়</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="packag in packages"
                                    :key="'package' + packag.id"
                                >
                                    <td>{{ packag.name }}</td>
                                    <td>{{ packag.packagePrice }}</td>
                                    <td>{{ packag.packageEarn }}</td>
                                    <td>{{ packag.packageEarn * 30 }}</td>
                                    <td>
                                        {{
                                            packag.packageEarn *
                                            packag.packageDuration
                                        }}
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            <div class="mt-5">
                <div class="bg-gd p-3 text-white">
                    <p class="fs-4 fw-bold text-center">
                        আইসিবি নেটওয়ার্ক টিম কমিশন প্রোগ্রাম
                    </p>
                    <div class="bg-gdd p-3 rounded-1">
                        <table style="width: 100%">
                            <thead>
                                <tr>
                                    <th class="text-white">পণ্যের নাম</th>
                                    <th class="text-white">মূল্য</th>
                                    <th class="text-white">শতাংশ</th>
                                    <th class="text-white">অর্থ</th>
                                    <th class="text-white">টিম</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="packag in packages"
                                    :key="'teamProfit' + packag.id"
                                >
                                    <td>{{ packag.name }}</td>
                                    <td>{{ packag.packagePrice }}</td>
                                    <td>{{ levels[0].comission }}</td>
                                    <td>
                                        {{
                                            (packag.packagePrice *
                                                levels[0].comission) /
                                            100
                                        }}
                                    </td>
                                    <td>
                                        {{ levels[0].task_commisition }}%
                                        {{ levels[1].task_commisition }}%
                                        {{ levels[2].task_commisition }}%
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
export default {
    data() {
        return {
            levels: {},
            packages: {},
        };
    },
    methods: {
        async getPackages() {
            var res = await this.callApi("get", `/api/get/packages`, []);
            this.packages = res.data;

            var resLevels = await this.callApi("get", `/api/admin/level`, []);
            this.levels = resLevels.data;
        },
    },
    mounted() {
        this.getPackages();
    },
};
</script>

<style scoped>
.bg-gd {
    background: var(--defaltColor);
}

th {
    border: 1px solid white;
    font-size: 14px;
    text-align: center;
    padding: 3px;
    color: black;
}
td {
    border: 1px solid white;
    background: var(--defaltColor);
    text-align: center;
    font-size: 10px;
}

thead {
    background: #6b5ca1;
}
.bg-gdd {
    background: rgb(219 255 19);
}
</style>
