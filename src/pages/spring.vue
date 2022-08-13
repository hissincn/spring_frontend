<template>
    <div>
        <div class="bg-red-200 border-red-600 text-red-600 border-l-4 p-4" role="alert" v-if="warnNeedInput">
            <p class="font-bold">提示</p>
            <p>需要输入一些文字...</p>
        </div>
        <div class="mb-3">
            <div class="relative inline-block w-10 mr-2 align-middle select-none">
                <input type="checkbox" name="toggle" :checked="springType" @click="springTypeChange()"
                    class="checked:bg-blue-500 outline-none focus:outline-none right-4 checked:right-0 duration-200 ease-in absolute block w-6 h-6 rounded-full bg-white border-4 appearance-none cursor-pointer" />
                <label for="Blue" class="block overflow-hidden h-6 rounded-full bg-gray-300 cursor-pointer">
                </label>
            </div>
            <span class="text-gray-400 font-medium" v-if="springType == true">{{
                    typeOptions.true
            }}</span>
            <span class="text-gray-400 font-medium" v-else>{{
                    typeOptions.false
            }}</span>
        </div>
        <label class="text-gray-700" for="name">
            <textarea v-model="userInput" @change="springContent()"
                class="flex-1 appearance-none border border-gray-300 w-full py-2 px-4 bg-white text-gray-700 placeholder-gray-400 rounded-lg text-base focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent"
                placeholder="倾听中..." rows="5" cols="40">
      </textarea>
        </label>
        <button @click="springContent()" type="button"
            class="mb-8 py-2 px-4 flex justify-center items-center bg-blue-600 hover:bg-blue-700 focus:ring-blue-500 focus:ring-offset-blue-200 text-white w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg">
            <svg width="20" height="20" fill="currentColor" class="mr-2 animate-spin" viewBox="0 0 1792 1792"
                xmlns="http://www.w3.org/2000/svg" v-if="loading">
                <path
                    d="M526 1394q0 53-37.5 90.5t-90.5 37.5q-52 0-90-38t-38-90q0-53 37.5-90.5t90.5-37.5 90.5 37.5 37.5 90.5zm498 206q0 53-37.5 90.5t-90.5 37.5-90.5-37.5-37.5-90.5 37.5-90.5 90.5-37.5 90.5 37.5 37.5 90.5zm-704-704q0 53-37.5 90.5t-90.5 37.5-90.5-37.5-37.5-90.5 37.5-90.5 90.5-37.5 90.5 37.5 37.5 90.5zm1202 498q0 52-38 90t-90 38q-53 0-90.5-37.5t-37.5-90.5 37.5-90.5 90.5-37.5 90.5 37.5 37.5 90.5zm-964-996q0 66-47 113t-113 47-113-47-47-113 47-113 113-47 113 47 47 113zm1170 498q0 53-37.5 90.5t-90.5 37.5-90.5-37.5-37.5-90.5 37.5-90.5 90.5-37.5 90.5 37.5 37.5 90.5zm-640-704q0 80-56 136t-136 56-136-56-56-136 56-136 136-56 136 56 56 136zm530 206q0 93-66 158.5t-158 65.5q-93 0-158.5-65.5t-65.5-158.5q0-92 65.5-158t158.5-66q92 0 158 66t66 158z">
                </path>
            </svg>
            {{ springType ? typeOptions.true : typeOptions.false }}
        </button>
    </div>

    <div class="bg-white dark:bg-gray-800 rounded-lg" v-if="!first">
        <div class="lg:flex w-full mx-auto py-5 px-5 sm:px-6 lg:py-5 lg:px-10 z-20">
            <h2 class="text-3xl font-extrabold text-gray-800 dark:text-white sm:text-4xl lg:w-1/2 overflow-visible">
                <span class="block mt-5 text-3xl">
                    根据Plutchik's Wheel和您提供的文本推算,您的情绪趋向为
                </span>
                <span class="block text-indigo-500">
                    {{ tend }}
                </span>
            </h2>
            <div class="lg:w-1/2">
                <div ref="container"></div>
            </div>
        </div>
    </div>

    <div v-if="!first">
        <div v-for="item in springData.result">

            <div class="bg-white dark:bg-gray-800 w-full mx-auto p-8 my-5 rounded-lg">

                <p class="text-gray-800 dark:text-white w-full mb-5 md:w-2/3 m-auto text-center  md:text-3xl text-xl font-extrabold">
                    <span class="font-bold text-indigo-500">｢</span>
                    {{ item.content }}
                    <span class="font-bold text-indigo-500">｣</span>
                </p>
                <div :id="`${item.id}`" class=""></div>
                <div class="flex items-center justify-center my-2">

                    <div class="flex ml-2 items-center justify-center">
                        <span class="font-semibold text-indigo-500 mr-2 text-md">
                            {{ item.author ? item.author : "无名氏" }}
                        </span>
                        <span class="text-gray-400 text-xl font-light">
                            -
                        </span>
                        <span class="text-gray-400 text-md ml-2">
                            {{ item.source ? item.source : "茶馆" }}
                        </span>
                    </div>
                </div>


            </div>
        </div>
    </div>

</template>

<script>
import DataSet from "@antv/data-set";
import { Chart } from "@antv/g2";
import axios from "axios";
const { DataView } = DataSet;
const api_similar = "https://springapi.hissin.cn/similar";
const api_contrary = "https://springapi.hissin.cn/contrary";

