---
title: Azure Servis Veri Servisi konu filtreleri | Microsoft Dokümanlar
description: Bu makalede, abonelerin filtreleri belirterek bir konudan hangi iletileri almak istediklerini nasıl tanımlayabilecekleri açıklanmaktadır.
services: service-bus-messaging
documentationcenter: ''
author: spelluru
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/16/2020
ms.author: spelluru
ms.openlocfilehash: fb6092b7ccb3d1a4214f8d26119d9dc50b0ed317
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81482058"
---
# <a name="topic-filters-and-actions"></a>Konu başlığı filtreleri ve eylemleri

Aboneler, bir konu başlığından hangi iletileri almak istediklerini tanımlayabilir. Bu iletiler, bir veya daha fazla adlandırılmış abonelik kuralı biçiminde belirtilir. Her kural, belirli iletileri seçen bir koşul ve seçili iletiye açıklama ekleyen bir eylemden oluşur. Eşleşen her kural koşulu için abonelik, iletinin, her eşleşme kuralı için farklı şekilde açıklama eklenebilecek bir kopyasını üretir.

Yeni oluşturulan her konu aboneliğinin ilk varsayılan abonelik kuralı vardır. Kural için bir filtre koşulu açıkça belirtmezseniz, uygulanan filtre tüm iletilerin aboneliğe seçilmesini sağlayan **gerçek** filtredir. Varsayılan kuralın ilişkili ek açıklama eylemi yoktur.

Servis Veri Servisi üç filtre koşullarını destekler:

-   *Boolean filtreleri* - TrueFilter ve **FalseFilter,** gelen tüm iletilerin **(doğru)** veya gelen iletilerin hiçbirinin abonelik için seçilmesine**neden**olur. **FalseFilter**

-   *SQL Filtreler -* **SqlFilter,** gelen iletilerin kullanıcı tanımlı özellikleri ve sistem özelliklerine göre broker'da değerlendirilen SQL benzeri koşullu bir ifade tutar. Tüm sistem özellikleri koşullu `sys.` ifadeile önceden belirlenmiş olmalıdır. [Filtre koşulları için SQL alt kümesi,](service-bus-messaging-sql-filter.md) özelliklerin`EXISTS`varlığı için test`IS NULL`eder ( ), null-values ( ), mantıksal Değİl/VE/VEYA, ilişkisel `LIKE`işleçler, basit sayısal aritmetik ve basit metin deseni ile eşleşen .

-   *Korelasyon Filtreleri* - **Korelasyon Filtresi,** gelen iletinin kullanıcı ve sistem özelliklerinden bir veya daha fazlasına göre eşleşen bir dizi koşul tutar. Ortak **kullanım, CorrelationId** özelliğiyle eşleştirmektir, ancak uygulama aşağıdaki özelliklere göre eşleşmeyi de seçebilir:

    - **Contenttype**
     - **Etiket**
     - **Messageıd**
     - **Replyto**
     - **Yanıttosessionid**
     - **Sessionıd** 
     - **Hedef**
     - kullanıcı tanımlı özellikler. 
     
     Gelen iletinin bir özellik için değeri korelasyon filtresinde belirtilen değere eşit olduğunda eşleşme vardır. Dize ifadeleri için karşılaştırma büyük/küçük harf duyarlıdır. Birden çok eşleme özelliği belirtirken, filtre bunları mantıksal ve koşul olarak birleştirir, yani filtrenin eşleşmesi için tüm koşulların eşleşmesi gerekir.

Tüm filtreler ileti özelliklerini değerlendirir. Filtreler ileti gövdesini değerlendiremez.

Karmaşık filtre kuralları işleme kapasitesi gerektirir. Özellikle, SQL filtre kurallarının kullanımı abonelik, konu ve ad alanı düzeyinde daha düşük genel ileti iş letibine neden olur. Mümkün olduğunda, uygulamalar sql benzeri filtreler yerine korelasyon filtrelerini seçmelidir, çünkü işlemede çok daha verimlidirler ve iş verime daha az etkiye sahiptirler.

## <a name="actions"></a>Eylemler

SQL filtre koşulları yla, özellikleri ve değerlerini ekleyerek, kaldırarak veya değiştirerek iletiyi açıklamaek sıralayabilen bir eylem tanımlayabilirsiniz. Eylem, SQL UPDATE deyimi sözdizimine gevşek bir şekilde yaslanan [SQL benzeri bir ifade kullanır.](service-bus-messaging-sql-filter.md) Eylem, eşleştikten sonra ve ileti aboneliğe seçilmeden önce ileti üzerinde yapılır. İleti özelliklerindeki değişiklikler, aboneliğe kopyalanan iletiye özeldir.

## <a name="usage-patterns"></a>Kullanım şekilleri

Bir konu için en basit kullanım senaryosu, her aboneliğin bir konuya gönderilen her iletinin bir kopyasını almasını ve bu da bir yayın deseni sağlamasıdır.

Filtreler ve eylemler iki desen grubu daha sağlar: bölümleme ve yönlendirme.

Bölümleme, iletileri varolan birkaç konu aboneliğine öngörülebilir ve birbirini dışlayan bir şekilde dağıtmak için filtreler kullanır. Bölümleme deseni, bir sistem, her biri genel verilerin bir alt kümesini tutan işlevsel olarak aynı bölmelerde birçok farklı bağlamı işlemek üzere ölçeklendirildiğinde kullanılır; örneğin, müşteri profili bilgileri. Bölümleme ile, bir yayımcı bölümleme modeli hakkında herhangi bir bilgi gerektirmeden bir konuya iletiyi gönderir. İleti daha sonra, bölümün ileti işleyicisi tarafından alınabileceği doğru aboneye taşınır.

Yönlendirme, iletileri konu abonelikleri arasında öngörülebilir bir şekilde dağıtmak için filtreler kullanır, ancak münhasır olması gerekmez. [Otomatik yönlendirme](service-bus-auto-forwarding.md) özelliğiyle birlikte, konu filtreleri, Bir Azure bölgesinde ileti dağıtımı için Hizmet Veri Gönderi ad alanı içinde karmaşık yönlendirme grafikleri oluşturmak için kullanılabilir. Azure İşbu'lar veya Azure Mantıksal Uygulamaları Azure Hizmet Veri Sitesi ad alanları arasında bir köprü görevi görürken, iş yeri uygulamalarına doğrudan entegrasyon içeren karmaşık küresel topolojiler oluşturabilirsiniz.


> [!NOTE]
> Şu anda Azure portalı abonelikler için filtre kuralları belirtmenize izin vermiyor. Desteklenen SDK'lardan veya Azure Kaynak Yöneticisi şablonlarından herhangi birini abonelik kurallarını tanımlamak için kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örneklere bakın: 

- [.NET - Temel gönderme ve filtrelerle öğretici alma](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/BasicSendReceiveTutorialwithFilters/BasicSendReceiveTutorialWithFilters)
- [.NET - Konu filtreleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters)
- [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/servicebus/service-bus/samples/javascript/advanced/topicFilters.js)
- [Komut dosyası yazın](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/servicebus/service-bus/samples/typescript/src/advanced/topicFilters.ts)
- [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/templates/microsoft.servicebus/2017-04-01/namespaces/topics/subscriptions/rules)


