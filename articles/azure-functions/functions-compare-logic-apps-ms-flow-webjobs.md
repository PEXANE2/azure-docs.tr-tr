---
title: Azure'da entegrasyon ve otomasyon platformu seçenekleri
description: 'Tümleştirme görevleri için en iyi duruma getirilmiş Microsoft bulut hizmetlerini karşılaştırın: Microsoft Akışı, Mantıksal Uygulamalar, İşlevler ve Web İşler.'
ms.topic: overview
ms.date: 04/09/2018
ms.custom: mvc
ms.openlocfilehash: bd9f3bfe1578b632707382cfe422f19514e7ce48
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241321"
---
> [!NOTE]
> Microsoft Flow artık Power Automate hizmetidir. Daha fazla bilgi için bu [bloga](https://aka.ms/flow-now-pa) bakın.
> 
> Bu içerik, önümüzdeki günlerde marka değişikliğini yansıtacak şekilde güncelleştirilecektir.
# <a name="choose-the-right-integration-and-automation-services-in-azure"></a>Azure'da doğru entegrasyon ve otomasyon hizmetlerini seçin

Bu makalede aşağıdaki Microsoft bulut hizmetleri karşılaştırılır:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Azure App Service Web İşleri](../app-service/webjobs-create.md)

Tüm bu hizmetler, tümleştirme sorunlarını çözebilir ve iş süreçlerini otomatikleştirebilir. Tümü giriş, eylemler, koşullar ve çıkış tanımı yapabilir. Her birini belirli bir zamanlamayla veya tetikleyiciyle çalıştırabilirsiniz. Her hizmetin benzersiz avantajları vardır ve bu makalede farklar açıklanmaktadır. 

Azure İşlevleri ve diğer Azure bilgi işlem seçenekleri arasında daha genel bir karşılaştırma arıyorsanız, [Azure bilgi işlem hizmeti seçme ölçütleri](/azure/architecture/guide/technology-choices/compute-comparison) ve mikro hizmetler için Azure bilgi işlem seçeneğini [seçme](/azure/architecture/microservices/design/compute-options)ölçütleri'ne bakın.

## <a name="compare-microsoft-flow-and-azure-logic-apps"></a>Microsoft Flow ve Azure Logic Apps karşılaştırması

Microsoft Flow ve Logic Apps, iş akışları oluşturabilen *ilk tasarımcı* tümleştirme hizmetleridir. Her iki hizmet de çeşitli SaaS uygulamaları ve kurumsal uygulamalarla tümleştirilir. 

Microsoft Flow, Logic Apps'ın üzerine kurulmuştur. Aynı iş akışı tasarımcısını ve aynı [bağlayıcıları paylaşırlar.](../connectors/apis-list.md) 

Microsoft Flow, herhangi bir ofis çalışanının geliştiricilere veya BT'ye geçmeden basit tümleştirmeler (örneğin, SharePoint Document Library'deki onay işlemi) gerçekleştirmesini sağlar. Logic Apps, kurumsal düzeyde Azure DevOps ve güvenlik uygulamalarının gerekli olduğu gelişmiş tümleştirmeleri (örneğin, B2B işlemleri) de etkinleştirebilir. Kurumsal iş akışının zamanla karmaşık hale gelmesi tipik bir durumdur. Buna göre, ilk başta bir akışla başlayabilir ve daha sonra gerektiğinde bir mantık uygulamasına dönüştürebilirsiniz.

Aşağıdaki tablo, Microsoft Flow veya Logic Apps'ın belirli bir tümleştirme için en iyisi olup olmadığını belirlemenize yardımcı olur:

