<template>
    <div class="container bg-slate-50 py-5">
        <form>
            <div>
                <h3 class="title">ចំនួនប្រាក់កម្ចី</h3>
                <div class="text-center range__value">
                    <span id="amount">{{ convertToMoney(amount) }}</span>
                </div>
                <div class="range__slider">
                    <input type="range" v-model.number="amount" step="100000" min="0" max="350000000" @mousemove="loanCalculator">
                </div>
            </div>
            <div>
                <h3 class="title">ចំនួនខែ</h3>
                <select v-model.number="month" @change="loanCalculator" class="border border-gray-200 text-gray-900 text-sm focus:outline-none block w-full p-2.5">
                    <option v-for="option in options" v-bind:value="option.id" v-bind:key="option.id">{{ option.name }}</option>
                </select>
            </div>
        </form>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 mt-5">
            <div class="bg-blue-500 rounded-sm w-full h-auto p-3">
                <div class="flex items-center justify-between">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="w-5 h-5 fill-white">
                        <path d="M320 96H192L144.6 24.88C137.5 14.24 145.1 0 157.9 0H354.1C366.9 0 374.5 14.24 367.4 24.88L320 96zM192 128H320C323.8 130.5 328.1 133.3 332.1 136.4C389.7 172.7 512 250.9 512 416C512 469 469 512 416 512H96C42.98 512 0 469 0 416C0 250.9 122.3 172.7 179 136.4C183.9 133.3 188.2 130.5 192 128V128zM276.1 224C276.1 212.9 267.1 203.9 255.1 203.9C244.9 203.9 235.9 212.9 235.9 224V230C230.3 231.2 224.1 232.9 220 235.1C205.1 241.9 192.1 254.5 188.9 272.8C187.1 283 188.1 292.9 192.3 301.8C196.5 310.6 203 316.8 209.6 321.3C221.2 329.2 236.5 333.8 248.2 337.3L250.4 337.9C264.4 342.2 273.8 345.3 279.7 349.6C282.2 351.4 283.1 352.8 283.4 353.7C283.8 354.5 284.4 356.3 283.7 360.3C283.1 363.8 281.2 366.8 275.7 369.1C269.6 371.7 259.7 373 246.9 371C240.9 370 230.2 366.4 220.7 363.2C218.5 362.4 216.3 361.7 214.3 361C203.8 357.5 192.5 363.2 189 373.7C185.5 384.2 191.2 395.5 201.7 398.1C202.9 399.4 204.4 399.9 206.1 400.5C213.1 403.2 226.4 407.4 235.9 409.6V416C235.9 427.1 244.9 436.1 255.1 436.1C267.1 436.1 276.1 427.1 276.1 416V410.5C281.4 409.5 286.6 407.1 291.4 405.9C307.2 399.2 319.8 386.2 323.1 367.2C324.9 356.8 324.1 346.8 320.1 337.7C316.2 328.7 309.9 322.1 303.2 317.3C291.1 308.4 274.9 303.6 262.8 299.9L261.1 299.7C247.8 295.4 238.2 292.4 232.1 288.2C229.5 286.4 228.7 285.2 228.5 284.7C228.3 284.3 227.7 283.1 228.3 279.7C228.7 277.7 230.2 274.4 236.5 271.6C242.1 268.7 252.9 267.1 265.1 268.1C269.5 269.7 283 272.3 286.9 273.3C297.5 276.2 308.5 269.8 311.3 259.1C314.2 248.5 307.8 237.5 297.1 234.7C292.7 233.5 282.7 231.5 276.1 230.3L276.1 224z"/>
                    </svg>
                    <span class="text-white text-lg font-bold">{{ convertToMoney(total) }}</span>
                </div>
                <h4 class="text-white text-base font-semibold capitalize mt-3">ចំនួន​សរុប</h4>
            </div>

            <div class="bg-blue-500 rounded-sm w-full h-auto p-3">
                <div class="flex items-center justify-between">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512" class="w-5 h-5 fill-white">
                        <path d="M374.6 73.39c-12.5-12.5-32.75-12.5-45.25 0l-320 320c-12.5 12.5-12.5 32.75 0 45.25C15.63 444.9 23.81 448 32 448s16.38-3.125 22.62-9.375l320-320C387.1 106.1 387.1 85.89 374.6 73.39zM64 192c35.3 0 64-28.72 64-64S99.3 64.01 64 64.01S0 92.73 0 128S28.7 192 64 192zM320 320c-35.3 0-64 28.72-64 64s28.7 64 64 64s64-28.72 64-64S355.3 320 320 320z"/>
                    </svg>
                    <span class="text-white text-lg font-bold">{{ convertToMoney(interest) }}</span>
                </div>
                <h4 class="text-white text-base font-semibold capitalize mt-3">ការប្រាក់សរុប</h4>
            </div>

            <div class="bg-blue-500 rounded-sm w-full h-auto p-3">
                <div class="flex items-center justify-between">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512" class="w-5 h-5 fill-white">
                        <path d="M374.6 73.39c-12.5-12.5-32.75-12.5-45.25 0l-320 320c-12.5 12.5-12.5 32.75 0 45.25C15.63 444.9 23.81 448 32 448s16.38-3.125 22.62-9.375l320-320C387.1 106.1 387.1 85.89 374.6 73.39zM64 192c35.3 0 64-28.72 64-64S99.3 64.01 64 64.01S0 92.73 0 128S28.7 192 64 192zM320 320c-35.3 0-64 28.72-64 64s28.7 64 64 64s64-28.72 64-64S355.3 320 320 320z"/>
                    </svg>
                    <span class="text-white text-lg font-bold">{{ rate }} %</span>
                </div>
                <h4 class="text-white text-base font-semibold capitalize mt-3">ការប្រាក់</h4>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'IndexPage',
        props: [],
        data() {
            return {
                title: 'Home Page',
                amount: "30000000",
                maxPrice: "350000000",
                interest: 0,
                rate: 0.7,
                month: 3,
                total: 0,
                options: [
                    { id: 3, name: '3 ខែ'},
                    { id: 6, name: '6 ខែ'},
                    { id: 12, name: '12 ខែ'},
                    { id: 18, name: '18 ខែ'},
                    { id: 24, name: '24 ខែ'},
                    { id: 36, name: '36 ខែ'},
                    { id: 48, name: '48 ខែ'},
                    { id: 60, name: '60 ខែ'}
                ]
            }
        },
        head() {
            return {
                title: this.title
            }
        },
        methods: {
            convertToMoney: function(value) {
                return '$' + parseFloat(value).toLocaleString('en-US', { maximumFractionDigits: 2 })
            },
            convertToPercent: function(value) {
                return Intl.NumberFormat('default', {
                    style: 'percent',
                    minimumFractionDigits: 2,
                    maximumFractionDigits: 2,
                }).format(value / 100);
            },
            loanCalculator: function() {
                this.interest = Math.ceil(Number(this.amount) * (this.rate * 0.01));
                this.total = ((Number(this.amount) / this.month) + this.interest);
            }
        },
        beforeMount() {
            this.loanCalculator(); // function is work when page is load
        }
    }
</script>
