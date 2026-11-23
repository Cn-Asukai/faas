## OpenFaaS &reg; - 让 Serverless 函数变得简单

[![Build Status](https://github.com/openfaas/faas/actions/workflows/build.yml/badge.svg)](https://github.com/openfaas/faas/actions/workflows/build.yml)
[![go.dev reference](https://img.shields.io/badge/go.dev-reference-007d9c?logo=go&logoColor=white&style=flat-square)](https://pkg.go.dev/github.com/openfaas/faas)
[![OpenFaaS](https://img.shields.io/badge/openfaas-serverless-blue.svg)](https://www.openfaas.com)

![OpenFaaS Logo](https://blog.alexellis.io/content/images/2017/08/faas_side.png)

OpenFaaS&reg; 让开发者无需重复编写样板代码，即可将事件驱动（event-driven）的函数和微服务部署到 Kubernetes。将你的代码或现有二进制文件打包成 OCI 兼容镜像，即可获得一个具备自动扩缩容和指标监控的高可扩展端点。

> “OpenFaaS 社区版（Community Edition，CE）仅授权用于个人探索和爱好者的非商业用途。商业 PoC 有 60 天的期限限制。商业及生产环境工作负载需要 OpenFaaS Standard 或 OpenFaaS for Enterprises。”

[![Follow @openfaas on X](https://img.shields.io/twitter/url/https/twitter.com/fold_left.svg?style=social&label=Follow%20%40openfaas)](https://x.com/openfaas)

**亮点**

* 通过 UI 门户和*一键式*安装，简单易用
* 使用[模板商店（Template Store）](https://www.openfaas.com/blog/template-store/)或 Dockerfile，可以用任意语言编写服务和函数
* 将你的代码构建并发布为 OCI 兼容的 Docker 镜像
* 可移植：借助 [Kubernetes](https://github.com/openfaas/faas-netes)，可运行在现有硬件或公有/私有云上
* 提供 [CLI](http://github.com/openfaas/faas-cli)，支持 YAML 格式来模板化和定义函数
* 随需求增长[自动扩缩容（包括缩容到零）](https://docs.openfaas.com/architecture/autoscaling/)
* [由 OpenFaaS 背后团队提供的商业支持 Pro 发行版](https://openfaas.com/pricing/)

**想更深入地了解 OpenFaaS？**

* 使用 [HTTP 或 Apache Kafka、AWS SQS 等事件源触发端点](https://docs.openfaas.com/reference/triggers/)
* 将任务卸载到内置的[队列与后台处理](https://docs.openfaas.com/reference/async/)
* 使用 [OpenFaaS Cloud 的 GitOps](https://docs.openfaas.com/openfaas-cloud/intro/) 快速开启你的 Kubernetes 之旅
* 安全至上：[必知的 5 条安全建议](https://www.openfaas.com/blog/five-security-tips/)
* 了解[走向生产环境](https://docs.openfaas.com/architecture/production/)所需的一切
* 通过[精选教程](https://docs.openfaas.com/tutorials/featured/#service-mesh)与 Istio 或 Linkerd 集成
* 部署到 [Kubernetes 或 OpenShift](https://docs.openfaas.com/deployment/)

## OpenFaaS 版本与定价

本仓库是 OpenFaaS 社区版（Community Edition，CE）的一部分，其授权仅限个人非商业用途，以及有时限的商业概念验证（PoC）试用。公司或企业内部使用需要获得许可。

OpenFaaS CE：

* 有使用限制，详情请阅读 [OpenFaaS CE EULA](EULA.md)
* 相比商业版本，仅具备基础或原始的功能与特性
* 除初始试用期内以外，不允许任何形式的商业使用

OpenFaaS Standard 和 OpenFaaS for Enterprises 是完整且独立的商业产品。

它们由全职团队独立维护和开发，提供商业支持，并针对 CVE 漏洞、Kubernetes 及云原生生态系统的更新保持积极维护。

在 [https://www.openfaas.com/pricing/](https://www.openfaas.com/pricing/) 了解更多版本详情

## OpenFaaS 概览（让 Serverless 函数变得简单）

![概念架构](/docs/of-layer-overview.png)

> 概念架构与技术栈，[文档中有更多细节](https://docs.openfaas.com/architecture/stack/)

### 代码示例

你可以使用 `faas-cli new` 命令来创建一个新函数，传入函数名称和你想使用的语言模板即可，例如：`faas-cli new --lang node20 stripe-webhooks`。

官方模板支持多种主流语言，并且通过 Dockerfile 可以轻松扩展。

在[文档中了解 OpenFaaS 模板](https://docs.openfaas.com/languages/overview/)

* Node.js（`node20`）示例：

    ```js
   "use strict"

    module.exports = async (event, context) => {
        return context
            .status(200)
            .headers({"Content-Type": "text/html"})
            .succeed(`
            <h1>
                👋 Hello World 🌍
            </h1>`);
    }

    ```
    *handler.js*

* Python 3 示例（`python3-http`）：

    ```python
    def handle(event, context):
        return {
            "statusCode": 200,
            "body": "Hello from OpenFaaS!"
        }
    ```

    *handler.py*

* Golang 示例（`golang-middleware`）：

    ```go
    package function
    
    import (
     	"fmt"
     	"io"
     	"net/http"
    )
    
    func Handle(w http.ResponseWriter, r *http.Request) {
   	    var input []byte
        
       	if r.Body != nil {
        		defer r.Body.Close()
        		body, _ := io.ReadAll(r.Body)
        		input = body
       	}
        
       	w.WriteHeader(http.StatusOK)
       	w.Write([]byte(fmt.Sprintf("Body: %s", string(input))))
    }
    ```

## 开始使用 OpenFaaS

### 官方培训资源

查看我们的[官方培训资料](https://docs.openfaas.com/tutorials/training)

### 官方电子书与视频课程
[![eBook logo](/docs/ebook-cover.png)](https://gumroad.com/l/serverless-for-everyone-else)

OpenFaaS 创始人撰写了 *Serverless For Everyone Else* 一书，通过使用 JavaScript 和 Node.js 的实战练习，帮助开发者理解函数的使用场景。尝试这些练习不需要任何编程经验。

书中的示例使用 faasd 项目——一种轻量且易用的方式，适合开始学习 OpenFaaS 和函数。

[在 Gumroad 上查看 Serverless For Everyone Else](https://gumroad.com/l/serverless-for-everyone-else)

### OpenFaaS 与 Golang

*Everyday Go* 是一本实用的上手指南，教你用 Go 编写 CLI、网页和微服务，其中还包含专门的一章，讲解如何使用 OpenFaaS 和 Go 进行函数的开发与测试。

* [Everyday Golang](https://openfaas.gumroad.com/l/everyday-golang)

### 社区博客与文档

* 阅读文档：[docs.openfaas.com](https://docs.openfaas.com/deployment)
* 在[官方博客](https://www.openfaas.com/blog/)上阅读最新资讯和教程

### 快速开始

![OpenFaaS 社区版 UI](/docs/inception.png)

> 这是 OpenFaaS 社区版 UI 的截图，其设计注重易用性。图中正在运行商店（store）中提供的 inception 函数。

立即参考[部署指南](https://docs.openfaas.com/deployment/)，将 OpenFaaS 部署到 Kubernetes、OpenShift 或 faasd

OpenFaaS Standard 和 OpenFaaS for Enterprises 拥有全新且支持多租户的专属仪表盘，在 [OpenFaaS Dashboard](https://docs.openfaas.com/openfaas-pro/dashboard/) 了解更多。

### 视频演讲

* [Meet faasd. Look Ma’ No Kubernetes! 2020](https://www.youtube.com/watch?v=ZnZJXI377ak&feature=youtu.be)
* [Getting Beyond FaaS: The PLONK Stack for Kubernetes Developers 2019](https://www.youtube.com/watch?v=NckMekZXRt8&feature=emb_title)
* [Serverless Beyond the Hype - Alex Ellis - GOTO 2018](https://www.youtube.com/watch?v=yOpYYYRuDQ0)
* [How LivePerson is Tailoring its Conversational Platform Using OpenFaaS - Simon Pelczer 2019](https://www.youtube.com/watch?v=bt06Z28uzPA)
* [Digital Transformation of Vision Banco Paraguay with Serverless Functions @ KubeCon 2018](https://kccna18.sched.com/event/GraO/digital-transformation-of-vision-banco-paraguay-with-serverless-functions-alex-ellis-vmware-patricio-diaz-vision-banco-saeca)
* [Introducing "faas" - Cool Hacks Keynote at Dockercon 2017](https://blog.docker.com/2017/04/dockercon-2017-mobys-cool-hack-sessions/)

### 社区活动与博客文章

写过关于 OpenFaaS 的博客？有演讲活动？欢迎向以下社区页面提交 Pull Request。

* [阅读关于 OpenFaaS 的博客/文章并查找相关活动](https://github.com/openfaas/faas/blob/master/community.md)

### 贡献

OpenFaaS 社区版使用 Golang 编写。所有对源代码的第三方贡献均基于 MIT 许可证授权；而 OpenFaaS CE 整体适用额外限制，其中来自 OpenFaaS Ltd 的贡献依据 [OpenFaaS CE EULA](EULA.md) 授权。我们欢迎各种形式的贡献——无论是提供反馈、测试现有或新特性，还是参与源代码开发。

#### 如何成为贡献者？

请参阅[社区与贡献指南](https://docs.openfaas.com/community/)

#### 仪表盘

以下是一个关联 OpenFaaS 的 Grafana 仪表盘示例，实时展示自动扩缩容的运行情况：[点此查看](https://grafana.com/dashboards/3526)

![OpenFaaS Pro 自动扩缩容 Grafana 仪表盘](https://pbs.twimg.com/media/FJ9EBVdWQAM9DeW?format=jpg&name=medium)
> 使用 Grafana 的 [OpenFaaS Pro 自动扩缩容](https://docs.openfaas.com/architecture/autoscaling/)仪表盘

另一个社区仪表盘可在[这里找到](https://grafana.com/dashboards/3434)

### 媒体 / 品牌 / 网站赞助

* 商业及内部使用的 OpenFaaS

    OpenFaaS Pro（Standard 和 For Enterprises）功能完整、有官方支持、面向生产环境打造。[社区版（CE）](EULA.md)授权用于 60 天的概念验证（PoC）和有限的体验。

    [了解更多 OpenFaaS 版本](https://openfaas.com/pricing/)

* 网站赞助 🌎

  如果你想通过在 [openfaas.com](https://www.openfaas.com/) 首页展示你的 Logo 来获得曝光，欢迎通过邮件联系我们，或浏览 [GitHub Sponsors](https://github.com/sponsors/openfaas) 上的赞助档位。

* 媒体 / 分析师

  想通过这些仓库的提交数和活跃度来评估？所有公开仓库都属于 OpenFaaS CE——OpenFaaS 的一个功能受限版本，旨在让人们无需注册信用卡即可低门槛试用。OpenFaaS CE 尽力而为地维护，但它并不代表 OpenFaaS 本身。所有 OpenFaaS 产品的开发都在私有仓库中进行，第三方无法追踪，也无法通过简单浏览 GitHub 来观察。

  GitHub Stars 和 Forks 是怎么统计的？OpenFaaS CE 并不是单体仓库（mono-repo），你不能只看某一个仓库就说“啊，这就是总数”——统计数据是从整个 [GitHub 组织](https://github.com/openfaas)汇总而来的。

### 治理

OpenFaaS &reg; 是由 [Alex Ellis](https://www.alexellis.io) 创建的独立开源项目，正由一个[不断壮大的贡献者社区](https://www.openfaas.com/team/)共同建设与塑造。

OpenFaaS 由 OpenFaaS Ltd（注册号：11076587）托管，该公司同时提供商业服务、首页赞助和支持。OpenFaaS &reg; 是英格兰和威尔士的注册商标。

### 用户

查看部分已授权在 [openfaas.com](https://www.openfaas.com/) 上展示 Logo 的终端用户公司。
