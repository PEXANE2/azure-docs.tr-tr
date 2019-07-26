---
title: Örnekler & ortak senaryolar-Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps için örnekler, senaryolar, öğreticiler ve izlenecek yollar
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.date: 01/31/2018
ms.openlocfilehash: 95eca4c7f3e8170f6559799fc4c706e95df70e9e
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385514"
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Azure Logic Apps için genel senaryolar, örnekler, öğreticiler ve izlenecek yollar

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) , şirket içi SQL Server veya SAP 'den Azure bilişsel hizmetler 'e kadar çeşitli [kullanıma açık bağlayıcılar](../connectors/apis-list.md)sunarak farklı Hizmetleri düzenlemenize ve tümleştirmenize yardımcı olur. Logic Apps hizmeti "sunucusuz" olduğundan ölçek veya örnekler hakkında endişelenmeniz gerekmez. Yapmanız gereken tek şey, bir tetikleyiciyle iş akışını ve iş akışının gerçekleştirdiği eylemleri tanımlar. Temel alınan platform ölçek, kullanılabilirlik ve performansı işler. Logic Apps, özellikle birden fazla sistem genelinde birden çok eylemi koordine etmeniz gereken kullanım durumları ve senaryolar için yararlıdır.

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) desteklediği birçok model ve özellik hakkında daha fazla bilgi edinmenize yardımcı olmak için, yaygın örnekler ve senaryolar aşağıda verilmiştir.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Mantıksal uygulama iş akışları için popüler başlangıç noktaları

