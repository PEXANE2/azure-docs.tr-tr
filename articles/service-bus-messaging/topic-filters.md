---
title: Azure Service Bus konu filtreleri | Microsoft Docs
description: Bu makalede, abonelerin filtreleri belirterek bir konudan hangi iletileri almak istediğini nasıl tanımlayabileceği açıklanmaktadır.
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
ms.openlocfilehash: 6e780268d4b8c1a512ce82b1ca10a2f6b7b894b7
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83125766"
---
# <a name="topic-filters-and-actions"></a>Konu başlığı filtreleri ve eylemleri

Aboneler, bir konu başlığından hangi iletileri almak istediklerini tanımlayabilir. Bu iletiler, bir veya daha fazla adlandırılmış abonelik kuralı biçiminde belirtilir. Her kural, belirli iletileri seçen bir koşuldan ve seçili iletiyi açıklama oluşturan bir eylemden oluşur. Eşleşen her kural koşulu için abonelik, iletinin, her eşleşme kuralı için farklı şekilde açıklama eklenebilecek bir kopyasını üretir.

Yeni oluşturulan her konu aboneliğinin bir ilk varsayılan abonelik kuralı vardır. Kural için açıkça bir filtre koşulu belirtmezseniz, uygulanan filtre, tüm iletilerin abonelikte seçili olmasını sağlayan **doğru** filtredir. Varsayılan kuralda ilişkili ek açıklama eylemi yok.

Service Bus üç filtre koşulu destekler:

-   *Boole filtreleri* - **truefilter** ve **yanlışfiltre** tüm gelen iletilerin (**true**) ya da abonelik için seçili olmayan iletilerin (**false**) hiçbirinin seçilmemesine neden olur.

-   *SQL filtreleri* -bir **sqlfilter** , gelen iletilerin Kullanıcı tanımlı özellikler ve sistem özelliklerine karşı ARACıDA değerlendirilen bir SQL benzeri koşullu ifade barındırır. Koşullu ifadede tüm sistem özelliklerinin ön eki olmalıdır `sys.` . [Filtre koşulları Için SQL-Language alt kümesi](service-bus-messaging-sql-filter.md) , özelliklerin ( `EXISTS` ), null-değerlerin ( `IS NULL` ), mantıksal olmayan/ve/veya ilişkisel işleçlerin, basit sayısal aritmetiğinin ve ile eşleşen basit metin deseninin varlığını sınar `LIKE` .

-   *Bağıntı filtreleri* -bir **correlationfilter** , gelen bir veya daha fazla iletinin Kullanıcı ve sistem özelliklerine göre eşleşen bir koşullar kümesi tutar. Yaygın olarak kullanılan bir kullanım, **CorrelationId** özelliğiyle eşleşmedir, ancak uygulama aşağıdaki özelliklerle eşleşmeyi de seçebilir:

    - **ContentType**
     - **Etiketle**
     - **Ileti**
     - **ReplyTo**
     - **Replytosessionıd**
     - **Kimliği** 
     - **Hedef**
     - Kullanıcı tanımlı tüm özellikler. 
     
     Bir özellik için gelen iletinin değeri, bağıntı filtresinde belirtilen değere eşitse bir eşleşme oluşur. Dize ifadeleri için karşılaştırma büyük/küçük harfe duyarlıdır. Birden çok eşleşme özelliği belirtirken, filtre bunları mantıksal ve koşul olarak birleştirir, yani filtrenin eşleşmesi için tüm koşulların eşleşmesi gerekir.

Tüm Filtreler ileti özelliklerini değerlendirir. Filtreler ileti gövdesini değerlendiremez.

Karmaşık filtre kuralları işlem kapasitesi gerektirir. Özellikle, SQL filtre kurallarının kullanımı, abonelik, konu ve ad alanı düzeyinde genel ileti aktarım hızını daha düşük bir şekilde oluşmasına neden olur. Mümkün olduğunda uygulamalar, işleme göre çok daha verimli olduklarından ve üretilen iş üzerinde daha az etkilendiklerinden SQL benzeri filtreler üzerinden bağıntı filtreleri seçmelidir.

## <a name="actions"></a>Eylemler

SQL filtre koşulları ile, özellikleri ve değerlerini ekleyerek, kaldırarak veya değiştirerek iletiye açıklama eklenebilir bir eylem tanımlayabilirsiniz. Bu eylem, SQL UPDATE deyimi söz dizimini kullanan [SQL benzeri bir ifade kullanır](service-bus-messaging-sql-filter.md) . İşlem eşleştirdikten sonra ve ileti abonelikte seçildikten sonra ileti üzerinde yapılır. İleti özelliklerindeki değişiklikler, aboneliğe kopyalanmış ileti için özeldir.

## <a name="usage-patterns"></a>Kullanım desenleri

Bir konu için en basit kullanım senaryosu, her aboneliğin bir konuya gönderilen her iletinin bir kopyasını aldığından ve bu da bir yayın deseninin yapılmasını sağlar.

Filtreler ve eylemler iki ek desen grubunu etkinleştirir: bölümlendirme ve yönlendirme.

Bölümlendirme, çeşitli mevcut konu aboneliklerine iletileri öngörülebilir ve birbirini dışlayan şekilde dağıtmak için filtreler kullanır. Bölümlendirme deseninin bir sistem, her biri, genel verilerin bir alt kümesini tutan bölmeleri özdeş olan çok sayıda farklı bağlamı işlemek üzere ölçeklenirken kullanılır. Örneğin, müşteri profili bilgileri. Bölümlendirme ile, Yayımcı, bölümleme modeli bilgisine gerek kalmadan iletiyi bir konuya gönderir. Daha sonra ileti, bölümün ileti işleyicisi tarafından alınabilecek doğru aboneliğe taşınır.

Yönlendirme, ileti aboneliklerine öngörülebilir bir şekilde ileti dağıtmak için filtreleri kullanır, ancak özel olması gerekmez. [Otomatik iletme](service-bus-auto-forwarding.md) özelliği ile birlikte, konu filtreleri bir Azure bölgesindeki ileti dağıtımı için bir Service Bus ad alanı içinde karmaşık yönlendirme grafikleri oluşturmak için kullanılabilir. Azure Service Bus ad alanları arasında bir köprü görevi gören Azure Işlevleri veya Azure Logic Apps, iş kolu uygulamalarına doğrudan tümleştirmeyle karmaşık küresel topolojiler oluşturabilirsiniz.


> [!NOTE]
> Şu anda Azure portal abonelikler için filtre kuralları belirtmenize izin vermez. Abonelik kurallarını tanımlamak için desteklenen SDK 'Lardan veya Azure Resource Manager şablonlardan herhangi birini kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örneklere bakın: 

- [Filtrelerle .NET-temel gönderme ve alma öğreticisi](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/BasicSendReceiveTutorialwithFilters/BasicSendReceiveTutorialWithFilters)
- [.NET-konu filtreleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters)
- [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/templates/microsoft.servicebus/2017-04-01/namespaces/topics/subscriptions/rules)


