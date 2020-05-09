---
title: Azure Otomasyonu hesabı sorunlarını giderme
description: Azure hesabıyla ilgili sorun giderme ve sorunları çözme hakkında bilgi edinin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 6a7defdaa9b4b0f0b3580a3ac6b1a0487434a97c
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82864139"
---
# <a name="troubleshoot-an-azure-automation-account"></a>Azure Otomasyonu hesabında sorun giderme

Bu makalede, bir Azure Otomasyonu hesabı kullanırken karşılaşabileceğiniz sorunlara yönelik çözümler ele alınmaktadır. Otomasyon hesapları hakkında genel bilgi için bkz. [Azure hesabı oluşturma](../automation-quickstart-create-account.md).

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

2. **Abonelikler**' e gidin ve aboneliğinizi seçin.   

3. **Ayarlar**altında **kaynak sağlayıcıları**' nı seçin.

4. Kaynak sağlayıcıları listesinden, **Microsoft. Automation** kaynak sağlayıcısının kayıtlı olduğunu doğrulayın.

5. Sağlayıcı listede yoksa, [kaynak sağlayıcısı kaydı hatalarını giderme](/azure/azure-resource-manager/resource-manager-register-provider-errors)bölümünde açıklandığı şekilde kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makale sorununuzu gidermezse, ek destek için aşağıdaki kanallardan birini deneyin:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* İle [@AzureSupport](https://twitter.com/azuresupport)bağlanın. Bu, Azure Community 'yi doğru kaynaklara bağlamaya yönelik resmi Microsoft Azure hesabıdır: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin.