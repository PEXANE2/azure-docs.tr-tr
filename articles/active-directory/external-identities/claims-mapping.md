---
title: B2B işbirliği Kullanıcı talepleri eşleme-Azure Active Directory
description: Azure Active Directory (Azure AD) B2B kullanıcıları için SAML belirtecinde verilen kullanıcı taleplerini özelleştirin.
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
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87910260"
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Azure Active Directory 'de B2B işbirliği Kullanıcı talepleri eşleme

Azure Active Directory (Azure AD), B2B işbirliği kullanıcıları için SAML belirtecinde verilen talepleri özelleştirmeyi destekler. Kullanıcı uygulamanın kimliğini doğruladığında, Azure AD, uygulamayı benzersiz bir şekilde tanımlayan kullanıcı hakkında bilgi (veya talepler) içeren bir SAML belirteci yayınlar. Bu, varsayılan olarak kullanıcının Kullanıcı adını, e-posta adresini, adını ve soyadını içerir.

[Azure Portal](https://portal.azure.com), SAML belirtecinde gönderilen talepleri uygulamaya görüntüleyebilir veya düzenleyebilirsiniz. Ayarlara erişmek için, çoklu oturum **Azure Active Directory**açma  >  > **Çoklu oturum**açma için yapılandırılmış uygulamayı >**Kurumsal uygulamalar** Azure Active Directory seçin. **Kullanıcı öznitelikleri** bölümünde SAML belirteci ayarları ' na bakın.

![Kullanıcı arabirimindeki SAML belirteci özniteliklerini gösterir](media/claims-mapping/view-claims-in-saml-token.png)

SAML belirtecinde verilen talepleri düzenlemeniz gerekebilecek iki nedeni vardır:

1. Uygulama farklı bir talep URI 'si veya talep değerleri kümesi gerektiriyor.

2. Uygulama, NameIdentifier talebinin Azure AD 'de depolanan kullanıcı asıl adı (UPN) dışında bir şey olmasını gerektirir.

Talepler ekleme ve düzenleme hakkında daha fazla bilgi için bkz. [Azure Active Directory 'de kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](../develop/active-directory-saml-claims-customization.md).

B2B işbirliği kullanıcıları için, eşleme NameID ve UPN çapraz kiracı, güvenlik nedenleriyle engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- B2B işbirliği Kullanıcı özellikleri hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B işbirliği kullanıcısının özellikleri](user-properties.md).
- B2B işbirliği kullanıcıları için Kullanıcı belirteçleri hakkında daha fazla bilgi için bkz. [Azure AD B2B işbirliği içindeki Kullanıcı belirteçlerini anlama](user-token.md).

