---
author: mmacy
ms.author: marsma
ms.date: 11/25/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: e54301e01d25fb075325ef34522daa5f1b691dd5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98760675"
---
## <a name="conditional-access-and-claims-challenges"></a>Koşullu erişim ve talep sorunları

Belirteçleri sessizce alırken, erişmeye çalıştığınız bir API 'nin MFA İlkesi gibi [koşullu erişim talep zorluğu](../articles/active-directory/develop/v2-conditional-access-dev-guide.md) gerektiğinde uygulamanız hatalar alabilir.

Bu hatayı işleme deseninin, MSAL kullanarak etkileşimli bir şekilde belirteç edinmesi. Bu, kullanıcıdan istemde bulunur ve gerekli koşullu erişim ilkesini karşılamak için bu fırsata yanıt verir.

Belirli durumlarda, koşullu erişim gerektiren bir API 'yi çağırırken API 'den hata ile bir talep zorluğu alabilirsiniz. Örneğin, koşullu erişim ilkesinde yönetilen bir cihaz (Intune) varsa, hata AADSTS53000 gibi bir şey olacaktır [: cihazınızın bu kaynağa erişmek için yönetilmesi gerekir](../articles/active-directory/develop/reference-aadsts-error-codes.md) veya benzer bir şeydir. Bu durumda, kullanıcıdan uygun ilkeyi karşılaması istenir diye, talepleri alma çağrısında geçirebilirsiniz.
