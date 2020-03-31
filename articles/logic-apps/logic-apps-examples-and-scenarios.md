---
title: Sık karşılaşılan senaryolar & örnekler
description: Azure Mantık Uygulamaları için örnekleri, yaygın senaryoları, öğreticileri ve gözden geçirme leri bulma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/28/2020
ms.openlocfilehash: c64483f18b2deb061704a4eb7171e575fdd561e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164636"
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Azure Mantık Uygulamaları için sık karşılaşılan senaryolar, örnekler, öğreticiler ve geçiş ler

[Azure Logic Apps,](../logic-apps/logic-apps-overview.md) şirket içi SQL Server veya SAP'den Azure Bilişsel Hizmetleri'ne kadar [yüzlerce kullanıma hazır konektör](../connectors/apis-list.md)sağlayarak farklı hizmetleri düzenlemenize ve tümleştirmenize yardımcı olur. Logic Apps hizmeti "sunucusuz"dur, bu nedenle ölçek veya örnekler hakkında endişelenmenize gerek kalmaz. Tek yapmanız gereken iş akışını bir tetikleyici yle ve iş akışının gerçekleştirdiği eylemlerle tanımlamaktır. Temel platform ölçek, kullanılabilirlik ve performansı işler. Logic Apps, özellikle birden çok sistem ve hizmet arasında eylemleri koordine etmeniz gereken kullanım durumları ve senaryolar için yararlıdır.

Azure Logic Apps'ın desteklediği özellikler ve desenler hakkında bilgi edinmenize yardımcı olmak için bu makalede, sık karşılaşılan başlangıç noktaları, örnekler ve senaryolar açıklanmaktadır.

## <a name="common-starting-points-for-logic-app-workflows"></a>Mantık uygulaması iş akışları için ortak başlangıç noktaları

