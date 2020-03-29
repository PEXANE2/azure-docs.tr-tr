---
title: Paket yakalamaları yönetme - Azure portalı
titleSuffix: Azure Network Watcher
description: Azure portalını kullanarak Ağ İzleyicisi'nin paket yakalama özelliğini nasıl yönetebilirsiniz öğrenin.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: damendo
ms.openlocfilehash: 6fc4a25e39fb8f27151b2e3bec1959d74a619233
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840836"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Portalı kullanarak Azure Ağ İzleyicisi ile paket yakalamaları yönetme

Network Watcher paket yakalama ve sanal bir makineden gelen trafiği izlemek için yakalama oturumları oluşturmanıza olanak sağlar. Yalnızca istediğiniz trafiği yakalamanızı sağlamak için yakalama oturumu için filtreler sağlanır. Paket yakalama, hem reaktif hem de proaktif olarak ağ anomalilerinin tanısına yardımcı olur. Diğer kullanım alanları arasında ağ istatistiklerinin toplanması, ağ ihlalleri hakkında bilgi edinme, istemci-sunucu iletişimini hata ayıklama ve çok daha fazlası yer almaktadır. Paket yakalamalarını uzaktan tetikleyebilmek, paket yakalamayı istenilen sanal makinede el ile çalıştırmanın yükünü hafifletir ve bu da değerli zamandan tasarruf sağlar.

Bu makalede, bir paket yakalamayı başlatmayı, durdurmayı, indirmeyi ve silmeyi öğrenirsiniz. 

## <a name="before-you-begin"></a>Başlamadan önce

Paket yakalama için aşağıdaki bağlantı gerektirir:
* Bağlantı noktası 443 üzerinden bir depolama hesabına giden bağlantı.
* 169.254.169.254'e gelen ve giden bağlantı
* 168.63.129.16'ya gelen ve giden bağlantı

Ağ arabirimi veya ağ arabiriminin bulunduğu alt ağla ilişkili bir ağ güvenlik grubu yla ilişkiliyse, önceki bağlantı noktalarına izin veren kuralların bulunduğundan emin olun. Benzer şekilde, ağınıza kullanıcı tanımlı trafik yolları eklemek, yukarıda belirtilen IP'lere ve bağlantı noktalarına bağlantı olmasını engelleyebilir. Lütfen ulaşılabildiklerinden emin olun. 

## <a name="start-a-packet-capture"></a>Paket yakalama başlatma

