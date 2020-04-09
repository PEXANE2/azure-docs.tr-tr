---
title: Gerekli parametrelerin toplanması
services: cognitive-services
author: aahill
manager: nitinme
description: Tüm Bilişsel Hizmetler kapları için parametreler
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 85c67384c38376dc6f5701cf26e879d6e1b76c7f
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875068"
---
## <a name="gathering-required-parameters"></a>Gerekli parametrelerin toplanması

Gerekli olan tüm Bilişsel Hizmetler 'kapları için üç birincil parametre vardır. Son kullanıcı lisans sözleşmesi (EULA) değeri ile `accept`birlikte olmalıdır. Ayrıca, hem Bir Endpoint URL'si hem de API Anahtarı gereklidir.

### <a name="endpoint-uri-endpoint_uri"></a>Uç NOKTA URI`{ENDPOINT_URI}`

**Endpoint** URI değeri, ilgili Bilişsel Hizmet kaynağının Azure portalı *Genel Bakış* sayfasında kullanılabilir. *Genel Bakış* sayfasına gidin, Bitiş Noktası'nın `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> üzerine gidin ve bir simge görüntülenir. Gerektiğinde kopyalayın ve kullanın.

![Daha sonra kullanmak için uç nokta uri toplamak](../media/overview-endpoint-uri.png)

### <a name="keys-api_key"></a>Anahtar`{API_KEY}`

Bu anahtar kapsayıcıyı başlatmak için kullanılır ve ilgili Bilişsel Hizmet kaynağının Azure portalının Keys sayfasında kullanılabilir. *Keys* sayfasına gidin ve simgeye `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> tıklayın.

![Daha sonra kullanmak için iki anahtardan birini alın](../media/keys-copy-api-key.png)

> [!IMPORTANT]
> Bu abonelik anahtarları Bilişsel Hizmet API'nıza erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Örneğin Azure Key Vault'u kullanarak güvenli bir şekilde saklayın. Ayrıca bu anahtarları düzenli olarak yenilemenizi öneririz. API araması yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yenilerken, hizmete sürekli erişim için ikinci anahtarı kullanabilirsiniz.