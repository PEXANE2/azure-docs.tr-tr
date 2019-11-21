---
title: What are Microsoft Flow, Logic Apps, Functions, and WebJobs? - Azure
description: 'Compare Microsoft cloud services that are optimized for integration tasks: Microsoft Flow, Logic Apps, Functions, and WebJobs.'
ms.topic: overview
ms.date: 04/09/2018
ms.custom: mvc
ms.openlocfilehash: 6cf820af8a5b97de536a9d07e49ec822ffb76681
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230882"
---
# <a name="what-are-microsoft-flow-logic-apps-functions-and-webjobs"></a>What are Microsoft Flow, Logic Apps, Functions, and WebJobs?

This article compares the following Microsoft cloud 
* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Azure App Service Web İşleri](../app-service/webjobs-create.md)

Tüm bu hizmetler, tümleştirme sorunlarını çözebilir ve iş süreçlerini otomatikleştirebilir. Tümü giriş, eylemler, koşullar ve çıkış tanımı yapabilir. Her birini belirli bir zamanlamayla veya tetikleyiciyle çalıştırabilirsiniz. Each service has unique advantages, and this article explains the differences. 

If you're looking for a more general comparison between Azure Functions and other Azure compute options, see [Criteria for choosing an Azure compute service](/azure/architecture/guide/technology-choices/compute-comparison) and [Choosing an Azure compute option for microservices](/azure/architecture/microservices/design/compute-options).

## <a name="compare-microsoft-flow-and-azure-logic-apps"></a>Microsoft Flow ve Azure Logic Apps karşılaştırması

Microsoft Flow and Logic Apps are both *designer-first* integration services that can create workflows. Her iki hizmet de çeşitli SaaS uygulamaları ve kurumsal uygulamalarla tümleştirilir. 

Microsoft Flow is built on top of Logic Apps. They share the same workflow designer and the same [connectors](../connectors/apis-list.md). 

Microsoft Flow empowers any office worker to perform simple integrations (for example, an approval process on a SharePoint Document Library) without going through developers or IT. Logic Apps can also enable advanced integrations (for example, B2B processes) where enterprise-level Azure DevOps and security practices are required. Kurumsal iş akışının zamanla karmaşık hale gelmesi tipik bir durumdur. Accordingly, you can start with a flow at first, and then convert it to a logic app as needed.

The following table helps you determine whether Microsoft Flow or Logic Apps is best for a particular integration:

