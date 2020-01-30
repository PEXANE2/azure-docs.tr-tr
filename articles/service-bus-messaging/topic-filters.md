---
title: Azure Service Bus konu filtreleri | Microsoft Docs
description: Bu makalede, abonelerin filtreleri belirterek bir konudan hangi iletileri almak istediğini nasıl tanımlayabileceği açıklanmaktadır.
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2020
ms.author: spelluru
ms.openlocfilehash: b8ffbb16763bfe6485ebf2ab770f4537ddbc8569
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76774491"
---
# <a name="topic-filters-and-actions"></a>Konu başlığı filtreleri ve eylemleri

Aboneler, bir konu başlığından hangi iletileri almak istediklerini tanımlayabilir. Bu iletiler bir veya daha fazla adlandırılmış abonelik kuralı biçiminde belirtilir. Her kural, belirli iletileri seçen bir koşuldan ve seçili iletiyi açıklama oluşturan bir eylemden oluşur. Eşleşen her kural koşulu için abonelik, iletinin, her eşleşme kuralı için farklı şekilde açıklama eklenebilecek bir kopyasını üretir.

Yeni oluşturulan her konu aboneliğinin bir ilk varsayılan abonelik kuralı vardır. Kural için açıkça bir filtre koşulu belirtmezseniz, uygulanan filtre, tüm iletilerin abonelikte seçili olmasını sağlayan **doğru** filtredir. Varsayılan kuralda ilişkili ek açıklama eylemi yok.

Service Bus üç filtre koşulu destekler:

-   *Boole filtreleri* - **truefilter** ve **yanlışfiltre** tüm gelen iletilerin (**true**) ya da abonelik için seçili olmayan iletilerin (**false**) hiçbirinin seçilmemesine neden olur.

-   *SQL filtreleri* -bir **sqlfilter** , gelen iletilerin Kullanıcı tanımlı özellikler ve sistem özelliklerine karşı ARACıDA değerlendirilen bir SQL benzeri koşullu ifade barındırır. Tüm sistem özelliklerinin koşullu ifadede `sys.` ön eki olmalıdır. [Filtre koşullarının SQL-Language alt kümesi](service-bus-messaging-sql-filter.md) , özelliklerin varlığını (`EXISTS``IS NULL`), mantıksal olmayan/ve/veya, İlişkisel işleçleri, basit sayısal aritmetiğini ve `LIKE`ile eşleşen basit metin düzenlerini sınar.

-   *Bağıntı filtreleri* -bir **correlationfilter** , gelen bir veya daha fazla iletinin Kullanıcı ve sistem özelliklerine göre eşleşen bir koşullar kümesi tutar. Yaygın olarak kullanılan bir kullanım, **CorrelationId** özelliğiyle eşleşmedir, ancak uygulama **ContentType**, **Label**, **MessageID**, **ReplyTo**, **replytosessionıd**, **SessionID**, **to**ve Kullanıcı tanımlı özelliklerle eşleşmeyi de seçebilir. Bir özellik için gelen iletinin değeri, bağıntı filtresinde belirtilen değere eşitse bir eşleşme oluşur. Dize ifadeleri için karşılaştırma büyük/küçük harfe duyarlıdır. Birden çok eşleşme özelliği belirtirken, filtre bunları mantıksal ve koşul olarak birleştirir, yani filtrenin eşleşmesi için tüm koşulların eşleşmesi gerekir.

Tüm Filtreler ileti özelliklerini değerlendirir. Filtreler ileti gövdesini değerlendiremez.

Karmaşık filtre kuralları işlem kapasitesi gerektirir. Özellikle, SQL filtre kurallarının kullanımı, abonelik, konu başlığı ve ad alanı düzeyinde genel ileti aktarım hızını daha düşük bir şekilde sonuçlanır. Mümkün olduğunda uygulamalar, işleme göre çok daha verimli olduklarından ve bu nedenle aktarım hızı üzerinde daha az etkisi olduğundan SQL benzeri filtreler üzerinden bağıntı filtreleri seçmelidir.

## <a name="actions"></a>Eylemler

SQL filtre koşulları ile, özellikleri ve değerlerini ekleyerek, kaldırarak veya değiştirerek iletiye açıklama eklenebilir bir eylem tanımlayabilirsiniz. Bu eylem, SQL UPDATE deyimi söz dizimini kullanan [SQL benzeri bir ifade kullanır](service-bus-messaging-sql-filter.md) . İşlem eşleştirdikten sonra ve ileti abonelikte seçildikten sonra ileti üzerinde gerçekleştirilir. İleti özelliklerindeki değişiklikler, aboneliğe kopyalanmış ileti için özeldir.

## <a name="usage-patterns"></a>Kullanım desenleri

Bir konu için en basit kullanım senaryosu, her aboneliğin bir konuya gönderilen her iletinin bir kopyasını aldığından ve bu da bir yayın deseninin yapılmasını sağlar.

Filtreler ve eylemler iki ek desen grubunu etkinleştirir: bölümlendirme ve yönlendirme.

Bölümlendirme, çeşitli mevcut konu aboneliklerine iletileri öngörülebilir ve birbirini dışlayan şekilde dağıtmak için filtreler kullanır. Bölümlendirme deseninin bir sistem, her biri, genel verilerin bir alt kümesini tutan bölmeleri özdeş olan çok sayıda farklı bağlamı işlemek üzere ölçeklenirken kullanılır. Örneğin, müşteri profili bilgileri. Bölümlendirme ile, Yayımcı, bölümleme modeli bilgisine gerek kalmadan iletiyi bir konuya gönderir. Daha sonra ileti, bölümün ileti işleyicisi tarafından alınabilecek doğru aboneliğe taşınır.

Yönlendirme, ileti aboneliklerine öngörülebilir bir şekilde ileti dağıtmak için filtreleri kullanır, ancak özel olması gerekmez. [Otomatik iletme](service-bus-auto-forwarding.md) özelliği ile birlikte, konu filtreleri bir Azure bölgesindeki ileti dağıtımı için bir Service Bus ad alanı içinde karmaşık yönlendirme grafikleri oluşturmak için kullanılabilir. Azure Service Bus ad alanları arasında bir köprü görevi gören Azure Işlevleri veya Azure Logic Apps, iş kolu uygulamalarına doğrudan tümleştirmeyle karmaşık küresel topolojiler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [SQLFilter söz dizimi](service-bus-messaging-sql-filter.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)