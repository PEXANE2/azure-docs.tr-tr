---
title: Azure Röle özel durumları ve bunları nasıl çözeceğiniz | Microsoft Dokümanlar
description: Azure Röle özel durumları ve bunları çözmeye yardımcı olmak için yapabileceğiniz önerilen eylemler listesi.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: spelluru
ms.openlocfilehash: fe8f057443b978e70e7cdd2591affd455fefdca8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60749045"
---
# <a name="azure-relay-exceptions"></a>Azure Röle özel durumları

Bu makalede, Azure Röle API'leri tarafından oluşturulabilecek bazı özel durumlar listelenir. Bu başvuru değişebilir, bu nedenle güncelleştirmeleri kontrol edin.

## <a name="exception-categories"></a>Özel durum kategorileri

Röle API'leri, aşağıdaki kategorilere düşebilecek özel durumlar oluşturur. Ayrıca, özel durumları çözmeye yardımcı olmak için alabileceğiniz önerilen eylemler de listelenir.

*   **Kullanıcı kodlama hatası**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Genel eylem**: Devam etmeden önce kodu düzeltmeye çalışın.
*   **Kurulum/yapılandırma hatası**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Genel eylem**: Yapılandırmanızı gözden geçirin. Gerekirse yapılandırmayı değiştirin.
*   **Geçici özel durumlar**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Genel eylem**: İşlemi yeniden deneyin veya kullanıcıları bilgilendirin.
*   **Diğer istisnalar**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Genel eylem**: Özel durum türüne özgü. Aşağıdaki bölümdeki tabloya bakın. 

## <a name="exception-types"></a>Özel durum türleri

Aşağıdaki tabloda ileti özel durum türleri ve bunların nedenleri listelenir. Ayrıca, özel durumları çözmeye yardımcı olmak için yapabileceğiniz eylemleri de not alır.

