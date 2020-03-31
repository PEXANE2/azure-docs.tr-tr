---
title: Otomasyon hesabı sorun giderme
description: Azure hesabıyla ilgili sorunları nasıl gidereceğinizi ve nasıl çözeceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: c66b1728144b8517f6ac444059b3a8def956c6e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80301011"
---
# <a name="automation-account-troubleshooting"></a>Otomasyon hesabı sorun giderme

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

5. Listede yoksa, kaynak sağlayıcı kaydı yla ilgili hataları gider'deki adımları izleyerek **Microsoft.Automation** [sağlayıcısını kaydedin.](/azure/azure-resource-manager/resource-manager-register-provider-errors)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemediyseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.