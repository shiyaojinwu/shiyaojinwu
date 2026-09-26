# AI-Story：把故事做成分镜与视频

[返回主页作品](../README.md#-作品存档)

AI-Story 是我们在**字节跳动训练营**中做的故事创作应用。用户输入故事后，可以生成和编辑分镜，查看图像、视频等素材，完成预览与导出。

我担任组长，负责需求梳理、任务分工、进度协调与 Android 端开发，主要搭建客户端的基础架构和数据层。Android 页面由大家分工完成，**Go 后端和模型部署由队友负责**，我们一起推进接口与页面流程的联调。

客户端使用 **Kotlin、Jetpack Compose 与 MVVM**，通过 Retrofit / OkHttp 访问接口、Room 保存本地数据，再用 Flow / StateFlow 将变化传给界面。

<details>
<summary>三周的开发过程</summary>

第一周先整理需求与分工，再搭建依赖、导航、主题、通用组件和本地存储。共用结构确定后，大家可以分别推进页面，减少每个页面各写一套基础逻辑的情况。

第二周主要整理网络客户端、请求与响应模型、Repository，以及统一的异常和界面状态。这个阶段还没有完成后端接口对接，先把客户端内部的数据流组织好。

第三周集中联调，处理生成状态轮询、页面跳转、参数编码、预览与下载。生成任务需要等待，页面要把等待、完成和失败都表达清楚，整个流程才方便使用。

</details>

<details>
<summary>数据层与长任务的处理</summary>

故事数据同时涉及接口和本地数据库。我把两类访问放进 Repository，页面通过 ViewModel 获取数据。[StoryRepository](https://github.com/shiyaojinwu/AI-Story/blob/main/frontend/app/src/main/java/com/shiyao/ai_story/model/repository/StoryRepository.kt)组织本地读写及分镜生成、视频生成与预览接口，本地列表以 Flow 提供给上层。

网络异常、业务失败和空响应都需要走到界面上。数据层统一处理请求结果，ViewModel 再用 UIState 表达初始、加载、成功和失败状态，页面据此更新，也方便联调时判断问题发生在哪一层。

[StoryViewModel](https://github.com/shiyaojinwu/AI-Story/blob/main/frontend/app/src/main/java/com/shiyao/ai_story/viewmodel/StoryViewModel.kt)发起视频生成后查询预览状态，轮询有次数上限，超时后给出提示；再次生成时先取消上一轮。当前使用固定间隔，重试和任务恢复仍有完善空间。

这段开发让我更关注接口调用之后的体验。作为组长，我也需要把这些跨端问题拆成明确任务，让后端、模型服务与客户端的工作接起来。

</details>

[GitHub 源码 ↗](https://github.com/shiyaojinwu/AI-Story)