| **Özel durum türü** | **Açıklama** | **Önerilen eylem** | **Otomatik veya anında yeniden deneme notu** |
| --- | --- | --- | --- |
| [Zaman aşımı](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu, [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout)tarafından denetlenen belirtilen süre içinde istenen işlemi yanıt vermedi. Sunucu istenen işlemi tamamlamış olabilir. Bu ağ veya diğer altyapı gecikmeleri nedeniyle olabilir. |Tutarlılık için sistem durumunu denetleyin ve gerekirse yeniden deneyin. [Bkz. TimeoutException](#timeoutexception). |Yeniden deneme bazı durumlarda yardımcı olabilir; koda yeniden deneme mantığı ekleyin. |
| [Geçersiz İşlem](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemine sunucu veya hizmet içinde izin verilmez. Ayrıntılar için özel durum iletisi bakın. |Kodu ve belgeleri kontrol edin. İstenen işlemin geçerli olduğundan emin olun. |Yeniden denemenin bir faydası olmaz. |
| [İşlem İptal Edildi](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapatılmış, iptal edilmiş veya elden çıkarılmış bir nesne üzerinde bir işlem çağırma girişiminde bulunulması. Nadir durumlarda, ortam işlemi zaten bertaraf edilir. |Kodu denetleyin ve elden çıkarılan bir nesne üzerinde işlem çağırmadığından emin olun. |Yeniden denemenin bir faydası olmaz. |
| [Yetkisiz Erişim](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesnesi bir belirteç elde edemedi, belirteç geçersiz veya belirteç işlemi gerçekleştirmek için gerekli iddiaları içermez. |Belirteç sağlayıcısının doğru değerlerle oluşturulduğundan emin olun. Access Control hizmetinin yapılandırmasını denetleyin. |Yeniden deneme bazı durumlarda yardımcı olabilir; koda yeniden deneme mantığı ekleyin. |
| [Bağımsız Değişken Özel Durumu](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Bağımsız değişken Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Bağımsız değişken kapsama alanı dışında](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Aşağıdakilerden biri veya birkaçı oluştu:<br />Yönteme verilen bir veya daha fazla bağımsız değişken geçersizdir.<br /> [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create'e](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) sağlanan URI, bir veya daha fazla yol kesimi içerir.<br />[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create'e](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) verilen URI şeması geçersizdir. <br />Özellik değeri 32 KB'den büyüktür. |Arama kodunu denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden denemenin bir faydası olmaz. |
| [Sunucu Meşgul](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Hizmet şu anda isteği işleyemez. |İstemci bir süre bekleyebilir ve işlemi yeniden deneyebilir. |İstemci belirli bir aralıktan sonra yeniden deneyebilir. Yeniden deneme farklı bir özel durumla sonuçlanırsa, bu özel durum la ilgili yeniden deneme davranışını denetleyin. |
| [Kota Aşıldı](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |İleti varlığı izin verilen maksimum boyutuna ulaştı. |Varlıktan veya alt sıralarından iletiler alarak varlıkta alan oluşturun. Bkz. [QuotaExceededException](#quotaexceededexception). |Bu arada iletiler kaldırıldıysa yeniden deneyin yardımcı olabilir. |
| [İleti Boyutu Aşıldı](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükü 256-KB sınırını aşıyor. 256-KB sınırının toplam ileti boyutu olduğunu unutmayın. Toplam ileti boyutu, sistem özelliklerini ve microsoft .NET genel merkezlerini içerebilir. |İleti yükünün boyutunu küçültün ve işlemi yeniden deneyin. |Yeniden denemenin bir faydası olmaz. |

## <a name="quotaexceededexception"></a>Quotaexceededexception

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

Röle için, bu özel durum [System.ServiceModel.QuotaExceededException'ı](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx)sarar, bu da bu bitiş noktası için en fazla dinleyici sayısının aşıldığını gösterir. Bu durum iletisinin **MaximumListenersPerEndpoint** değerinde gösterilir.

## <a name="timeoutexception"></a>Timeoutexception
[TimeoutException,](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlem zaman aşımından daha uzun sürdüğünü gösterir. 

[ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) özelliğinin değerini denetleyin. Bu sınıra ulaşmak, [Bir TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)neden olabilir.

Röle için, bir röle gönderen bağlantısını ilk açtığınızda zaman acısı özel durumları alabilirsiniz. Bu özel durum için iki ortak nedeni vardır:

*   [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) değeri çok küçük olabilir (saniyenin bir kısmı yla bile olsa).
* Şirket içi röle dinleyicisi yanıt vermiyor olabilir (veya dinleyicilerin yeni istemci bağlantılarını kabul etmelerini yasaklayan güvenlik duvarı kuralları sorunlarıyla karşılaşabilir) ve [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) değeri yaklaşık 20 saniyeden kısadır.

Örnek:

```
'System.TimeoutException’: The operation did not complete within the allotted timeout of 00:00:10.
The time allotted to this operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Olası nedenler
Bu hatanın iki yaygın nedeni vardır:

*   **Hatalı yapılandırma**
    
    Operasyon zaman, operasyonel durum için çok küçük olabilir. İstemci SDK'daki işlem zaman anına varsayılan değer 60 saniyedir. Kodunuzdaki değerin çok küçük bir şeye ayarlanıp ayarlanıp ayarlanıp ayarlanıp ayarlmadığını denetleyin. CPU kullanımının ve ağın durumunun bir işlemin tamamlanması için gereken süreyi etkileyebileceğini unutmayın. Operasyon zaman larını çok küçük bir değere ayarlamamak iyi bir fikirdir.
*   **Geçici servis hatası**

    Bazen, Röle hizmeti istekleri işlemede gecikmeler yaşayabilir. Bu, örneğin, yüksek trafik dönemlerinde olabilir. Bu durumda, işlem başarılı olana kadar bir gecikmeden sonra işleminizi yeniden deneyin. Aynı işlem birden çok denemeden sonra başarısız olmaya devam ederse, bilinen hizmet kesintileri olup olmadığını görmek için [Azure hizmet durum sitesini](https://azure.microsoft.com/status/) denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Röle SSS'leri](relay-faq.md)
* [Röle ad alanı oluşturma](relay-create-namespace-portal.md)
* [Azure Röle ve .NET ile başlayın](relay-hybrid-connections-dotnet-get-started.md)
* [Azure Röle ve Düğüm ile başlayın](relay-hybrid-connections-node-get-started.md)