|  | Microsoft Flow | Logic Apps |
| --- | --- | --- |
| Kullanıcılar |Ofis çalışanları, iş kullanıcıları veya SharePoint yöneticileri |Uzman tümleştiriciler ve geliştiriciler, BT uzmanları |
| Senaryolar |Self servis |Gelişmiş tümleştirmeler |
| Design tool |Tarayıcı içi ve mobil uygulama, yalnızca kullanıcı arabirimi |Tarayıcı içi ve [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Cod görünümü](../logic-apps/logic-apps-author-definitions.md) sağlanır |
| Application lifecycle management (ALM) |Design and test in non-production environments, promote to production when ready |Azure DevOps: source control, testing, support, automation, and manageability in [Azure Resource Manager](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Admin experience |Manage Microsoft Flow environments and data loss prevention (DLP) policies, track licensing: [Microsoft Flow Admin Center](https://admin.flow.microsoft.com) |Manage resource groups, connections, access management, and logging: [Azure portal](https://portal.azure.com) |
| Güvenlik |Office 365 Security and Compliance audit logs, DLP, [encryption at rest](https://wikipedia.org/wiki/Data_at_rest#Encryption) for sensitive data |Security assurance of Azure: [Azure security](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity), [Azure Security Center](https://azure.microsoft.com/services/security-center/), [audit logs](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) |

## <a name="compare-azure-functions-and-azure-logic-apps"></a>Azure İşlevleri ve Azure Logic Apps karşılaştırması

İşlevler ve Logic Apps, sunucusuz iş yüklerine olanak tanıyan Azure hizmetleridir. Azure Functions is a serverless compute service, whereas Azure Logic Apps provides serverless workflows. Both can create complex *orchestrations*. Düzenleme, Logic Apps’te karmaşık bir görevin gerçekleştirilmesi için yürütülen, *eylemler* olarak adlandırılan işlevlerin veya adımların bir koleksiyonudur. For example, to process a batch of orders, you might execute many instances of a function in parallel, wait for all instances to finish, and then execute a function that computes a result on the aggregate.

Azure İşlevleri için düzenlemeleri kod yazarak ve [Dayanıklı İşlevler uzantısını](durable/durable-functions-overview.md) kullanarak geliştirirsiniz. Logic Apps için düzenlemeleri, GUI kullanarak veya yapılandırma dosyalarını düzenleyerek oluşturursunuz.

Düzenleme oluşturduğunuzda, mantıksal uygulamalardan işlev çağırdığınızda ve işlevlerden mantıksal uygulama çağırdığınızda hizmetleri karıştırıp eşleştirebilirsiniz. Hizmet özelliklerine veya kişisel tercihinize göre her düzenlemenin nasıl oluşturulacağını seçin. The following table lists some of the key differences between these  
|  | Dayanıklı İşlevler | Logic Apps |
| --- | --- | --- |
| Geliştirme | Koda öncelik veren (kesinlik temelli) | Tasarımcıya öncelik veren (bildirim temelli) |
| Bağlantı | [Yaklaşık bir düzine bağlama türü](functions-triggers-bindings.md#supported-bindings), özel bağlamalar için kod yazma | [Bağlayıcılardan oluşan büyük koleksiyon](../connectors/apis-list.md), [B2B senaryoları için Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md), [özel bağlayıcı oluşturma](../logic-apps/custom-connector-overview.md) |
| Eylemler | Her etkinlik bir Azure işlevidir; eylem işlevleri için kod yazma |[Hazır eylemlerden oluşan büyük koleksiyon](../logic-apps/logic-apps-workflow-actions-triggers.md)|
| İzleme | [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) | [Azure portal](../logic-apps/quickstart-create-first-logic-app-workflow.md), [Azure Monitor logs](../logic-apps/logic-apps-monitor-your-logic-apps.md)|
| Yönetim | [REST API](durable/durable-functions-http-api.md), [Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer) | [Azure portalı](../logic-apps/quickstart-create-first-logic-app-workflow.md), [REST API](https://docs.microsoft.com/rest/api/logic/), [PowerShell](https://docs.microsoft.com/powershell/module/az.logicapp), [Visual Studio](https://docs.microsoft.com/azure/logic-apps/manage-logic-apps-with-visual-studio) |
| Yürütme bağlamı | Can run [locally](functions-runtime-overview.md) or in the cloud | Runs only in the cloud|

<a name="function"></a>

## <a name="compare-functions-and-webjobs"></a>İşlevler Web İşleri karşılaştırması

Azure İşlevleri gibi, WebJobs SDK ile Azure App Service WebJobs da geliştiriciler için tasarlanmış, *koda öncelik veren* bir tümleştirme hizmetidir. Her ikisi de [Azure App Service](../app-service/overview.md) üzerinde derlenmiş olup [source control integration](../app-service/deploy-continuous-deployment.md), [authentication](../app-service/overview-authentication-authorization.md) ve [monitoring with Application Insights integration](functions-monitoring.md) gibi özellikleri destekler.

### <a name="webjobs-and-the-webjobs-sdk"></a>Web İşleri ve Web İşleri SDK’sı

You can use the *WebJobs* feature of App Service to run a script or code in the context of an App Service web app. *WebJobs SDK*, Azure hizmetlerine yanıt olarak yazdığınız kodu kolaylaştıran WebJobs için tasarlanmış bir çerçevedir. For example, you might respond to the creation of an image blob in Azure Storage by creating a thumbnail image. WebJobs SDK, WebJob’a dağıtabileceğiniz bir .NET konsol uygulaması olarak çalıştırılır. 

WebJobs ve WebJobs SDK birlikte en iyi şekilde çalışır; ancak WebJobs’ı WebJobs SDK olmadan kullanabilirsiniz; bunun tersi de olabilir. Bir Web İşi, App Service korumalı alanında çalışan herhangi bir programı veya betiği çalıştırabilir. Web İşleri SDK konsolu uygulaması, şirket içi sunucular gibi konsol uygulamalarının çalıştığı her yerde çalışabilir.

### <a name="comparison-table"></a>Karşılaştırma tablosu

Azure İşlevleri, WebJobs SDK’da derlendiğinden diğer Azure hizmetlerine yönelik aynı bağlantıların ve olay tetikleyicilerinin birçoğunu paylaşır. Here are some factors to consider when you're choosing between Azure Functions and WebJobs with the WebJobs SDK:

|  | İşlevler | WebJobs SDK ile WebJobs |
| --- | --- | --- |
|[Otomatik ölçeklendirme](functions-scale.md#how-the-consumption-and-premium-plans-work) ile [sunucusuz uygulama modeli](https://azure.microsoft.com/solutions/serverless/)|✔||
|[Tarayıcıda geliştirme ve test etme](functions-create-first-azure-function.md) |✔||
|[Kullanım başına ödeme fiyatlandırması](functions-scale.md#consumption-plan)|✔||
|[Logic Apps ile tümleştirme](functions-twitter-email.md)|✔||
| Tetikleyici olayları |[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md)<br>[Azure Event Grid](functions-bindings-event-grid.md)|[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[Dosya sistemi](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Files/FileTriggerAttribute.cs)|
| Desteklenen diller  |C#<br>F#<br>JavaScript<br>Java<br>Python |C#<sup>1</sup>|
|Paket yöneticileri|NPM ve NuGet|NuGet<sup>2</sup>|

<sup>1</sup> WebJobs (without the WebJobs SDK) supports C#, Java, JavaScript, Bash, .cmd, .bat, PowerShell, PHP, TypeScript, Python, and more. This is not a comprehensive list. WebJob, App Service korumalı alanında çalıştırılabilen herhangi bir programı veya betiği çalıştırabilir.

<sup>2</sup> WebJobs (without the WebJobs SDK) supports NPM and NuGet.

### <a name="summary"></a>Özet

Azure Functions offers more developer productivity than Azure App Service WebJobs does. It also offers more options for programming languages, development environments, Azure service integration, and pricing. Çoğu senaryo için bu en iyi seçenektir.

WebJobs’ın en iyi seçenek olduğu iki senaryo aşağıda verilmiştir:

* Olayları dinleyen kod (`JobHost` nesnesi) üzerinde daha fazla denetime ihtiyacınız vardır. İşlevler, [host.json](functions-host-json.md) dosyasında `JobHost` davranışını özelleştirmek için sınırlı sayıda yöntem sunar. Bazen bir JSON dosyasındaki dize tarafından belirtilemeyen şeyler yapmanız gerekir. Örneğin, yalnızca WebJobs SDK, Azure Depolama için özel bir yeniden deneme ilkesi yapılandırmanıza olanak sağlar.
* You have an App Service app for which you want to run code snippets, and you want to manage them together in the same Azure DevOps environment.

Azure veya üçüncü taraf hizmetleri tümleştirmek için kod parçacıklarını çalıştırmak istediğiniz diğer durumlarda, WebJobs SDK ile WebJobs üzerinden Azure İşlevleri’ni seçin.

<a name="together"></a>

## <a name="microsoft-flow-logic-apps-functions-and-webjobs-together"></a>Microsoft Flow, Logic Apps, Functions, and WebJobs together

You don't have to choose just one of these services. They integrate with each other as well as they do with external services.

Akış bir mantıksal uygulamayı çağırabilir. Mantıksal uygulama bir işlevi çağırabilir ve işlev de bir mantıksal uygulamayı çağırabilir. Örneğin, bkz. [Azure Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md).

The integration between Microsoft Flow, Logic Apps, and Functions continues to improve over time. Bir hizmette bir şey oluşturabilir ve bunu diğer hizmetlerde kullanabilirsiniz.

You can get more information on integration services by using the following links:

* [Tümleştirme senaryoları için Azure İşlevleri ve Azure App Service'ten yararlanma - Christopher Anderson](https://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Tümleştirmeler Basitleşti - Charles Lamanna](https://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps Live webcast](https://aka.ms/logicappslive)
* [Microsoft Flow frequently asked questions](/power-automate/frequently-asked-questions)

## <a name="next-steps"></a>Sonraki adımlar

İlk akışınızı, mantıksal uygulamanızı veya işlev uygulamanızı oluşturarak başlayın. Select any of the following links:

* [Microsoft Flow’u kullanmaya başlama](/power-automate/getting-started)
* [Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Create your first Azure function](functions-create-first-azure-function.md)
