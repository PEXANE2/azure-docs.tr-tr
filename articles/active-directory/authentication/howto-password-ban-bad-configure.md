---
title: Azure AD 'de zayıf parolalar nasıl yapılır-Azure Active Directory
description: Azure AD dinamik olarak yasaklanmış passwrords ile envirnonment 'ten zayıf parolalar
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: c58371edffa4400c2c2b3efd37c3ce6d7c3cad95
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74847924"
---
# <a name="configuring-the-custom-banned-password-list"></a>Özel yasaklanmış parola listesini yapılandırma

Birçok kuruluş, kullanıcılarını bir okul, spor ekibi veya çok büyük kişi gibi yaygın yerel kelimeleri kullanarak, tahmin etmek kolay bırakarak parolalar oluşturur. Microsoft 'un özel yasaklanmış parola listesi, kuruluşların, kullanıcılar ve yöneticiler bir parolayı değiştirmeyi veya sıfırlamayı denediklerinde, küresel yasaklanmış parola listesine ek olarak, değerlendirmek ve engellemek üzere dizeler eklemesine olanak tanır.

## <a name="add-to-the-custom-list"></a>Özel listeye ekle

Özel yasaklanmış parola listesini yapılandırmak için Azure Active Directory Premium P1 veya P2 lisansı gerekir. Azure Active Directory lisanslama hakkında daha ayrıntılı bilgi için [Azure Active Directory fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

1. [Azure Portal](https://portal.azure.com) oturum açın ve **Azure Active Directory**, **kimlik doğrulama yöntemleri**ve **parola koruması**' na gidin.
1. **Özel liste Uygula**seçeneğini **Evet**olarak ayarlayın.
1. **Özel yasaklanmış parola listesine**dizeler ekleme, satır başına bir dize
   * Özel yasaklanmış parola listesi, en fazla 1000 terim içerebilir.
   * Özel yasaklanmış parola listesi, büyük/küçük harfe duyarsızdır.
   * Özel yasaklanmış parola listesi, ortak karakter değişimini dikkate alır.
      * Örnek: "o" ve "0" ya da "a" ve "\@"
   * En küçük dize uzunluğu dört karakterdir ve en yüksek değer 16 karakterdir.
1. Tüm dizeleri ekledikten sonra **Kaydet**' e tıklayın.

> [!NOTE]
> Özel yasaklanmış parola listesindeki güncelleştirmelerin uygulanması birkaç saat sürebilir.

> [!NOTE]
> Özel yasaklanmış parola listesi en fazla 1000 terim olacak şekilde sınırlıdır. Parolaların son derece büyük listesini engellemek için tasarlanmamıştır. Özel yasaklanmış parola listesinin avantajlarından tamamen yararlanmak için, Microsoft, önce özel yasaklanmış parola listesinin (bkz. [özel yasaklanmış parola listesi](concept-password-ban-bad.md#custom-banned-password-list)) ve ayrıca parola değerlendirme algoritması 'nın amaçlanan tasarımını ve kullanımını incelemenizi ve anlamanızı önerir (bkz. [parolalar nasıl değerlendirilir](concept-password-ban-bad.md#how-are-passwords-evaluated)).

![Azure portal kimlik doğrulama yöntemleri altında özel yasaklanmış parola listesini değiştirin](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Nasıl çalışır

Bir kullanıcı veya yönetici bir Azure AD parolasını sıfırlayıp değiştirdiğinde, bir listede olmadığı onaylanacak şekilde yasaklanmış parola listelerinden akar. Bu denetim, Azure AD kullanılarak ayarlanan veya değiştirilen tüm parolalara dahildir.

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Bir Kullanıcı yasaklanmış olabilecek bir parolayı sıfırlamaya çalıştığında, aşağıdaki hata iletilerinden birini görürler:

* Ne yazık ki parolanız, parolanızın kolayca tahmin edilebilir olmasını sağlayan bir sözcük, tümcecik veya model içerir. Lütfen farklı bir parola ile yeniden deneyin.
* Ne yazık ki, yöneticiniz tarafından engellenmiş sözcükler veya karakterler içerdiğinden bu parolayı kullanamazsınız. Lütfen farklı bir parola ile yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

[Yasaklanmış parola listelerine kavramsal genel bakış](concept-password-ban-bad.md)

[Azure AD parola korumasına kavramsal genel bakış](concept-password-ban-bad-on-premises.md)

[Yasaklanmış parola listeleriyle şirket içi tümleştirmeyi etkinleştir](howto-password-ban-bad-on-premises.md)
