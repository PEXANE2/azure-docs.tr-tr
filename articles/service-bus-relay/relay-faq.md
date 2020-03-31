---
title: Azure Röle SSS | Microsoft Dokümanlar
description: Bu makalede, Azure Röle hizmeti hakkında sık sorulan bazı soruların yanıtları verilmektedir.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: d5032b427316a3c4e07013af4e8214e239a6efb3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514009"
---
# <a name="azure-relay-faqs"></a>Azure Röle SSS'leri

Bu makalede, [Azure Röle](https://azure.microsoft.com/services/service-bus/)hakkında sık sorulan bazı soruları (SSS) yanıtlar. Genel Azure fiyatlandırma ve destek bilgileri için [Azure Destek SSS'lerine](https://azure.microsoft.com/support/faq/)bakın.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-relay"></a>Azure Geçiş nedir?
[Azure Röle hizmeti,](relay-what-is-it.md) kurumsal bir kuruluş ağında bulunan hizmetleri genel buluta daha güvenli bir şekilde maruz bırakmanıza yardımcı olarak karma uygulamalarınızı kolaylaştırır. Hizmetleri güvenlik duvarı bağlantısı açmadan ve kurumsal ağ altyapısında müdahaleci değişikliklergerektirmeden ortaya çıkarabilirsiniz.

### <a name="what-is-a-relay-namespace"></a>Röle ad alanı nedir?
[Ad alanı,](relay-create-namespace-portal.md) uygulamanızdaki Röle kaynaklarını ele almak için kullanabileceğiniz bir kapsam kapsayıcısır. Relay'i kullanmak için bir ad alanı oluşturmanız gerekir. Bu başlamak için ilk adımlardan biridir.

### <a name="what-happened-to-service-bus-relay-service"></a>Servis Otobüsü Röle hizmetine ne oldu?
Daha önce adlandırılmış Servis Veri Günü hizmeti artık [WCF Röle](service-bus-relay-tutorial.md)olarak adlandırılır. Bu hizmeti her zamanki gibi kullanmaya devam edebilirsiniz. Karma Bağlantılar özelliği, Azure BizTalk Hizmetleri'nden nakledilen bir hizmetin güncelleştirilmiş bir sürümüdür. WCF Röle ve Hibrit Bağlantılar desteklenmeye devam etmektedir.

## <a name="pricing"></a>Fiyatlandırma
Bu bölümde, Röle fiyatlandırma yapısı hakkında sık sorulan bazı soruları yanıtlar. Ayrıca, genel Azure fiyatlandırma bilgileri için [Azure Destek SSS'lerini](https://azure.microsoft.com/support/faq/) de görebilirsiniz. Röle fiyatlandırması hakkında tam bilgi için [Servis Otobüsü fiyatlandırma ayrıntılarına][Pricing overview]bakın.

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Hibrit Bağlantılar ve WCF Röle için nasıl ücret alabilirsiniz?
Röle fiyatlandırması hakkında tam bilgi için, Servis Veri Kurumu fiyatlandırma ayrıntıları sayfasındaki [Karma Bağlantılar ve WCF Röleler][Pricing overview] tablosuna bakın. Söz konusu sayfada belirtilen fiyatlara ek olarak, başvurunuzun sağlandığı veri merkezinin dışındaki çıkışlar için ilişkili veri aktarımları için ücretlendirilirsiniz.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Hibrit Bağlantılar için nasıl faturalandırılırım?
Karma Bağlantılar için üç örnek faturalandırma senaryosu aşağıda verilmiştir:

*   Senaryo 1:
    *   Hibrit Bağlantılar Yöneticisi'nin yüklü ve tüm ay boyunca sürekli olarak çalışan bir örneği gibi tek bir dinleyiciniz vardır.
    *   Ay boyunca bağlantı üzerinden 3 GB veri gönderirsiniz. 
    *   Toplam ücretin 5 dolar.
*   Senaryo 2:
    *   Hibrit Bağlantılar Yöneticisi'nin yüklü ve tüm ay boyunca sürekli olarak çalışan bir örneği gibi tek bir dinleyiciniz vardır.
    *   Ay boyunca bağlantı üzerinden 10 GB veri gönderirsiniz.
    *   Toplam ücretin 7.50 dolar. Bu bağlantı için 5 $ ve ilk 5 GB + 2,50 $ veri ek 5 GB için' s.
*   Senaryo 3:
    *   Hibrit Bağlantılar Yöneticisi'nin yüklü ve tüm ay boyunca sürekli olarak çalıştırılan Iki örneği vardır: A ve B.
    *   Ay boyunca A bağlantısı üzerinden 3 GB veri gönderirsiniz.
    *   Ay boyunca B bağlantısı üzerinden 6 GB veri gönderirsiniz.
    *   Toplam ücretin 10.50 dolar. Bu bağlantı için 5 $ A + 5 $ bağlantı B + $ 0,50 (bağlantı B altıncı gigabayt için) için.

Örneklerde kullanılan fiyatların yalnızca Karma Bağlantılar önizleme döneminde geçerli olduğunu unutmayın. Fiyatlar, Hibrit Bağlantıların genel kullanılabilirliğine bağlı olarak değişebilir.

### <a name="how-are-hours-calculated-for-relay"></a>Röle için saatler nasıl hesaplanır?

WCF Relay yalnızca Standart katman ad alanlarında kullanılabilir. Aksi takdirde röleler için fiyatlandırma ve [bağlantı kotaları](../service-bus-messaging/service-bus-quotas.md) değişmedi. Bu, rölelerin ileti sayısına (işlem değil) ve röle saatlerine bağlı olarak ücretlendirilmeye devam ettiği anlamına gelir. Daha fazla bilgi için fiyatlandırma ayrıntıları sayfasındaki ["Karma Bağlantılar ve WCF Röleleri"](https://azure.microsoft.com/pricing/details/service-bus/) tablosuna bakın.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-specific-relay"></a>Belirli bir röleye bağlı birden fazla dinleyicim varsa ne olur?
Bazı durumlarda, tek bir röle birden çok bağlı dinleyiciye sahip olabilir. En az bir röle dinleyicisi bağlandığında bir röle açık olarak kabul edilir. Açık röleye dinleyici eklemek ek röle saatlerine neden olur. Bir röleye bağlı olan röle gönderenlerin (rölelere ileti çağıran veya gönderen istemciler) sayısı, röle saatlerinin hesaplanmasını etkilemez.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>WCF Röleleri için mesaj ölçer nasıl hesaplanır?
(**Bu yalnızca WCF röleleri için geçerlidir. İletiler Karma Bağlantılar için bir maliyet değildir.**)

Genel olarak, geçişler için faturalandırılabilir iletiler, aracılı varlıklar (kuyruklar, konular ve abonelikler) için daha önce açıklanan aynı yöntem kullanılarak hesaplanır. Ancak, bazı önemli farklılıklar vardır.

Servis Veri Servisi rölesine ileti göndermek, iletiyi alan röle dinleyicisine "tam yol" gönderiliyor olarak kabul edilir. Servis Veri Servisi rölesi için bir gönderme işlemi olarak kabul edilmez ve ardından röle dinleyicisine bir teslimat yapılır. Bir röle dinleyicisine karşı bir istek-yanıt stili hizmet çağrısı (64 KB'ye kadar) iki faturalandırılabilir iletiyle sonuçlanır: istek için bir faturalandırılabilir ileti ve yanıt için bir faturalandırılabilir ileti (yanıtın da 64 KB veya daha küçük olduğunu varsayarsak). Bu, istemci ve hizmet arasında aracılık etmek için bir sıra kullanmaktan farklıdır. İstemci ve hizmet arasında aracılık etmek için bir kuyruk kullanırsanız, aynı istek-yanıt deseni sıraya bir istek gönderilmesini ve ardından sıradan hizmete bir dequeue/teslim gönderilmesini gerektirir. Bunu, başka bir kuyruğa bir yanıt gönderme ve bu sıradan istemciye bir dequeue/teslim takip eder. (64 KB'ye kadar) boyunca aynı boyuttaki varsayımları kullanarak, aracılı sıra deseni 4 faturalandırılabilir iletiyle sonuçlanır. Röle kullanarak gerçekleştirdiğiniz aynı deseni uygulamak için ileti sayısının iki katı için faturalandırılırsınız. Tabii ki, dayanıklılık ve yük tesviye gibi bu desen, elde etmek için kuyrukları kullanmanın yararları vardır. Bu avantajlar ek gideri haklı çıkarabilir.

**NetTCPRelay** WCF bağlama kullanılarak açılan röleler iletileri tek tek iletiler olarak değil, sistemde akan bir veri akışı olarak ele adatır. Bu bağlamayı kullandığınızda, yalnızca gönderen ve dinleyici, gönderilen ve alınan tek tek iletilerin çerçevelenmesinde görünürlüğe sahiptir. **netTCPRelay** bağlama kullanan röleler için, tüm veriler faturalandırılabilir iletileri hesaplamak için bir akış olarak kabul edilir. Bu durumda, Servis Veri Servisi, her bir röle üzerinden gönderilen veya alınan toplam veri miktarını 5 dakikalık olarak hesaplar. Daha sonra, bu dönem boyunca bu röle için faturalandırılabilir iletisayısını belirlemek için bu toplam veri miktarını 64 KB'ye böler.

## <a name="quotas"></a>Kotalar
| Kota adı | Kapsam |  Notlar | Değer |
| --- | --- | --- | --- |
| Röle üzerinde eşzamanlı dinleyiciler |Varlık |Sonraki ek bağlantılar için istekler reddedilir ve arama kodu tarafından bir özel durum alınır. |25 |
| Bir hizmet ad alanında tüm röle uç noktaları başına eşzamanlı röle bağlantıları |Ad Alanı |- |5.000 |
| Hizmet ad alanı başına uç noktaları röle |Ad Alanı |- |10,000 |
| [NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) ve [NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) röleleri için mesaj boyutu |Ad Alanı |Bu kotaları aşan gelen iletiler reddedilir ve arama kodu tarafından bir özel durum alınır. |64 KB |
| [HttpRelayTransportBindingElement](/dotnet/api/microsoft.servicebus.httprelaytransportbindingelement) ve [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) röleleri için mesaj boyutu |Ad Alanı |İleti boyutunda sınır yok. |Sınırsız |

### <a name="does-relay-have-any-usage-quotas"></a>Röle'nin herhangi bir kullanım kotası var mı?
Varsayılan olarak, herhangi bir bulut hizmeti için Microsoft, müşterinin tüm abonelikleri arasında hesaplanan toplam aylık kullanım kotası ayarlar. Zaman zaman ihtiyaçlarınız bu sınırları aşabilir. İhtiyaçlarınızı anlayabilmemiz ve bu limitleri uygun şekilde ayarlayabilmemiz için müşteri hizmetleriyle istediğiniz zaman iletişim kurabilirsiniz. Servis Veri Servisi için toplu kullanım kotaları aşağıdaki gibidir:

* 5 milyar mesaj
* 2 milyon röle saati

Aylık kullanım kotalarını aşan bir hesabı devre dışı bırakma hakkımızı saklı tutsak da, e-posta bildirimi sağlarız ve herhangi bir işlem yapmadan önce müşteriyle iletişim kurmak için birden çok girişimde bulunuz. Bu kotaları aşan müşteriler yine de fazla ücretlerden sorumludur.

### <a name="naming-restrictions"></a>Adlandırma kısıtlamaları
Röle ad alanı adı 6 ile 50 karakter uzunluğunda olmalıdır.

## <a name="subscription-and-namespace-management"></a>Abonelik ve ad alanı yönetimi
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Ad alanını başka bir Azure aboneliğine nasıl geçirebilirim?

Bir ad alanını bir Azure aboneliğinden başka bir aboneye taşımak için [Azure portalını](https://portal.azure.com) kullanabilir veya PowerShell komutlarını kullanabilirsiniz. Ad alanını başka bir aboneye taşımak için ad alanının zaten etkin olması gerekir. Komutları çalıştıran kullanıcı, hem kaynak hem de hedef aboneliklerde bir Yönetici kullanıcısı olmalıdır.

#### <a name="azure-portal"></a>Azure portalında

Azure Röle ad alanlarını bir abonelikten diğerine geçirmek için Azure portalını kullanmak [için bkz.](../azure-resource-manager/management/move-resource-group-and-subscription.md#use-the-portal) 

#### <a name="powershell"></a>PowerShell

Bir ad alanını bir Azure aboneliğinden başka bir aboneye taşımak için PowerShell'i kullanmak için aşağıdaki komut sırasını kullanın. Bu işlemi yürütmek için ad alanının zaten etkin olması ve PowerShell komutlarını çalıştıran kullanıcının hem kaynak hem de hedef aboneliklerde bir Yönetici kullanıcısı olması gerekir.

```azurepowershell-interactive
# Create a new resource group in the target subscription.
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move the namespace from the source subscription to the target subscription.
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Sorun giderme
### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Azure Röle API'leri tarafından oluşturulan bazı özel durumlar ve yapabileceğiniz eylemler nelerdir?
Sık karşılaşılan özel durumlar ve yapabileceğiniz eylemlerin açıklaması için [Röle özel durumlara][Relay exceptions]bakın.

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-to-generate-a-signature"></a>Paylaşılan erişim imzası nedir ve imza oluşturmak için hangi dilleri kullanabilirim?
Paylaşılan Erişim İmzaları (SAS), SHA-256 güvenli iş birliğini veya URI'leri temel alan bir kimlik doğrulama mekanizmasıdır. Düğüm.js, PHP, Python, Java, C ve C#'da kendi imzalarınızı nasıl oluşturacağınızla ilgili bilgi [için, paylaşılan erişim imzaları içeren Hizmet Veri Veri Kurumu kimlik doğrulaması][Shared Access Signatures]bölümüne bakın.

### <a name="is-it-possible-to-whitelist-relay-endpoints"></a>Uç noktaları beyaz listeye almak mümkün mü?
Evet. Röle istemcisi, tam nitelikli alan adları kullanarak Azure Röle hizmetine bağlantı sağlar. Müşteriler, DNS `*.servicebus.windows.net` beyaz listesini destekleyen güvenlik duvarlarına giriş ekleyebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared Access Signatures]: ../service-bus-messaging/service-bus-sas.md