1. Tarayıcınızda [Azure portalına](https://portal.azure.com) gidin ve **Tüm hizmetleri**seçin ve ardından **Ağ Bölümü'nde** **Ağ İzleyicisi'ni** seçin.
2. **Ağ tanılama araçları**altında Paket **yakalama'yı** seçin. Varolan paket yakalamalar, durumları ne olursa olsun listelenir.
3. Paket yakalama oluşturmak için **Ekle'yi** seçin. Aşağıdaki özellikler için değerleri seçebilirsiniz:
   - **Abonelik**: Paket yakalamayı oluşturmak istediğiniz sanal makinenin içinde olduğu abonelik.
   - **Kaynak grubu**: Sanal makinenin kaynak grubudur.
   - **Hedef sanal makine**: Paket yakalama oluşturmak istediğiniz sanal makine.
   - **Paket yakalama adı**: Paket yakalama adı.
   - **Depolama hesabı veya dosyası**: **Depolama hesabı,** **Dosya**veya her ikisini seçin. **Dosya'yı**seçerseniz, yakalama sanal makine içindeki bir yola yazılır.
   - **Yerel dosya yolu**: Paket yakalamanın kaydedildiği sanal makinedeki yerel yol (yalnızca *Dosya* seçildiğinde geçerlidir). Yol geçerli bir yol olmalıdır. Bir Linux sanal makine kullanıyorsanız, yol */var/captures*ile başlamalıdır.
   - **Depolama hesapları**: *Depolama hesabını*seçtiyseniz, varolan bir depolama hesabı seçin. Bu seçenek yalnızca Depolama **Alanını**seçtiyseniz kullanılabilir.
   
     > [!NOTE]
     > Premium depolama hesapları şu anda paket yakalamaları depolamak için desteklenmez.

   - **Paket başına maksimum bayt**: Yakalanan her paketteki bayt sayısı. Boş bırakılırsa, tüm baytlar yakalanır.
   - **Oturum başına maksimum bayt**: Yakalanan toplam bayt sayısı. Değere ulaşıldıktan sonra paket yakalama durur.
   - **Zaman sınırı (saniye)**: Paket yakalama durdurulmadan önceki zaman sınırı. Varsayılan değer 18.000 saniyedir.
   - Filtreleme (İsteğe bağlı). Seç **+ Filtre ekle**
     - **Protokol**: Paket yakalama için filtreleme protokolü. Kullanılabilir değerler TCP, UDP ve Any'dir.
     - **Yerel IP adresi**: Yerel IP adresinin bu değerle eşleştiği paketler için paket yakalamayı filtreler.
     - **Yerel bağlantı noktası**: Yerel bağlantı noktasının bu değerle eşleştiği paketler için paket yakalamayı filtreler.
     - **Uzak IP adresi**: Uzak IP adresinin bu değerle eşleştiği paketler için paket yakalamayı filtreler.
     - **Uzak bağlantı noktası**: Uzak bağlantı noktasının bu değerle eşleştiği paketler için paket yakalamayı filtreler.
    
     > [!NOTE]
     > Bağlantı noktası ve IP adresi değerleri tek bir değer, değer aralığı veya bağlantı noktası için 80-1024 gibi bir aralık olabilir. İhtiyacınız olan kadar filtre tanımlayabilirsiniz.

4. **Tamam'ı**seçin.

Paket yakalamada ayarlanan süre süresi dolduktan sonra paket yakalama durdurulur ve gözden geçirilebilir. Paket yakalama oturumunu el ile de durdurabilirsiniz.

> [!NOTE]
> Portal otomatik olarak:
>  * Bölge zaten bir ağ izleyicisi yoksa, seçtiğiniz sanal makinenin bulunduğu bölgeyle aynı bölgede bir ağ izleyicisi oluşturur.
>  * *AzureNetworkWatcherExtension* [Linux](../virtual-machines/linux/extensions-nwa.md) veya [Windows](../virtual-machines/windows/extensions-nwa.md) sanal makine uzantısı, henüz yüklenmemişse sanal makineye ekler.

## <a name="delete-a-packet-capture"></a>Paket yakalamayı silme

1. Paket yakalama görünümünde, paket yakalamanın sağ tarafında **...** seçeneğini belirleyin veya varolan bir paket yakalamayı sağ tıklatın ve **Sil'i**seçin.
2. Paket yakalamayı silmek istediğinizi onaylamanız istenir. **Evet'i**seçin.

> [!NOTE]
> Paket yakalamayı silmek, depolama hesabındaki veya sanal makinedeki yakalama dosyasını silmez.

## <a name="stop-a-packet-capture"></a>Paket yakalamayı durdurma

Paket yakalama görünümünde, paket yakalamanın sağ tarafında **...** seçeneğini belirleyin veya varolan bir paket yakalamayı sağ tıklatın ve **Durdur'u**seçin.

## <a name="download-a-packet-capture"></a>Paket yakalama indirme

Paket yakalama oturumunuz tamamlandıktan sonra, yakalama dosyası blob depolama alanına veya sanal makinedeki yerel bir dosyaya yüklenir. Paket yakalamanın depolama konumu, paket yakalama nın oluşturulması sırasında tanımlanır. Bir depolama hesabına kaydedilen yakalama dosyalarına erişmek için kullanışlı bir araç Microsoft Azure Depolama Gezgini'dir ve buradan [indirebilirsiniz.](https://storageexplorer.com/)

Bir depolama hesabı belirtilirse, paket yakalama dosyaları aşağıdaki konumdaki bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

**Yakalamayı** oluşturduğunuzda Dosya'yı seçtiyseniz, sanal makinede yapılandırdığınız yoldan dosyayı görüntüleyebilir veya indirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Sanal makine uyarılarıyla paket yakalamaları nasıl otomatikleştirin, [bkz.](network-watcher-alert-triggered-packet-capture.md)
- Sanal makineye belirli bir trafiğe izin verilip verilmediğini belirlemek için [bkz.](diagnose-vm-network-traffic-filtering-problem.md)