|  | Microsoft Flow | Logic Apps |
| --- | --- | --- |
| **Kullanıcılar** |Ofis çalışanları, iş kullanıcıları veya SharePoint yöneticileri |Uzman tümleştiriciler ve geliştiriciler, BT uzmanları |
| **Senaryolar** |Self servis |Gelişmiş tümleştirmeler |
| **Tasarım aracı** |Tarayıcı içi ve mobil uygulama, yalnızca kullanıcı arabirimi |Tarayıcı içi ve [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Cod görünümü](../logic-apps/logic-apps-author-definitions.md) sağlanır |
| **Uygulama yaşam döngüsü yönetimi (ALM)** |Üretim dışı ortamlarda tasarım ve test, hazır olduğunda üretime teşvik |Azure DevOps: [Azure Kaynak Yöneticisi'nde](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) kaynak denetimi, test, destek, otomasyon ve yönetilebilirlik |
| **Yönetici deneyimi** |Microsoft Akış ortamlarını ve veri kaybıönleme (DLP) ilkelerini yönetin, lisanslamayı izleyin: [Microsoft Flow Admin Center](https://admin.flow.microsoft.com) |Kaynak gruplarını, bağlantıları, erişim yönetimini ve günlüğe kaydetmeyi yönetin: [Azure portalı](https://portal.azure.com) |
| **Güvenlik** |Office 365 Güvenlik ve Uyumluluk denetim günlükleri, DLP, hassas veriler için [hazır şifreleme](https://wikipedia.org/wiki/Data_at_rest#Encryption) |Azure'un güvenlik güvencesi: [Azure güvenliği,](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity) [Azure Güvenlik Merkezi,](https://azure.microsoft.com/services/security-center/) [denetim günlükleri](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) |

## <a name="compare-azure-functions-and-azure-logic-apps"></a>Azure İşlevleri ve Azure Logic Apps karşılaştırması

İşlevler ve Logic Apps, sunucusuz iş yüklerine olanak tanıyan Azure hizmetleridir. Azure İşlevleri sunucusuz bir bilgi işlem hizmetidir, Azure Logic Apps ise sunucusuz iş akışları sağlar. Her ikisi de karmaşık *orkestrasyon*oluşturabilirsiniz. Düzenleme, Logic Apps’te karmaşık bir görevin gerçekleştirilmesi için yürütülen, *eylemler* olarak adlandırılan işlevlerin veya adımların bir koleksiyonudur. Örneğin, bir dizi siparişi işlemek için, bir işlevin birçok örneğini paralel olarak yürütebilir, tüm örneklerin tamamlanmasını bekleyebilir ve ardından toplamda bir sonucu oluşturan bir işlevi çalıştırabilirsiniz.

Azure İşlevleri için düzenlemeleri kod yazarak ve [Dayanıklı İşlevler uzantısını](durable/durable-functions-overview.md) kullanarak geliştirirsiniz. Logic Apps için düzenlemeleri, GUI kullanarak veya yapılandırma dosyalarını düzenleyerek oluşturursunuz.

Düzenleme oluşturduğunuzda, mantıksal uygulamalardan işlev çağırdığınızda ve işlevlerden mantıksal uygulama çağırdığınızda hizmetleri karıştırıp eşleştirebilirsiniz. Hizmet özelliklerine veya kişisel tercihinize göre her düzenlemenin nasıl oluşturulacağını seçin. Aşağıdaki tabloda, bunlar arasındaki temel farklardan bazıları listelenebisre:

|  | Dayanıklı İşlevler | Logic Apps |
| --- | --- | --- |
| **Geliştirme** | Koda öncelik veren (kesinlik temelli) | Tasarımcıya öncelik veren (bildirim temelli) |
| **Bağlantı** | [Yaklaşık bir düzine bağlama türü](functions-triggers-bindings.md#supported-bindings), özel bağlamalar için kod yazma | [Bağlayıcılardan oluşan büyük koleksiyon](../connectors/apis-list.md), [B2B senaryoları için Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md), [özel bağlayıcı oluşturma](../logic-apps/custom-connector-overview.md) |
| **Eylemler** | Her etkinlik bir Azure işlevidir; eylem işlevleri için kod yazma |[Hazır eylemlerden oluşan büyük koleksiyon](../logic-apps/logic-apps-workflow-actions-triggers.md)|
| **İzleme** | [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) | [Azure portalı](../logic-apps/quickstart-create-first-logic-app-workflow.md), [Azure Monitor günlükleri](../logic-apps/logic-apps-monitor-your-logic-apps.md)|
| **Yönetim** | [REST API](durable/durable-functions-http-api.md), [Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer) | [Azure portalı](../logic-apps/quickstart-create-first-logic-app-workflow.md), [REST API](https://docs.microsoft.com/rest/api/logic/), [PowerShell](https://docs.microsoft.com/powershell/module/az.logicapp), [Visual Studio](https://docs.microsoft.com/azure/logic-apps/manage-logic-apps-with-visual-studio) |
| **Yürütme bağlamı** | Yerel [olarak](functions-runtime-overview.md) veya bulutta çalıştırılabilir | Yalnızca bulutta çalışır|

<a name="function"></a>

## <a name="compare-functions-and-webjobs"></a>İşlevler Web İşleri karşılaştırması

Azure İşlevleri gibi, WebJobs SDK ile Azure App Service WebJobs da geliştiriciler için tasarlanmış, *koda öncelik veren* bir tümleştirme hizmetidir. Her ikisi de [Azure App Service](../app-service/overview.md) üzerinde derlenmiş olup [source control integration](../app-service/deploy-continuous-deployment.md), [authentication](../app-service/overview-authentication-authorization.md) ve [monitoring with Application Insights integration](functions-monitoring.md) gibi özellikleri destekler.

### <a name="webjobs-and-the-webjobs-sdk"></a>Web İşleri ve Web İşleri SDK’sı

Bir Uygulama Hizmeti web uygulaması bağlamında bir komut dosyası veya kod çalıştırmak için Uygulama Hizmeti'nin *Web İşler* özelliğini kullanabilirsiniz. *WebJobs SDK*, Azure hizmetlerine yanıt olarak yazdığınız kodu kolaylaştıran WebJobs için tasarlanmış bir çerçevedir. Örneğin, Azure Depolama'da küçük resim görüntüsü oluşturarak bir resim blob'unun oluşturulmasına yanıt verebilirsiniz. WebJobs SDK, WebJob’a dağıtabileceğiniz bir .NET konsol uygulaması olarak çalıştırılır. 

WebJobs ve WebJobs SDK birlikte en iyi şekilde çalışır; ancak WebJobs’ı WebJobs SDK olmadan kullanabilirsiniz; bunun tersi de olabilir. Bir Web İşi, App Service korumalı alanında çalışan herhangi bir programı veya betiği çalıştırabilir. Web İşleri SDK konsolu uygulaması, şirket içi sunucular gibi konsol uygulamalarının çalıştığı her yerde çalışabilir.

### <a name="comparison-table"></a>Karşılaştırma tablosu

Azure İşlevleri, WebJobs SDK’da derlendiğinden diğer Azure hizmetlerine yönelik aynı bağlantıların ve olay tetikleyicilerinin birçoğunu paylaşır. Azure İşleri ve Web İşleri arasında Web İşler Ii sdk ile seçim yaparken göz önünde bulundurmanız gereken bazı etkenler şunlardır:

|  | İşlevler | WebJobs SDK ile WebJobs |
| --- | --- | --- |
|[Otomatik ölçeklendirme](functions-scale.md#how-the-consumption-and-premium-plans-work) ile [sunucusuz uygulama modeli](https://azure.microsoft.com/solutions/serverless/)|✔||
|[Tarayıcıda geliştirme ve test etme](functions-create-first-azure-function.md) |✔||
|[Kullanım başına ödeme fiyatlandırması](functions-scale.md#consumption-plan)|✔||
|[Logic Apps ile tümleştirme](functions-twitter-email.md)|✔||
| Tetikleyici olayları |[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md)<br>[Azure Event Grid](functions-bindings-event-grid.md)|[Zamanlayıcı](functions-bindings-timer.md)<br>[Azure Depolama kuyrukları ve blobları](functions-bindings-storage-blob.md)<br>[Azure Service Bus kuyrukları ve konuları](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[Dosya sistemi](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Files/FileTriggerAttribute.cs)|
| Desteklenen diller  |C#<br>F#<br>JavaScript<br>Java<br>Python<br>PowerShell |C#<sup>1</sup>|
|Paket yöneticileri|NPM ve NuGet|NuGet<sup>2</sup>|

<sup>1</sup> WebJobs (WebJobs SDK olmadan) C#, Java, JavaScript, Bash, .cmd, .bat, PowerShell, PHP, TypeScript, Python ve daha fazlasını destekler. Bu kapsamlı bir liste değil. WebJob, App Service korumalı alanında çalıştırılabilen herhangi bir programı veya betiği çalıştırabilir.

<sup>2</sup> WebJobs (WebJobs SDK olmadan) NPM ve NuGet destekler.

### <a name="summary"></a>Özet

Azure İşlevleri, Azure App Service WebJobs'tan daha fazla geliştirici üretkenliği sunar. Ayrıca programlama dilleri, geliştirme ortamları, Azure hizmet tümleştirmesi ve fiyatlandırma için daha fazla seçenek sunar. Çoğu senaryo için bu en iyi seçenektir.

WebJobs’ın en iyi seçenek olduğu iki senaryo aşağıda verilmiştir:

* Olayları dinleyen kod (`JobHost` nesnesi) üzerinde daha fazla denetime ihtiyacınız vardır. İşlevler, [host.json](functions-host-json.md) dosyasında `JobHost` davranışını özelleştirmek için sınırlı sayıda yöntem sunar. Bazen bir JSON dosyasındaki dize tarafından belirtilemeyen şeyler yapmanız gerekir. Örneğin, yalnızca WebJobs SDK, Azure Depolama için özel bir yeniden deneme ilkesi yapılandırmanıza olanak sağlar.
* Kod parçacıklarını çalıştırmak istediğiniz bir Uygulama Hizmeti uygulamanız vardır ve bunları aynı Azure DevOps ortamında birlikte yönetmek istersiniz.

Azure veya üçüncü taraf hizmetleri tümleştirmek için kod parçacıklarını çalıştırmak istediğiniz diğer durumlarda, WebJobs SDK ile WebJobs üzerinden Azure İşlevleri’ni seçin.

<a name="together"></a>

## <a name="microsoft-flow-logic-apps-functions-and-webjobs-together"></a>Microsoft Akışı, Mantıksal Uygulamalar, Fonksiyonlar ve Web İşler birlikte

Bu hizmetlerden sadece birini seçmek zorunda değilsiniz. Dış hizmetlerle olduğu kadar birbirleriyle de bütünleşirler.

Akış bir mantıksal uygulamayı çağırabilir. Mantıksal uygulama bir işlevi çağırabilir ve işlev de bir mantıksal uygulamayı çağırabilir. Örneğin, bkz. [Azure Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md).

Microsoft Akışı, Logic Apps ve Fonksiyonlar arasındaki tümleştirme zaman içinde iyileşmeye devam eder. Bir hizmette bir şey oluşturabilir ve bunu diğer hizmetlerde kullanabilirsiniz.

Aşağıdaki bağlantıları kullanarak entegrasyon hizmetleri hakkında daha fazla bilgi alabilirsiniz:

* [Tümleştirme senaryoları için Azure İşlevleri ve Azure App Service'ten yararlanma - Christopher Anderson](https://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Tümleştirmeler Basitleşti - Charles Lamanna](https://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Mantık Uygulamaları Canlı web yayını](https://aka.ms/logicappslive)
* [Microsoft Flow sık sorulan sorular](/power-automate/frequently-asked-questions)

## <a name="next-steps"></a>Sonraki adımlar

İlk akışınızı, mantıksal uygulamanızı veya işlev uygulamanızı oluşturarak başlayın. Aşağıdaki bağlantılardan birini seçin:

* [Microsoft Flow’u kullanmaya başlama](/power-automate/getting-started)
* [Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [İlk Azure işlevinizi oluşturun](functions-create-first-azure-function.md)
