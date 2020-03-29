---
title: Belirli konumlardan Azure bölgelerine göreli gecikmesüreleri görüntüleyin | Microsoft Dokümanlar
description: Belirli konumlardan Internet sağlayıcılarından Azure bölgelerine göreli gecikmeleri nasıl görüntüleyebilirsiniz öğrenin.
services: network-watcher
documentationcenter: ''
author: damendo
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: damendo
ms.custom: ''
ms.openlocfilehash: c3a85de3a201a89d6d9500e4f4b2df9e667e3537
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840545"
---
# <a name="view-relative-latency-to-azure-regions-from-specific-locations"></a>Belirli konumlardan Azure bölgelerine göreli gecikmeyi görün

> [!WARNING]
> Bu özellik şu anda önizlemede ve kararlılık için test ediliyor.

Bu eğitimde, kullanıcı demografinize bağlı olarak uygulamanızı veya hizmetinizi hangi Azure bölgesinde dağıtacağına karar vermenize yardımcı olmak için Azure [Ağ İzleyicisi](network-watcher-monitoring-overview.md) hizmetini nasıl kullanacağınızı öğrenin. Ayrıca, hizmet sağlayıcıların Azure'a olan bağlantılarını değerlendirmeye yardımcı olmak için kullanabilirsiniz.  
        

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-network-watcher"></a>Ağ izleyicisi oluşturma

En az bir Azure [bölgesinde](https://azure.microsoft.com/regions)zaten bir ağ izleyiciniz varsa, bu bölümdeki görevleri atlayabilirsiniz. Ağ izleyicisi için bir kaynak grubu oluşturun. Bu örnekte, kaynak grubu Doğu ABD bölgesinde oluşturulur, ancak kaynak grubunu herhangi bir Azure bölgesinde oluşturabilirsiniz.

```powershell
New-AzResourceGroup -Name NetworkWatcherRG -Location eastus
```

Ağ izleyicisi oluşturun. En az bir Azure bölgesinde bir ağ izleyiciniz oluşturulmuş olmalıdır. Bu örnekte, Doğu ABD Azure bölgesinde bir ağ izleyicisi oluşturulur.

```powershell
New-AzNetworkWatcher -Name NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG -Location eastus
```

## <a name="compare-relative-network-latencies-to-a-single-azure-region-from-a-specific-location"></a>Belirli bir konumdan belirli bir Azure bölgesiyle göreli ağ gecikmelerini karşılaştırma

Hizmet sağlayıcılarını değerlendirin veya belirli bir konumdan hizmetin dağıtıldığı azure bölgesine kadar "site yavaştı" gibi bir sorunu bildiren bir kullanıcının sorun giderin. Örneğin, aşağıdaki komut, 13-15 Aralık 2017 tarihleri arasında ABD'deki Washington eyaleti ile Batı ABD 2 Azure bölgesi arasındaki ortalama göreli Internet servis sağlayıcısı gecikmelerini döndürür:

```powershell
Get-AzNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Location "West US 2" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

> [!NOTE]
> Önceki komutta belirttiğiniz bölgenin, ağ izleyicisini geri aldığınızda belirttiğiniz bölgeyle aynı olması gerekmez. Önceki komut, varolan bir ağ izleyicisini belirtmenizi gerektirir. Ağ izleyicisi herhangi bir bölgede olabilir. Değerleri `-Country` için ve `-State`, geçerli olmalıdır belirtirseniz. Değerler büyük/küçük harf duyarlıdır. Veriler sınırlı sayıda ülke/bölge, eyalet ve şehir için kullanılabilir. [Kullanılabilir ülkelerin/bölgelerin, eyaletlerin, şehirlerin ve sağlayıcıların](#view-available) listesini önceki komutla kullanmak üzere kullanılabilir ülkelerin/bölgelerin, şehirlerin ve sağlayıcıların listesini görüntülemek için Görünüm'deki komutları çalıştırın. 

> [!WARNING]
> Son 30 gün içinde bir tarih `-StartTime` `-EndTime`belirtmeniz gerekir. Önceki bir tarih belirtilmesi, hiçbir verinin döndürülmemesine neden olur.

Önceki komutun çıktısı aşağıdaki gibidir:

```powershell
AggregationLevel   : State
ProviderLocation   : {
                       "Country": "United States",
                       "State": "washington"
                     }
ReachabilityReport : [
                       {
                         "Provider": "Qwest Communications Company, LLC - ASN 209",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 92
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 92
                           }
                         ]
                       },
                       {
                         "Provider": "Comcast Cable Communications, LLC - ASN 7922",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 96
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 96
                           }
                         ]
                       }
                     ]
