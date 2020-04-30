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
ms.openlocfilehash: 11c1fd05055922b07801c20d525d852d5360b069
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679341"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Değişiklik İzleme ve envanter sorunlarını giderme

Bu makalede Değişiklik İzleme ve envanter sorunlarının nasıl giderileceği açıklanmaktadır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Senaryo: Windows makineler için Değişiklik İzleme ve envanter kayıtları gösterilmiyor

#### <a name="issue"></a>Sorun

Eklendi olan Windows makineler için herhangi bir Değişiklik İzleme ve envanter sonucu görmezsiniz.

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerle olabilir:

* Windows için Log Analytics Aracısı çalışmıyor.
* Otomasyon hesabına geri iletişim engelleniyor.
* Değişiklik İzleme ve envanter yönetim paketleri indirilmez.
* Eklendi olan VM, Windows için Log Analytics aracısıyla Sysprep yüklenmemiş bir kopyalanmış makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

Log Analytics Aracısı makinesinde, **C:\Program Files\Microsoft izleme araçları \ araç \ araçlar** ' a gidin ve aşağıdaki komutları çalıştırın:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Hala yardıma ihtiyacınız varsa tanılama bilgilerini toplayabilir ve desteğe başvurabilirsiniz. 

> [!NOTE]
> Günlük analiz aracıları, varsayılan olarak hata izleme imkanı sunar. Önceki örnekte olduğu gibi ayrıntılı hata iletilerini etkinleştirmek için `VER` parametresini kullanın. Bilgi izlemeleri için çağrılırken `INF` `StartTracing.cmd`kullanın.

##### <a name="log-analytics-agent-for-windows-not-running"></a>Windows için Log Analytics Aracısı çalışmıyor

Makinede Windows (**HealthService. exe**) Log Analytics aracısının çalıştığını doğrulayın.

##### <a name="communication-to-automation-account-blocked"></a>Otomasyon hesabına yönelik iletişim engellendi

Makinedeki Olay Görüntüleyicisi denetleyin ve içindeki sözcüğe `changetracking` sahip tüm olayları arayın.

Değişiklik İzleme ve envanterin çalışması için izin verilmesi gereken adresler ve bağlantı noktaları hakkında bilgi edinmek için bkz. [karma runbook çalışanı kullanarak veri merkezinizdeki veya buluttaki kaynakları otomatikleştirin](../automation-hybrid-runbook-worker.md#network-planning) .

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

Çözüm için eklendi olan Linux makineler için herhangi bir envanter ve Değişiklik İzleme sonuç görmezsiniz. 

#### <a name="cause"></a>Nedeni
Bu soruna özel nedenler aşağıda verilmiştir:
* Linux için Log Analytics Aracısı çalışmıyor.
* Linux için Log Analytics Aracısı doğru yapılandırılmamış.
* Dosya bütünlüğü Izleme (FIM) çakışmaları var.

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

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* Azure Community [@AzureSupport](https://twitter.com/azuresupport)'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
