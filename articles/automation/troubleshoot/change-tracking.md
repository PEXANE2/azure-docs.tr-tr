---
title: Değişiklik İzleme ve envanterle ilgili sorunları giderme
description: Azure Otomasyonu Değişiklik İzleme ve envanter çözümüyle ilgili sorunları nasıl giderebileceğinizi ve çözeceğinizi öğrenin.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4f230cd0965d58f690d333cd62f2c7c1d499e8d1
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82582151"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Değişiklik İzleme ve envanter sorunlarını giderme

Bu makalede, Azure Otomasyonu Değişiklik İzleme ve envanter sorunlarını giderme işlemleri açıklanmaktadır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Senaryo: Windows makineler için Değişiklik İzleme ve envanter kayıtları gösterilmiyor

#### <a name="issue"></a>Sorun

Eklendi olan Windows makineler için herhangi bir Değişiklik İzleme ve envanter sonucu görmezsiniz.

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerle olabilir:

* Windows için Azure Log Analytics Aracısı çalışmıyor.
* Otomasyon hesabına geri iletişim engelleniyor.
* Değişiklik İzleme ve envanter yönetim paketleri indirilmez.
* Eklendi olan VM, Windows için Log Analytics aracısıyla Sysprep yüklenmemiş bir kopyalanmış makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

Log Analytics Aracısı makinesinde, **C:\Program Files\Microsoft Monitoring Tors T\tors \ araçlar** ' a gidin ve aşağıdaki komutları çalıştırın:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Hala yardıma ihtiyacınız varsa tanılama bilgilerini toplayabilir ve desteğe başvurabilirsiniz.

> [!NOTE]
> Log Analytics Aracısı varsayılan olarak hata izleme imkanı sunar. Önceki örnekte olduğu gibi ayrıntılı hata iletilerini etkinleştirmek için `VER` parametresini kullanın. Bilgi izlemeleri için öğesini çağırdığınızda `INF` `StartTracing.cmd`kullanın.

##### <a name="log-analytics-agent-for-windows-not-running"></a>Windows için Log Analytics Aracısı çalışmıyor

Makinede Windows (**HealthService. exe**) Log Analytics aracısının çalıştığını doğrulayın.

##### <a name="communication-to-automation-account-blocked"></a>Otomasyon hesabına yönelik iletişim engellendi

Makinedeki Olay Görüntüleyicisi denetleyin ve içindeki sözcüğe `changetracking` sahip tüm olayları arayın.

Değişiklik İzleme ve envanterin çalışması için izin verilmesi gereken adresler ve bağlantı noktaları hakkında bilgi edinmek için bkz. [karma runbook çalışanı kullanarak veri merkezinizdeki veya buluttaki kaynakları otomatikleştirme](../automation-hybrid-runbook-worker.md#network-planning).

##### <a name="management-packs-not-downloaded"></a>Yönetim paketleri indirilmedi

Aşağıdaki Değişiklik İzleme ve stok yönetim paketlerinin yerel olarak yüklü olduğunu doğrulayın:

* `Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*`
* `Microsoft.IntelligencePacks.InventoryChangeTracking.*`
* `Microsoft.IntelligencePacks.SingletonInventoryCollection.*`

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Kopyalanmış makineden Sysprep uygulanmamış VM

Kopyalanmış bir görüntü kullanıyorsanız, önce görüntüyü Sysprep yapın ve ardından Windows için Log Analytics aracısını yükler.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-and-inventory-results-on-linux-machines"></a>Senaryo: Linux makinelerde Değişiklik İzleme ve envanter sonucu yok

#### <a name="issue"></a>Sorun

Çözüm için eklendi olan Linux makineler için herhangi bir Değişiklik İzleme ve envanter sonucu görmezsiniz. 

#### <a name="cause"></a>Nedeni
Bu soruna özel nedenler aşağıda verilmiştir:
* Linux için Log Analytics Aracısı çalışmıyor.
* Linux için Log Analytics Aracısı doğru yapılandırılmamış.
* Dosya bütünlüğü izleme (FIM) çakışmaları var.

#### <a name="resolution"></a>Çözüm 

##### <a name="log-analytics-agent-for-linux-not-running"></a>Linux için Log Analytics Aracısı çalışmıyor

Linux (**omsagent**) için Log Analytics aracısının arka plan programının makinenizde çalıştığını doğrulayın. Otomasyon hesabınıza bağlı Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Makinenizi sorgu sonuçlarında görmüyorsanız, son zamanlarda iade edilmedi demektir. Büyük olasılıkla yerel bir yapılandırma sorunu var ve aracıyı yeniden yüklemeniz gerekir. Yükleme ve yapılandırma hakkında bilgi için bkz. [Log Analytics aracısında günlük verileri toplama](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent).

Makineniz sorgu sonuçlarında görünüyorsa, kapsam yapılandırmasını doğrulayın. Bkz. [Azure izleyici 'de izleme çözümlerini hedefleme](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting).

Bu sorunu giderme hakkında daha fazla bilgi için bkz. [sorun: herhangi bir Linux verisi görmüyorsunuz](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>Linux için Log Analytics Aracısı doğru yapılandırılmamış

Linux için Log Analytics Aracısı, OMS günlük Toplayıcı Aracı kullanılarak günlük ve komut satırı çıkış koleksiyonu için doğru şekilde yapılandırılmamış olabilir. [Değişiklik izleme ve envanter çözümü ile ortamınızdaki değişiklikleri izleme](../change-tracking.md)bölümüne bakın.

##### <a name="fim-conflicts"></a>FIM çakışmaları

Azure Güvenlik Merkezi 'nin FIM özelliği, Linux dosyalarınızın bütünlüğünü yanlış şekilde doğruluyor olabilir. FIM 'nin çalışır durumda olduğunu ve Linux dosya izleme için doğru şekilde yapılandırıldığını doğrulayın. [Değişiklik izleme ve envanter çözümü ile ortamınızdaki değişiklikleri izleme](../change-tracking.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu burada görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabıyla bağlanın. Azure desteği, Azure Community 'yi yanıtlar, destek ve uzmanlar için bağlar.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin.
