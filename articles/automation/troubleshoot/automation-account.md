---
title: Otomasyon hesabı sorunlarını giderme
description: Azure hesabıyla ilgili sorunları nasıl gidereceğinizi ve nasıl çözeceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 7b5e7171ac679384966e9dce79425cd1fa881c53
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679390"
---
# <a name="troubleshoot-the-automation-account"></a>Otomasyon hesabının sorun giderme

Bu makalede, bir Otomasyon hesabı kullandığınızda karşılaşabileceğiniz sorunların çözümleri açıklan.' Aşağıdaki bölümlerde, her biri için belirli hata iletileri ve olası çözümler vurgulanır. Otomasyon hesapları hakkında genel bilgi için [bkz.](../automation-quickstart-create-account.md)

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Senaryo: Abonelikler için Otomasyon Kaynak Sağlayıcısı'nı kaydedemiyoruz

### <a name="issue"></a>Sorun

Otomasyon hesabınızda yönetim çözümleriyle çalışırken aşağıdaki hatalarla karşılaşırsınız:

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Nedeni

Otomasyon Kaynak Sağlayıcısı aboneye kayıtlı değildir.

### <a name="resolution"></a>Çözüm

Otomasyon Kaynak Sağlayıcısı'nı kaydetmek için Azure portalında aşağıdaki adımları izleyin:

1. Tarayıcınızdan [Azure portalına](https://portal.azure.com)gidin.

2. **Abonelikler'e** gidin ve Abonelikler sayfasından aboneliğinizi seçin.   

3. **Ayarlar** **altında, Kaynak Sağlayıcıları**seçin.

4. Kaynak sağlayıcıları listesinden, **Microsoft.Automation** kaynak sağlayıcısının kayıtlı olduğunu doğrulayın.

5. Sağlayıcı listede yoksa, kaynak sağlayıcı kaydı için hataları çözümle'de açıklandığı gibi [kaydedin.](/azure/azure-resource-manager/resource-manager-register-provider-errors)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.