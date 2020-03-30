---
title: Azure AD uygulamaları için gelişmiş SAML belirteç sertifikası imzalama seçenekleri
description: Azure Active Directory'deki önceden tümleşik uygulamalar için SAML belirtecinde gelişmiş sertifika imzalama seçeneklerini nasıl kullanacağınızı öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: mimart
ms.reviewer: jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc911ff06208b1fd0af7651c8274a45c958bf0cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159208"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Azure Active Directory'deki galeri uygulamaları için SAML belirtecinde gelişmiş sertifika imzalama seçenekleri

Bugün Azure Active Directory (Azure AD), Azure Active Directory App Gallery'de binlerce önceden tümleşik uygulamayı destekler. Uygulamaların 500'den [fazlası, NetSuite](https://azuremarketplace.microsoft.com/marketplace/apps/aad.netsuite) uygulaması gibi [Security Assertion Markup Language](https://wikipedia.org/wiki/Security_Assertion_Markup_Language) (SAML) 2.0 protokolünü kullanarak tek oturum açma özelliğini destekler. Bir müşteri SAML kullanarak Azure AD aracılığıyla bir uygulamaya kimlik doğruladığında, Azure AD uygulamaya bir belirteç gönderir (http post aracılığıyla). Uygulama daha sonra bir kullanıcı adı ve parola için istek yerine müşteri oturum vermek için belirteci doğrular ve kullanır. Bu SAML belirteçleri, Azure AD'de ve belirli standart algoritmalarda oluşturulan benzersiz sertifikayla imzalanır.

Azure AD, galeri uygulamaları için varsayılan ayarlardan bazılarını kullanır. Varsayılan değerler, uygulamanın gereksinimlerine göre ayarlanır.

Azure AD'de sertifika imzalama seçeneklerini ve sertifika imzalama algoritmasını ayarlayabilirsiniz.

## <a name="certificate-signing-options"></a>Sertifika imzalama seçenekleri

Azure AD üç sertifika imzalama seçeneğini destekler:

* **Sign SAML iddiası**. Bu varsayılan seçenek galeri uygulamalarının çoğu için ayarlanır. Bu seçeneği belirlerseniz, Kimlik Sağlayıcısı olarak Azure AD (IdP), UYGULAMANIN [X.509](https://wikipedia.org/wiki/X.509) sertifikasıyla SAML iddiasını ve sertifikasını imzalar.

* **Oturum SAML yanıtı**. Bu seçeneği seçerseniz, IdP olarak Azure AD, uygulamanın X.509 sertifikasıyla SAML yanıtını imzalar.

* **Sign SAML yanıtı ve iddiası**. Bu seçeneği belirlerseniz, IdP olarak Azure AD, uygulamanın X.509 sertifikasıyla tüm SAML belirteciyle imzalar.

## <a name="certificate-signing-algorithms"></a>Sertifika imzalama algoritmaları

Azure AD, SAML yanıtını imzalamak için iki imzalama algoritmasını veya güvenli karma algoritmaları (SHA) destekler:

* **SHA-256**. Azure AD, SAML yanıtını imzalamak için bu varsayılan algoritmayı kullanır. Bu en yeni algoritma ve SHA-1 daha güvenlidir. Uygulamaların çoğu SHA-256 algoritmasını destekler. Bir uygulama imzalama algoritması olarak yalnızca SHA-1'i destekliyorsa, bunu değiştirebilirsiniz. Aksi takdirde, SAML yanıtını imzalamak için SHA-256 algoritmasını kullanmanızı öneririz.

* **SHA-1**. Bu algoritma daha eski ve SHA-256'dan daha az güvenli olarak kabul edilir. Bir uygulama yalnızca bu imzalama algoritmasını destekliyorsa, Bu seçeneği **İmzalama Algoritması** açılır listesinde seçebilirsiniz. Azure AD daha sonra SHA-1 algoritmasıyla SAML yanıtını imzalar.

## <a name="change-certificate-signing-options-and-signing-algorithm"></a>Sertifika imzalama seçeneklerini ve imzalama algoritması değiştirme

Bir uygulamanın SAML sertifika imzalama seçeneklerini ve sertifika imzalama algoritmasını değiştirmek için, söz konusu uygulamayı seçin:

1. Azure [Active Directory portalında](https://aad.portal.azure.com/)hesabınızda oturum açın. **Azure Etkin Dizin yöneticisi merkezi** sayfası görüntülenir.
1. Sol bölmede **Kurumsal uygulamalar**’ı seçin. Hesabınızdaki kurumsal uygulamaların listesi görüntülenir.
1. Bir uygulama seçin. Uygulama için genel bir sayfa görüntülenir.

   ![Örnek: Uygulamaya genel bakış sayfası](./media/certificate-signing-options/application-overview-page.png)

Ardından, bu uygulama için SAML belirtecisertifika imzalama seçeneklerini değiştirin:

1. Uygulamaya genel bakış sayfasının sol bölmesinde **Tek oturum açma'yı**seçin.
1. **SAML ile Tek Oturum Açma - Önizleme** sayfası görünüyorsa, adım 5'e gidin.
1. Tek **bir oturum açma yöntemi** sayfası seç seçeneği görünmüyorsa, bu sayfayı görüntülemek için tek oturum **açma modlarını değiştir'i** seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında, varsa **SAML'yi** seçin. **(SAML** kullanılamıyorsa, uygulama SAML'yi desteklemez ve bu yordamın ve makalenin geri kalanını göz ardı edebilirsiniz.)
1. **SAML ile Tek Oturum Açma - Önizleme** sayfasında **SAML İmza Sertifikası** başlığını bulun ve **Edit** simgesini (kalem) seçin. **SAML İmzaSertifikası** sayfası görüntülenir.

   ![Örnek: SAML imzalama sertifikası sayfası](./media/certificate-signing-options/saml-signing-page.png)

1. **İmzalama Seçeneği** açılır listesinde, **Sign SAML yanıtını,** **Sign SAML iddiasını**veya Sign **SAML yanıtını ve iddiasını**seçin. Bu seçeneklerin açıklamaları, sertifika imzalama [seçeneklerinde](#certificate-signing-options)bu makalede daha önce görünür.
1. İmza **Algoritması** açılır listesinde **SHA-1** veya **SHA-256'yı**seçin. Bu seçeneklerin açıklamaları, sertifika imzalama [algoritmaları](#certificate-signing-algorithms) bölümünde bu makalede daha önce görünür.
1. Seçeneklerinizden memnunsanız, yeni SAML imzalama sertifikası ayarlarını uygulamak için **Kaydet'i** seçin. Aksi takdirde, değişiklikleri atmak için **X'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory App Gallery'de olmayan uygulamalarda tek oturum açma yapılandırma](configure-federated-single-sign-on-non-gallery-applications.md)
* [Sorun giderme SAML tabanlı tek oturum açma](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
