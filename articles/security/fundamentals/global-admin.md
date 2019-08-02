---
title: Tüm Azure yöneticileri için MFA 'yı etkinleştir
description: Genel yöneticiyi etkinleştirme Kılavuzu
ms.service: security
ms.subservice: security-fundamentals
author: barclayn
manager: barbkess
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: 702feded450aed7368836c7eff799e969b9f396b
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727311"
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Abonelik yöneticileri için Multi-Factor Authentication 'ı (MFA) zorlama

Yöneticilerinizi genel yönetici hesabınız dahil olmak üzere oluşturduğunuzda, çok güçlü kimlik doğrulama yöntemleri kullanmanız önemlidir.

Exchange Yöneticisi veya parola Yöneticisi gibi belirli yönetici rollerini, gerektiğinde BT personelinin Kullanıcı hesaplarına atayarak gündelik yönetim gerçekleştirebilirsiniz.
Ayrıca, yöneticileriniz için [Azure Multi-Factor Authentication 'ı (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) etkinleştirmek, Kullanıcı oturum açma işlemlerine ve işlemlerine ikinci bir güvenlik katmanı ekler. Azure MFA Ayrıca, güvenliği aşılmış bir kimlik bilgisinin kuruluş verilerine erişimine sahip olma olasılığını azaltmaya yardımcı olur.

Örneğin: Kullanıcılarınız için Azure MFA 'yı zorunlu kılabilirsiniz ve doğrulama olarak bir telefon araması veya kısa mesaj kullanacak şekilde yapılandırırsınız. Kullanıcının kimlik bilgileri tehlikeye atılırsa saldırgan, kullanıcının telefonuna erişemeyeceği için hiçbir kaynağa erişemez. Çok sayıda kimlik koruması olmayan kuruluşlar, kimlik bilgisi hırsızlığı saldırısında daha açıktır ve bu da veri güvenliğinin aşılmasına yol açabilir.

Şirket içinde tüm kimlik doğrulama denetimini tutmak isteyen kuruluşlar için bir alternatif, "MFA on-premises" olarak da adlandırılan [Azure Multi-Factor Authentication sunucusu](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server)kullanmaktır. Bu yöntemi kullanarak, MFA sunucusunu şirket içinde tutarken çok faktörlü kimlik doğrulamasını zorunlu kılabilirsiniz.

Kuruluşunuzun yönetim ayrıcalıklarına sahip olduğunu denetlemek için aşağıdaki Microsoft Azure AD v2 PowerShell komutunu kullanabilirsiniz:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>MFA etkinleştiriliyor

Devam etmeden önce [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) 'nın nasıl çalıştığını gözden geçirin.

Kullanıcıların Azure Multi-Factor Authentication içeren lisansları olduğu sürece, Azure MFA’yı etkinleştirmek için hiçbir işlem yapmanız gerekmez. Her kullanıcı için iki aşamalı doğrulama istemeye başlayabilirsiniz. Azure MFA'yı etkinleştiren lisanslar şunlardır:

- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Kullanıcılar için iki aşamalı doğrulamayı açma

Bir kullanıcı veya grup için, Azure MFA 'yı kullanmaya başlamak üzere [iki adımlı doğrulama gerektirme](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) bölümünde listelenen yordamlardan birini kullanın. Tüm oturum açma işlemleri için iki aşamalı doğrulamayı zorunlu kılabilirsiniz veya yalnızca sizin için önemli olduğunda iki adımlı doğrulama gerektirecek şekilde koşullu erişim ilkeleri oluşturabilirsiniz.

