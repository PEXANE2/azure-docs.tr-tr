---
title: B2B işbirliği kullanıcı iddiaları eşleme - Azure Active Directory
description: Azure Etkin Dizin (Azure AD) B2B kullanıcıları için SAML belirtecinde verilen kullanıcı taleplerini özelleştirin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/06/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3508865d9f89501cf70e09087c6a609beb6662b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74273194"
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Azure Active Directory'de B2B işbirliği kullanıcı talepleri haritalama

Azure Etkin Dizin (Azure AD), B2B işbirliği kullanıcıları için SAML belirtecinde verilen talepleri özelleştirmeyi destekler. Bir kullanıcı uygulamaya kimlik doğruladığında, Azure AD, uygulamayla ilgili bilgileri (veya talepleri) içeren bir SAML belirteci yayınlar. Varsayılan olarak, bu kullanıcının kullanıcı adı, e-posta adresi, ad ve soyadı içerir.

Azure [portalında,](https://portal.azure.com)SAML jetonunda uygulamaya gönderilen talepleri görüntüleyebilir veya edebilirsiniz. Ayarlara erişmek için, tek oturum açma > **tek**oturum açma için yapılandırılan uygulama > **Azure Active Directory** > **Enterprise uygulamalarını** seçin. **Kullanıcı Öznitelikleri** bölümündeki SAML belirteç ayarlarına bakın.

![UI'deki SAML belirteç özniteliklerini gösterir](media/claims-mapping/view-claims-in-saml-token.png)

SAML belirtecinde verilen talepleri düzenlemeniz in iki olası nedeni olabilir:

1. Uygulama, farklı bir talep URI'leri veya talep değerleri kümesi gerektirir.

2. Uygulama, NameIdentifier'in Azure AD'de depolanan kullanıcı ana adı (UPN) dışında bir şey olduğunu iddia etmesini gerektirir.

Taleplerin nasıl ekleyip düzenleyeceğiniz hakkında bilgi için, [Azure Etkin Dizini'ndeki kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](../develop/active-directory-saml-claims-customization.md)ye bakın.

B2B işbirliği kullanıcıları için, nameid ve UPN çapraz kiracı eşleme güvenlik nedenleriyle engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- B2B işbirliği kullanıcı özellikleri hakkında bilgi için azure [etkin dizin B2B işbirliği kullanıcısının özellikleri](user-properties.md)bölümüne bakın.
- B2B işbirliği kullanıcıları için kullanıcı belirteçleri hakkında bilgi için Bkz. [Azure AD B2B işbirliğinde kullanıcı belirteçlerini anlayın.](user-token.md)

