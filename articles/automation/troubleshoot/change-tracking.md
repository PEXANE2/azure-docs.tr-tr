---
title: Değişiklik İzleme ve Stok ile ilgili sorun giderme sorunları
description: Azure Otomasyon Değişikliği İzleme ve Envanter çözümüyle ilgili sorunları nasıl gidereceğinizi ve gidereceğinizi öğrenin.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 11c1fd05055922b07801c20d525d852d5360b069
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679341"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Sorun Giderme Değişiklik İzleme ve Stok sorunları

Bu makalede, Değişiklik İzleme ve Stok sorunlarının nasıl giderilengiderilen açıklanmaktadır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Senaryo: Windows makineleri için İzleme yi değiştir ve Stok kayıtları görünmüyor

#### <a name="issue"></a>Sorun

Yerleşik Windows makineleri için değişiklik izleme ve stok sonuçları görmezsiniz.

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenleri olabilir:

* Windows için Log Analytics aracısı çalışmıyor.
* Otomasyon hesabına geri dönüş engellendi.
* Değişiklik İzleme ve Stok yönetim paketleri indirilmez.
* Gemiye binen VM, Windows için Log Analytics aracısının yüklü olduğu şekilde sysprepped olmayan klonlanmış bir makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

Log Analytics aracısı makinesinde **C:\Program Files\Microsoft Monitoring Agent\Agent\Tools** adresine gidin ve aşağıdaki komutları çalıştırın:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Hala yardıma ihtiyacınız varsa, tanılama bilgilerini toplayabilir ve desteğe başvurabilirsiniz. 

> [!NOTE]
> Log Analyticss aracısı varsayılan olarak hata izleme sağlar. Önceki örnekte olduğu gibi ayrıntılı hata iletilerini etkinleştirmek için parametreyi `VER` kullanın. Bilgi izlemeleri için, `INF` çağırArak `StartTracing.cmd`kullanın.

##### <a name="log-analytics-agent-for-windows-not-running"></a>Windows çalışmayanlar için Günlük Analizi aracısı

Windows için Log Analytics aracısının **(HealthService.exe)** makinede çalıştığını doğrulayın.

##### <a name="communication-to-automation-account-blocked"></a>Otomasyona iletişim hesabı engellendi

Makinede Olay Görüntüleyicisi'ni denetleyin ve `changetracking` içinde sözcük olan olayları arayın.

Değişiklik İzleme ve Envanter'in çalışmasına izin verilmesi gereken adresler ve bağlantı noktaları hakkında bilgi edinmek için [Karma Runbook Worker'ı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları otomatikleştir'e](../automation-hybrid-runbook-worker.md#network-planning) bakın.

##### <a name="management-packs-not-downloaded"></a>Yönetim paketleri indirilmedi

Aşağıdaki Değişiklik İzleme ve Stok yönetim paketlerinin yerel olarak yüklendiğinden doğrulayın:

* `Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*`
* `Microsoft.IntelligencePacks.InventoryChangeTracking.*`
* `Microsoft.IntelligencePacks.SingletonInventoryCollection.*`

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Sysprepped olmamıştır klonlanmış makineden VM

Klonlanmış bir resim kullanıyorsanız, önce görüntüyü sysprep ve ardından Windows için Log Analytics aracısını yükleyin.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-and-inventory-results-on-linux-machines"></a>Senaryo: Linux makinelerinde Değişiklik İzleme ve Envanter sonuçları yok

#### <a name="issue"></a>Sorun

Çözüm için gemide bulunan Linux makineleri için herhangi bir Envanter ve Değişiklik İzleme sonucu görmezsiniz. 

#### <a name="cause"></a>Nedeni
Bu soruna özgü olası nedenler şunlardır:
* Linux için Log Analytics aracısı çalışmıyor.
* Linux için Log Analytics aracısı doğru şekilde yapılandırılmamıştır.
* Dosya Bütünlüğü İzleme (FIM) çakışmaları vardır.

#### <a name="resolution"></a>Çözüm 

##### <a name="log-analytics-agent-for-linux-not-running"></a>Linux için Log Analytics aracısı çalışmıyor

Linux için Log Analytics aracısının **(omsagent)** daemonunun makinenizde çalıştığını doğrulayın. Otomasyon hesabınıza bağlı Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Makinenizi sorgu sonuçlarında görmüyorsanız, yakın zamanda iade edilmemiş. Büyük olasılıkla yerel bir yapılandırma sorunu vardır ve aracıyı yeniden yüklemeniz gerekir. Yükleme ve yapılandırma hakkında daha fazla bilgi için, [Log Analytics aracısıyla günlük verilerini topla bölümüne](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent)bakın. 

Makineniz sorgu sonuçlarında ortaya çıkarsa, kapsam yapılandırmasını doğrulayın. Bkz. [Azure Monitor'da Hedefleme izleme çözümleri.](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting)

Bu sorunun daha fazla sorun giderme için, [Bkz. Sorun: Herhangi bir Linux verisi görmüyorsunuz.](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data)

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>Linux için Log Analytics aracısı doğru yapılandırılmamıştır

Linux için Log Analytics aracısı, OMS Log Collector aracını kullanarak günlük ve komut satırı çıktı toplama için doğru şekilde yapılandırılamayabilir. [Değişiklik İzleme ve Stok çözümü yle ortamınızdaki değişiklikleri izleyin'i](../change-tracking.md)görün.

##### <a name="fim-conflicts"></a>FIM çakışmaları

Azure Güvenlik Merkezi'nin FIM özelliği, Linux dosyalarınızın bütünlüğünü yanlış doğruluyor olabilir. FIM'in çalışır durumda olduğunu ve Linux dosya izleme için doğru şekilde yapılandırıldığından doğrulayın. [Değişiklik İzleme ve Stok çözümü yle ortamınızdaki değişiklikleri izleyin'i](../change-tracking.md)görün.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
