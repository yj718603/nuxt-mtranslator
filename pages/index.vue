<template>
  <div>
    <Header v-on:refreshLanguag="refreshLanguag"/>
    <div class="page-content">
      <div style="display:flex;z-index:1">
        <div style="flex:1">
          <div style="height:40px;line-height:40px;display:flex">
            {{sourceCode|currentLanguage(languagCode)}}
            <img :class="{'icon-arrow-rotate':isArrow0}" @click="isArrow0=!isArrow0" class="icon-arrow"
                 src="~@/assets/icon-arrow.png"/>
          </div>
          <div style="margin-top:6px;position:relative">
            <el-input
              @input="checkLanguage"
              maxlength="3000"
              placeholder="请输入内容"
              show-word-limit
              type="textarea"
              v-model="sourceText">
            </el-input>
            <div @click="translateSource" class="translate-btn">
              翻译
            </div>
            <div @click="clearSource" class="translate-clear">
              <img src="~@/assets/clear.png"/>
            </div>
          </div>
        </div>
        <div style="flex:1;margin-left:10px;display:flex;flex-direction:column;position:relative">
          <div style="height:40px;line-height:40px;padding-left:15px;display:flex">
            {{targetCode|currentLanguage(languagCode)}} <img :class="{'icon-arrow-rotate':isArrow1}"
                                                             @click="isArrow1=!isArrow1"
                                                             class="icon-arrow"
                                                             src="~@/assets/icon-arrow.png"/>
          </div>
          <div style="margin-top:6px;flex:1;background:#F5F5FA;padding:10px">
            <span :key="index" class="target-item" v-for="(item,index) in targetText">{{item.t}}</span>
          </div>
          <div class="target-hint">
            <img class="target-icon" src="~@/assets/t-mSLLM.png"/>
            mSLLM
          </div>
          <img class="exchange" src="~@/assets/exchange.png"/>
        </div>
        <div class="animate__fadeInRight" style="flex:1;margin-left:10px;display:flex;flex-direction:column">
          <div style="height:40px;line-height:40px"></div>
          <div style="margin-top:6px;flex:1;background:#F5F5FA;padding:10px">
            <span :key="index" class="target-item" v-for="(item,index) in targetText2">{{item.t}}</span>
          </div>
          <div class="target-hint">
            <img class="target-icon" src="~@/assets/t-Google-Translate.png"/> Google Translat
          </div>
        </div>
      </div>
      <div style="height:40px;line-height:40px;position:absolute;right:0;top:0;text-align:right;display:flex">
        <div :key="item.code" style="display: flex" v-for="item in cities"
             v-if="checkedCities.indexOf(item.code)!=-1">
          <img :src="item.icon" class="el-checkbox-icon"/>
          <span>{{item.value}}</span>
        </div>
        <div style="display:flex;margin-left:10px">
          <el-popover trigger="click">
            <el-checkbox-group v-model="checkedCities">
              <div :key="item.code" class="checkbox-group-translator-item" v-for="item in cities">
                <img :src="item.icon" class="el-checkbox-icon"/>
                <el-checkbox :label="item.code">
                  {{item.value}}
                  <span
                    style="background:#F2EBFD;color:rgba(124, 58, 237, 1);display:inline-block;font-size:12px;width:30px;height:16px;line-height:16px;border-radius:8px;border:1px solid rgba(124, 58, 237, 1);text-align:center">
                                     Pro
                                 </span>
                </el-checkbox>
              </div>
            </el-checkbox-group>
            <img :class="{'icon-arrow-rotate':isArrow}" @click="isArrow=!isArrow" class="icon-arrow"
                 slot="reference"
                 src="~@/assets/icon-arrow.png"/>
          </el-popover>
        </div>
      </div>
      <div class="original-languages-popover" v-show="(isArrow0 && !isArrow1) || (!isArrow0 && isArrow1)">
        <div style="border-bottom: 1px solid #D9D9D9;position:relative">
          <el-input @input="searchLanguages" clearable placeholder="搜索语言" v-model="searchKeyword"></el-input>
          <img class="translator-search" src="~@/assets/search.png"/>
        </div>
        <div class="original-languages">
          <div :key="item.code" class="item-language" v-for="(item) in configLanguages">
            <span style="width:40px;display: inline-block"></span>
            {{item.code|currentLanguage(languagCode)}}
          </div>
        </div>
      </div>
    </div>
    <div style="display:none">多语言:{{$t("hello")}}</div>
    <Footer/>
  </div>
