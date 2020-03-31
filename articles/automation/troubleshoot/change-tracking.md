---
title: Azure Değişiklik İzleme ile sorun giderme sorunları
description: Azure Otomasyon Değişikliği İzleme ve Envanter özelliğiyle ilgili sorunları nasıl gidereceğinizi ve gidereceğinizi öğrenin.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6cadaea1a20743071acbe8860df02ca7bbdde954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198539"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Değişiklik İzleme ve Envanter sorunlarını giderme

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Senaryo: Windows makineleri için İzleme kayıtlarını değiştir

#### <a name="issue"></a>Sorun

Değişiklik İzleme için gemide bulunan Windows makineleri için değişiklik İzleme veya Stok sonuçları görmezsiniz.

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenleri olabilir:

* Microsoft İzleme Aracısı çalışmıyor.
* Otomasyon hesabına geri dönüş engellendi.
* Değişiklik İzleme için yönetim paketleri indirilmez.
* Gemiye binen VM, Microsoft Monitoring Agent yüklü olan klonlanmış bir makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

Aşağıda açıklanan çözümler sorununuzu çözmenize yardımcı olabilir. Hala yardıma ihtiyacınız varsa, tanılama bilgilerini toplayabilir ve desteğe başvurabilirsiniz. Aracı makinesinde C:\Program Files\Microsoft Monitoring Agent\Tools'a gidin ve aşağıdaki komutları çalıştırın:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Varsayılan olarak, hata izleme etkinleştirilir. Önceki örnekte olduğu gibi ayrıntılı hata iletilerini etkinleştirmek için *VER* parametresini kullanın. Bilgi izlemeleri için **StartTracing.cmd'yi**ararken *INF'yi* kullanın.

##### <a name="microsoft-monitoring-agent-not-running"></a>Microsoft İzleme Aracısı çalışmıyor

Microsoft İzleme Aracısı'nın (HealthService.exe) makinede çalıştığını doğrulayın.

##### <a name="communication-to-automation-account-blocked"></a>Otomasyona iletişim hesabı engellendi

Makinede Olay Görüntüleyicisi'ni denetleyin ve içinde "changetracking" sözcüğü olan olayları arayın.

Değişiklik İzleme'nin çalışmasına izin verilmesi gereken adresler ve bağlantı noktaları hakkında bilgi edinmek için [Karma Runbook Worker'ı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları otomatikleştir'e](../automation-hybrid-runbook-worker.md#network-planning) bakın.

##### <a name="management-packs-not-downloaded"></a>Yönetim paketleri indirilmedi

Aşağıdaki Değişiklik İzleme ve Stok yönetim paketlerinin yerel olarak yüklendiğinden doğrulayın:

* Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*
* Microsoft.IntelligencePacks.InventoryChangeTracking.*
* Microsoft.IntelligencePacks.SingletonInventoryCollection.*

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Sysprepped olmamıştır klonlanmış makineden VM

Klonlanmış bir görüntü kullanıyorsanız, önce görüntüyü sysprep ve sonra Microsoft İzleme Aracısı yükleyin.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-or-inventory-results-on-linux-machines"></a>Senaryo: Linux makinelerinde Değişiklik İzleme veya Envanter sonuçları yok

#### <a name="issue"></a>Sorun

Değişiklik İzleme için gemide bulunan Linux makineleri için herhangi bir Envanter veya Değişiklik İzleme sonucu görmezsiniz. 

#### <a name="cause"></a>Nedeni
Bu soruna özgü olası nedenler şunlardır:
* Linux için Log Analytics aracısı çalışmıyor.
* Linux için Log Analytics aracısı doğru şekilde yapılandırılmamıştır.
* Dosya Bütünlüğü İzleme (FIM) çakışmaları vardır.

#### <a name="resolution"></a>Çözüm 

##### <a name="log-analytics-agent-for-linux-not-running"></a>Linux için Log Analytics aracısı çalışmıyor

Linux için Log Analytics aracısının (omsagent) daemonunun makinenizde çalıştığını doğrulayın. Otomasyon hesabınıza bağlı Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Makinenizi sorgu sonuçlarında görmüyorsanız, yakın zamanda iade edilmemiş. Büyük olasılıkla yerel bir yapılandırma sorunu vardır ve aracıyı yeniden yüklemeniz gerekir. Yükleme ve yapılandırma hakkında daha fazla bilgi için, [Log Analytics aracısıyla günlük verilerini topla bölümüne](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent)bakın. 

Makineniz sorgu sonuçlarında ortaya çıkarsa, kapsam yapılandırmasını doğrulayın. Bkz. [Azure Monitor'da Hedefleme izleme çözümleri.](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting)

Bu sorunun daha fazla sorun giderme için, [Sorun bakın: Herhangi bir Linux veri görmüyoruz.](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data)

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>Linux için Log Analytics aracısı doğru yapılandırılmamıştır

Linux için Log Analytics aracısı, OMS Log Collector aracını kullanarak günlük ve komut satırı çıktı toplama için doğru şekilde yapılandırılamayabilir. [Değişiklik İzleme çözümü ile ortamınızdaki değişiklikleri izleyin'i](../change-tracking.md)görün.

##### <a name="fim-conflicts"></a>FIM çakışmaları

Azure Güvenlik Merkezi'nin FIM özelliği, Linux dosyalarınızın bütünlüğünü yanlış doğruluyor olabilir. FIM'in çalışır durumda olduğunu ve Linux dosya izleme için doğru şekilde yapılandırıldığından doğrulayın. [Değişiklik İzleme çözümü ile ortamınızdaki değişiklikleri izleyin'i](../change-tracking.md)görün.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini kullanın.

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
