---
title: Azure Service Bus mesajlaşmasına genel bakış | Microsoft Belgeleri
description: Bu makalede, tam olarak yönetilen kurumsal tümleştirme iletisi aracısı olan Azure Hizmet Veri Servisi'ne üst düzey bir genel bakış sağlanmaktadır.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: overview
ms.date: 11/04/2019
ms.custom: mvc
ms.author: aschhab
ms.openlocfilehash: 49a54491c36ef29209d1a53094cc5baf57057557
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240635"
---
# <a name="what-is-azure-service-bus"></a>Azure Service Bus nedir?

Microsoft Azure Service Bus tam olarak yönetilen bir kurumsal tümleştirme ileti aracısıdır. Servis Otobüsü uygulamaları ve hizmetleri ayırabilir. Service Bus, veri ve durum aktarımı için güvenilir ve güvenli bir platform sunar.

Veriler, farklı uygulamalar ve hizmetler arasında *iletiler* kullanılarak aktarılır. İleti ikili biçimdedir ve JSON, XML veya yalnızca metin içerebilir. Daha fazla bilgi için [Entegrasyon Hizmetleri'ne](https://azure.com/integration)bakın.

Yaygın olarak kullanılan bazı mesajlaşma senaryoları:

* *Mesajlaşma*. Satış veya satınalma siparişleri, günlükler veya stok hareketleri gibi iş verilerini aktarın.
* *Uygulamaları ayırın.* Uygulamaların ve hizmetlerin güvenilirliğini ve ölçeklenebilirliğini artırın. İstemci ve hizmetaynı anda çevrimiçi olmak zorunda değildir.
* *Konular ve abonelikler*. Etkinleştir 1: yayıncılar ve aboneler arasındaki*n* ilişkileri.
* *İleti oturumları.* İleti siparişi veya ileti erteleme gerektiren iş akışlarını uygulayın.

## <a name="namespaces"></a>Ad Alanları

Ad alanı, tüm ileti bileşenleri için bir kapsayıcıdır. Birden çok sıra ve konu tek bir ad alanında olabilir ve ad alanları genellikle uygulama kapsayıcıları olarak hizmet vermektedir.

## <a name="queues"></a>Kuyruklar

İletiler *kuyruklara* gönderilir ve bunlardan alınır. Kuyruklar, iletileri alıcı uygulama alınıp işlemek için kullanılabilir olana kadar depolar.

![Kuyruk](./media/service-bus-messaging-overview/about-service-bus-queue.png)

Kuyruktaki iletiler sıralanır ve varışta zaman damgası. Kabul edildikten sonra ileti yedekli depolama alanında güvenli bir şekilde tutulur. İletiler *çekme* modunda teslim edilir, yalnızca istendiğinde iletiteslim edilir.

## <a name="topics"></a>Konular

İleti göndermek ve almak için *konu başlıklarını* da kullanabilirsiniz. Bir kuyruk genellikle noktadan noktaya iletişim için kullanılır, buna karşın konu başlıkları yayımlama/abone olma senaryolarında yararlıdır.

![Konu başlığı](./media/service-bus-messaging-overview/about-service-bus-topic.png)

Konuların birden fazla ve bağımsız abonesi olabilir. Bir konu başlığının bir abonesi konu başlığına gönderilen her iletinin bir kopyasını alabilir. Abonelikler varlıklar olarak adlandırılır. Abonelikler devam eder, ancak süresi dolabilir veya autodelete.

Tek tek aboneliklerin bir konuya gönderilen tüm iletileri almasını istemeyebilirsiniz. Bu durumda, isteğe bağlı *eylemleri*tetikleyen koşulları tanımlamak için *kuralları* ve *filtreleri* kullanabilirsiniz. Belirtilen iletilere filtre uygulayabilir ve ileti özelliklerini ayarlayabilir veya değiştirebilirsiniz. Daha fazla bilgi için [Konu filtreleri ve eylemleri'ne](topic-filters.md)bakın.

## <a name="advanced-features"></a>Gelişmiş özellikler

Service Bus, daha karmaşık ileti sorunlarını çözmenizi sağlayan gelişmiş özellikler içerir. Aşağıdaki bölümlerde bu özelliklerden birkaçı açıklayınız.

### <a name="message-sessions"></a>İleti oturumları

Servis Veri Servisi'nde ilk,ilk çıkış (FIFO) garantisi oluşturmak için oturumları kullanın. İleti oturumları, sınırsız sayıda birbiriyle ilgili iletinin birlikte ve düzenli olarak işlenmesini sağlar. Daha fazla bilgi için İleti [oturumlarına bakın: ilk olarak, ilk çıkan (FIFO)](message-sessions.md).

### <a name="autoforwarding"></a>Otomatik yönlendirme

Otomatik iletme özelliği, başka bir kuyruğa veya konuya bir kuyruk veya abonelik zincirler. Aynı ad alanının bir parçası olmalılar. Otomatik yönlendirme ile, Servis Veri Servisi iletileri bir kuyruk veya abonelikten otomatik olarak kaldırır ve bunları farklı bir sıraya veya konuya yerle bir eder. Daha fazla bilgi için, [otomatik yönlendirme ile Zincirleme Servis Veri Günü varlıkları](service-bus-auto-forwarding.md)na bakın.

### <a name="dead-letter-queue"></a>Ölü harf sırası

Service Bus ölü harf sıranı (DLQ) destekler. Bir DLQ, herhangi bir alıcıya teslim edilemeyecek iletiler tutar. İşlenilemeyecek iletiler tutar. Servis Veri Servisi, iletileri DLQ'dan kaldırmanızı ve incelemenizi sağlar. Daha fazla bilgi için, [Servis Veri Servisi ölü harf kuyruklarına Genel Bakış](service-bus-dead-letter-queues.md)bölümüne bakın.

### <a name="scheduled-delivery"></a>Zamanlanmış teslim

İletileri gecikmiş işleme için bir kuyruğa veya konuya gönderebilirsiniz. Bir işi belirli bir zamanda bir sistem tarafından işlenebilecek şekilde zamanlayabilirsiniz. Daha fazla bilgi için [Zamanlanmış iletilere](message-sequencing.md#scheduled-messages)bakın.

### <a name="message-deferral"></a>İleti erteleme

Bir kuyruk veya abonelik istemcisi iletinin alınmasını daha sonraki bir zamana erteleyebilir. Bu erteleme, uygulamadaki özel koşullar nedeniyle olabilir. İleti sırada veya abonelikte kalır, ancak bir kenara bırakılır. Daha fazla bilgi için [İleti erteleme](message-deferral.md)bölümüne bakın.

### <a name="batching"></a>Toplu İşleme

İstemci tarafında toplu iş, bir kuyruk veya konu başlığı istemcisinin bir iletiyi göndermeyi belirli bir süre için ertelemesini sağlar. İstemci bu süre içinde başka iletiler gönderirse, iletileri tek bir toplu iş olarak gönderir. Daha fazla bilgi [için, Bkz. İstemci tarafı toplu iş.](service-bus-performance-improvements.md#client-side-batching)

### <a name="transactions"></a>İşlemler

Bir hareket, iki veya daha fazla işlemi yürütme *kapsamına*gruplatır. Service Bus, tek bir işlem kapsamında tek bir ileti kuruluşuna karşı gruplandırma işlemlerini destekler. İleti varlığı bir sıra, konu veya abonelik olabilir. Daha fazla bilgi için, [Servis Veri Servisi Hareket İşlemi Genel Bakış'ı'na](service-bus-transactions.md)bakın.

### <a name="filtering-and-actions"></a>Filtreleme ve eylemler

Aboneler, bir konu başlığından hangi iletileri almak istediklerini tanımlayabilir. Bu iletiler, bir veya daha fazla adlandırılmış abonelik kuralı biçiminde belirtilir. Her eşleşen kural koşulu için abonelik, her eşleşen kural için farklı açıklamalı olabilecek iletinin bir kopyasını üretir. Daha fazla bilgi için [Konu filtreleri ve eylemleri'ne](topic-filters.md)bakın.

### <a name="autodelete-on-idle"></a>Boşta autodelete

Boşta autodelete, sıranın otomatik olarak silindiği boşta bir aralık belirtmenizi sağlar. En düşük süre 5 dakikadır. Daha fazla bilgi için [QueueDescription.AutoDeleteOnIdle Özelliği'ne](/dotnet/api/microsoft.servicebus.messaging.queuedescription.autodeleteonidle)bakın.

### <a name="duplicate-detection"></a>Yineleme algılama

Bir hata istemcinin bir gönderme işleminin sonucu hakkında şüpheye neden olabilir. Yinelenen algılama, gönderenin aynı iletiyi yeniden göndermesini sağlar. Başka bir seçenek, sıranın veya konunun yinelenen kopyaları atmasıdır. Daha fazla bilgi için [Yinelenen algılamaya](duplicate-detection.md)bakın.

### <a name="security-protocols"></a>Güvenlik protokolleri
<a name="sas-rbac-and-managed-identities-for-azure-resources"></a>

Service Bus, [Paylaşılan Erişim İmzaları](service-bus-sas.md) (SAS), [Rol Tabanlı Erişim Denetimi](authenticate-application.md) (RBAC) ve [Azure kaynakları için Yönetilen kimlikler](service-bus-managed-service-identity.md) gibi güvenlik protokollerini destekler.

### <a name="geo-disaster-recovery"></a>Coğrafi olağanüstü durum kurtarma

Azure bölgeleri veya veri merkezleri bir kesinti yaşadığında, Coğrafi olağanüstü durum kurtarma veri işlemenin başka bir bölge veya veri merkezinde devam etmesini olanaklı kılar. Daha fazla bilgi için Bkz. [Azure Hizmet Veri Yolu Coğrafi felaket kurtarma.](service-bus-geo-dr.md)

### <a name="security"></a>Güvenlik

Service Bus, standart [AMQP 1.0](service-bus-amqp-overview.md) ve [HTTP/REST](/rest/api/servicebus/) protokollerini destekler.

## <a name="client-libraries"></a>İstemci kitaplıkları

Service Bus [,.NET](https://github.com/Azure/azure-service-bus-dotnet/tree/master), [Java](https://github.com/Azure/azure-service-bus-java/tree/master)ve [JMS](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client)için istemci kitaplıklarını destekler.

## <a name="integration"></a>Tümleştirme

Service Bus, aşağıdaki Azure hizmetleriyle tam olarak tümleşiktir:

* [Event Grid](https://azure.microsoft.com/services/event-grid/)
* [Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Dynamics 365](https://dynamics.microsoft.com)
* [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşmasını kullanmaya başlamak için aşağıdaki makalelere bakın:

* Azure ileti hizmetlerini karşılaştırmak için [bkz.](../event-grid/compare-messaging-services.md?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json)
* [.NET](service-bus-dotnet-get-started-with-queues.md), [Java](service-bus-java-how-to-use-queues.md)veya [JMS](service-bus-java-how-to-use-jms-api-amqp.md)için hızlı başlatmaları deneyin.
* Servis Veri Kurumu kaynaklarını yönetmek için [Service Bus Explorer'a](https://github.com/paolosalvatori/ServiceBusExplorer/releases)bakın.
* Standart ve Premium katmanlar ve bunların fiyatlandırması hakkında daha fazla bilgi edinmek için [Servis Veri Yolu fiyatlandırmasına](https://azure.microsoft.com/pricing/details/service-bus/)bakın.
* Premium katman için performans ve gecikme sürelerini öğrenmek için Premium [Mesajlaşma'ya](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Premium-Messaging-How-fast-is-it/ba-p/370722)bakın.
