---
title: Gerekli parametreler toplanıyor
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Tüm bilişsel hizmetler kapsayıcıları için parametreler
ms.service: cognitive-services
ms.topic: include
ms.date: 7/24/2019
ms.author: dapine
ms.openlocfilehash: 636a41fde345a08db1549e53626522962f9cf74f
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68488742"
---
## <a name="gathering-required-parameters"></a>Gerekli parametreler toplanıyor

Gerekli olan tüm bilişsel hizmetler için üç birincil parametre vardır. Son Kullanıcı Lisans Sözleşmesi 'nin `accept`(**EULA**) değeri ile mevcut olması gerekir. Ayrıca, hem bir uç nokta URL 'SI hem de API anahtarı gereklidir.

### <a name="endpoint-uri-endpointuri"></a>Uç nokta URI 'SI`{Endpoint_URI}`

**Uç nokta** URI değeri, karşılık gelen bilişsel hizmet kaynağının Azure Portal *genel bakış* sayfasında bulunur. *Genel bakış* sayfasına gidin, uç noktanın üzerine gelin ve bir `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> simge görüntülenir. Gerektiğinde kopyalayın ve kullanın.

![Uç nokta URI 'sini daha sonra kullanılmak üzere toplayın](../media/overview-endpoint-uri.png)

### <a name="keys-apikey"></a>Belirlenmesine`{API_Key}`

Bu anahtar, kapsayıcıyı başlatmak için kullanılır ve ilgili bilişsel hizmet kaynağının Azure portal tuşları sayfasında kullanılabilir. *Anahtarlar* sayfasına gidin ve `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> simgesine tıklayın.

![Daha sonra kullanmak üzere iki anahtardan birini al](../media/keys-copy-api-key.png)

> [!IMPORTANT]
> Bu abonelik anahtarları bilişsel hizmet API 'nize erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Azure Key Vault kullanarak güvenli bir şekilde depolayın. Ayrıca, bu anahtarların düzenli olarak yeniden oluşturulması önerilir. API çağrısı yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yeniden oluştururken, hizmete devam eden erişim için ikinci anahtarı kullanabilirsiniz.