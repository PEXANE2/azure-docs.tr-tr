---
title: Desteklenen Microsoft Graph işlemleri
titleSuffix: Azure AD B2C
description: Kullanıcılar, Kullanıcı akışları, kimlik sağlayıcıları, özel ilkeler, ilke anahtarları ve daha fazlası dahil Azure AD B2C kaynaklarının yönetimi için desteklenen Microsoft Graph işlemlerinin bir dizini.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/20/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5eeae1ab6866435311eeec944b4a51ecf2793dee
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77522977"
---
# <a name="microsoft-graph-operations-available-for-azure-ad-b2c"></a>Azure AD B2C için kullanılabilir Microsoft Graph işlemler

Aşağıdaki Microsoft Graph API işlemleri, kullanıcılar, kimlik sağlayıcıları, Kullanıcı akışları, özel ilkeler ve ilke anahtarları dahil Azure AD B2C kaynaklarının yönetimi için desteklenir.

Aşağıdaki bölümlerde yer alan her bağlantı, bu işlem için Microsoft Graph API başvurusu içinde karşılık gelen sayfayı hedefler.

## <a name="user-management"></a>Kullanıcı yönetimi

- [Kullanıcıları listeleme](https://docs.microsoft.com/graph/api/user-list)
- [Tüketici kullanıcısı oluşturma](https://docs.microsoft.com/graph/api/user-post-users)
- [Kullanıcı al](https://docs.microsoft.com/graph/api/user-get)
- [Kullanıcı güncelleştirme](https://docs.microsoft.com/graph/api/user-update)
- [Kullanıcı silme](https://docs.microsoft.com/graph/api/user-delete)

Microsoft Graph API 'siyle Azure AD B2C Kullanıcı hesaplarını yönetme hakkında daha fazla bilgi için bkz. [Microsoft Graph ile Azure AD B2C Kullanıcı hesaplarını yönetme](manage-user-accounts-graph-api.md).

## <a name="identity-providers-user-flow"></a>Kimlik sağlayıcıları (Kullanıcı akışı)

Azure AD B2C kiracınızdaki Kullanıcı akışlarınız için kullanılabilen kimlik sağlayıcılarını yönetin.

- [Azure AD B2C kiracısında kayıtlı kimlik sağlayıcılarını listeleyin](https://docs.microsoft.com/graph/api/identityprovider-list)
- [Kimlik sağlayıcısı oluşturma](https://docs.microsoft.com/graph/api/identityprovider-post-identityproviders)
- [Kimlik sağlayıcısı al](https://docs.microsoft.com/graph/api/identityprovider-get)
- [Kimlik sağlayıcısını Güncelleştir](https://docs.microsoft.com/graph/api/identityprovider-update)
- [Bir kimlik sağlayıcısını silme](https://docs.microsoft.com/graph/api/identityprovider-delete)

## <a name="user-flow"></a>Kullanıcı akışı

Kaydolma, oturum açma, Birleşik kayıt ve oturum açma, parola sıfırlama ve profil güncelleştirme için önceden oluşturulmuş ilkeleri yapılandırın.

- [Kullanıcı akışlarını listeleme](https://docs.microsoft.com/graph/api/identityuserflow-list)
- [Kullanıcı akışı oluşturma](https://docs.microsoft.com/graph/api/identityuserflow-post-userflows)
- [Kullanıcı akışı al](https://docs.microsoft.com/graph/api/identityuserflow-get)
- [Kullanıcı akışını silme](https://docs.microsoft.com/graph/api/identityuserflow-delete)

## <a name="custom-policies"></a>Özel ilkeler

Aşağıdaki işlemler, [özel ilkeler](custom-policy-overview.md)olarak bilinen Azure AD B2C güven çerçevesi ilkelerini yönetmenizi sağlar.

- [Bir kiracıda yapılandırılmış tüm güven çerçevesi ilkelerini listeleme](https://docs.microsoft.com/graph/api/trustframework-list-trustframeworkpolicies)
- [Güven çerçevesi İlkesi Oluştur](https://docs.microsoft.com/graph/api/trustframework-post-trustframeworkpolicy)
- [Mevcut bir güven çerçevesi ilkesinin özelliklerini okuyun](https://docs.microsoft.com/graph/api/trustframeworkpolicy-get)
- [Güven altyapısı ilkesini güncelleştirin veya oluşturun.](https://docs.microsoft.com/graph/api/trustframework-put-trustframeworkpolicy)
- [Mevcut bir güven çerçevesini silme ilkesi](https://docs.microsoft.com/graph/api/trustframeworkpolicy-delete)

## <a name="policy-keys"></a>İlke anahtarları

Kimlik deneyimi çerçevesi, bileşenler arasında güven sağlamak için bir özel ilkede başvurulan gizli dizileri depolar. Bu gizlilikler simetrik veya asimetrik anahtarlar/değerler olabilir. Azure portal, bu varlıklar **ilke anahtarları**olarak gösterilir.

Microsoft Graph API 'sindeki ilke anahtarları için en üst düzey kaynak, [güvenilir çerçeve anahtar kümesi](https://docs.microsoft.com/graph/api/resources/trustframeworkkeyset)' dir. Her **anahtar kümesi** en az bir **anahtar**içerir. Anahtar oluşturmak için, önce boş bir anahtar kümesi oluşturun ve sonra anahtar kümesi içinde bir anahtar oluşturun. El ile gizli dizi oluşturabilir, bir sertifikayı veya PKCS12 anahtarını karşıya yükleyebilirsiniz. Anahtar, oluşturulan bir gizli dizi, tanımladığınız bir dize (Facebook uygulaması gizli dizisi gibi) veya karşıya yüklediğiniz bir sertifika olabilir. Bir anahtar kümesi birden fazla anahtara sahipse, anahtarlardan yalnızca biri etkin olur.

### <a name="trust-framework-policy-keyset"></a>Güven çerçevesi ilkesi anahtar kümesi

- [Güven çerçevesi anahtar kümelerini listeleyin](https://docs.microsoft.com/graph/api/trustframework-list-keysets)
- [Güven çerçevesi anahtar kümeleri oluşturma](https://docs.microsoft.com/graph/api/trustframework-post-keysets)
- [Anahtar kümesi al](https://docs.microsoft.com/graph/api/trustframeworkkeyset-get)
- [Güven çerçevesi anahtar kümelerini güncelleştirme](https://docs.microsoft.com/graph/api/trustframeworkkeyset-update)
- [Güven çerçevesi anahtar kümelerini silme](https://docs.microsoft.com/graph/api/trustframeworkkeyset-delete)

### <a name="trust-framework-policy-key"></a>Güven çerçevesi ilke anahtarı

- [Anahtar kümesi içinde şu anda etkin olan anahtarı al](https://docs.microsoft.com/graph/api/trustframeworkkeyset-getactivekey)
- [Anahtar kümesi içinde anahtar oluşturma](https://docs.microsoft.com/graph/api/trustframeworkkeyset-generatekey)
- [Dize tabanlı gizli dizi yükle](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadsecret)
- [X. 509.440 sertifikasını karşıya yükle](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadcertificate)
- [PKCS12 biçim sertifikasını karşıya yükle](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadpkcs12)

## <a name="applications"></a>Uygulamalar

- [Uygulamaları listeleme](https://docs.microsoft.com/graph/api/application-list)
- [Uygulama oluşturma](https://docs.microsoft.com/graph/api/resources/application)
- [Uygulamayı Güncelleştir](https://docs.microsoft.com/graph/api/application-update)
- [Hizmet sorumlusu oluşturma](https://docs.microsoft.com/graph/api/resources/serviceprincipal)
- [Oauth2Permission Grant oluştur](https://docs.microsoft.com/graph/api/resources/oauth2permissiongrant)
- [Uygulamayı Sil](https://docs.microsoft.com/graph/api/application-delete)

## <a name="application-extension-properties"></a>Uygulama uzantısı özellikleri

- [Uzantı özelliklerini listele](https://docs.microsoft.com/graph/api/application-list-extensionproperty)

Azure AD B2C, Kullanıcı başına 100 özel öznitelik tutan bir dizin sağlar. Kullanıcı akışları için, bu uzantı özellikleri [Azure Portal kullanılarak yönetilir](custom-policy-custom-attributes.md). Özel ilkeler için Azure AD B2C, ilke uzantı özelliğine bir değer yazdığında, özelliği sizin için oluşturur.

## <a name="audit-logs"></a>Denetim günlükleri

- [Denetim günlüklerini listeleme](https://docs.microsoft.com/graph/api/directoryaudit-list)

Microsoft Graph API 'siyle Azure AD B2C denetim günlüklerine erişme hakkında daha fazla bilgi için bkz. [Azure AD B2C denetim günlüklerine erişme](view-audit-logs.md).
