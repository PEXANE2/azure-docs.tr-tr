---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/27/2020
ms.author: mimart
ms.openlocfilehash: f4944881e6eff3e9a56a74bc997c280f9fff6be8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78187022"
---
## <a name="ropc-flow-notes"></a>ROPC akış notları
Azure Active Directory B2C (Azure AD B2C) ' de, aşağıdaki seçenekler desteklenir:

- **Yerel istemci**: kimlik doğrulaması sırasında Kullanıcı etkileşimi, kod bir Kullanıcı tarafı cihazda çalıştırıldığında oluşur. Cihaz, Android ve iOS gibi yerel bir işletim sisteminde çalışan bir mobil uygulama olabilir.
- **Ortak istemci akışı**: API çağrısında yalnızca bir uygulama tarafından toplanan kullanıcı kimlik bilgileri gönderilir. Uygulamanın kimlik bilgileri gönderilmez.
- **Yeni talepler Ekle**: kimlik belirteci içerikleri yeni talepler eklemek için değiştirilebilir.

Aşağıdaki akışlar desteklenmez:

- **Sunucudan sunucuya**: kimlik koruma sisteminin, etkileşimin bir parçası olarak çağırandan (yerel istemci) toplanan GÜVENILIR bir IP adresi olması gerekir. Sunucu tarafı API çağrısında yalnızca sunucunun IP adresi kullanılır. Başarısız kimlik doğrulamaları için dinamik bir eşik aşılırsa, kimlik koruma sistemi bir saldırgan olarak yinelenen bir IP adresini tanımlayabilir.
- **Gizli istemci akışı**: uygulama istemci kimliği onaylanır, ancak uygulama gizli anahtarı doğrulanmaz.

ROPC akışını kullanırken aşağıdakileri göz önünde bulundurun:

- Kimlik doğrulama akışında Kullanıcı etkileşimi gerektiren herhangi bir kesinti olduğunda ROPC çalışmaz. Örneğin, bir parolanın süresi dolduğunda veya değiştirilmesi gerektiğinde, çok faktörlü kimlik doğrulaması gerekir ya da oturum açma sırasında daha fazla bilginin toplanması gerekir (örneğin, kullanıcı onayı).
- ROPC yalnızca yerel hesapları destekler. Kullanıcılar Microsoft, Google +, Twitter, AD-FS veya Facebook gibi federal kimlik sağlayıcılarıyla oturum açamaz.
- Oturumumu Açık bırak (KMSI) dahil olmak üzere oturum yönetimi geçerli değildir.
