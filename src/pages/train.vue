<template>


    <div class="bg-yellow-200 border-yellow-600 text-yellow-600 border-l-4 p-4 mb-4" role="alert"
        v-if="status == 'cancel'">
        <p class="font-bold">
            已暂停
        </p>
    </div>

    <div class="bg-green-200 border-green-600 text-green-600 border-l-4 p-4 mb-4" role="alert"
        v-if="status == 'finish'">
        <p class="font-bold">
            成功提交
        </p>
    </div>

    <div class="container mx-auto p-4 sm:p-6 lg:p-8 bg-white dark:bg-gray-800" v-if="step == 1">
        <div class="flex flex-wrap -mx-8">
            <div class="w-full lg:w-1/2 px-8">
                <div class="mb-12 lg:mb-0 pb-12 lg:pb-0 border-b lg:border-b-0">
                    <h2 class="mb-4 text-3xl lg:text-4xl font-bold font-heading dark:text-white">
                        Training Plan For Spring.
                    </h2>
                    <p class="mb-8 leading-loose text-gray-500 dark:text-gray-300">
                        Make it better.
                    </p>
                    <div class="w-full md:w-full">
                        <button type="button" @click="getNew"
                            class="py-2 px-4  bg-indigo-600 hover:bg-indigo-700 focus:ring-indigo-500 focus:ring-offset-indigo-200 text-white w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2  rounded-lg ">
                            Let it go
                        </button>
                    </div>
                </div>
            </div>
            <div class="w-full lg:w-1/2 px-8">
                <ul class="space-y-12">
                    <li class="flex -mx-4">
                        <div class="px-4">
                            <span
                                class="flex w-16 h-16 mx-auto items-center justify-center text-2xl font-bold font-heading rounded-full bg-blue-50 text-blue-600">
                                🍪
                            </span>
                        </div>
                        <div class="px-4">
                            <h3 class="my-4 text-xl font-semibold dark:text-white">
                                Read the sentences on the screen and then fill the emotion wheel.
                            </h3>
                            <p class="text-gray-500 dark:text-gray-300 leading-loose">
                                Easier than you think.
                            </p>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
    <div v-if="step == 2">
        <article class="p-6 bg-white sm:p-8 rounded-xl ring ring-indigo-50">
            <div class="flex items-start">
                <div class="hidden sm:grid sm:h-20 sm:w-20 sm:shrink-0 sm:place-content-center sm:rounded-full sm:border-2 sm:border-indigo-500"
                    aria-hidden="true">
                    <div class="flex items-center gap-1">
                        <span class="h-8 w-0.5 rounded-full bg-indigo-500"></span>
                        <span class="h-6 w-0.5 rounded-full bg-indigo-500"></span>
                        <span class="h-4 w-0.5 rounded-full bg-indigo-500"></span>
                        <span class="h-6 w-0.5 rounded-full bg-indigo-500"></span>
                        <span class="h-8 w-0.5 rounded-full bg-indigo-500"></span>
                    </div>
                </div>

                <div class="sm:ml-8">
                    <strong
                        class="rounded border border-indigo-500 bg-indigo-500 px-3 py-1.5 text-[10px] font-medium text-white">
                        Spring ID #{{ raw.id }}
                    </strong>

                    <h2 class="mt-4 text-lg font-medium sm:text-xl">
                        <a href="" class="hover:underline"> {{ raw.content }} </a>
                    </h2>

                    <div class="mt-4 sm:flex sm:items-center sm:gap-2">
                        <div class="flex items-center text-gray-500">
                            👉
                            <p class="ml-1 text-xs font-medium">{{ raw.type }}</p>
                        </div>

                        <span class="hidden sm:block" aria-hidden="true">&middot;</span>

                        <p class="mt-2 text-xs font-medium text-gray-500 sm:mt-0">
                            {{ raw.author ? raw.author : "一言" }} - {{ raw.source ? raw.source : "茶馆" }}
                        </p>
                    </div>
                </div>
            </div>
        </article>

        <div class="p-6 bg-white sm:p-8 rounded-xl ring ring-indigo-50 my-4">

            <div class="bg-green-200 border-green-600 text-green-600 border-l-4 p-4" role="alert">
                <p class="font-bold">
                    滑动以修改情绪评分,同颜色为相反情绪。评分越高，情绪越强烈。
                </p>
            </div>

            <div class="leading-loose text-2xl mb-4 md:flex my-8">
                <div class="md:w-1/2">
                    😁 <input type="range" min="0" max="50" :value="result.joy * 10" ref="joy"
                        @change="result.joy = this.$refs.joy.value / 10" class="range w-1/2" /> 喜悦 {{ result.joy }}<br>


                    🤝 <input type="range" min="0" max="50" :value="result.trust * 10" ref="trust"
                        @change="result.trust = this.$refs.trust.value / 10" class="range range-primary w-1/2" />
                    信任 {{ result.trust }}<br>

                    😨 <input type="range" min="0" max="50" :value="result.fear * 10" ref="fear"
                        @change="result.fear = this.$refs.fear.value / 10" class="range range-secondary w-1/2" />
                    恐惧 {{ result.fear }}<br>

                    😲 <input type="range" min="0" max="50" :value="result.surprise * 10" ref="surprise"
                        @change="result.surprise = this.$refs.surprise.value / 10" class="range range-accent w-1/2" />
                    惊喜 {{ result.surprise }}<br>
                </div>
                <div class="md:w-1/2">
                    😢 <input type="range" min="0" max="50" :value="result.sadness * 10" ref="sadness"
                        @change="result.sadness = this.$refs.sadness.value / 10" class="range w-1/2" /> 悲伤 {{
                                result.sadness
                        }}<br>

                    🤢 <input type="range" min="0" max="50" :value="result.disgust * 10" ref="disgust"
                        @change="result.disgust = this.$refs.disgust.value / 10" class="range range-primary w-1/2" />
                    厌恶 {{ result.disgust }}<br>

                    😠 <input type="range" min="0" max="50" :value="result.anger * 10" ref="anger"
                        @change="result.anger = this.$refs.anger.value / 10" class="range range-secondary w-1/2" />
                    愤怒 {{ result.anger }}<br>

                    🤨 <input type="range" min="0" max="50" :value="result.anticipation * 10" ref="anticipation"
                        @change="result.anticipation = this.$refs.anticipation.value / 10"
                        class="range range-accent w-1/2" />
                    期待 {{ result.anticipation }}<br>
                </div>
            </div>

            <div class="mt-11 text-center mb-4">
                <button @click="cancel" type="button" class="btn btn-outline btn-primary mr-2">暂停</button>
                <label for="prepush" class="btn btn-secondary w-24">下一个</label>
                <button @click="refresh" class="btn btn-primary ml-2">换一个</button>

                <input type="checkbox" id="prepush" class="modal-toggle" />
                <div class="modal">
                    <div class="modal-box">
                        <h3 class="font-bold text-lg">确认提交?</h3>
                        <p class="py-4">在提交之前，请确认您的情绪评分是否正确。</p>
                        <div class="modal-action">
                            <label for="prepush" class="btn btn-ghost">修改一下</label>
                            <label for="prepush" class="btn btn-secondary" @click="pushResult">确认无误</label>
                        </div>
                    </div>
                </div>


            </div>
        </div>


    </div>
