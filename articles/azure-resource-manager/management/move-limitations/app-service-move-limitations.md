---
title: Azure App Service kaynaklarını taşıma
description: App Service kaynaklarını yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
ms.topic: conceptual
ms.date: 12/13/2019
ms.openlocfilehash: 338b0559a5de9468ff60024b88d0f676a9fc3e8b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75479806"
---
# <a name="move-guidance-for-app-service-resources"></a>App Service kaynakları için kılavuzu taşıma

Bu makalede App Service kaynaklarını taşıma adımları açıklanır. App Service kaynaklarını yeni bir aboneliğe taşımak için özel gereksinimler vardır.

## <a name="move-across-subscriptions"></a>Abonelikler arasında geçiş

Bir Web uygulamasını abonelikler arasında taşırken aşağıdaki kılavuz geçerli olur:

- Hedef kaynak grubu mevcut App Service kaynaklar olmaması gerekir. App Service kaynakları şunlardır:
    - Web Apps
    - App Service planları
    - Karşıya yüklenen veya içeri aktarılan SSL sertifikaları
    - App Service Ortamları
- Kaynak grubundaki tüm App Service kaynakların birlikte taşınması gerekir. App Service ortamların yeni bir kaynak grubuna veya yeni bir aboneliğe taşınamayacağını unutmayın.
- Sertifika, kaynak grubundaki diğer tüm kaynaklarla taşındığı sürece, SSL bağlamalarını silmeden bir sertifikayı bir Web 'e taşıyabilirsiniz.
- App Service kaynaklarını, bunlar ilk olarak oluşturulduğu kaynak grubunun yalnızca taşınabilir. Bir App Service kaynağı artık özgün kaynak grubunda yoksa, özgün kaynak grubuna geri taşıyın. Ardından, kaynağı abonelikler arasında taşıyın.

Özgün kaynak grubunu anımsamıyorsanız tanılama aracılığıyla bulabilirsiniz. Web uygulamanız için **Tanılama ve sorunları çözme**' ı seçin. Ardından **yapılandırma ve yönetim**' i seçin.

![Tanılamayı seçin](./media/app-service-move-limitations/select-diagnostics.png)

**Geçiş seçeneklerini**belirleyin.

![Geçiş seçeneklerini belirleyin](./media/app-service-move-limitations/select-migration.png)

Web uygulamasını taşımak için önerilen adımlar seçeneğini belirleyin.

![Önerilen adımları seçin](./media/app-service-move-limitations/recommended-steps.png)

Kaynakları taşımadan önce gerçekleştirilecek önerilen eylemleri görürsünüz. Bilgiler, Web uygulaması için özgün kaynak grubunu içerir.

![Öneriler](./media/app-service-move-limitations/recommendations.png)

## <a name="move-support"></a>Taşıma desteği

Hangi App Service kaynaklarının taşınabileceğini öğrenmek için bkz. destek durumunu taşıma:

- [Microsoft. AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft. CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft. DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft. Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../move-resource-group-and-subscription.md).
