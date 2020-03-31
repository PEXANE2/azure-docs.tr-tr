---
title: Azure Servis Veri Servisi sık sorulan sorular (SSS) | Microsoft Dokümanlar
description: Bu makalede, Azure Hizmet Veri Servisi hakkında sık sorulan bazı soruların (SSS) yanıtları verilmektedir.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 3cd4e69481fb452391e6dc027cb41fd6dae71b7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760258"
---
# <a name="azure-service-bus---frequently-asked-questions-faq"></a>Azure Servis Veri Servisi - Sık sorulan sorular (SSS)

Bu makalede, Microsoft Azure Hizmet Veri Servisi hakkında sık sorulan bazı sorular anlatılmaktadır. Ayrıca, genel Azure fiyatlandırması ve destek bilgileri için [Azure Destek SSS'lerini](https://azure.microsoft.com/support/faq/) de ziyaret edebilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-questions-about-azure-service-bus"></a>Azure Servis Veri Servisi hakkında genel sorular
### <a name="what-is-azure-service-bus"></a>Azure Service Bus nedir?
[Azure Hizmet Veri Servisi,](service-bus-messaging-overview.md) ayrılmış sistemler arasında veri göndermenize olanak tanıyan eşzamanlı bir ileti bulutu platformudur. Microsoft bu özelliği bir hizmet olarak sunar, bu da kullanmak için kendi donanımınızı barındırmanız gerekmediğiniz anlamına gelir.

### <a name="what-is-a-service-bus-namespace"></a>Service Bus ad alanı nedir?
[Ad alanı,](service-bus-create-namespace-portal.md) uygulamanızdaki Servis Veri Gönderi kaynaklarını ele almak için bir kapsam kapsayıcısı sağlar. Hizmet Veri Servisi'ni kullanmak için bir ad alanı oluşturmak gereklidir ve başlamanın ilk adımlarından biridir.

### <a name="what-is-an-azure-service-bus-queue"></a>Azure Servis Veri Servisi kuyruğu nedir?
[Hizmet Veri Servisi kuyruğu,](service-bus-queues-topics-subscriptions.md) iletilerin depolandığı bir varlıktır. Kuyruklar, birden çok uygulamanız veya dağıtılmış bir uygulamanın birbiriyle iletişim kurması gereken birden çok bölümünüz olduğunda yararlıdır. Sıra, birden çok ürünün (iletilerin) alındığı ve bu konumdan gönderildiği bir dağıtım merkezine benzer.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Azure Hizmet Veri Servisi konuları ve abonelikleri nelerdir?
Bir konu sıra olarak görselleştirilebilir ve birden çok abonelik kullanırken daha zengin bir ileti modeli haline gelir; aslında bir-to-çok iletişim aracı. Bu yayımla/abone olun modeli (veya *pub/alt),* birden çok aboneliği olan bir konuya ileti gönderen bir uygulamanın bu iletiyi birden çok uygulama tarafından alınmasını sağlar.

### <a name="what-is-a-partitioned-entity"></a>Bölümlenmiş varlık nedir?
Geleneksel bir sıra veya konu tek bir ileti aracısı tarafından işlenir ve tek bir ileti deposunda depolanır. Yalnızca Temel ve Standart ileti katmanlarında desteklenen [bölümlenmiş bir sıra veya konu,](service-bus-partitioning.md) birden çok ileti aracısı tarafından işlenir ve birden çok ileti mağazasında depolanır. Bu özellik, bölümlenmiş bir sıranın veya konunun genel iş bürünün artık tek bir ileti aracıcısının veya ileti deposunun performansıyla sınırlı olmadığı anlamına gelir. Ayrıca, bir ileti deposunun geçici kesintisi, bölümlenmiş bir sırayı veya konuyu kullanılamaz hale getirmez.

Bölümlenmiş varlıklar kullanırken sıralama sağlanmaz. Bir bölümün kullanılamaması durumunda, yine de diğer bölümlerden ileti gönderip alabilirsiniz.

 Bölümlenmiş varlıklar artık [Premium SKU'da](service-bus-premium-messaging.md)desteklenmez. 

### <a name="what-ports-do-i-need-to-open-on-the-firewall"></a>Güvenlik duvarında açmak için hangi bağlantı noktalarına ihtiyacım var? 
İleti göndermek ve almak için Azure Hizmet Veri Servisi ile aşağıdaki protokolleri kullanabilirsiniz:

- Gelişmiş İleti Sıraya Alma Protokolü (AMQP)
- Servis Veri Servisi Mesajlaşma Protokolü (SBMP)
- HTTP

Azure Etkinlik Hub'ları ile iletişim kurmak için bu protokolleri kullanmak için açmanız gereken giden bağlantı noktaları için aşağıdaki tabloya bakın. 

| Protokol | Bağlantı Noktaları | Ayrıntılar | 
| -------- | ----- | ------- | 
| AMQP | 5671 ve 5672 | [Bkz. AMQP protokol kılavuzu](service-bus-amqp-protocol-guide.md) | 
| SBMP | 9350 ile 9354 arası | Bkz. [Bağlantı modu](/dotnet/api/microsoft.servicebus.connectivitymode?view=azure-dotnet) |
| HTTP, HTTPS | 80, 443 | 

### <a name="what-ip-addresses-do-i-need-to-whitelist"></a>Beyaz listelemek için hangi IP adreslerine ihtiyacım var?
Bağlantılarınız için beyaz listeye doğru IP adreslerini bulmak için aşağıdaki adımları izleyin:

1. Komut isteminden aşağıdaki komutu çalıştırın: 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. Döndürülen IP adresini `Non-authoritative answer`not edin. Bu IP adresi statiktir. Zaman içinde değişeceği tek nokta, ad alanını farklı bir kümeye geri yüklemenizdir.

Ad alanınız için bölge artıklığını kullanıyorsanız, birkaç ek adım yapmanız gerekir: 

1. İlk olarak, ad alanında nslookup çalıştırın.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. Aşağıdaki biçimlerden birinde bulunan **yetkili olmayan yanıt** bölümündeki adı not edin: 

    ```
    <name>-s1.servicebus.windows.net
    <name>-s2.servicebus.windows.net
    <name>-s3.servicebus.windows.net
    ```
3. S1, s2 ve s3 ekleri olan her biri için nslookup çalıştırarak üç kullanılabilirlik bölgesinde çalışan her üç örneğin IP adreslerini alın, 


## <a name="best-practices"></a>En iyi uygulamalar
### <a name="what-are-some-azure-service-bus-best-practices"></a>Bazı Azure Hizmet Veri Mes'leri en iyi uygulamaları nelerdir?
[Hizmet Veri Servisi'ni kullanarak performans iyileştirmeleri için en iyi uygulamalara][Best practices for performance improvements using Service Bus] bakın – bu makalede, ileti alışverişinde performansı en iyi duruma getirme nin nasıl olduğu açıklanmaktadır.

### <a name="what-should-i-know-before-creating-entities"></a>Varlıkları oluşturmadan önce ne leri bilmem gerekir?
Sıranın ve konunun aşağıdaki özellikleri değişmezdir. Bu özellikler yeni bir yedek varlık oluşturmadan değiştirilemeyeceğiiçin, varlıklarınızı sağlarken bu sınırlamayı göz önünde bulundurun.

* Bölümleme
* Oturumlar
* Yineleme algılama
* Ekspres varlık

## <a name="pricing"></a>Fiyatlandırma
Bu bölümde, Servis Veri Servisi fiyatlandırma yapısı hakkında sık sorulan bazı soruları yanıtlar.

[Servis Otobüsü fiyatlandırma ve faturalandırma](https://azure.microsoft.com/pricing/details/service-bus/) maddesi, Servis Otobüsü'ndeki faturalandırma sayaçlarını açıklar. Servis Veri Otobüsü fiyatlandırma seçenekleri hakkında özel bilgi için [Servis Otobüsü fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/service-bus/)bakın.

Genel Azure fiyatlandırma bilgileri için [Azure Destek SSS'lerini](https://azure.microsoft.com/support/faq/) de ziyaret edebilirsiniz. 

### <a name="how-do-you-charge-for-service-bus"></a>Servis Otobüsü için nasıl ücret alabilirsiniz?
Servis Otobüsü fiyatlandırması hakkında tam bilgi için [Servis Otobüsü fiyatlandırma ayrıntılarına][Pricing overview]bakın. Belirtilen fiyatlara ek olarak, başvurunuzun sağlandığı veri merkezinin dışındaki çıkışlar için ilişkili veri aktarımları için ücretlendirilirsiniz.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Servis Veri Servisi'nin hangi kullanımı veri aktarımının konusudur? Ne değil ki?
Belirli bir Azure bölgesindeki tüm veri aktarımları ve gelen veri aktarımları ücretsiz olarak sağlanır. Bir bölge dışında veri [aktarımı, burada](https://azure.microsoft.com/pricing/details/bandwidth/)bulunan çıkış ücretlerine tabidir.

### <a name="does-service-bus-charge-for-storage"></a>Servis Otobüsü depolama için ücret lendirmiyor mu?
Hayır, Servis Otobüsü depolama için ücret almaz. Ancak, sıra/konu başına kalıcı olabilecek maksimum veri miktarını sınırlayan bir kota vardır. Bir sonraki SSS'ye bakın.

### <a name="i-have-a-service-bus-standard-namespace-why-do-i-see-charges-under-resource-group-system"></a>Service Bus Standard ad alanım var. Neden kaynak grubu '$system' altında ücret görüyorum?
Azure Hizmet Veri Servisi yakın zamanda faturalandırma bileşenlerini yükseltti. Bu nedenle, Bir Hizmet Veri Mes'ü Ad alanınız varsa, kaynak grubu '$system' altında kaynak '/abonelikler/<azure_subscription_id>/resourceGroups/$system/providers/Microsoft.ServiceBus/namespaces/$system' için satır öğeleri görebilirsiniz.

Bu ücretler, Hizmet Veri Hizmeti Veri Mes'ü Standart ad alanı sağlayan Azure aboneliği başına temel ücreti temsil eder. 

Bunların yeni ücretler olmadığını, yani önceki faturalandırma modelinde de var olduklarını belirtmek önemlidir. Tek değişiklik, artık '$system' altında listelenmiş olmasıdır. Bu, '$system' kaynak kimliği altında belirli bir kaynağa bağlı olmayan abonelik düzeyi ücretlerini gruplandıran yeni faturalandırma sistemindeki ihlaller nedeniyle yapılır.

## <a name="quotas"></a>Kotalar

Servis Veri Günü limitleri ve kotaları listesi [için, Servis Veri Otobüsü kotalarına genel bakış][Quotas overview]bölümüne bakın.

### <a name="does-service-bus-have-any-usage-quotas"></a>Servis Veritobu'nun herhangi bir kullanım kotası var mı?
Varsayılan olarak, herhangi bir bulut hizmeti için Microsoft, müşterinin tüm abonelikleri arasında hesaplanan toplam aylık kullanım kotası ayarlar. Bu sınırlardan daha fazlasına ihtiyacınız varsa, ihtiyaçlarınızı anlamak ve bu sınırları uygun şekilde ayarlamak için istediğiniz zaman müşteri hizmetleriyle iletişime geçebilirsiniz. Servis Veri Servisi için toplam kullanım kotası ayda 5 milyar mesajdır.

Microsoft, belirli bir ay içinde kullanım kotalarını aşan bir müşteri hesabını devre dışı bırakma hakkını saklı tutarken, e-posta bildirimleri gönderilir ve herhangi bir işlem yapmadan önce bir müşteriyle iletişim kurmak için birden çok girişimde bulunulr. Bu kotaları aşan müşteriler, kotaları aşan ücretlerden hala sorumludur.

Azure'daki diğer hizmetlerde olduğu gibi, Hizmet Veri Servisi kaynakların adil kullanımı olduğundan emin olmak için bir dizi özel kota uygular. Bu kotalar hakkında daha fazla detaya [Servis Veri Servisi kotalarına genel bakışta][Quotas overview]bulabilirsiniz.

### <a name="how-to-handle-messages-of-size--1-mb"></a>Boyutu > 1 MB iletileri nasıl işleyebilir?
Servis Veri Servisi mesajlaşma hizmetleri (kuyruklar ve konular/abonelikler), uygulamanın 256 KB (standart katman) veya 1 MB (premium katman) boyutuna kadar ileti göndermesine olanak sağlar. 1 MB'dan büyük boyutlarda iletilerle uğraşıyorsanız, bu blog [gönderisinde](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern)açıklanan talep denetimi modelini kullanın.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>Neden başka bir abonelikten silen bir ad alanı oluşturamıyorum? 
Bir ad alanını abonelikten sildiğinizde, başka bir abonelikte aynı adla yeniden oluşturmadan önce 4 saat bekleyin. Aksi takdirde, aşağıdaki hata iletisi alabilirsiniz: `Namespace already exists`. 

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Azure Hizmet Veri Servisi API'leri tarafından oluşturulan özel durumlar ve önerilen eylemler nelerdir?
Olası Servis Veri Servisi özel durumlarının listesi için özel [durumlara genel bakış][Exceptions overview]bölümüne bakın.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Paylaşılan Erişim İmzası nedir ve hangi diller imza oluşturmayı destekler?
Paylaşılan Erişim İmzaları, SHA-256 güvenli iş birliğini veya URI'leri temel alan bir kimlik doğrulama mekanizmasıdır. Düğüm.js, PHP, Java, Python ve C#'da kendi imzalarınızı nasıl oluşturacağınızı öğrenmek için [Paylaşılan Erişim İmzaları][Shared Access Signatures] makalesine bakın.

## <a name="subscription-and-namespace-management"></a>Abonelik ve ad alanı yönetimi
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Ad alanını başka bir Azure aboneliğine nasıl geçirebilirim?

[Azure portalını](https://portal.azure.com) veya PowerShell komutlarını kullanarak bir ad alanını bir Azure aboneliğinden diğerine taşıyabilirsiniz. İşlemi yürütmek için ad alanının zaten etkin olması gerekir. Komutları yürüten kullanıcı, hem kaynak hem de hedef aboneliklerde yönetici olmalıdır.

#### <a name="portal"></a>Portal

Hizmet Veri Yolundan ad alanlarını başka bir aboneye geçirmek için Azure portalını kullanmak için [buradaki](../azure-resource-manager/management/move-resource-group-and-subscription.md#use-the-portal)yönergeleri izleyin. 

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell komutları dizisi bir ad alanını bir Azure aboneliğinden diğerine taşır. Bu işlemi yürütmek için ad alanının zaten etkin olması ve PowerShell komutlarını çalıştıran kullanıcının hem kaynak hem de hedef aboneliklerde yönetici olması gerekir.

```powershell
# Create a new resource group in target subscription
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Sonraki adımlar
Servis Veri Yolu hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Hizmet Veri Otobüsü Premium Tanıtımı (blog gönderisi)](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Hizmet Veri Otobüsü Premium(Kanal9) Tanıtımı](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Servis Veri Servisi'ne genel bakış](service-bus-messaging-overview.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