```

Döndürülen çıktıda, **Puan** değeri bölgeler ve sağlayıcılar arasında göreli gecikme dir. 1 puanı en kötü (en yüksek) gecikme, 100 ise en düşük gecikme dir. Göreceli gecikmeler gün için ortalama. Önceki örnekte, gecikmelerin her iki gün de aynı olduğu ve iki sağlayıcının gecikmesi arasında küçük bir fark olduğu açık olmakla birlikte, her iki sağlayıcıiçin de gecikme saatlerinin 1-100 ölçeğinde düşük olduğu açıktır. Bu beklenirken, ABD'deki Washington eyaleti fiziksel olarak Batı ABD 2 Azure bölgesine yakın olduğundan, bazen sonuçlar beklendiği gibi değildir. Belirttiğiniz tarih aralığı ne kadar büyükse, zaman içinde gecikme süresini o kadar fazla o kadar fazla alabilirsiniz.

## <a name="compare-relative-network-latencies-across-azure-regions-from-a-specific-location"></a>Azure bölgelerindeki göreli ağ gecikmelerini belirli bir konumdan karşılaştırın

Belirli bir konum la belirli bir Azure bölgesi arasındaki göreli `-Location`gecikme sürelerini belirli bir fiziksel konumdan tüm Azure bölgelerine göreli gecikme sürelerini belirlemek yerine bunu da yapabilirsiniz. Örneğin, aşağıdaki komut, birincil kullanıcılarınız Washington eyaletinde bulunan Comcast kullanıcılarıysa, bir hizmeti hangi azure bölgesinde dağıtabileceğinizi değerlendirmenize yardımcı olur:

```powershell
Get-AzNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Provider "Comcast Cable Communications, LLC - ASN 7922" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

> [!NOTE]
> Tek bir konum belirtmenizin aksine, bir konum belirtmezseniz veya "Batı US2", "Batı ABD" gibi birden çok konum belirtmezseniz, komutu çalıştırırken bir Internet servis sağlayıcısı belirtmeniz gerekir. 

## <a name="view-available-countriesregions-states-cities-and-providers"></a><a name="view-available"></a>Kullanılabilir ülkeleri/bölgeleri, eyaletleri, şehirleri ve sağlayıcıları görüntüleyin

Veriler belirli Internet servis sağlayıcıları, ülkeler/bölgeler, eyaletler ve şehirler için kullanılabilir. Verileri görüntüleyebileceğiniz tüm kullanılabilir Internet servis sağlayıcılarının, ülkelerin/bölgelerinin, devletlerin ve şehirlerin listesini görüntülemek için aşağıdaki komutu girin:

```powershell
Get-AzNetworkWatcherReachabilityProvidersList -NetworkWatcherName NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG
```

Veriler yalnızca önceki komutla döndürülen ülkeler/bölgeler, eyaletler ve şehirler için kullanılabilir. Önceki komut, varolan bir ağ izleyicisini belirtmenizi gerektirir. Örnek, *NetworkWatcherRG*adlı bir kaynak grubunda *NetworkWatcher_eastus* ağ izleyicisini belirtebilir, ancak varolan ağ izleyicisini belirtebilirsiniz. Varolan bir ağ izleyiciniz yoksa, ağ izleyicisi [oluştur'daki](#create-a-network-watcher)görevleri tamamlayarak bir tane oluşturun. 

Önceki komutu çalıştırdıktan sonra, istenirse **Ülke**, **Eyalet**ve **Şehir**için geçerli değerler belirterek döndürülen çıktıyı filtreleyebilirsiniz.  Örneğin, Seattle, Washington'da bulunan Internet servis sağlayıcılarının listesini ABD'de görüntülemek için aşağıdaki komutu girin:

```powershell
Get-AzNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -City Seattle `
  -Country "United States" `
  -State washington
```

> [!WARNING]
> **Ülke** için belirtilen değer daha büyük ve küçük olmalıdır. **Eyalet** ve **Şehir** için belirtilen değerler daha küçük olmalıdır. Değerler **Ülke,** **Devlet**ve **Şehir**için hiçbir değer olmadan komutçalıştırdıktan sonra döndürülen çıktıda listelenmelidir. Yanlış servis talebi belirtirseniz veya bu özellikler için hiçbir değer olmadan komutu çalıştırdıktan sonra döndürülen çıktıda olmayan **Ülke,** **Eyalet**veya **Şehir** için bir değer belirtirseniz, döndürülen çıktı boşkalır.
