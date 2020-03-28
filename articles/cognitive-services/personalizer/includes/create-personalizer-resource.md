---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 01/15/2020
ms.author: diberry
ms.openlocfilehash: 11abd52681d7c9962af4e5bf0728f97b256223c1
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122904"
---
## <a name="create-a-personalizer-azure-resource"></a>Personalizer Azure kaynağı oluşturma

Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Personalizer için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak 7 gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)satışa sunulacaktır.
* Azure [portalında](https://portal.azure.com/)kaynağınızı görüntüleyin.

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, iki [ortam değişkeni](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)oluşturun:

* `PERSONALIZER_RESOURCE_KEY`kaynak anahtarı için.
* `PERSONALIZER_RESOURCE_ENDPOINT`kaynak bitiş noktası için.

Azure portalında, hem anahtar hem de bitiş noktası değerleri **hızlı başlangıç** sayfasından kullanılabilir.
