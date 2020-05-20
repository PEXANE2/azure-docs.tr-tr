---
title: Başkalarıyla işbirliği yapma-Soru-Cevap Oluşturma
description: ''
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: ef0823fc60424dc861ab70f4112e4689c1b97fb0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665987"
---
# <a name="collaborate-with-other-authors-and-editors"></a>Diğer yazarlarla ve düzenleyicilerle işbirliği yapın

Soru-Cevap Oluşturma kaynağına yerleştirilmiş rol tabanlı erişim denetimi (RBAC) kullanarak diğer yazarlarla ve düzenleyicilerle işbirliği yapın.

## <a name="access-is-provided-on-the-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağında erişim sağlanır

Tüm izinler Soru-Cevap Oluşturma kaynağına yerleştirilmiş izinlerle denetlenir. Bu izinler, okuma, yazma, yayımlama ve tam erişim için hizalanır.

Bu RBAC özelliği şunları içerir:
* Azure Active Directory (AAD), sahipler ve katkıda bulunanlar için anahtar tabanlı kimlik doğrulama ile %100 geriye dönük olarak uyumludur. Müşteriler, isteklerinde anahtar tabanlı kimlik doğrulaması veya RBAC tabanlı kimlik doğrulaması kullanabilir.
* Denetim, Bilgi Bankası düzeyinde değil kaynak düzeyinde olduğundan, kaynak içindeki tüm bilgi bankalarına yazar ve düzenleyiciler hızlıca ekleyin.

## <a name="access-is-provided-by-a-defined-role"></a>Erişim, tanımlı bir rol tarafından sağlanır

[!INCLUDE [RBAC permissions table](../includes/role-based-access-control.md)]

## <a name="authentication-flow"></a>Kimlik doğrulaması akışı

Aşağıdaki diyagramda, Soru-Cevap Oluşturma portalında oturum açmak ve yazma API 'Lerini kullanmak için yazarın perspektifinden akış gösterilmektedir.

> [!div class="mx-imgBorder"]
> ![Aşağıdaki diyagramda, Soru-Cevap Oluşturma portalında oturum açmak ve yazma API 'Lerini kullanmak için yazarın perspektifinden akış gösterilmektedir.](../media/qnamaker-how-to-collaborate-knowledge-base/rbac-flow-from-portal-to-service.png)

|Adımlar|Açıklama|
|--|--|
|1|Portal Soru-Cevap Oluşturma kaynak için belirteç alır.|
|2|Portal anahtarlar yerine belirteci geçirerek uygun Soru-Cevap Oluşturma yazma API 'sini (APıM) çağırır.|
|3|Soru-Cevap Oluşturma API'si belirteci doğrular.|
|4 |Soru-Cevap Oluşturma API'si QnAMaker hizmetini çağırır.|

Yazma API 'Lerini çağırmak istiyorsanız] (.. /How-To/collaborate-knowledge-base.md), kimlik doğrulamasını ayarlama hakkında daha fazla bilgi edinin.

## <a name="authenticate-by-qna-maker-portal"></a>Soru-Cevap Oluşturma portalına göre kimlik doğrulaması

Soru-Cevap Oluşturma portalını kullanarak yazar ve işbirliği yaparsanız, [ortak çalışan için kaynağa uygun rolü](../How-To/collaborate-knowledge-base.md)ekledikten sonra, soru-cevap oluşturma portalı tüm erişim izinlerini yönetir.

## <a name="authenticate-by-qna-maker-apis-and-sdks"></a>Soru-Cevap Oluşturma API 'Ler ve SDK 'Lar tarafından kimlik doğrulama

API 'Leri kullanarak, REST veya SDK 'Lar aracılığıyla yazar ve işbirliği yaparsanız, kimlik doğrulamasını yönetmek için [bir hizmet sorumlusu oluşturmanız](../../authentication.md#assign-a-role-to-a-service-principal) gerekir.

## <a name="next-step"></a>Sonraki adım

* [Diller](design-language-culture.md) ve [istemci uygulamaları](integration-with-other-applications.md) için Bilgi Bankası tasarlama