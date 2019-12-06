---
title: Azure Değişiklik İzleme sorunlarını giderme
description: Bu makale, sorun giderme hakkında bilgi sağlar Değişiklik İzleme
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 482b263b7c8427276d4fdb8681372c2be1ae69ee
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74849403"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Değişiklik İzleme ve Envanter sorunlarını giderme

## <a name="windows"></a>Windows

### <a name="records-not-showing-windows"></a>Senaryo: Windows makineler için Değişiklik İzleme kayıtları gösterilmiyor

#### <a name="issue"></a>Sorun

Değişiklik İzleme için eklendi olan Windows makineler için herhangi bir envanter veya Değişiklik İzleme sonucu görmezsiniz.

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden kaynaklanıyor olabilir:

1. **Microsoft Monitoring Agent** çalışmıyor
2. Otomasyon hesabına geri iletişim engelleniyor.
3. Değişiklik İzleme için yönetim paketleri indirilmez.
4. Eklendi olan VM, Microsoft Monitoring Agent yüklü olarak Sysprep uygulanmamış bir kopyalanmış makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözünürlük

1. Makinede **Microsoft Monitoring Agent** (HealthService. exe) çalıştığını doğrulayın.
1. Makinedeki **Olay Görüntüleyicisi** denetleyin ve word içinde `changetracking` sözcük içeren tüm olayları arayın.
1. Değişiklik İzleme çalışması için hangi adreslere ve bağlantı noktalarına izin verileceğini öğrenmek için [ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning) ' nı ziyaret edin.
1. Aşağıdaki Değişiklik İzleme ve stok yönetim paketlerinin yerel olarak mevcut olduğunu doğrulayın:
    * Microsoft. ıntelligencepacks. ChangeTrackingDirectAgent. *
    * Microsoft. ıntelligencepacks. ınventorychangetracking. *
    * Microsoft. ıntelligencepacks. Singletonınventorycollection. *
1. Kopyalanmış bir görüntü kullanılıyorsa, önce görüntüyü Sysprep yapın ve Microsoft Monitoring Agent aracısını yüklemeden sonra yükler.

Bu çözümler sorununuzu gidermezse ve desteğe başvurursanız, aracıdaki tanılamayı toplamak için aşağıdaki komutları çalıştırabilirsiniz

Aracı makinede `C:\Program Files\Microsoft Monitoring Agent\Agent\Tools` ' a gidin ve aşağıdaki komutları çalıştırın:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Varsayılan olarak hata izleme etkindir, önceki örnekte olduğu gibi ayrıntılı hata iletilerini etkinleştirmek istiyorsanız `VER` parametresi kullanın. Bilgi izlemeleri için `StartTracing.cmd`çağrılırken `INF` kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