Her mantıksal uygulama bir [*tetikleyici*](../logic-apps/logic-apps-overview.md#logic-app-concepts)ile başlar ve mantıksal uygulama iş akışınızı başlatan ve bu tetikleyicinin bir parçası olarak herhangi bir verilerde geçiş yapan yalnızca bir tetikleyici. Bazı bağlayıcılar bu türlerde gelen Tetikleyiciler sağlar:

* *Yoklama Tetikleyicileri*: Yeni veriler için bir hizmet uç noktasını düzenli olarak denetler. Yeni veriler mevcut olduğunda, tetikleyici verileri girdi olarak kullanarak yeni bir iş akışı örneği oluşturur ve çalıştırır.

* *Gönderim Tetikleyicileri*: Bir hizmet uç noktasındaki verileri dinler ve belirli bir olay gerçekleşene kadar bekler. Olay gerçekleştiğinde, tetikleyici hemen başlatılır, giriş olarak kullanılabilir verileri kullanan yeni bir iş akışı örneği oluşturur ve çalışır.

İşte yalnızca birkaç popüler tetikleyici örnekleri:

* Masını 

  * [ **Zamanlama-yinelenme** tetikleyicisi](../connectors/connectors-native-recurrence.md) , mantıksal uygulamanızı başlatmak için başlangıç tarihi ve saati ile yinelenme süresini ayarlamanıza olanak sağlar. 
  Örneğin, mantıksal uygulamanızı tetiklemek için haftanın günlerini ve günün saatlerini seçebilirsiniz.

  * "E-posta alındığında" tetikleyicisi, mantıksal uygulamanızın Logic Apps tarafından desteklenen herhangi bir posta sağlayıcısından yeni e-postayı denetlemesini sağlar; örneğin, [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [Outlook.com](https://docs.microsoft.com/connectors/outlook/)vb.

  * [ **Http** tetikleyicisi](../connectors/connectors-native-http.md) , mantıksal uygulamanızın http üzerinden iletişim kurarak belirtilen bir hizmet uç noktasını denetlemesini sağlar.
  
* Hareketle

  * [ **Istek/yanıt-istek** tetikleyicisi](../connectors/connectors-native-reqres.md) , mantıksal uygulamanızın http isteklerini almasını ve belirli bir şekilde olaylara gerçek zamanlı olarak yanıt vermesini sağlar.

  * [ **Http Web kancası** tetikleyicisi](../connectors/connectors-native-webhook.md) , bu hizmete bir *geri çağırma URL 'si* kaydederek bir hizmet uç noktasına abone olur. 
  Bu şekilde, hizmet yalnızca belirtilen olay gerçekleştiğinde tetikleyiciyi bilgilendirebilir, böylece tetikleyici hizmeti yoklamayı gerektirmez.

Yeni veriler veya bir olay hakkında bildirim aldıktan sonra tetikleyici ateşlenir, yeni bir mantıksal uygulama iş akışı örneği oluşturur ve iş akışındaki eylemleri çalıştırır. İş akışı boyunca tetikleyiciden herhangi bir veriye erişebilirsiniz. Örneğin, "yeni bir tweet üzerinde" tetikleyicisi Tweet içeriğini mantıksal uygulama çalıştırmasına geçirir. 

## <a name="respond-to-triggers-and-extend-actions"></a>Tetiklere yanıt verme ve eylemleri genişletme

Yayımlanmış bağlayıcılar bulunmayan sistemler ve hizmetler için Logic Apps 'i de genişletebilirsiniz.

* [Özel Tetikleyiciler veya eylemler oluşturma](../logic-apps/logic-apps-create-api-app.md)
* [İş akışı çalıştırmaları için uzun süre çalışan eylemler ayarlama](../logic-apps/logic-apps-create-api-app.md)
* [Web kancaları ile dış olaylara ve eylemlere yanıt verme](../logic-apps/logic-apps-create-api-app.md)
* [HTTP isteklerine zaman uyumlu yanıtlar ile çağrı, tetikleyici veya iç içe geçmiş iş akışları](../logic-apps/logic-apps-http-endpoint.md)
* [Öğretici: Logic Apps ve Power BI bir şekilde bir AI destekli sosyal pano oluşturun](https://aka.ms/logicappsdemo)
* [Video: Twilio SMS Web kancalarına yanıt verin ve bir metin yanıtı gönderin](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Denetim akışı, hata işleme ve günlüğe kaydetme özellikleri

Logic Apps, koşullar, anahtarlar, döngüler ve kapsamlar gibi gelişmiş denetim akışı için zengin yetenekler içerir. Dayanıklı çözümler sağlamak için, iş akışlarınızda hata ve özel durum işleme de uygulayabilirsiniz. İş akışı çalıştırma durumu için bildirim ve tanılama günlükleri için Azure Logic Apps Ayrıca izleme ve uyarılar da sağlar.

* [Koşullu deyimler](../logic-apps/logic-apps-control-flow-conditional-statement.md) ve [Switch deyimlerini](../logic-apps/logic-apps-control-flow-switch-statement.md) temel alan farklı eylemler gerçekleştirin
* [Döngülerle adımları yineleyin veya diziler ve koleksiyonlardaki öğeleri işleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [İşlemleri kapsamlar ile birlikte gruplandırma](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* [Bir iş akışında yazma hatası ve özel durum işleme](../logic-apps/logic-apps-exception-handling.md)
* [Kullanım örneği: Sağlık şirketi HL7 FHıR iş akışları için mantıksal uygulama özel durum işlemeyi nasıl kullanır?](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Mevcut Logic Apps için izleme, günlüğe kaydetme ve uyarıları açma](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Mantıksal uygulamalar oluştururken izleme ve tanılama günlüğünü açma](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)

## <a name="deploy-and-manage-logic-apps"></a>Logic Apps dağıtma ve yönetme

Visual Studio, Azure DevOps veya diğer kaynak denetimleri ve otomatikleştirilmiş derleme araçlarıyla mantıksal uygulamaları tamamen geliştirebilir ve dağıtabilirsiniz. Bir kaynak şablonundaki iş akışları ve bağımlı bağlantılar için dağıtımı desteklemek amacıyla Logic Apps, Azure kaynak dağıtım şablonlarını kullanır. Visual Studio Araçları otomatik olarak bu şablonları oluşturur. Bu şablonlar, sürüm oluşturma için kaynak denetimine iade edebilirsiniz.

* [Visual Studio ile mantıksal uygulamalar oluşturma ve dağıtma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Mevcut Logic Apps için izleme, günlüğe kaydetme ve uyarıları açma](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Mantıksal uygulama dağıtımını otomatikleştirme](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Bir çalıştırma içindeki içerik türleri, dönüştürmeler ve dönüşümler

Azure Logic Apps [iş akışı tanımlama dilindeki](https://aka.ms/logicappsdocs)birçok işlevi kullanarak birden çok içerik türüne erişebilir, dönüşüm ve dönüştürme yapabilirsiniz. Örneğin, `@json()` ve `@xml()` iş akışı ifadeleriyle bir String, JSON ve XML arasında dönüştürme yapabilirsiniz. Logic Apps altyapısı, hizmetler arasında kayıpsız bir biçimde içerik aktarımını desteklemek için içerik türlerini korur.

* [Logic Apps 'te iş akışı ifadeleri nasıl çalışır](../logic-apps/logic-apps-author-definitions.md)
* , Ve gibi `application/xml` `application/octet-stream` [JSON olmayan içerik türlerini işleyin](../logic-apps/logic-apps-content-type.md)`multipart/formdata`
* [Azure Logic Apps için iş akışı tanımı dil şeması](https://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Diğer tümleştirmeler ve yetenekler

Logic Apps Ayrıca Azure Işlevleri, Azure API Management, Azure uygulama hizmetleri ve özel HTTP uç noktaları gibi birçok hizmet ile tümleştirme sunar. Örneğin, REST ve SOAP.

* [Azure sunucusuz ile gerçek zamanlı sosyal pano oluşturma](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Logic Apps 'ten Azure Işlevleri çağırma](../logic-apps/logic-apps-azure-functions.md)
* [Öğretici: Azure Işlevleri ile mantıksal uygulamaları tetikleme](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Öğretici: Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md)
* [Öğretici: Twitter Post yaklaşımını çözümlemek için Azure Logic Apps ve Azure bilişsel hizmetler ile tümleşen bir işlev oluşturun](../azure-functions/functions-twitter-email.md)
* [Öğretici: IoT Hub 'ınızı ve posta kutunuzu bağlama Azure Logic Apps IoT uzaktan izleme ve bildirimler](../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md)
* [Blog: Mantıksal uygulamalardan SOAP uç noktalarını çağırma](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Uçtan uca senaryolar

* [İncelemesi Logic Apps gibi Azure hizmetleriyle uçtan uca durum yönetimi tümleştirmesi](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="customer-stories"></a>Müşteri hikayeleri

Diğer Azure hizmetleri ve Microsoft ürünleriyle birlikte Azure Logic Apps, [Bu şirketlerin](https://aka.ms/logic-apps-customer-stories) çeviklerinin geliştirilmesine ve karmaşık süreçlerini basitleştirerek, düzenleyerek, otomatikleştirerek ve organize ederek temel işletmelere odaklanmaya nasıl yardımcı olduğunu öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* [JSON ile mantıksal uygulama tanımlarında derleme](../logic-apps/logic-apps-author-definitions.md)
* [Mantık uygulamalarında hataları ve özel durumları işleme](../logic-apps/logic-apps-exception-handling.md)
* [Azure Logic Apps geliştirmeye yönelik yorumlarınızı, sorularınızı, geri bildiriminizi veya önerileri gönderin](https://feedback.azure.com/forums/287593-logic-apps)
