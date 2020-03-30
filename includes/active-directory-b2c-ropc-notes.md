---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/27/2020
ms.author: mimart
ms.openlocfilehash: f4944881e6eff3e9a56a74bc997c280f9fff6be8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187022"
---
## <a name="ropc-flow-notes"></a>ROPC akış notları
Azure Etkin Dizin B2C'de (Azure AD B2C) aşağıdaki seçenekler desteklenir:

- **Yerel İstemci**: Kimlik doğrulama sırasında kullanıcı etkileşimi, kod kullanıcı tarafındaki bir aygıtta çalıştığında gerçekleşir. Cihaz, Android ve iOS gibi yerel bir işletim sisteminde çalışan bir mobil uygulama olabilir.
- **Ortak istemci akışı**: Yalnızca bir uygulama tarafından toplanan kullanıcı kimlik bilgileri API çağrısında gönderilir. Uygulamanın kimlik bilgileri gönderilmez.
- **Yeni talepler ekle**: Kimlik belirteci içeriği yeni talepler eklemek için değiştirilebilir.

Aşağıdaki akışlar desteklenmez:

- **Sunucudan sunucuya**: Kimlik koruma sistemi, etkileşimin bir parçası olarak arayan kişiden (yerel istemci) toplanan güvenilir bir IP adresine ihtiyaç duyar. Sunucu tarafındaki API çağrısında yalnızca sunucunun IP adresi kullanılır. Başarısız kimlik doğrulamadinamik bir eşik aşılırsa, kimlik koruma sistemi bir saldırgan olarak yinelenen bir IP adresi tanımlayabilir.
- **Gizli istemci akışı**: Uygulama istemci kimliği doğrulanır, ancak uygulama sırrı doğrulanmaz.

ROPC akışını kullanırken aşağıdakileri göz önünde bulundurun:

- Kullanıcı etkileşimi gerektiren kimlik doğrulama akışında herhangi bir kesinti olduğunda ROPC çalışmaz. Örneğin, bir parolanın süresi dolduğunda veya değiştirilmesi gerektiğinde, çok faktörlü kimlik doğrulama sı veya oturum açma sırasında daha fazla bilgi nin toplanması gerektiğinde (örneğin, kullanıcı onayı).
- ROPC yalnızca yerel hesapları destekler. Kullanıcılar Microsoft, Google+, Twitter, AD-FS veya Facebook gibi federe kimlik sağlayıcılarıyla oturum açamıyor.
- Oturum Yönetimi, beni oturumda tutmak (KMSI) dahil olmak üzere, geçerli değildir.