</template>
<script>
  import support from "../plugins/SupportLanguages.js";

  export default {
    name: 'IndexPage',
    components: {},
    head() {
      return {
        title: "我是自定义标题"
      }
    },
    data() {
      return {
        isDark: false,
        sourceCode: "und",
        sourceText: "",
        //
        targetCode: "zh_Hans",
        targetText: [],
        copyData: "copyData",
        targetText2: [],
        copyData2: "copyData2",
        //
        isArrow: false,
        isArrow0: false,
        isArrow1: false,
        //
        itemCities: [],
        cities: [
          {
            icon: require("~/assets/t-mSLLM.png"),
            value: "mSLLM",
            code: "mSLLM"
          },
          {
            icon: require("~/assets/t-Google-Translate.png"),
            value: "Google Translate",
            code: "Google Translate"
          },
          {
            icon: require("~/assets/t-OpenAI-GPT.png"),
            value: "OpenAI-GPT",
            code: "OpenAI-GPT"
          },
          {
            icon: require("~/assets/t-Gemini.png"),
            value: "Gemini",
            code: "Gemini"
          },
          {
            icon: require("~/assets/t-Claude.png"),
            value: "Claude",
            code: "Claude"
          },
          {
            icon: require("~/assets/t-Llama.png"),
            value: "Llama",
            code: "Llama"
          },
          {
            icon: require("~/assets/t-Qwen.png"),
            value: "Qwen",
            code: "Qwen"
          },
          {
            icon: require("~/assets/t-DeepSeek.png"),
            value: "DeepSeek",
            code: "DeepSeek"
          },
          {
            icon: require("~/assets/t-Yi.png"),
            value: "Yi",
            code: "Yi"
          }],
        checkedCities: ["mSLLM", "Google Translate"],
        //
        searchKeyword: "",
        configLanguages: [],
        languagCode: this.$cookies.get("languagCode") || "zh_Hans"
      };
    },
    filters: {
      currentLanguage(sourceCode, code) {
        if (sourceCode == "und") {
          return "英语（自动检测）";
        }
        const current = support.configs.find(item => item["code"] == sourceCode);
        if (!current) {
          return null;
        }
        return current[code];
      }
    },
    methods: {
      checkLanguage() {
        /*if (this.sourceText.length > 5) {
          const cldFactoryPromise = cld3Asm.loadModule();
          cldFactoryPromise.then((cldFactory) => {
            console.log(cldFactory.create(5, 1000).findLanguage(this.sourceText));
          });
        }*/
      },
      theme() {
        this.isDark = !this.isDark;
      },
      refreshLanguag(value) {
        //标签i18n
        this.$i18n.locale = value;
        ///内容i18n
        this.languagCode = value;
        //把语言保存在localStorage中
        this.$cookies.set("languagCode", value);
      },
      translateSource() {
        /*const params = {
          contents: ["The 2024 Nobel Prize in physics has been awarded to John Hopfield and Geoffrey Hinton for their fundamental discoveries in machine learning, which paved the way for how artificial intelligence is used today.\n\nHopfield, a professor at Princeton University and Hinton, a computer scientist at the University of Toronto, were praised for laying the foundations for the machine learning that powers many of today’s AI-based products and applications. Hinton, however, has also expressed fears about AI’s future development, cutting ties with his former employer Google in order to speak more freely on the issue.\n\n“Their work was fundamental in laying the cornerstones for what we experience today as artificial intelligence,” Mark Pearce, a member of the Nobel committee of physics, told CNN.\n\nThe committee announced the prestigious honor, seen as the pinnacle of scientific achievement, in Sweden on Monday. The prize carries a cash award of 11 million Swedish kronor ($1 million).\n\nHinton, who has been dubbed the “godfather” of artificial intelligence (AI), said he was “flabbergasted” to receive the prize."],
          sourceLanguageCode: "en",
          targetLanguageCode: "zh-Hans",
          models: ["mSLLM", "GPT"]
        };
        apiRequest.translateDemo(params).then((res) => {
          this.targetText = res.r[0].translation;
          this.targetText2 = res.r[1].translation;
        });*/
      },
      clearSource() {
        this.sourceText = "";
      },
      onCopySuccess(e) {
        alert("复制成功！");
      },
      searchLanguages() {
        if (this.searchKeyword != "") {
          let arrLanguage = [];
          support.configs.forEach(item => {
            if (item[this.languagCode].search(this.searchKeyword) != -1) {
              arrLanguage.push(item);
            }
          });
          this.configLanguages = arrLanguage;
        } else {
          this.configLanguages = support.configs;
        }
      }
    },
    created() {
      this.configLanguages = support.configs;
    }
  };
