---
title: Otomasyon hesabı sorunlarını giderme
description: Azure hesabıyla ilgili sorun giderme ve sorunları çözme hakkında bilgi edinin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 7b5e7171ac679384966e9dce79425cd1fa881c53
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679390"
---
# <a name="troubleshoot-the-automation-account"></a>Otomasyon hesabı sorunlarını giderme

Bu makalede, bir Otomasyon hesabı kullanırken karşılaşabileceğiniz sorunlara yönelik çözümler ele alınmaktadır. Aşağıdaki bölümlerde, belirli hata iletileri ve her biri için olası çözümler vurgulanacak. Otomasyon hesapları hakkında genel bilgi için bkz. [Azure hesabı oluşturma](../automation-quickstart-create-account.md).

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Senaryo: abonelikler için Otomasyon kaynak sağlayıcısı kaydedilemiyor

### <a name="issue"></a>Sorun

Otomasyon hesabınızda yönetim çözümleriyle çalışırken aşağıdaki hatayla karşılaşırsınız:

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Nedeni

Otomasyon kaynak sağlayıcısı abonelikte kayıtlı değil.

### <a name="resolution"></a>Çözüm

Otomasyon kaynak sağlayıcısını kaydetmek için Azure portal aşağıdaki adımları izleyin:

1. Tarayıcınızdan [Azure Portal](https://portal.azure.com)gidin.

2. **Abonelikler ' e** gidin ve abonelikler sayfasından aboneliğinizi seçin.   

3. **Ayarlar**altında **kaynak sağlayıcıları**' nı seçin.

4. Kaynak sağlayıcıları listesinden, **Microsoft. Automation** kaynak sağlayıcısının kayıtlı olduğunu doğrulayın.

5. Sağlayıcı listede yoksa, [kaynak sağlayıcısı kaydı hatalarını giderme](/azure/azure-resource-manager/resource-manager-register-provider-errors)bölümünde açıklandığı şekilde kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* Azure Community [@AzureSupport](https://twitter.com/azuresupport)'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.