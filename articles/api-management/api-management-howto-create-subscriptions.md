---
title: Azure API Management abonelik oluşturma | Microsoft Docs
description: Azure API Management abonelik oluşturmayı öğrenin. API 'lere erişim izni veren abonelik anahtarları almak için bir abonelik gereklidir.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 191323a4c150c00c93245be35c9c8af381e26b42
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87904878"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Azure API Management'ta abonelik oluşturma

API 'Leri Azure API Management aracılığıyla yayımladığınızda, abonelik anahtarları kullanılarak bu API 'lere güvenli bir şekilde erişim sağlamak kolay ve yaygındır. Yayımlanan API 'Leri kullanması gereken istemci uygulamalar, bu API 'lere çağrılar yaparken HTTP isteklerinde geçerli bir abonelik anahtarı içermelidir. API 'Lere erişim için bir abonelik anahtarı almak üzere bir abonelik gerekir. Abonelikler hakkında daha fazla bilgi için bkz. [Azure API Management abonelikler](api-management-subscriptions.md).

Bu makalede Azure portal abonelik oluşturma adımları anlatılmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları gerçekleştirmek için önkoşullar aşağıdaki gibidir:

+ [API Management bir örnek oluşturun](get-started-create-service-instance.md).
+ [API Management abonelikleri](api-management-subscriptions.md)anlayın.

## <a name="create-a-new-subscription"></a>Yeni abonelik oluştur

1. Soldaki menüden **abonelikler** ' i seçin.
2. **Abonelik Ekle**' yi seçin.
3. Aboneliğin adını belirtin ve kapsamı seçin.
4. İsteğe bağlı olarak, aboneliğin bir kullanıcıyla ilişkilendirilmesi gerekip gerekmediğini seçin.
5. **Kaydet**’i seçin.

![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

Aboneliği oluşturduktan sonra API 'Lere erişmek için iki API anahtarı sağlanır. Bir anahtar birincildir ve biri ikincildir. 

## <a name="next-steps"></a>Sonraki adımlar
API Management hakkında daha fazla bilgi alın:

+ API Management diğer [kavramları](api-management-terminology.md) öğrenin.
+ API Management hakkında daha fazla bilgi edinmek için [öğreticilerimizi](import-and-publish.md) izleyin.
+ Sık sorulan sorular için [SSS sayfamıza](api-management-faq.md) bakın.