</script>

<style type="text/css">
  .exchange {
    width: 30px;
    height: 30px;
    position: absolute;
    left: -20px;
    top: 5px;
  }

  .checkbox-group-translator-item {
    display: flex;
  }

  .checkbox-group-translator-item .el-checkbox-icon {
    width: 30px;
    height: 30px;
  }

  .checkbox-group-translator-item .el-checkbox {
    position: relative;
    height: 30px;
    line-height: 30px;
    width: 100%;
    padding-right: 20px;
  }

  .checkbox-group-translator-item .el-checkbox .el-checkbox__input {
    position: absolute;
    right: 0;
    line-height: 30px;
  }

  .checkbox-group-translator-item .el-checkbox__label {
    font-size: 16px;
    color: rgba(51, 51, 51, 1);
  }

  .translator-search {
    width: 18px;
    height: 18px;
    position: absolute;
    left: 10px;
    top: 14px;
    z-index: 2;
  }

  .el-checkbox-group {
    font-size: 16px;
  }

  .el-textarea__inner {
    min-height: 300px !important;
    resize: none;
  }

  .original-languages-popover {
    position: absolute;
    top: 46px;
    left: 0px;
    z-index: 2;
    width: 100%;
    background: #ffffff;
    border-radius: 4px;
    border: 1px solid #D9D9D9;
    box-shadow: 0px 9px 28px 8px #F8F5FE;
    box-shadow: 0px 6px 16px 0px #EBEBEB;
    box-shadow: 0px 3px 6px -4px #D9D9D9;
  }

  .original-languages-popover .el-input__inner {
    background-color: #ffffff;
    text-indent: 20px;
  }

  .original-languages {
    display: grid;
    grid-template-columns: repeat(6, 16.66%);
  }

  .item-language {
    height: 30px;

    text-align: left;
    cursor: pointer;
    line-height: 30px;
  }

  .item-language-active {
    background: #F2EBFD;
  }

  .item-language:hover {
    background: #F5F5F5;
  }

  .el-textarea .el-input__count {
    left: 10px;
  }

  .translate-btn {
    width: 100px;
    height: 44px;
    line-height: 44px;
    border-radius: 4px;
    color: #ffffff;
    background: #7C3AED;
    text-align: center;
    position: absolute;
    right: 10px;
    bottom: 10px;
    cursor: pointer;
  }

  .translate-clear {
    position: absolute;
    right: 5px;
    top: 5px;
    cursor: pointer;
  }

  .translate-clear img {
    width: 24px;
    height: 24px;
  }

  .target-item {
    cursor: pointer;
    white-space: pre-wrap;
    word-break: break-word;
    word-wrap: break-word;
    display: inline-block;
  }

  .target-item:hover {
    background: rgba(124, 58, 237, 0.15);
  }

  .target-hint {
    background: #F5F5FA;
    display: flex;
    color: #999999;
    padding: 10px;
    font-size: 16px;
    position: relative;
  }

  .target-icon {
    width: 20px;
    height: 20px;
    margin-top: 2px;
  }

  .target-btn-clipboard {
    position: absolute;
    right: 10px;
  }

  .target-btn-clipboard img {
    width: 24px;
    height: 24px;
  }
</style>
