---

copyright:
  years: 2016

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Liberty 的離線模式
{: #offline_mode}
*前次更新：2016 年 7 月 20 日*
{: .last-updated}

將 Liberty 應用程式推送至 {{site.data.keyword.Bluemix}} 後，Liberty 建置套件即可存取 Bluemix 的外部網站，以取得應用程式所需的構件。下面是 Liberty 建置套件可以存取的外部網站。在 [Bluemix 專用](../../dedicated/index.html#dedicated)和 [Bluemix 本端](../../local/index.html#local)環境中，需要將這些網站設定為*白名單*。

* https://download.run.pivotal.io, and https://java-buildpack.cloudfoundry.org 可用來存取下列項目的元件：
  * [AppDynamics 代理程式](https://www.appdynamics.com/)
  * [MariaDB JDBC 驅動程式](https://mariadb.com/)
  * [New Relic 代理程式](newRelic.html)
  * [OpenJDK](customizingJRE.html#OpenJDK)
  * [PostgreSQL JDBC 驅動程式](https://www.postgresql.org)
* https://dl.zeroturnaround.com/jrebel/ 可用來存取 [JRebel](https://zeroturnaround.com/software/jrebel/) 的元件。
* https://download.ruxit.com/agent/paas/cloudfoundry/java 可用來存取 [Dynatrace Ruxit 代理程式](dynatrace.html)的元件。
* http://downloads.dynatracesaas.com/cloudfoundry/buildpack/java/ 可用來存取 [Dynatrace 代理程式](dynatrace.html)。

## 使用 Proxy
{: #working_with_proxy}

在像 [Bluemix 專用](../../dedicated/index.html#dedicated)和 [Bluemix 本端](../../local/index.html#local)這些環境中，可以配置 Proxy。如需詳細資訊，請參閱[使用 Proxy](../../manageapps/workingWithProxy.html)。

# 相關鏈結
{: #rellinks}
## 一般
{: #general}
* [Liberty 運行環境](index.html)
* [Liberty 設定檔概觀](http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.wlp.nd.doc/ae/cwlp_about.html)
