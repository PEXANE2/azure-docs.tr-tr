---
title: Belirli konumlardan Azure bölgelerine yönelik göreli gecikme sürelerini görüntüleme | Microsoft Docs
description: Belirli konumlardan Azure bölgelerine Internet sağlayıcıları genelinde göreli gecikme sürelerini görüntülemeyi öğrenin.
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
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840545"
---
# <a name="view-relative-latency-to-azure-regions-from-specific-locations"></a>Belirli konumlardan Azure bölgelerinin göreli gecikmesini görüntüleme

> [!WARNING]
> Bu özellik şu anda önizleme aşamasındadır ve hala kararlılık için test ediliyor.

Bu öğreticide, Azure [Ağ İzleyicisi](network-watcher-monitoring-overview.md) hizmetini kullanarak uygulamanızı veya hizmetinizi hangi Azure bölgesine dağıtacağına karar vermenize yardımcı olması için Kullanıcı demolarınızın temel alınarak ilgili bilgi edinebilirsiniz. Ayrıca, hizmet sağlayıcılarının Azure bağlantılarını değerlendirmeye yardımcı olması için bu hizmeti kullanabilirsiniz.  
        

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-network-watcher"></a>Ağ İzleyicisi oluşturma

En az bir Azure [bölgesinde](https://azure.microsoft.com/regions)zaten bir ağ izleyicisine sahipseniz, bu bölümdeki görevleri atlayabilirsiniz. Ağ İzleyicisi için bir kaynak grubu oluşturun. Bu örnekte, kaynak grubu Doğu ABD bölgesinde oluşturulur, ancak kaynak grubunu herhangi bir Azure bölgesinde oluşturabilirsiniz.

```powershell
New-AzResourceGroup -Name NetworkWatcherRG -Location eastus
```

Ağ izleyicisi oluşturun. En az bir Azure bölgesinde oluşturulmuş bir ağ izleyicisine sahip olmanız gerekir. Bu örnekte, Azure bölgesinde Doğu ABD bir ağ izleyicisi oluşturulur.

```powershell
New-AzNetworkWatcher -Name NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG -Location eastus
```

## <a name="compare-relative-network-latencies-to-a-single-azure-region-from-a-specific-location"></a>Göreli ağ gecikmelerinin belirli bir konumdan tek bir Azure bölgesine karşılaştırın

Hizmet sağlayıcılarını değerlendirin veya bir kullanıcının belirli bir konumdan bir hizmetin dağıtıldığı Azure bölgesine "Site yavaş" gibi bir sorun bildirdiği bir kullanıcıya sorun giderin. Örneğin, aşağıdaki komut, Birleşik Devletler Washington Eyaleti ile 13-15 Aralık 2017 arasında Batı ABD 2 Azure bölgesindeki ortalama göreli Internet hizmet sağlayıcısı gecikme sürelerini döndürür:

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
> Önceki komutta belirttiğiniz bölgenin, Ağ İzleyicisi 'ni aldığınızda belirttiğiniz bölgeyle aynı olması gerekmez. Önceki komut basitçe var olan bir ağ izleyicisi belirtmenizi gerektirir. Ağ İzleyicisi herhangi bir bölgede olabilir. `-Country` ve `-State`için değerler belirtirseniz, bunların geçerli olmaları gerekir. Değerler büyük/küçük harfe duyarlıdır. Veriler, sınırlı sayıda ülke/bölge, eyalet ve şehir için kullanılabilir. Kullanılabilir ülke/bölge, [eyalet, şehir ve sağlayıcıları görüntüleme](#view-available) içindeki komutları çalıştırarak, önceki komutla birlikte kullanılacak mevcut ülkelerin/bölgelerin, şehirlerin ve durumların bir listesini görüntüleyin. 

> [!WARNING]
> Son 30 gün içinde `-StartTime` ve `-EndTime`için bir tarih belirtmeniz gerekir. Önceki bir tarih belirtildiğinde hiçbir veri döndürülmeyecektir.

Önceki komutun çıktısı şöyledir:

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

Döndürülen çıktıda, **puan** değeri bölgeler ve sağlayıcılar arasındaki göreli gecikmedir. 1 skoru en kötü (en yüksek) gecikmedir, ancak 100 en düşük gecikmedir. Gün için göreli gecikme süreleri ortalaması alınır. Önceki örnekte, gecikme sürelerinin her iki günde aynı olduğunu ve iki sağlayıcının gecikmesi arasında küçük bir farklılık olduğunu fark ederken, her iki sağlayıcının da gecikme sürelerinin 1-100 ölçeğinde düşük olduğunu da temizleyebilir. Bu beklenirken, Birleşik Devletler Washington Eyaleti Batı ABD 2 Azure bölgesine fiziksel olarak yakın olduğundan sonuçlar bazen beklenmez. Belirttiğiniz tarih aralığı arttıkça, zaman içinde ortalama gecikme süresi daha fazla olabilir.

## <a name="compare-relative-network-latencies-across-azure-regions-from-a-specific-location"></a>Belirli bir konumdan Azure bölgelerinde göreli Ağ gecikmeleri karşılaştırın

`-Location`kullanarak belirli bir konum ve belirli bir Azure bölgesi arasındaki göreli gecikme sürelerini belirtmek yerine, belirli bir fiziksel konumdan tüm Azure bölgelerinin göreli gecikme sürelerini belirlemek istediğinizde, bunu da yapabilirsiniz. Örneğin, aşağıdaki komut, birincil kullanıcılarınızın Washington durumunda bulunan kullanıcıları Comcast durumunda ' de bir hizmeti hangi Azure bölgesine dağıtacağınızı değerlendirmenize yardımcı olur:

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
> Tek bir konum belirttiğinizde farklı olarak, bir konum belirtmezseniz veya "Batı ABD2", "Batı ABD" gibi birden çok konum belirtmezseniz, komutu çalıştırırken bir Internet hizmeti sağlayıcısı belirtmeniz gerekir. 

## <a name="view-available"></a>Kullanılabilir ülkeleri/bölgeleri, durumları, şehirleri ve sağlayıcıları görüntüleme

Veriler belirli Internet hizmet sağlayıcıları, ülkeler/bölgeler, durumlar ve şehirlerde kullanılabilir. Veri görüntüleyebildiğiniz tüm kullanılabilir Internet hizmet sağlayıcılarının, ülkelerin/bölgelerin, durumların ve şehirlerin listesini görüntülemek için aşağıdaki komutu girin:

```powershell
Get-AzNetworkWatcherReachabilityProvidersList -NetworkWatcherName NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG
```

Veriler yalnızca önceki komut tarafından döndürülen ülkeler/bölgeler, durumlar ve şehirlerde kullanılabilir. Önceki komut, var olan bir ağ izleyicisi belirtmenizi gerektirir. Örnek, *NetworkWatcherRG*adlı bir kaynak grubunda *NetworkWatcher_eastus* Ağ İzleyicisini belirtti, ancak var olan herhangi bir ağ izleyicisini belirtebilirsiniz. Mevcut bir ağ izleyicisine sahip değilseniz, [ağ Izleyicisi oluşturma](#create-a-network-watcher)' daki görevleri tamamlayarak bir tane oluşturun. 

Önceki komutu çalıştırdıktan sonra, isterseniz **ülke**, **eyalet**ve **şehir**için geçerli değerler belirterek döndürülen çıktıyı filtreleyebilirsiniz.  Örneğin, Seattle, Washington 'da bulunan Internet hizmet sağlayıcılarının listesini görüntülemek için Birleşik Devletler, aşağıdaki komutu girin:

```powershell
Get-AzNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -City Seattle `
  -Country "United States" `
  -State washington
```

> [!WARNING]
> **Ülke** için belirtilen değer büyük ve küçük harf olmalıdır. **Eyalet** ve **şehir** için belirtilen değerler küçük harfle yazılmalıdır. Değerler **ülke**, **eyalet**ve **şehir**için hiçbir değer olmadan çalıştırıldıktan sonra döndürülen çıktıda listelenmelidir. Yanlış bir durum belirtirseniz veya bir **ülke**, **eyalet**veya **şehir** için, komutu bu özellikler için hiçbir değer olmadan çalıştırıldıktan sonra döndürülen çıktıda olmayan bir değer belirtirseniz, döndürülen çıkış boştur.
