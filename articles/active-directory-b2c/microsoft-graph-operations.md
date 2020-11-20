---
title: Desteklenen Microsoft Graph işlemleri
titleSuffix: Azure AD B2C
description: Kullanıcılar, Kullanıcı akışları, kimlik sağlayıcıları, özel ilkeler, ilke anahtarları ve daha fazlası dahil Azure AD B2C kaynaklarının yönetimi için desteklenen Microsoft Graph işlemlerinin bir dizini.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/15/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9db46d13c9a798204958a7c295df9cca169fc08f
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94954044"
---
# <a name="microsoft-graph-operations-available-for-azure-ad-b2c"></a>Azure AD B2C için kullanılabilir Microsoft Graph işlemler

Aşağıdaki Microsoft Graph API işlemleri, kullanıcılar, kimlik sağlayıcıları, Kullanıcı akışları, özel ilkeler ve ilke anahtarları dahil Azure AD B2C kaynaklarının yönetimi için desteklenir.

Aşağıdaki bölümlerde yer alan her bağlantı, bu işlem için Microsoft Graph API başvurusu içinde karşılık gelen sayfayı hedefler.

## <a name="user-management"></a>Kullanıcı yönetimi

- [Kullanıcıları listele](/graph/api/user-list)
- [Tüketici kullanıcısı oluşturma](/graph/api/user-post-users)
- [Kullanıcı al](/graph/api/user-get)
- [Kullanıcı güncelleştirme](/graph/api/user-update)
- [Kullanıcı silme](/graph/api/user-delete)

Microsoft Graph API 'siyle Azure AD B2C Kullanıcı hesaplarını yönetme hakkında daha fazla bilgi için bkz. [Microsoft Graph ile Azure AD B2C Kullanıcı hesaplarını yönetme](manage-user-accounts-graph-api.md).

## <a name="user-phone-number-management"></a>Kullanıcı telefon numarası yönetimi

