---
title: Self servis parola sıfırlama lisansı - Azure Active Directory
description: Azure AD self servis parola sıfırlama lisans gereksinimleri
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 598f3bd8500a59cd41cc4126915e6cccbd4fb2f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848570"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Azure AD self servis parola sıfırlama gereksinimleri

Azure Active Directory (Azure AD) çeşitli sürümlerde gelir: Ücretsiz, Premium P1 ve Premium P2. Azure AD'nin farklı sürümlerinde bulunan değişiklik, sıfırlama, kilidini açma ve geri yazma dahil olmak üzere self servis parola sıfırlamayı oluşturan birkaç farklı özellik vardır. Bu makalede, farklılıkları açıklamaya çalışır. Her Azure REKLAM sürümünde yer alan özelliklerin daha fazla ayrıntısını [Azure Active Directory fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/active-directory/)bulabilirsiniz.

## <a name="compare-editions-and-features"></a>Sürümleri ve özellikleri karşılaştırın

Azure AD self servis parola sıfırlama kullanıcı başına lisanslıdır, uyumluluk kuruluşlarının kullanıcılarına uygun lisansı ataması gerekir.

* Bulut kullanıcıları için Self Servis Şifre Değişikliği
   * Yalnızca **bulut kullanıcısıyım** ve şifremi biliyorum.
      * Şifremi yeni bir şeyle **değiştirmek** istiyorum.
   * Bu işlev, Azure AD'nin tüm sürümlerinde yer almaktadır.

* Bulut kullanıcıları için Self Servis Parola Sıfırlama
   * Yalnızca **bulut kullanıcısıyım** ve şifremi unuttum.
      * Bildiğim bir şey için şifremi **sıfırlamak** istiyorum.
   * Bu işlev Azure AD Premium P1 veya P2, Microsoft 365 Business veya Office 365'e dahildir.

* **Şirket içi geri yazıyla** Self Servis Şifre Sıfırlama/Değiştirme/Açma
   * Karma bir **kullanıcıyım** şirket içi Active Directory kullanıcı hesabım Azure AD Connect'i kullanarak Azure AD hesabımla senkronize edilir. Şifremi değiştirmek, şifremi unuttum veya kilitlendim.
      * Parolamı değiştirmek veya bildiğim bir şeyle sıfırlamak veya hesabımın kilidini açmak **istiyorum ve** bu değişikliğin şirket içi Active Directory'ye yeniden eşitlebilmesini istiyorum.
   * Bu işlev Azure AD Premium P1 veya P2 veya Microsoft 365 Business'a dahildir.

> [!WARNING]
> Bağımsız Office 365 lisanslama planları *"Şirket içi yazma yla Self Servis Parola Sıfırlama/Değiştirme/Açma"yı desteklemez* ve bu işlevselliğin çalışması için Azure AD Premium P1, Premium P2 veya Microsoft 365 Business içeren bir plan gerektirir.
>

Maliyetler de dahil olmak üzere ek lisans bilgileri aşağıdaki sayfalarda bulunabilir:

* [Azure Active Directory fiyatlandırma sitesi](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory özellikleri ve özellikleri](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 İş hizmeti açıklaması](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Grup veya kullanıcı tabanlı lisanslamayı etkinleştirme

Azure AD artık grup tabanlı lisanslamayı destekliyor. Yöneticiler, lisansları teker teker atamak yerine bir kullanıcı grubuna toplu olarak atayabilir. Daha fazla bilgi için [bkz.](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses)

Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Lisansın bir kullanıcıya atanabilmesi için, yöneticinin kullanıcıüzerindeki **Kullanım konumu** özelliğini belirtmesi gerekir. Lisansların atanması Azure portalındaki **Kullanıcı** > **Profili** > **Ayarları** bölümü altında yapılabilir. *Grup lisans atamasını kullandığınızda, kullanım yeri belirtilmeyen kullanıcılar dizinin konumunu devralır.*

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Sanırım bir şey kırıldı. SSPR'yi nasıl giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
