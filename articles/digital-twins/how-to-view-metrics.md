---
title: Azure Izleyici ile ölçümleri görüntüleme
titleSuffix: Azure Digital Twins
description: Bkz. Azure Izleyici 'de Azure dijital TWINS ölçümlerini görüntüleme.
author: baanders
ms.author: baanders
ms.date: 7/24/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: c6db571d64b0fd276519f15a3984848e80c4e18a
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87387685"
---
# <a name="view-and-understand-azure-digital-twins-metrics"></a>Azure dijital TWINS ölçümlerini görüntüleyin ve anlayın

Bu ölçümler, Azure aboneliğinizdeki Azure dijital TWINS kaynaklarının durumu hakkında bilgi verir. Azure dijital TWINS ölçümleri, Azure dijital TWINS hizmetinin genel sistem durumunu ve ona bağlı kaynakları değerlendirmenize yardımcı olur. Bu kullanıcıya yönelik istatistikler, Azure dijital Tağınızda neler olduğunu görmenizi ve Azure desteği ile iletişim kurmaya gerek kalmadan sorunlar için kök neden analizi gerçekleştirmeye yardımcı olur.

Ölçümler varsayılan olarak etkindir. [Azure Portal](https://portal.azure.com)Azure dijital TWINS ölçümlerini görüntüleyebilirsiniz.

## <a name="how-to-view-azure-digital-twins-metrics"></a>Azure dijital TWINS ölçümlerini görüntüleme

1. Azure dijital TWINS örneği oluşturun. [*Nasıl yapılır: bir örneği ve kimlik doğrulamasını ayarlama*](how-to-set-up-instance-scripted.md)konusunda bir Azure dijital TWINS örneği ayarlama hakkında yönergeler bulabilirsiniz.

2. [Azure Portal](https:/portal.azure.com) Azure dijital TWINS örneğinizi bulun (adını Portal arama çubuğuna yazarak bu sayfayı açabilirsiniz). 

    Örneğin menüsünden **ölçümler**' i seçin.
   
    :::image type="content" source="media/how-to-view-metrics/azure-digital-twins-metrics.png" alt-text="Azure dijital TWINS için ölçüm sayfasını gösteren ekran görüntüsü":::

    Bu sayfa, Azure dijital TWINS örneğiniz için ölçümleri görüntüler. Ayrıca, listeden görmek istediklerinizi seçerek ölçümlerinizin özel görünümlerini de oluşturabilirsiniz.
    
3. Menüdeki **Tanılama ayarları** ' na ve ardından **Tanılama ayarı Ekle**' ye tıklayarak ölçüm verilerinizi bir Event Hubs uç noktasına veya bir Azure depolama hesabına göndermenizi seçebilirsiniz.

    :::image type="content" source="media/how-to-view-metrics/diagnostic-settings.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü":::

    Bu işlem hakkında daha fazla bilgi için bkz. [*sorun giderme: tanılamayı ayarlama*](troubleshoot-diagnostics.md).

## <a name="azure-digital-twins-metrics-and-how-to-use-them"></a>Azure dijital TWINS ölçümleri ve bunların nasıl kullanılacağı

Azure dijital TWINS, örneğinizin sistem durumuna ve ilişkili kaynaklarına ilişkin bir genel bakış sunmak için çeşitli ölçümler sunar. Ayrıca, örneğinizin durumunun büyük bir resmini boyamak için birden fazla ölçüden bilgi birleştirebilirsiniz. 

Aşağıdaki tablolarda, her bir Azure dijital TWINS örneği tarafından izlenen ölçümler ve her ölçümün örneğinizin genel durumuyla nasıl ilişkili olduğu açıklanır.

#### <a name="api-request-metrics"></a>API isteği ölçümleri

API istekleriyle yapılacak ölçümler:

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü| Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| ApiRequests | API Istekleri (Önizleme) | Count | Toplam | Dijital TWINS okuma, yazma, silme ve sorgu işlemleri için yapılan API Isteklerinin sayısı. |  Kimlik Doğrulaması </br>İşlem </br>Protokol </br>Durum Kodu </br>Durum kodu sınıfı </br>Durum metni |
| Apirequestslatlik | API Istekleri gecikmesi (Önizleme) | Mayacak | Ortalama | API istekleri için yanıt süresi. Bu, hizmet, dijital TWINS okuma, yazma, silme ve sorgu işlemleri için başarılı/başarısız sonucu gönderene kadar Azure Digital TWINS tarafından isteğin alındığı zamana başvurur. | Kimlik Doğrulaması </br>İşlem </br>Protokol |
| ApiRequestsFailureRate | API Isteği başarısızlık oranı (Önizleme) | Yüzde | Ortalama | Dijital TWINS okuma, yazma, silme ve sorgu işlemleri için bir iç hata (500) yanıt kodu veren örneğiniz için hizmetin aldığı API isteklerinin yüzdesi. | Kimlik Doğrulaması </br>İşlem </br>Protokol </br>Durum Kodu </br>Durum kodu sınıfı </br>Durum metni

#### <a name="routing-metrics"></a>Yönlendirme ölçümleri

Yönlendirme ile yapmak zorunda olan ölçümler:

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü| Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| Yönlendirme | Yönlendirme (Önizleme) | Count | Toplam | Olay Hub 'ı, Service Bus veya Event Grid gibi bir uç nokta Azure hizmetine yönlendirilen ileti sayısı. | Çalışma </br>Sonuç |
| RoutingLatency | Yönlendirme gecikmesi (Önizleme) | Mayacak | Ortalama | Azure dijital TWINS 'den, Olay Hub 'ı, Service Bus veya Event Grid gibi bir uç nokta Azure hizmetine gönderildiğinde bir olay arasında geçen süre. | Çalışma </br>Sonuç |
| RoutingFailureRate | Yönlendirme hatası oranı (Önizleme) | Yüzde | Ortalama | Azure dijital TWINS 'den, Olay Hub 'ı, Service Bus veya Event Grid gibi bir uç nokta Azure hizmetine yönlendirildikleri için hataya neden olan olayların yüzdesi. | Çalışma </br>Sonuç |

#### <a name="billing-metrics"></a>Faturalandırma ölçümleri

Faturalandırma ile yapılacak ölçümler:

>[!NOTE]
> Önizleme süresince **faturalandırma sıfır maliyetlidir**. Bu ölçümler seçilebilir listede görüntülenmeye devam ederken, önizleme sırasında uygulanmaz ve hizmet Önizleme dışına çıkana kadar sıfır olarak kalır.

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü| Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| BillingApiOperations | Faturalama API 'SI Işlemleri (Önizleme) | Count | Toplam | Azure Digital TWINS hizmetinde yapılan tüm API isteklerinin sayısı için faturalandırma ölçümü. | `MeterId` |
| BillingQueryUnits | Faturalama sorgu birimleri (Önizleme) | Count | Toplam | Sorgu birimlerinin sayısı, sorgu yürütmek için tüketilen, hizmet kaynağı kullanımının dahili olarak hesaplanan bir ölçümüdür. Sorgu birimlerini ölçmek için bir yardımcı API de mevcuttur: [Querychargehelper sınıfı](https://docs.microsoft.com/dotnet/api/azure.digitaltwins.core.querychargehelper?view=azure-dotnet-preview) | `MeterId` |
| Billingiletide Işlendi | Işlenen faturalama Iletileri (Önizleme) | Count | Toplam | Azure dijital TWINS 'den dış uç noktalara gönderilen ileti sayısı için faturalandırma ölçümü. | `MeterId` |

## <a name="dimensions"></a>Boyutlar

Boyutlar ölçümler hakkında daha fazla ayrıntı belirlemenize yardımcı olur. Yönlendirme ölçümlerinden bazıları uç nokta başına bilgi sağlar. Aşağıdaki tabloda bu boyutlar için olası değerler listelenmiştir.

| Boyut | Değerler |
| --- | --- |
| Kimlik Doğrulaması | OAuth |
| İşlem (API Istekleri için) | Microsoft. DigitalTwins/digitaltwıns/Delete</br>Microsoft. DigitalTwins/digitaltwıns/Write</br>Microsoft. DigitalTwins/digitaltwıns/okuma </br>Microsoft. DigitalTwins/eventroutes/okuma </br>Microsoft. DigitalTwins/eventroutes/Write </br>Microsoft. DigitalTwins/eventrotalar/Sil </br>Microsoft. DigitalTwins/modeller/okuma </br>Microsoft. DigitalTwins/modeller/yazma </br>Microsoft. DigitalTwins/modeller/silme </br>Microsoft. DigitalTwins/sorgu/eylem |
İşlem (yönlendirme için) | Event Grid </br>Olay Hub'ı </br>Service Bus |
| Protokol | HTTPS |
| Sonuç | Başarılı </br>Hata |
| Durum Kodu | 200, 404, 500 vb. |
| Durum kodu sınıfı | 2xx, 4xx, 5xx vb. |
| Durum metni | İç sunucu hatası, bulunamadı, vb. |

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS için kaydedilmiş ölçümleri yönetme hakkında daha fazla bilgi için bkz. [*sorun giderme: tanılamayı ayarlama*](troubleshoot-diagnostics.md).

Ya da Azure dijital TWINS ölçümlerine genel bir bakış gördüğünüze göre, Azure dijital TWINS 'i yönetme hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [*Nasıl yapılır: Azure dijital TWINS API 'Leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md)
* [*Nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md)
* [*Nasıl yapılır: dijital TWINS 'i yönetme*](how-to-manage-twin.md)
* [*Nasıl yapılır: ikizi grafiğini ilişkilerle yönetme*](how-to-manage-graph.md)
* [*Nasıl yapılır: uç noktaları ve yolları yönetme*](how-to-manage-routes.md)