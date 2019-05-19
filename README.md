# repo_sample

[repo](https://github.com/java-module/repo) 项目示例

## 如何使用

在`setting.gradle` 添加安装脚本
```groovy
apply from: "https://raw.githubusercontent.com/java-module/repo/master/install.groovy"
//apply from: "repo/install.groovy"
apply from: 'plugin/RepoModulePlugin.groovy'.repoDownload()

repoConfig {
    scriptDownload = false //是否下载仓库定义脚本
    modules = [ // 要导入的GitHub仓库
            // 格式: /{git仓库}/{用户}/{项目名}/{分支/tag}/({子工程}.groovy)
            // 导入 https://github.com/wittyneko/gradle-tools
            'github/wittyneko/gradle-tools/master',
            // 导入 https://github.com/wittyneko/ktx-base 下的 base 模块 
            'github/wittyneko/ktx-base/master/base.groovy',
    ]
}
repoConfig.execute(settings)
```

`apply from: "https://raw.githubusercontent.com/java-module/repo/master/install.groovy"` 
会导入 GitHub `java-module/repo` 仓库 `master` 分支根目录的 `install.groovy` 文件到 `setting.gradle`。
这个脚本会下载三个脚本到 `{项目根目录(rootDir)}/repo` 下。三个文件分别是 
1. `install.groovy` 也就是自身，方便之后使用`apply from: "repo/install.groovy"` 减少等待网络请求时间
2. `meta/io.groovy` 实现了 `下载`、`压缩`、`解压`、`复制` 功能
3. `meta/modules.groovy`  实现了仓库地址、缓存目录解析通用方法

apply from: 'plugin/RepoModulePlugin.groovy'.repoDownload()，
`repoDownload()` 方法会下载 `java-module/repo` 对用的文件并返回下载地址。
这里下载 `https://github.com/java-module/repo/raw/master/plugin/RepoModulePlugin.groovy` 并返回下载文件给 `setting.gradle` 导入这个脚本。

 `RepoModulePlugin.groovy` 创建 `repoConfig` 配置，最后执行`repoConfig.execute(settings)` 开始下载依赖源码并导入项目。
 
 最终会自动下载 `https://github.com/wittyneko/gradle-tools` 仓库zip文件并解压到 `modules` 目录下对应目录, 导入工程重命名为 `github.wittyneko.gradle-tools`。
 同样会下载 `https://github.com/wittyneko/ktx-base` 但只导入 `base`，命名也会加速模块名为 `github.wittyneko.ktx-base.base`
  
 