</template>

<script>
import axios from "axios";
const apiUrl = "https://webapi-spring-iwfjnxhgbm.cn-shanghai.fcapp.run";

export default {
    name: 'train',
    data() {
        return {
            step: 1,
            raw: {},
            token: '',
            result: {},
            status: "ready",
        }
    },
    methods: {
        getNew() {
            axios.post(apiUrl+'/train/new', {})
                .then((res) => {
                    this.raw = res.data.raw;
                    this.token = res.data.token;
                    this.result = res.data.raw.emotion;
                    this.step = 2;
                }).catch(function (err) {
                    console.log(err);
                })
        },
        refresh() {
            axios.post(apiUrl+'/train/new', {})
                .then((res) => {
                    this.raw = res.data.raw;
                    this.token = res.data.token;
                    this.result = res.data.raw.emotion;
                }).catch(function (err) {
                    console.log(err);
                })
        },
        pushResult() {
            axios.post(apiUrl+'/train/update', {
                id: this.raw.id,
                token: this.token,
                emotion: this.result
            })
                .then((res) => {
                    console.log("success to push");
                    this.getNew();
                    this.status = 'finish';
                    setTimeout(() => {
                        this.status = 'ready';
                    }, 1000);
                }).catch(function (err) {
                    console.log(err);
                })
        },
        cancel() {
            axios.post(apiUrl+'/train/cancel', {
                id: this.raw.id,
                token: this.token
            })
                .then((res) => {
                    console.log("success to cancel");
                    this.step = 1;
                    this.status = 'cancel';
                    setTimeout(() => {
                        this.status = 'ready';
                    }, 1000);
                }).catch(function (err) {
                    console.log(err);
                })
        }
    }
}
</script>