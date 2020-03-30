---
title: Azure FXT Edge Filer donanımını izleme
description: Azure FXT Edge Filer hibrit depolama önbelleği için donanım durumu nasıl izlenir?
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: rohogue
ms.openlocfilehash: 3f422339af2040ad81c585c0e193e6cb3667b135
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72254877"
---
# <a name="monitor-azure-fxt-edge-filer-hardware-status"></a>Azure FXT Edge Filer donanım durumunu izleyin

Azure FXT Edge Filer hibrit depolama önbelleği sistemi, yöneticilerin donanımın nasıl çalıştığını anlamalarına yardımcı olmak için kasada yerleşik birden çok durum ışığına sahiptir.

## <a name="system-health-status"></a>Sistem sağlık durumu

Önbellek işlemlerini daha yüksek bir düzeyde izlemek için, Denetim Masası Pano [Kılavuzu'nda](https://azure.github.io/Avere/legacy/dashboard/4_7/html/ops_dashboard_index.html) açıklandığı gibi yazılım Denetim Masası'nın **Pano** sayfasını kullanın

## <a name="hardware-status-leds"></a>Donanım durumu LED'leri

Bu bölümde, Azure FXT Edge Filer donanımında yerleşik olan çeşitli durum ışıkları açıklanmaktadır.

### <a name="hard-drive-status-leds"></a>Sabit disk durumu LED'leri

![sabit diskin ön, yatay, ek etiketleri 2 (sol üst köşe), 1 (sol alt köşe) ve 3 (sağ taraf) resmi](media/fxt-monitor/fxt-drive-callouts.png)

Her sürücü taşıyıcının iki durum LED'i vardır: etkinlik göstergesi (1) ve durum göstergesi (2). 

* Etkinlik LED 'i (1) sürücü kullanımdayken yanar.  
* Durum LED (2) aşağıdaki tabloda kodları kullanarak sürücünün durumunu gösterir.

| Sürücü durumu LED durumu              | Anlamı  |
|-------------------------------------|----------------------------------------------------------|
| Saniyede iki kez yeşil yanıp söner      | Sürücünün tanımlanması *veya* <br> Kaldırma için sürücü hazırlama  |
| Kapalı (yanmamış)                         | Sistem başlatmayı tamamlamadı *veya* <br>Sürücü kaldırılmaya hazır |
| Yanıp söner yeşil, kehribar, ve kapalı       | Sürücü arızası tahmin edilir   |
| Saniyede dört kez kehribar yanıp söner | Sürücü başarısız oldu   |
| Katı yeşil                         | Sürücü çevrimiçi |

Sürücünün sağ tarafı (3) sürücünün kapasitesi ve diğer bilgilerle etiketlenir.

Sürücü numaraları sürücüler arasındaki boşluğa yazdırılır. Azure FXT Edge Filer'de sürücü 0 sol üst sürücüdür ve sürücü 1 doğrudan altındadır. Numaralandırma bu şekilde devam eder. 

![sürücü numaralarını ve kapasite etiketlerini gösteren, FXT kasasında ki bir sabit sürücü yuvasının fotoğrafı](media/fxt-drives-photo.png)

## <a name="left-control-panel"></a>Sol kontrol paneli

Sol ön kontrol paneli çeşitli durum LED göstergeleri (1) ve büyük ışıklı sistem sağlık göstergesi (2) vardır. 

![sol durum paneli, solda 1 etiketleme durum göstergesi ve 2 sağdaki büyük sistem sağlık göstergesi ışığıetiketleme](media/fxt-monitor/fxt-control-panel-left.jpg)

### <a name="control-panel-status-indicators"></a>Kontrol paneli durum göstergeleri 

Soldaki durum göstergeleri, bu sistemde bir hata varsa katı bir kehribar ışığı gösterir. Aşağıdaki tabloda hataların olası nedenleri ve çözümleri açıklanmaktadır. 

Bu çözümleri denedikten sonra hala hata nız varsa, yardım için [desteğe başvurun.](fxt-support-ticket.md) 

| Simge | Açıklama | Hata koşulu | Olası çözümler |
|----------------|---------------|--------------------|----------------------|
| ![sürücü simgesi](media/fxt-monitor/fxt-hd-icon.jpg) | Sürücü durumu | Sürücü hatası | Sürücünün bir hatası olup olmadığını belirlemek için Sistem Olay Günlüğü'ne bakın veya <br>Uygun çevrimiçi tanılama testini çalıştırın; sistemi yeniden başlatın ve gömülü tanılama (ePSA) çalıştırın veya <br>Sürücüler RAID dizisinde yapılandırılırsa, sistemi yeniden başlatın ve ana bilgisayar bağdaştırıcısı yapılandırma programı girin |
|![sıcaklık simgesi](media/fxt-monitor/fxt-temp-icon.jpg) | Sıcaklık durumu | Termal hata - örneğin, bir fan başarısız oldu veya ortam sıcaklığı kapsama alanı dışında | Aşağıdaki adreslenebilir koşulları denetleyin: <br>Soğutma fanı eksik veya arızalı <br>Sistemin kapağı, hava örtüsü, bellek modülü boş veya arka dolgu braketi kaldırılır <br>Ortam sıcaklığı çok yüksek. <br>Dış hava akımı engellenir |
|![elektrik simgesi](media/fxt-monitor/fxt-electric-icon.jpg) | Elektrik durumu | Elektrikhatası - örneğin, voltaj aralığı dışında, başarısız PSU veya başarısız voltaj regülatörü |  Belirli bir sorun için sistem olay günlüğünü veya sistem iletilerini denetleyin. PSU sorunu varsa, PSU durum LED'ini kontrol edin ve gerekirse PSU'yu yeniden oturtun. | 
|![bellek simgesi](media/fxt-monitor/fxt-memory-icon.jpg) | Bellek durumu | Bellek hatası | Başarısız belleğin konumu için sistem olay günlüğünü veya sistem iletilerini denetleyin; bellek modüllerini yeniden oturtun. |
|![PCIe simgesi](media/fxt-monitor/fxt-pcie-icon.jpg) | PCIe durumu | PCIe kart hatası | Sistemi yeniden başlatın; PCIe kart sürücülerini güncellemek; kartı yeniden yükleyin |


### <a name="system-health-status-indicator"></a>Sistem durumu durumu göstergesi

Sol kontrol panelinin sağındaki büyük ışıklı düğme genel sistem durumunu gösterir ve sistem kimliği modunda birim bulucu ışığı olarak da kullanılır.

Sistem kimliği modu ile sistem durumu modu arasında geçiş yapmak için sistem durumu ve kimlik düğmesine basın.

|Sistem durumu durumu durumu | Koşul |
|-------------------------------------------|-----------------------------------------------|
| Masif mavi | Normal çalışma: sistem açık, normal çalışıyor ve sistem kimliği modu etkin değil. <br/>Sistem kimliği moduna geçmek istiyorsanız sistem durumu ve kimlik düğmesine basın. |
| Yanıp sönen mavi | Sistem kimliği modu etkin. Sistem durumu moduna geçmek istiyorsanız sistem durumu ve sistem kimliği düğmesine basın. |
| Katı kehribar | Sistem arıza emniyeti modunda. Sorun devam ederse, [Microsoft Müşteri Hizmetleri ve Destek'e başvurun.](fxt-support-ticket.md) |
| Yanıp sönen kehribar | Sistem hatası. Belirli hata iletileri için sistem olay günlüğünü denetleyin. Sistem firmware'leri ve sistem bileşenlerini izleyen aracılar tarafından oluşturulan olay ve hata iletileri hakkında bilgi için qrl.dell.com'daki Hata Kodu Arama sayfasına bakın. |


