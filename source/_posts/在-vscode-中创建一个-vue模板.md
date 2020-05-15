---
title: 在 vscode 中创建一个.vue模板
abbrlink: 49080
date: 2019-03-05 17:06:24
tags:
---











## step1：新建模板并保存

 文件  -->  首选项  -->  用户代码片段  -->  输入vue，选择vue.json，复制一下代码粘贴进去

```
"Print to console": {
		"prefix": "vue",
		"body": [
			"<template>",
			"  <div class=\"myDiv\">$0</div>",
			"</template>",
			"",
			"<script>",
			"export default {",
			"  components:{},",
			"  props:{},",
			"  data(){",
			"    return {",
			"    }",
			"  },",
			"  created: function() {},",
			"  mounted: function() {},",
			"  methods:{},",
			"  watch:{},",
			"  computed:{}",
			"}",
			"</script>",
			"<style lang=\"scss\" scoped>",
			".myDiv{}",
			"</style>"
		],
		"description": "A vue file template"
	}
```

<!-- more -->

![](/在-vscode-中创建一个-vue模板/1.png)



## step2：添加配置

 文件 --> 首选项 --> 设置 ---> 添加这2项 

![](/在-vscode-中创建一个-vue模板/2.png)

```
"editor.snippetSuggestions": "top",
"editor.formatOnPaste": true
```

![](/在-vscode-中创建一个-vue模板/3.png)

![4](/在-vscode-中创建一个-vue模板/4.png)



## step3：重启vscode并测试

新建一个.vue的文件，并在该文件中输入vue，按下tab键

成功

