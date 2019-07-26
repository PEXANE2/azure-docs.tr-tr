---
title: Azure Active Directory Kimlik Koruması ile kullanıcıların engellemesini kaldırma | Microsoft Docs
description: Azure Active Directory Kimlik Koruması ilkesi tarafından engellenen kullanıcıların engellemesini kaldırma hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: c28e30b8e44b6888cdb7416b9c7b563b955a68ce
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335370"
---
# <a name="how-to-unblock-users"></a>Nasıl Yapılır: Kullanıcıların engelini kaldırma

Azure Active Directory Kimlik Koruması, yapılandırılan koşullar karşılandıysa kullanıcıları engelleyecek ilkeler yapılandırabilirsiniz. Genellikle, engellenen bir Kullanıcı yardım masasının engeli kaldırılmış hale gelir. Bu makalede, engellenen bir kullanıcının engellemesini kaldırmak için gerçekleştirebileceğiniz adımlar açıklanmaktadır.

## <a name="determine-the-reason-for-blocking"></a>Engelleme nedenini belirleme

Bir kullanıcının engellemesini kaldırmaya yönelik ilk adım olarak, bir sonraki adımlarınız kendisine bağlı olduğundan kullanıcıyı engellediği ilke türünü belirlemeniz gerekir.
Azure Active Directory Kimlik Koruması, bir Kullanıcı bir oturum açma risk ilkesi veya bir Kullanıcı risk ilkesi tarafından engelleniyor olabilir.

Bir kullanıcıyı, oturum açma girişimi sırasında kullanıcıya sunulan iletişim kutusunda bulunan başlığından engellenen ilke türünü alabilirsiniz:

| İlke | Kullanıcı iletişim kutusu |
| --- | --- |
| Oturum açma riski |![Engellenen oturum açma](./media/howto-unblock-user/02.png) |
| Kullanıcı riski |![Engellenen Hesap](./media/howto-unblock-user/104.png) |

Tarafından engellenen bir Kullanıcı:

* Bir oturum açma risk ilkesi, şüpheli oturum açma olarak da bilinir
* Risk altında hesap olarak da bilinen bir Kullanıcı risk ilkesi

## <a name="unblocking-suspicious-sign-ins"></a>Şüpheli oturum açma işlemlerinin engellemesini kaldırma

Şüpheli bir oturum açma engelini kaldırmak için aşağıdaki seçeneklere sahipsiniz:

1. **Tanıdık bir konumdan veya cihazdan oturum açma** -, engellenen şüpheli oturum açma işlemlerinin yaygın bir nedeni, bilmediğiniz konumlardan veya cihazlardan gelen oturum açma çabalarıdır. Kullanıcılarınız, bilinen bir konumdan veya cihazdan oturum açmaya çalışırken bunun engelleme nedeni olup olmadığını hızlı bir şekilde tespit edebilir.
2. **Ilkeden hariç tut** -oturum açma ilkenizin geçerli yapılandırmasının belirli kullanıcılar için sorunlara neden olduğunu düşünüyorsanız kullanıcıları bu bilgisayardan dışlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).
3. **Ilkeyi devre dışı bırak** -ilke yapılandırmanızın tüm kullanıcılarınız için sorunlara neden olduğunu düşünüyorsanız, ilkeyi devre dışı bırakabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).

## <a name="unblocking-accounts-at-risk"></a>Risk altındaki hesapların engellemesini kaldırma

Bir hesabın riskini ortadan kaldırmak için aşağıdaki seçeneklere sahip olursunuz:

1. **Parolayı sıfırlama** -kullanıcının parolasını sıfırlayabilirsiniz. 
2. **Tüm risk olaylarını kapat** -Kullanıcı risk ilkesi, erişimi engellemek için yapılandırılmış Kullanıcı risk düzeyine ulaşıldığında kullanıcıyı engeller. Bildirilen risk olaylarını el ile kapatarak bir kullanıcının risk düzeyini azaltabilirsiniz. 
3. **Ilkeden hariç tut** -oturum açma ilkenizin geçerli yapılandırmasının belirli kullanıcılar için sorunlara neden olduğunu düşünüyorsanız kullanıcıları bu bilgisayardan dışlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).
4. **Ilkeyi devre dışı bırak** -ilke yapılandırmanızın tüm kullanıcılarınız için sorunlara neden olduğunu düşünüyorsanız, ilkeyi devre dışı bırakabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).

## <a name="next-steps"></a>Sonraki adımlar
 
Azure AD Kimlik Koruması hakkında daha fazla bilgi edinmek istiyor musunuz? [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)kullanıma alma.
