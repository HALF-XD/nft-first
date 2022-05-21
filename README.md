- [My first NFT on testnet](https://testnets.opensea.io/assets/rinkeby/0x72fca63bfa4a81de64c62902e7b5246afbbec8c0/0)
- [参考视频](https://www.bilibili.com/video/BV1S341177sQ)
- 用到的网站：
  - https://app.pinata.cloud
  - https://testnets.opensea.io
  - https://eth-converter.com/

#### 教程中给到的需求

Contracts Part
1、建立 ETH 互动的智能合約
2、NFT 要能够被 mint
3、NFT 要能够设置总量
4、NFT 要能够设定每個地址最大持有量
5、NFT 要能够限制单次的 mint 量
6、NFT 要能够设定开关去公开发售

拼图的部分
1、如何快速制作多种拼图以及 meta 资料
2、如何上传到 IPFS 星际网络系統

#### 我的实际操作

合约部分：

- UP 的 github 地址：https://github.com/niclin/nic_meta
  - 在 remix 里编译部署，部署时候 constructor 的参数可不填写
  - 部署成功后，调用 `mintNTF`方法
  - `flipSaleActive` 切换状态，是否可交易。`_isSaleActive`可得。
  - ETH 单位转换 `https://eth-converter.com/`
  - 可在 `https://testnets.opensea.io/` 查看名下 nft.

拼图部分：

- github https://github.com/HashLips/hashlips_art_engine
  文件 src > main.js > startCreating 函数的
  `let i = network == NETWORK.sol ? 0 : 1;` 修改为 `let i = 0;`
- 在 hashlips_art_engine 目录下

  - 下载依赖包 `yarn install`
  - `src > config.js > growEditionSizeTo` 设置为 `10`（或任意想要生成图片的数量）
  - `yarn run build` 会创建 `build` 文件夹，里面有 `images` 和 `json` 文件

- 存放图片云端 `https://app.pinata.cloud/pinmanager`
  - 在网站上上传 build 中的 images 文件夹，获得的 CID 粘贴在
    `src > config.js > baseUri` 里为 `ipfs://CID`
  - 执行`yarn run update_info`指令,更新 metadata 资料, `build > json`中的每个文件 image 都更新了
  - 在网站上上传 build 中的 json 文件夹
  - 新增一个 `unpack.png` 作为盲盒图片, 同样上传网站后获取 `CID` 放进新建的`unpack.json`文件中, 上传 json 文件
  - remix 中 `setNotRevealedURI` 参数填入 上传 unpack.json 文件后生成的 CID `ipfs://CID`
  - remix 中 `setBaseURI` 参数填入 上传 json 文件夹生成的 CID `ipfs://CID/`
    tips:记得加最后的斜杠，路由问题
  - `https://testnets.opensea.io` 此时网站刷新可看到盲盒的图片了
  - remix 中 `flipReveal` 对盲盒的功能关闭，opensea 里刷新可以解密刚才 mintNTF 的 nft 的样子了。