Her mantık uygulaması, mantık uygulaması iş akışınızı başlatan ve tetikleyicinin bir parçası olarak herhangi bir veriyi aktaran bir [*tetikleyici*](../logic-apps/logic-apps-overview.md#logic-app-concepts)ve yalnızca bir tetikleyiciyle başlar. Bazı bağlayıcılar, bu türde gelen tetikleyiciler sağlar:

* *Yoklama tetikleyicileri*: Yeni veriler için bir hizmet bitiş noktasını düzenli olarak denetler. Yeni veriler olduğunda, tetikleyici veri girişi olarak yeni bir iş akışı örneği oluşturur ve çalıştırAr.

* *İtme tetikleyicileri*: Bir hizmet bitiş noktasındaverileri dinler ve belirli bir olay gerçekleşene kadar bekler. Olay gerçekleştiğinde, tetikleyici hemen devreye girer ve kullanılabilir verileri giriş olarak kullanan yeni bir iş akışı örneği oluşturur ve çalıştırar.

Sık kullanılan tetikleyicileri açıklayan örnekler aşağıda verilmiştir:

* *Yoklama* tetikleyicileri:

  * [ **Yineleme** ](../connectors/connectors-native-recurrence.md) tetikleyicisi, başlangıç tarihini ve saatini ve mantık uygulamanızı ateşlemek için yinelemeyi ayarlamanızı sağlar. Örneğin, mantık uygulamanızı tetiklemek için haftanın günlerini ve günün saatlerini seçebilirsiniz. Daha fazla bilgi için şu konulara bakın:<p>

    * [Azure Logic Apps ile yinelenen otomatik görevleri, işlemleri ve iş akışlarını zamanlama ve çalıştırma](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)
    * [Öğretici: Azure Logic Apps'ı kullanarak otomatik, zamanlama tabanlı yinelenen iş akışları oluşturun](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)

  * **Bir e-posta tetiklendiğinde,** mantık uygulamanızın Logic Apps tarafından desteklenen herhangi bir posta sağlayıcısından gelen yeni e-postaları kontrol etmenizi sağlar, örneğin, [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [Outlook.com,](https://docs.microsoft.com/connectors/outlook/)ve benzeri. Daha fazla bilgi için şu konulara bakın:<p>

    * [Öğretici: Azure Logic Apps'ı kullanarak otomatik onay tabanlı iş akışları oluşturun](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)
    * [Öğretici: Azure Logic Apps, Azure İşlevleri ve Azure Depolama'yı kullanarak e-postaları işlemek için görevleri otomatikleştirin](../logic-apps/tutorial-process-email-attachments-workflow.md)

  * [ **HTTP** ](../connectors/connectors-native-http.md) tetikleyicisi, HTTP veya HTTPS üzerinden bir hizmet bitiş noktasını arayabilir. Daha fazla bilgi [için, HTTP uç noktalarını kullanarak Arama, tetikleme veya yuva iş akışlarına](../logic-apps/logic-apps-http-endpoint.md)bakın.

* *İtme* tetikleyicileri:

  * [ **İstek** tetikleyicisi](../connectors/connectors-native-reqres.md) gelen HTTPS isteklerini alabilir.

  * [ **HTTP Webhook** tetikleyicisi,](../connectors/connectors-native-webhook.md) bu hizmete bir *geri arama URL'si* kaydederek bir hizmet bitiş noktasına abone olur. Bu şekilde, tetikleyicinin hizmeti yoklaması gerekmeden, belirtilen olay gerçekleştiğinde hizmet tetikleyiciyi bildirebilir.

Belirtilen olay meydana gelen olay dan sonra, yeni bir mantık uygulaması iş akışı örneği oluşturan ve iş akışındaki eylemleri çalıştıran tetikleyici yangınları. İş akışı boyunca tetikleyiciden herhangi bir veriye erişebilirsiniz. Örneğin, Twitter **Yeni bir tweet** tetikleyici sitiği yle tweet içeriğini mantık uygulaması na aktarın. Azure Logic Apps'a başlamak için şu hızlı başlangıç konularını deneyin:

* [Hızlı başlangıç: Azure Logic Apps - Azure portalını kullanarak ilk otomatik iş akışınızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Hızlı başlatma: Azure Logic Apps 'ı kullanarak otomatik görevler, işlemler ve iş akışları oluşturun - Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Hızlı başlatma: Visual Studio Code'u kullanarak otomatik mantık uygulaması iş akışları oluşturma ve yönetme](../logic-apps/quickstart-create-logic-apps-visual-studio-code.md)

## <a name="control-flow-and-error-handling-capabilities"></a>Kontrol akışı ve hata işleme yetenekleri

Mantık uygulamaları, koşullar, anahtarlar, döngüler ve kapsamlar gibi gelişmiş denetim akışı için zengin özellikler içerir. Esnek çözümler sağlamak için, iş akışlarınızda hata ve özel durum işleme de uygulayabilirsiniz.

* [Koşullu deyimlere](../logic-apps/logic-apps-control-flow-conditional-statement.md) dayalı farklı eylemler gerçekleştirme ve [ifadeleri değiştirin](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Döngülerle diziler ve koleksiyonlarda adımları yineleme veya öğeleri işleme](../logic-apps/logic-apps-control-flow-loops.md)
* [Eylemleri kapsamlarla birlikte gruplandırma](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* [İş akışına hata ve özel durum işleme ekleme](../logic-apps/logic-apps-exception-handling.md)
* [Kullanım örneği: Bir sağlık şirketi HL7 FHIR iş akışları için mantık uygulaması özel durum işlemeyi nasıl kullanır?](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

## <a name="create-custom-apis-and-connectors"></a>Özel API'ler ve bağlayıcılar oluşturun

Yayınlanmış konektörleri olmayan sistemler ve hizmetler için mantık uygulamalarını da genişletebilirsiniz.

* [Azure Logic Apps'tan aramak için özel API'ler oluşturun](../logic-apps/logic-apps-create-api-app.md)
* [Yoklama tetikleyici deseni kullanarak düzenli olarak yeni verileri veya olayları denetleme](../logic-apps/logic-apps-create-api-app.md#polling-triggers)
* [Webhook tetikleme deseni kullanarak yeni verileri veya olayları bekleyin ve dinleyin](../logic-apps/logic-apps-create-api-app.md#webhook-triggers)
* [Yoklama eylem deseni kullanarak uzun çalışan görevleri gerçekleştirin](../logic-apps/logic-apps-create-api-app.md#async-pattern)
* [Webhook eylem deseni kullanarak uzun süren görevleri gerçekleştirin](../logic-apps/logic-apps-create-api-app.md#webhook-actions)
* [Azure Mantık Uygulamalarında özel bağlayıcılar](../logic-apps/custom-connector-overview.md)

## <a name="build-business-to-business-b2b-solutions"></a>İşletmeler arası (B2B) çözümler oluşturun

Kurumsal tümleştirme çözümleri ve kuruluşlar arasında sorunsuz iletişim için, Azure Mantık Uygulamaları ile Kurumsal Tümleştirme Paketi'ni (EIP) kullanarak bu senaryolar için otomatik ölçeklenebilir iş akışları oluşturabilirsiniz. Kuruluşlar farklı protokoller ve biçimler kullansa lar da, iletileri elektronik olarak değiştirebilirler. EIP, farklı biçimleri kuruluşların sistemlerinizin işleyebilir ve AS2, X12, EDIFACT ve RosettaNet gibi endüstri standardı protokolleri destekleyebilir bir biçime dönüştürür. Bu çözümleri oluşturmak için, mantık uygulaması iş akışlarınızla tanımladığınız ve kullandığınız yapılar için güvenli, ölçeklenebilir ve yönetilebilir bir kapsayıcı sağlayan ayrı bir Azure kaynağı olan bir tümleştirme hesabı oluşturursunuz. Örneğin, eserler ticaret ortakları, anlaşmalar, haritalar, şemalar, sertifikalar ve toplu iş yapılandırmaları içerir.

* [Genel Bakış: Azure Logic Apps ve Kurumsal Tümleştirme Paketi ile B2B kurumsal tümleştirme çözümleri](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Azure Logic Apps'ta B2B kurumsal tümleştirmeleri için entegrasyon hesapları oluşturma ve yönetme](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

## <a name="access-azure-virtual-network-resources"></a>Azure sanal ağ kaynaklarına erişin

Bazen, mantıksal uygulamalarınız ve tümleştirme hesaplarınız, Azure sanal ağında bulunan sanal makineler (VM'ler) ve diğer sistemler veya hizmetler gibi güvenli kaynaklara erişmeye ihtiyaç duyar. Bu erişimi ayarlamak için, mantık uygulamalarınızı oluşturabileceğiniz ve çalıştırabileceğiniz bir entegrasyon hizmeti ortamı (İmKB) oluşturabilirsiniz. Bir ISE, depolama gibi özel kaynakları kullanan ve "genel", çok kiracılı Logic Apps hizmetinden ayrı çalışan Logic Apps hizmetinin özel ve yalıtılmış bir örneğidir. Yalıtılmış özel örneğini ve genel genel örneğini ayırmak, diğer Azure kiracılarının uygulamalarınızın performansı üzerindeki etkisini azaltmaya da yardımcı olur ve bu da "gürültülü komşular" etkisi olarak da bilinir.

* [Genel Bakış: Azure Logic Apps'tan Azure sanal ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Azure Logic Apps'tan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

## <a name="deploy-manage-and-monitor-logic-apps"></a>Mantık uygulamalarını dağıtma, yönetme ve izleme

Visual Studio, Azure DevOps veya diğer kaynak denetim ve otomatik yapı araçlarıyla mantık uygulamaları geliştirebilir ve dağıtabilirsiniz. Kaynak şablonundaki iş akışları ve bağımlı bağlantılar için dağıtımı desteklemek için, mantıksal uygulamalar Azure kaynak dağıtım şablonlarını kullanır. Visual Studio araçları, sürüm oluşturma için kaynak denetimine giriş yapabileceğiniz bu şablonları otomatik olarak oluşturur. Azure Logic Apps, iş akışı çalıştırma durumu için bildirim ve tanılama günlükleri için izleme ve uyarılar da sağlar.

### <a name="deploy"></a>Dağıtma

* [Hızlı başlatma: Azure Logic Apps 'ı kullanarak otomatik görevler, işlemler ve iş akışları oluşturun - Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Genel Bakış: Mantık uygulaması dağıtımını otomatikleştirin](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
* [Azure Mantık Uygulamaları için dağıtımı otomatikleştirmek için Azure Kaynak Yöneticisi şablonları oluşturun](../logic-apps/logic-apps-create-azure-resource-manager-templates.md)
* [Azure Mantık Uygulamaları için Azure Kaynak Yöneticisi şablonlarını dağıtma](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)
* [Örnek: Azure Mantıksal Uygulamaları'ndan Azure Servis Veri Mes'üne bağlanın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtın](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamaları'ndan Azure Depolama hesaplarına bağlanın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtma](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamaları için bir işlev uygulaması eylemi ayarlayın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtma](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamaları'ndan bir tümleştirme hesabına bağlanın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtma](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamalarını kullanarak Azure Ardışık Hatlar'ı düzenleme](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-pipeline-orchestration/azure-devops-orchestration-with-logic-apps/)

### <a name="manage"></a>Yönetme

* [Visual Studio'u kullanarak mantık uygulamalarını yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md)
* [B2B kurumsal tümleştirmeleri için entegrasyon hesapları oluşturma ve yönetme](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)
* [Azure Logic Apps'ta entegrasyon hizmeti ortamını (Ise) yönetme](../logic-apps/ise-manage-integration-service-environment.md)

### <a name="monitor"></a>İzleme

* [Çalışma durumunu izleyin, tetikleyici geçmişi gözden geçirin ve Azure Mantık Uygulamaları için uyarılar ayarlayın](../logic-apps/monitor-logic-apps.md)
* [Azure Monitor günlüklerini ayarlayın ve Azure Logic Apps için tanılama verilerini toplayın](../logic-apps/monitor-logic-apps-log-analytics.md)
* [Azure Logic Apps'ta Azure Monitor günlüklerini ayarlayın ve B2B iletileri için tanılama verileri toplayın](../logic-apps/monitor-b2b-messages-log-analytics.md)
* [Azure Mantıksal Uygulamaları için Azure Monitor günlüklerinde izleme ve izleme sorgularını görüntüleme ve oluşturma](../logic-apps/create-monitoring-tracking-queries.md)

## <a name="handle-content-types-conversions-and-transformations"></a>İçerik türlerini, dönüşümleri ve dönüşümleri işleme

Azure Logic Apps [iş akışı tanım dilindeki](https://aka.ms/logicappsdocs)birçok işlevi kullanarak birden çok içerik türüne erişebilir, dönüştürebilir ve dönüştürebilirsiniz. Örneğin, bir dize, JSON ve XML arasında `@json()` `@xml()` ve iş akışı ifadeleri ile dönüştürebilirsiniz. Logic Apps altyapısı, hizmetler arasında kayıpsız bir şekilde içerik aktarımını desteklemek için içerik türlerini korur.

* [Azure Logic Apps'taki içerik türlerini](../logic-apps/logic-apps-content-type.md)( `application/` `application/octet-stream``multipart/formdata`
* [Azure Mantık Uygulamaları ve Güç Otomatikleştirme ifadelerinde işlevleri kullanma başvuru kılavuzu](../logic-apps/workflow-definition-language-functions-reference.md)
* [Azure Mantık Uygulamaları için İş Akışı Tanımı Dil şeması](../logic-apps/logic-apps-workflow-definition-language.md)

## <a name="other-integrations-and-capabilities"></a>Diğer entegrasyonlar ve yetenekler

Azure Logic Apps, Azure İşlevleri, Azure API Yönetimi, Azure Uygulama Hizmeti ve REST ve SOAP gibi özel HTTP uç noktaları gibi birçok hizmetle tümleşir.

* [Azure Logic Apps'tan Azure İşlevlerini Arayın](../logic-apps/logic-apps-azure-functions.md)
* [Öğretici: Azure İşlevleri ve Azure Hizmet Veri Servisi'ni kullanarak mantık uygulamalarını arayın veya tetikleyin](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Öğretici: Azure Logic Apps ve Azure Fonksiyonları ile akışlı müşteri öngörüleri panosu oluşturun](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Öğretici: Twitter gönderi duygularını analiz etmek için Azure Logic Apps ve Azure Bilişsel Hizmetleri ile entegre bir işlev oluşturun](../azure-functions/functions-twitter-email.md)
* [Öğretici: Power BI ve Azure Logic Apps'ı kullanarak AI ile çalışan bir sosyal pano oluşturun](https://aka.ms/logicappsdemo)
* [Öğretici: Azure Olay Izgara ve Mantık Uygulamalarını kullanarak sanal makine değişikliklerini izleyin](../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md)
* [Öğretici: IoT hub'ınızı ve posta kutunuzu bağlayan Azure Logic Apps ile IoT uzaktan izleme ve bildirimler](../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md)
* [Blog: Azure Logic Apps'ı kullanarak SOAP hizmetlerini arayın](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Uçtan uca senaryolar

* [Teknik Inceleme: Logic Apps gibi Azure hizmetleriyle uçuca servis yönetimi tümleştirmesi](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="customer-stories"></a>Müşteri hikayeleri

Azure Logic Apps'ın diğer Azure hizmetleri ve Microsoft ürünleriyle birlikte, karmaşık işlemleri basitleştirerek, düzenleyerek, otomatikleştirerek ve düzenleyerek [bu şirketlerin](https://aka.ms/logic-apps-customer-stories) çevikliklerini artırmalarına ve temel işlerine odaklanmalarına nasıl yardımcı olduğunu öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* Logic [Apps için bağlayıcılar](../connectors/apis-list.md) hakkında bilgi edinin
* Azure [Logic Apps ile B2B kurumsal tümleştirme senaryoları](../logic-apps/logic-apps-enterprise-integration-overview.md) hakkında bilgi edinin
