---
title: Parola koruma işlemleri ve raporları-Azure Active Directory
description: Azure AD parola koruması dağıtım sonrası işlemleri ve raporlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: d8602a9c5b69c47c2f663dab461394ef5e1a9bed
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75762867"
---
# <a name="azure-ad-password-protection-operational-procedures"></a>Azure AD parola koruması işlem yordamları

Şirket içi [Azure AD parola korumasını yüklemeyi](howto-password-ban-bad-on-premises-deploy.md) tamamladıktan sonra, Azure Portal yapılandırılması gereken birkaç öğe vardır.

## <a name="configure-the-custom-banned-password-list"></a>Özel yasaklanmış parola listesini yapılandırın

Kuruluşunuzun yasaklanmış parola listesini özelleştirme adımları için [özel yasaklanmış Password listesini yapılandırma](howto-password-ban-bad-configure.md) makalesindeki yönergeleri izleyin.

## <a name="enable-password-protection"></a>Parola korumasını etkinleştir

1. [Azure Portal](https://portal.azure.com) oturum açın ve **Azure Active Directory**, **güvenlik**,**kimlik doğrulama yöntemleri**ve **parola koruması**' na gidin.
1. **Windows Server 'Da parola korumasını etkinleştir Active Directory** **Evet** olarak ayarlayın
1. [Dağıtım kılavuzunda](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy)bahsedildiği gibi, başlangıçta **modunun** **Denetim** olarak ayarlanması önerilir
   * Özelliği rahatladıktan sonra, **modu** **Zorlanmış** olarak değiştirebilirsiniz
1. **Kaydet**’e tıklayın

![Azure portal Azure AD parola koruma bileşenlerini etkinleştirme](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Denetim modu

Denetim modu, "durum" modunda yazılımı çalıştırmanın bir yolu olarak hazırlanmıştır. Her DC Aracısı hizmeti, geçerli etkin ilkeye göre gelen bir parolayı değerlendirir. Geçerli ilke denetim modunda olacak şekilde yapılandırıldıysa, "kötü" parolalar olay günlüğü iletilerine neden olur ancak kabul edilir. Bu, denetim ve zorlama modu arasındaki tek farktır; diğer tüm işlemler aynı şekilde çalışır.

> [!NOTE]
> Microsoft, ilk dağıtımın ve testin her zaman denetim modunda başlamasını önerir. Daha sonra, var olan bir işlem işleminin zorla modu etkinleştirildikten sonra olumsuz olup olmayacağını tahmin etmek için olay günlüğündeki olayların izlenmesi gerekir.

## <a name="enforce-mode"></a>Zorla modu

Zorla modu, son yapılandırma olarak tasarlanmıştır. Yukarıdaki denetim modunda olduğu gibi, her DC Aracısı hizmeti, gelen parolaları geçerli etkin ilkeye göre değerlendirir. Ancak zorla modu etkinleştirilirse, ilkeye göre güvenli olmayan olarak kabul edilen bir parola reddedilir.

Azure AD parola koruması DC Aracısı tarafından zorlama modunda bir parola reddedildiğinde, bir son kullanıcı tarafından görülen görünür etki, parolasının geleneksel şirket içi parola karmaşıklığı zorlaması tarafından reddedilip reddedildikleri ile aynıdır. Örneğin, bir Kullanıcı Windows logon\parola değiştirme ekranında aşağıdaki geleneksel hata iletisini görebilir:

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

Bu ileti birkaç olası sonuç için yalnızca bir örnektir. Belirli bir hata iletisi, güvenli olmayan bir parola ayarlamaya çalışan gerçek yazılıma veya senaryoya bağlı olarak değişebilir.

Etkilenen son kullanıcıların, yeni gereksinimleri anlamak ve güvenli parolalar seçebilmeleri için BT personeli ile çalışması gerekebilir.

> [!NOTE]
> Zayıf bir parola reddedildiğinde, Azure AD parola korumasının istemci makine tarafından görüntülenmekte olan belirli bir hata iletisi üzerinde denetimi yoktur.

## <a name="enable-mode"></a>Modu etkinleştir

Bu ayar, varsayılan etkin (Evet) durumunda bırakılmalıdır. Bu ayarın devre dışı olarak yapılandırılması (Hayır), tüm dağıtılan Azure AD parola koruma DC aracılarının tüm parolaların olduğu gibi kabul edildiği bir quiescent moduna geçmesine neden olur ve hiçbir doğrulama etkinliği yürütülecektir (örneğin, hatta denetim olayları). yayılır).

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola koruması için izleme](howto-password-ban-bad-on-premises-monitor.md)
