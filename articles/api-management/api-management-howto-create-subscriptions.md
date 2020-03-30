---
title: Azure API Yönetimi'nde abonelik oluşturma | Microsoft Dokümanlar
description: Azure API Yönetimi'nde abonelikoluşturma yı öğrenin.
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
ms.openlocfilehash: f8b2238eb0fab9aeeb42d11b4176c0d681b5f8e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70073535"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Azure API Management'ta abonelik oluşturma

ApI'leri Azure API Yönetimi aracılığıyla yayımladığınızda, abonelik anahtarlarını kullanarak bu API'lere erişimi güvence altına almak kolay ve yaygındır. Yayımlanmış API'leri tüketmesi gereken istemci uygulamaları, bu API'lere çağrı yaparken HTTP isteklerinde geçerli bir abonelik anahtarı içermelidir. API'lere erişmek için abonelik anahtarı almak için abonelik gerekir. Abonelikler hakkında daha fazla bilgi için [Azure API Yönetimi'ndeki Abonelikler'e](api-management-subscriptions.md)bakın.

Bu makale, Azure portalında abonelik oluşturma adımlarını gözden geçirir.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları atmak için ön koşullar aşağıdaki gibidir:

+ [BIR API Yönetimi örneği oluşturun.](get-started-create-service-instance.md)
+ [API Yönetimi'ndeki abonelikleri](api-management-subscriptions.md)anlayın.

## <a name="create-a-new-subscription"></a>Yeni bir abonelik oluşturma

1. Soldaki menüden **Abonelikler'i** seçin.
2. **Abonelik Ekle'yi**seçin.
3. Aboneliğin adını sağlayın ve kapsamı seçin.
4. İsteğe bağlı olarak, aboneliğin bir kullanıcıyla ilişkilendirilip ilişkilendirilmemesi gerektiğini seçin.
5. **Kaydet'i**seçin.

![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

Aboneliği oluşturduktan sonra, API'lere erişmek için iki API anahtarı sağlanır. Bir anahtar birincil, diğeri ikincildir. 

## <a name="next-steps"></a>Sonraki adımlar
API Yönetimi hakkında daha fazla bilgi alın:

+ API Yönetimi'nde diğer [kavramları](api-management-terminology.md) öğrenin.
+ API Yönetimi hakkında daha fazla bilgi edinmek için [eğitimlerimizi](import-and-publish.md) takip edin.
+ Sık sorulan sorular için [SSS sayfamızı](api-management-faq.md) kontrol edin.