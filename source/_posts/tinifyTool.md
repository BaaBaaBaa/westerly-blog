---
title: vue3项目图片压缩脚本工具
typora-root-url: tinifyTool
date: 2024-01-05 10:34:58
tags:
---

# 写在前面
图片加载时间是首屏体验中很重要的部分，谁会想访问一个一直在处加载状态的网站呢？所以需要图片尽可能的小来减少加载时间。

一方面，我们日常使用的图片一般是jpg、png格式，我们可以采用一些压缩更好图片格式（例如svg、webp、avif、apng，还有未来可期的heif、jpeg-xl）；

另一方面就是直接压缩图片了，[tinypng](https://tinypng.com/) 是一个在线压缩图片的网站，支持手动拖拽。但是如果压缩的图片多拖来拖去就很麻烦。

所以写了个脚本工具解放双手，将在vue3项目中使用。

# 准备
1. vue3项目
2. node 16+环境 （或者nvm）
3. 一个有图片的文件夹

# tinyPng API
首先获取tinypng的api key -> [点击访问tinypng](https://tinypng.com/)
![](./17044356164108.jpg)

输入邮箱，会收到一封带链接的邮件，像下面这样。点击`Log in with magic link`按钮。
![](./17044359222573.jpg)
进入开发控制台
![](./17044362771852.jpg)
激活api key，并复制这个api key（就是我打码的这个）。
![](./17044362356415.jpg)

> 这里也可以看到，tinyPng提供了每月500张的免费压缩，超过就要收费了。


# 安装依赖
```shell
npm i tinify -D // tinypng官方的包 https://tinypng.com/developers/reference/nodejs
npm i ora -D    // 命令行loading（为了好看）
npm i picocolors -D // 命令行带颜色的文字（为了好看）
```
全部包都装到devDependencies了，因为咱们只会在开发环境去压缩图片，并不想徒增打包后的体积。

# 新建tinify.mjs文件
src下新建目录tools，新建一个名为tinify.mjs的文件，是mjs哦，不是js。
> mjs后缀文件是使用es module规范的js文件，而普通的js后缀使用的是commonjs module规范。

tinify.mjs:
```javascript
import tinify from 'tinify';
import * as fs from 'fs/promises';
import pc from "picocolors";
import ora from 'ora';   // 使用了Intl.Segmenter，需要node 16+
const spinner = ora('Loading');

tinify.key = ‘【你的api key】’
const IMAGE_SOURCE_ROOT = 'src/assets/romimg/activity/2023/christmas';

const minify = async () => {
    // 1.读取当前目录下文件
    spinner.start(pc.cyan('Reading images...'));
    let files = await fs.readdir(IMAGE_SOURCE_ROOT);

    // 2.压缩其中的图片
    spinner.text = pc.cyan('Compressing images...');
    files = files.filter(fileName => /\.(jpg|jpeg|png)$/gi.test(fileName));
    const compressedImages = await Promise.all(files.map(async (fileName) => {
        const filePath = `${IMAGE_SOURCE_ROOT}/${fileName}`;
        await tinify.fromFile(filePath).toFile(filePath);
        return fileName;
    }))

    spinner.info('The following images are compressed:');
    const tabSpace = '    ';
    console.log(tabSpace + pc.blue(compressedImages.join(`\n${tabSpace}`)));
    spinner.succeed(pc.green('Minify finished!'));
    spinner.stop();
}

minify().then();

```
注意配置`tinify.key` 和 `IMAGE_SOURCE_ROOT`，`tinify.key`是上一步获取的api key，`IMAGE_SOURCE_ROOT`是需要压缩的图片目录，比如我的是 'src/assets/romimg/activity/2023/christmas'。

# 配置package.json
在`script`中新增一条指令。
```
"tiny": "node src/tools/tinify.mjs",
```
![](./17044380088332.jpg)

配置完成后，可以在`package.json`同级目录下执行 
```shell
npm run tinify
```
执行结果：
![](./17044389253668.jpg)
ok，有loading，有颜色的字看着也挺美观。

# 使用.local中的环境变量
前面说了，每个接口每月只能压缩500张图片。公司愿意为其付费当然是极好的，大家共用一个key。但如果公司不愿意呢？每个开发者使用自己的api key，这个时候就可以用到`.local`后缀的环境变量文件了。

## vite中的环境变量
vite本身支持不同环境（开发环境、生产环境）的环境变量，通过指令中的--mode指定是哪个环境，vite还使用了`dotenv`将对应的环境变量文件中的变量合并到了import.meta.env对象中，供客户端访问。贴一张vite中文官网的截图。[点击访问原链接](https://vitejs.cn/vite3-cn/guide/env-and-mode.html#env-files)。
![](./17044416300532.jpg)
除此之外，还推荐在git中忽略掉`.local`后缀的环境变量文件，可以让每个人维护各自在`.env.*.local`中各自的环境变量。
![](./17044397074751.jpg)

## 定义.local中的变量
以此基础上，我们直接把`api key`和`IMAGE_SOURCE_ROOT`放到`.env.*.local`文件里。
.env.development.local：

```
VITE_TINIFY_API_KEY=【你的api key】
VITE_TINIFY_DIR=src/assets/romimg/activity/2023/christmas
```
> 因为我的dev指令是"vite --mode development.local",所以我的变量加在`.env.development.local`文件中。
> 如果你的指令中的--mode是development，那就把它改成development.local并新建'.env.development.local'文件。
> （当然环境的名字是自定义的，我的叫development环境，兴许你的叫dev环境，不要太死板）

## 使用dotenv
前面也说到，vite也是用了dotenv。那我们也来装个`dotenv`。dotenv 是用来获取换取环境变量的，将环境变量的值直接合并到`process.env`对象中。
```shell
npm i dotenv -D
```

tinify.mjs中：
```javascript
...

// 修改前
tinify.key = ‘【你的api key】’
const IMAGE_SOURCE_ROOT = 'src/assets/romimg/activity/2023/christmas';

// 修改后：
import 'dotenv/config';
tinify.key = process.env.VITE_TINIFY_API_KEY || '【你的api key】';
const IMAGE_SOURCE_ROOT = process.env.VITE_TINIFY_DIR || 'src/assets/romimg/activity/2023/christmas';

...

```

## 修改package.json
然后在指令中，指定使用哪个环境变量文件。
package.json：
```json
...

// 修改前
"tiny": "node src/tools/tinify.mjs",

// 修改后
"tiny": "node src/tools/tinify.mjs dotenv_config_path=.env.development.local",

...

```

# 写在最后
这样就完成了！最后贴一下完整的`tinify.mjs`
```javascript
import tinify from 'tinify';
import * as fs from 'fs/promises';
import pc from "picocolors";
import ora from 'ora';   // 使用了Intl.Segmenter，需要node 16+
const spinner = ora('Loading');

import 'dotenv/config';
tinify.key = process.env.VITE_TINIFY_API_KEY || '【你的api key】';
const IMAGE_SOURCE_ROOT = process.env.VITE_TINIFY_DIR || 'src/assets/romimg/activity/2023/christmas';

const minify = async () => {
    // 1.读取当前目录下文件
    spinner.start(pc.cyan('Reading images...'));
    let files = await fs.readdir(IMAGE_SOURCE_ROOT);

    // 2.压缩其中的图片
    spinner.text = pc.cyan('Compressing images...');
    files = files.filter(fileName => /\.(jpg|jpeg|png)$/gi.test(fileName));
    const compressedImages = await Promise.all(files.map(async (fileName) => {
        const filePath = `${IMAGE_SOURCE_ROOT}/${fileName}`;
        await tinify.fromFile(filePath).toFile(filePath);
        return fileName;
    }))

    spinner.info('The following images are compressed:');
    const tabSpace = '    ';
    console.log(tabSpace + pc.blue(compressedImages.join(`\n${tabSpace}`)));
    spinner.succeed(pc.green('Minify finished!'));
    spinner.stop();
}

minify().then();
```
