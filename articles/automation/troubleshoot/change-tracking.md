---
title: Azure Değişiklik İzleme sorunlarını giderme
description: Azure Otomasyonu Değişiklik İzleme ve envanter özelliği ile ilgili sorunları nasıl giderebileceğinizi ve çözeceğinizi öğrenin.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6cadaea1a20743071acbe8860df02ca7bbdde954
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198539"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Değişiklik İzleme ve Envanter sorunlarını giderme

## <a name="windows"></a>Windows

### <a name="records-not-showing-windows"></a>Senaryo: Windows makineler için Değişiklik İzleme kayıtları gösterilmiyor

#### <a name="issue"></a>Sorun

Değişiklik İzleme için eklendi olan Windows makineler için herhangi bir Değişiklik İzleme veya envanter sonucu görmezsiniz.

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerle olabilir:

* Microsoft Monitoring Agent çalışmıyor.
* Otomasyon hesabına geri iletişim engelleniyor.
* Değişiklik İzleme için yönetim paketleri indirilmez.
* Eklendi olan VM, Microsoft Monitoring Agent yüklü olarak Sysprep uygulanmamış bir kopyalanmış makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

Aşağıda açıklanan çözümler sorununuzu çözmeye yardımcı olur. Hala yardıma ihtiyacınız varsa tanılama bilgilerini toplayabilir ve desteğe başvurabilirsiniz. Aracı makinede, C:\Program Files\Microsoft Izleme Araçları \ araç \ Araçlar ' a gidin ve aşağıdaki komutları çalıştırın:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Varsayılan olarak, hata izleme etkindir. Önceki örnekte olduğu gibi ayrıntılı hata iletilerini etkinleştirmek için *ver* parametresini kullanın. Bilgi izlemeleri için, **StartTracing. cmd** *dosyasını çağırırken INF* kullanın.

##### <a name="microsoft-monitoring-agent-not-running"></a>Microsoft Monitoring Agent çalışmıyor

Makinede Microsoft Monitoring Agent (HealthService. exe) çalıştığını doğrulayın.

##### <a name="communication-to-automation-account-blocked"></a>Otomasyon hesabına yönelik iletişim engellendi

Makinedeki Olay Görüntüleyicisi denetleyin ve "şifre oluşturma" sözcüğünün bulunduğu tüm olayları arayın.

Değişiklik İzleme çalışması için izin verilmesi gereken adresler ve bağlantı noktaları hakkında bilgi edinmek için bkz. [karma runbook çalışanı kullanarak veri merkezinizdeki veya buluttaki kaynakları otomatikleştirin](../automation-hybrid-runbook-worker.md#network-planning) .

##### <a name="management-packs-not-downloaded"></a>Yönetim paketleri indirilmedi

Aşağıdaki Değişiklik İzleme ve stok yönetim paketlerinin yerel olarak yüklü olduğunu doğrulayın:

* Microsoft. ıntelligencepacks. ChangeTrackingDirectAgent. *
* Microsoft. ıntelligencepacks. ınventorychangetracking. *
* Microsoft. ıntelligencepacks. Singletonınventorycollection. *

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Kopyalanmış makineden Sysprep uygulanmamış VM

Kopyalanmış bir görüntü kullanıyorsanız, önce görüntüyü Sysprep yapın ve ardından Microsoft Monitoring Agent yükler.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-or-inventory-results-on-linux-machines"></a>Senaryo: Linux makinelerde Değişiklik İzleme veya envanter sonucu yok

#### <a name="issue"></a>Sorun

Değişiklik İzleme için eklendi olan Linux makineler için herhangi bir envanter veya Değişiklik İzleme sonucu görmezsiniz. 

#### <a name="cause"></a>Nedeni
Bu soruna özel nedenler aşağıda verilmiştir:
* Linux için Log Analytics Aracısı çalışmıyor.
* Linux için Log Analytics Aracısı doğru yapılandırılmamış.
* Dosya bütünlüğü Izleme (FIM) çakışmaları var.

#### <a name="resolution"></a>Çözüm 

##### <a name="log-analytics-agent-for-linux-not-running"></a>Linux için Log Analytics Aracısı çalışmıyor

Linux (omsagent) için Log Analytics aracısının arka plan programının makinenizde çalıştığını doğrulayın. Otomasyon hesabınıza bağlı Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Makinenizi sorgu sonuçlarında görmüyorsanız, son zamanlarda iade edilmedi demektir. Büyük olasılıkla yerel bir yapılandırma sorunu var ve aracıyı yeniden yüklemeniz gerekir. Yükleme ve yapılandırma hakkında bilgi için bkz. [Log Analytics aracısında günlük verileri toplama](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent). 

Makineniz sorgu sonuçlarında görünüyorsa, kapsam yapılandırmasını doğrulayın. Bkz. [Azure izleyici 'de izleme çözümlerini hedefleme](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting).

Bu sorunu gidermek için sorun [: herhangi bir Linux verisi görmüyorsunuz](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>Linux için Log Analytics Aracısı doğru yapılandırılmamış

Linux için Log Analytics Aracısı, OMS günlük Toplayıcı Aracı kullanılarak günlük ve komut satırı çıkış koleksiyonu için doğru şekilde yapılandırılmamış olabilir. Bkz. [değişiklik izleme çözümü ile ortamınızdaki değişiklikleri izleme](../change-tracking.md).

##### <a name="fim-conflicts"></a>FIM çakışmaları

Azure Güvenlik Merkezi 'nin FIM özelliği, Linux dosyalarınızın bütünlüğünü yanlış şekilde doğruluyor olabilir. FIM 'nin çalışır durumda olduğunu ve Linux dosya izleme için doğru şekilde yapılandırıldığını doğrulayın. Bkz. [değişiklik izleme çözümü ile ortamınızdaki değişiklikleri izleme](../change-tracking.md).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemezseniz, daha fazla destek için aşağıdaki kanallardan birini kullanın.

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