export default {
    name: 'spring',
    data() {
        return {
            springType: true,
            typeOptions: {
                true: "灌满",
                false: "镜子",
            },
            loading: false,
            springData: null,
            warnNeedInput: false,
            tend: "",
            first: true,
            need: 10,
            userInput:"",

        };
    },
    methods: {
        springTypeChange() {
            this.springType = !this.springType;
        },
        springContent() {
            if (!this.userInput) {
                this.warnNeedInput = true;
                setTimeout(() => this.warnNeedInput = false, 3000);
                throw new Exception("Please enter a user input");
            }
            this.loading = true;
            let url = this.springType ? api_contrary : api_similar;
            axios.post(url, {
                sentence: this.userInput,
                need: this.need
            })
                .then((res) => {
                    this.loading = false;
                    this.tend = res.data.raw.tend;
                    this.selfEmotion(res.data.raw.emotion);
                    this.showSentence(res.data.raw.emotion, res.data.result);
                    this.springData = res.data;
                    this.first = false;
                })
                .catch(function (err) {
                    console.log(err);
                    this.loading = false;
                });
        },
        selfEmotion(raw) {
            let selfEmotionRaw = JSON.parse(raw);
            const selfEmotion = new DataView().source([
                { item: '😁', value: selfEmotionRaw.joy },
                { item: '🤝', value: selfEmotionRaw.trust },
                { item: '😨', value: selfEmotionRaw.fear },
                { item: '😲', value: selfEmotionRaw.surprise },
                { item: '😢', value: selfEmotionRaw.sadness },
                { item: '🤢', value: selfEmotionRaw.disgust },
                { item: '😠', value: selfEmotionRaw.anger },
                { item: '🤨', value: selfEmotionRaw.anticipation }
            ]).transform({
                type: 'fold',
                fields: ['value'], // 展开字段集
                key: 'user', // key字段
                value: 'score' // value字段
            })

            if (this.springData == null) {
                this.$nextTick(
                    function () {
                        var chart = new Chart({
                            container: this.$refs.container,
                            autoFit: true,
                            height: 300,
                        })
                        this.chart = chart;
                        chart.data(selfEmotion.rows);
                        chart.scale('score', {
                            min: 0,
                            max: 5
                        })
                        chart.coordinate('polar', {
                            radius: 0.8
                        })
                        chart.tooltip({
                            shared: true,
                            showCrosshairs: true,
                            crosshairs: {
                                line: {
                                    style: {
                                        lineDash: [4, 4],
                                        stroke: '#333'
                                    }
                                }
                            }
                        })
                        chart.axis('item', {
                            line: null,
                            tickLine: null,
                            grid: {
                                line: {
                                    style: {
                                        lineDash: null
                                    }
                                }
                            }
                        })
                        chart.axis('score', {
                            line: null,
                            tickLine: null,
                            grid: {
                                line: {
                                    type: 'line',
                                    style: {
                                        lineDash: null
                                    }
                                }
                            }
                        })
                        chart.line().position('item*score').color('user').size(2)
                        chart.point().position('item*score').color('user').shape('circle').size(4)
                            .style({
                                stroke: '#fff',
                                lineWidth: 1,
                                fillOpacity: 1
                            })
                        chart.area().position('item*score').color('user')
                        chart.render()
                    }
                )
            }
            else {
                this.chart.changeData(selfEmotion.rows)
            }
        },
        showSentence(raw, heres) {

            raw = JSON.parse(raw);
            let senChart = {};

            for (let here of heres) {
                let sentencesEmotion = new DataView().source([
                    { item: '😁', you: raw.joy, here: here.emotion.joy },
                    { item: '🤝', you: raw.trust, here: here.emotion.trust },
                    { item: '😨', you: raw.fear, here: here.emotion.fear },
                    { item: '😲', you: raw.surprise, here: here.emotion.surprise },
                    { item: '😢', you: raw.sadness, here: here.emotion.sadness },
                    { item: '🤢', you: raw.disgust, here: here.emotion.disgust },
                    { item: '😠', you: raw.anger, here: here.emotion.anger },
                    { item: '🤨', you: raw.anticipation, here: here.emotion.anticipation }
                ]).transform({
                    type: 'fold',
                    fields: ['you', 'here'], // 展开字段集
                    key: 'user', // key字段
                    value: 'score' // value字段
                })


                let first = this.first;

                setTimeout(
                    function () {
                        senChart[here.id] = new Chart({
                            container: `${here.id}`,
                            autoFit: true,
                            height: 200,
                        });
                        let chart = senChart[here.id];
                        chart.data(sentencesEmotion.rows);
                        chart.scale('score', {
                            min: 0,
                            max: 5
                        })
                        chart.coordinate('polar', {
                            radius: 0.7,
                        })
                        chart.tooltip({
                            shared: true,
                            showCrosshairs: true,
                            crosshairs: {
                                line: {
                                    style: {
                                        lineDash: [4, 4],
                                        stroke: '#333'
                                    }
                                }
                            }
                        })
                        chart.axis('item', {
                            line: null,
                            tickLine: null,
                            grid: {
                                line: {
                                    style: {
                                        lineDash: null
                                    }
                                }
                            }
                        })
                        chart.axis('score', {
                            line: null,
                            tickLine: null,
                            grid: {
                                line: {
                                    type: 'line',
                                    style: {
                                        lineDash: null
                                    }
                                }
                            }
                        })
                        chart.line().position('item*score').color('user').size(2)
                        chart.point().position('item*score').color('user').shape('circle').size(4)
                            .style({
                                stroke: '#fff',
                                lineWidth: 1,
                                fillOpacity: 1
                            })
                        chart.area().position('item*score').color('user')
                        chart.render()
                        if (first == false) document.getElementById(`${here.id}`).firstElementChild.remove();
                    }
                )

            }
        },


    }
};
</script>