- [Ekle](https://docs.microsoft.com/graph/api/authentication-post-phonemethods)
- [Al](https://docs.microsoft.com/graph/api/b2cauthenticationmethodspolicy-get)
- [Güncelleştirme](https://docs.microsoft.com/graph/api/b2cauthenticationmethodspolicy-update)
- [Silme](https://docs.microsoft.com/graph/api/phoneauthenticationmethod-delete)

Kullanıcının Microsoft Graph API 'SI ile oturum açma telefon numarasını yönetme hakkında daha fazla bilgi için bkz. [B2C kimlik doğrulama yöntemleri](https://docs.microsoft.com/graph/api/resources/b2cauthenticationmethodspolicy).

## <a name="identity-providers-user-flow"></a>Kimlik sağlayıcıları (Kullanıcı akışı)

Azure AD B2C kiracınızdaki Kullanıcı akışlarınız için kullanılabilen kimlik sağlayıcılarını yönetin.

- [Azure AD B2C kiracısında kayıtlı kimlik sağlayıcılarını listeleyin](/graph/api/identityprovider-list)
- [Kimlik sağlayıcısı oluşturma](/graph/api/identityprovider-post-identityproviders)
- [Kimlik sağlayıcısı al](/graph/api/identityprovider-get)
- [Kimlik sağlayıcısını Güncelleştir](/graph/api/identityprovider-update)
- [Bir kimlik sağlayıcısını silme](/graph/api/identityprovider-delete)

## <a name="user-flow"></a>Kullanıcı akışı

Kaydolma, oturum açma, Birleşik kayıt ve oturum açma, parola sıfırlama ve profil güncelleştirme için önceden oluşturulmuş ilkeleri yapılandırın.

- [Kullanıcı akışlarını listeleme](/graph/api/identityuserflow-list)
- [Kullanıcı akışı oluşturma](/graph/api/identityuserflow-post-userflows)
- [Kullanıcı akışı al](/graph/api/identityuserflow-get)
- [Kullanıcı akışını silme](/graph/api/identityuserflow-delete)

## <a name="custom-policies"></a>Özel ilkeler

Aşağıdaki işlemler, [özel ilkeler](custom-policy-overview.md)olarak bilinen Azure AD B2C güven çerçevesi ilkelerini yönetmenizi sağlar.

- [Bir kiracıda yapılandırılmış tüm güven çerçevesi ilkelerini listeleme](/graph/api/trustframework-list-trustframeworkpolicies)
- [Güven çerçevesi İlkesi Oluştur](/graph/api/trustframework-post-trustframeworkpolicy)
- [Mevcut bir güven çerçevesi ilkesinin özelliklerini okuyun](/graph/api/trustframeworkpolicy-get)
- [Güven altyapısı ilkesini güncelleştirin veya oluşturun.](/graph/api/trustframework-put-trustframeworkpolicy)
- [Mevcut bir güven çerçevesini silme ilkesi](/graph/api/trustframeworkpolicy-delete)

## <a name="policy-keys"></a>İlke anahtarları

Kimlik deneyimi çerçevesi, bileşenler arasında güven sağlamak için bir özel ilkede başvurulan gizli dizileri depolar. Bu gizlilikler simetrik veya asimetrik anahtarlar/değerler olabilir. Azure portal, bu varlıklar **ilke anahtarları** olarak gösterilir.

Microsoft Graph API 'sindeki ilke anahtarları için en üst düzey kaynak, [güvenilir çerçeve anahtar kümesi](/graph/api/resources/trustframeworkkeyset)' dir. Her **anahtar kümesi** en az bir **anahtar** içerir. Anahtar oluşturmak için, önce boş bir anahtar kümesi oluşturun ve sonra anahtar kümesi içinde bir anahtar oluşturun. El ile gizli dizi oluşturabilir, bir sertifikayı veya PKCS12 anahtarını karşıya yükleyebilirsiniz. Anahtar, oluşturulan bir gizli dizi, tanımladığınız bir dize (Facebook uygulaması gizli dizisi gibi) veya karşıya yüklediğiniz bir sertifika olabilir. Bir anahtar kümesi birden fazla anahtara sahipse, anahtarlardan yalnızca biri etkin olur.

### <a name="trust-framework-policy-keyset"></a>Güven çerçevesi ilkesi anahtar kümesi

- [Güven çerçevesi anahtar kümelerini listeleyin](/graph/api/trustframework-list-keysets)
- [Güven çerçevesi anahtar kümeleri oluşturma](/graph/api/trustframework-post-keysets)
- [Anahtar kümesi al](/graph/api/trustframeworkkeyset-get)
- [Güven çerçevesi anahtar kümelerini güncelleştirme](/graph/api/trustframeworkkeyset-update)
- [Güven çerçevesi anahtar kümelerini silme](/graph/api/trustframeworkkeyset-delete)

### <a name="trust-framework-policy-key"></a>Güven çerçevesi ilke anahtarı

- [Anahtar kümesi içinde şu anda etkin olan anahtarı al](/graph/api/trustframeworkkeyset-getactivekey)
- [Anahtar kümesi içinde anahtar oluşturma](/graph/api/trustframeworkkeyset-generatekey)
- [Dize tabanlı gizli dizi yükle](/graph/api/trustframeworkkeyset-uploadsecret)
- [X. 509.440 sertifikasını karşıya yükle](/graph/api/trustframeworkkeyset-uploadcertificate)
- [PKCS12 biçim sertifikasını karşıya yükle](/graph/api/trustframeworkkeyset-uploadpkcs12)

## <a name="applications"></a>Uygulamalar

- [Uygulamaları listeleme](/graph/api/application-list)
- [Uygulama oluşturma](/graph/api/resources/application)
- [Uygulamayı güncelleştirme](/graph/api/application-update)
- [Hizmet sorumlusu oluşturma](/graph/api/resources/serviceprincipal)
- [Oauth2Permission Grant oluştur](/graph/api/resources/oauth2permissiongrant)
- [Uygulamayı Sil](/graph/api/application-delete)

## <a name="application-extension-properties"></a>Uygulama uzantısı özellikleri

- [Uzantı özelliklerini listele](/graph/api/application-list-extensionproperty)

Azure AD B2C, Kullanıcı başına 100 özel öznitelik tutan bir dizin sağlar. Kullanıcı akışları için, bu uzantı özellikleri [Azure Portal kullanılarak yönetilir](custom-policy-custom-attributes.md). Özel ilkeler için Azure AD B2C, ilke uzantı özelliğine ilk kez bir değer yazdığında, sizin için özelliği oluşturur.

## <a name="audit-logs"></a>Denetim günlükleri

- [Denetim günlüklerini listeleme](/graph/api/directoryaudit-list)

Microsoft Graph API 'siyle Azure AD B2C denetim günlüklerine erişme hakkında daha fazla bilgi için bkz. [Azure AD B2C denetim günlüklerine erişme](view-audit-logs.md).