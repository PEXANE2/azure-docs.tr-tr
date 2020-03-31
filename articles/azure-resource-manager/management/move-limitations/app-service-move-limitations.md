---
title: Azure Uygulama Hizmeti kaynaklarını taşıma
description: Uygulama Hizmeti kaynaklarını yeni bir kaynak grubuna veya aboneye taşımak için Azure Kaynak Yöneticisi'ni kullanın.
ms.topic: conceptual
ms.date: 12/13/2019
ms.openlocfilehash: 338b0559a5de9468ff60024b88d0f676a9fc3e8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75479806"
---
# <a name="move-guidance-for-app-service-resources"></a>Uygulama Hizmeti kaynakları için kılavuzu taşıma

Bu makalede, Uygulama Hizmeti kaynaklarını taşımak için adımlar açıklanmaktadır. Uygulama Hizmeti kaynaklarını yeni bir aboneye taşımak için özel gereksinimler vardır.

## <a name="move-across-subscriptions"></a>Abonelikler arasında taşıma

Bir Web Uygulamasını abonelikler arasında hareket ettirirken aşağıdaki kılavuz geçerlidir:

- Hedef kaynak grubunun varolan App Service kaynakları olmamalıdır. Uygulama Hizmeti kaynakları şunlardır:
    - Web Apps
    - App Service planları
    - Yüklenen veya içe aktarılan SSL sertifikaları
    - App Service ortamları
- Kaynak grubundaki tüm App Service kaynakları birlikte taşınmalıdır. Uygulama Hizmeti Ortamları'nın yeni bir Kaynak Grubuna veya yeni bir Aboneye taşılamayacağını unutmayın.
- Sertifika, kaynak grubundaki diğer tüm kaynaklarla birlikte taşındığı sürece, SSL bağlamalarını silmeden web'e bağlı bir sertifikayı taşıyabilirsiniz.
- Uygulama Hizmeti kaynakları yalnızca ilk oluşturuldukları kaynak grubundan taşınabilir. Bir Uygulama Hizmeti kaynağı artık özgün kaynak grubunda değilse, kaynağı özgün kaynak grubuna geri taşıyın. Ardından, kaynağı abonelikler arasında taşıyın.

Özgün kaynak grubunu hatırlamıyorsanız, tanılama yoluyla bulabilirsiniz. Web uygulamanız için **Tanıla'yı seçin ve sorunları çözün.** Ardından Yapılandırma **ve Yönetim'i**seçin.

![Tanılama seçin](./media/app-service-move-limitations/select-diagnostics.png)

**Geçiş Seçenekleri'ni**seçin.

![Geçiş seçeneklerini seçin](./media/app-service-move-limitations/select-migration.png)

Web uygulamasını taşımak için önerilen adımlar için seçeneği belirleyin.

![Önerilen adımları seçin](./media/app-service-move-limitations/recommended-steps.png)

Kaynakları taşımadan önce alınması önerilen eylemleri görürsünüz. Bilgiler, web uygulamasının özgün kaynak grubunu içerir.

![Öneriler](./media/app-service-move-limitations/recommendations.png)

## <a name="move-support"></a>Taşıma desteği

Hangi Uygulama Hizmeti kaynaklarının taşınabileceğini belirlemek için aşağıdakiler için taşı destek durumuna bakın:

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Sonraki adımlar

Komutların kaynakları taşıması [için](../move-resource-group-and-subscription.md)bkz.
