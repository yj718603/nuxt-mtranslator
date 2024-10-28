<template>
  <div class="page-header">
    <div class="page-header-content">
      <img src="~@/assets/t-logo.png" class="logo"/>
      <div class="nav-item">定价</div>
      <div class="nav-item">下载</div>
      <div class="nav-item">资源</div>
      <div style="flex:1">
      </div>
      <div class="nva-item-light">
        <img src="~@/assets/light.png"/>
      </div>
      <div class="nva-item-tellurian">
        <img src="~@/assets/tellurian.png"/>
        <el-select @change="changeLanguag" v-model="languagCode" placeholder="请选择">
          <el-option v-for="item in languageConfig"
                     :key="item.value"
                     :label="item.label"
                     :value="item.value">
          </el-option>
        </el-select>
      </div>
      <div class="nva-item-theme">
        <img src="~@/assets/theme-light.png" @click="theme"/>
      </div>
      <div class="nva-item-btn">
        登录
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: "x-header",
    data() {
      return {
        languageConfig: [
          {
            value: "en",
            label: "English"
          }, {
            value: "zh_Hans",
            label: "中文（简体）"
          }, {
            value: "zh_Hant",
            label: "中文（繁體）"
          }, {
            value: "es",
            label: "español"
          },
          {
            value: "fr",
            label: "français"
          },
          {
            value: "pt",
            label: "português "
          },
          {
            value: "ru",
            label: "русский"
          },
          {
            value: "de",
            label: "Deutsch"
          },
          {
            value: "ja",
            label: "日本語"
          },
          {
            value: "it",
            label: "italiano"
          },
          {
            value: "ko",
            label: "한국어"
          }
        ],
        languagCode: this.$cookies.get("languagCode") || "zh_Hans"
      };
    },
    methods: {
      theme() {
        const appDocument = document.getElementsByTagName("HTML")[0];
        if (appDocument.getAttribute("data-theme") == "light") {
          appDocument.setAttribute("data-theme", "dark");
        } else {
          appDocument.setAttribute("data-theme", "light");
        }
      },
      changeLanguag(value) {
        this.$emit("refreshLanguag", value);
      }
    }
  };
</script>

<style type="text/css">
  .nav-item {
    width: auto;
    margin-left: 40px;
    cursor: pointer;
  }

  .nva-item-light {
    width: 44px;
    height: 44px;
    border-radius: 4px;
    background: #EBE1FC;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .nva-item-light img {
    width: 26px;
    height: 26px;
  }

  .nva-item-tellurian {
    width: 200px;
    margin-left: 20px;
    background: #F5F5FA;
    padding-left: 10px;
    border-radius: 4px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .nva-item-tellurian img {
    width: 26px;
    height: 26px;
  }

  .nva-item-theme {
    width: 44px;
    height: 44px;
    border-radius: 4px;
    background: #F5F5FA;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-left: 10px;
  }

  .nva-item-theme img {
    width: 26px;
    height: 26px;
    cursor: pointer;
  }

  .nva-item-btn {
    width: 100px;
    height: 44px;
    border-radius: 4px;
    color: #ffffff;
    background: #7C3AED;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-left: 10px;
    cursor: pointer;
  }

  .el-input__inner {
    border: 0px;
    background-color: #F5F5FA;
    height: 44px;
    line-height: 44px;
  }

  .el-select .el-input.is-focus .el-input__inner {
    border: 0px;
  }

  .el-select-dropdown__wrap {
    max-height: 100%;
  }

  .el-select-dropdown__item.selected {
    color: #7C3AED;
    font-weight: 400;
  }
</style>
