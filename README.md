# xiuxiu

秀米编辑器浏览器插件
目前支持谷歌浏览器
TODO：行内有斜体的需要保持斜体

------

本项目使用  [Plasmo](https://plasmo.com/) 构建

## 开始

按下列步骤

### 依赖安装

```bash
pnpm install
# or
yarn install
# or
npm install
```

推荐使用pnpm

pnpm install 安装sharp失败时，需要设置sharp的代理

```bash
pnpm config set sharp_binary_host "https://npmmirror.com/mirrors/sharp"
pnpm config set sharp_libvips_binary_host "https://npmmirror.com/mirrors/sharp-libvips"
```

### 运行 dev 服务

依赖安装完成后，运行开发服务

```bash
pnpm dev
```

此时会在根目录发现 build 文件夹，内有chrome-mv3-dev文件，即为谷歌浏览器插件包
谷歌浏览器插件打开开发者模式选项然后“加载已解压的扩展程序”

### 为特定浏览器构建 dev 包

举例为Firefox构建

```bash
plasmo dev --target=firefox-mv2
```

最终的捆绑包将在目录build/firefox-mv2-dev中，其他支持的浏览器可以访问 [链接](https://docs.plasmo.com/framework/workflows/faq#what-are-the-officially-supported-browser-targets)

## 生成可执行的 zip 包

```bash
pnpm build --zip
```

(build 目录下压缩包为生成后插件文件)

## Submit to the webstores

The easiest way to deploy your Plasmo extension is to use the built-in [bpp](https://bpp.browser.market) GitHub action. Prior to using this action however, make sure to build your extension and upload the first version to the store to establish the basic credentials. Then, simply follow [this setup instruction](https://docs.plasmo.com/framework/workflows/submit) and you should be on your way for automated submission!
