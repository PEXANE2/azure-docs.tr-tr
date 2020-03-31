---
title: Desteklenen Microsoft Graph işlemleri
titleSuffix: Azure AD B2C
description: Kullanıcılar, kullanıcı akışları, kimlik sağlayıcıları, özel ilkeler, ilke anahtarları ve daha fazlası dahil olmak üzere Azure AD B2C kaynaklarının yönetimi için desteklenen Microsoft Graph işlemlerinin dizini.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 27fe1a41365d96a4179f8c659b63dc22c7b9fc93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184257"
---
# <a name="microsoft-graph-operations-available-for-azure-ad-b2c"></a>Azure AD B2C için Microsoft Graph işlemleri kullanılabilir

Aşağıdaki Microsoft Graph API işlemleri, kullanıcılar, kimlik sağlayıcıları, kullanıcı akışları, özel ilkeler ve ilke anahtarları da dahil olmak üzere Azure AD B2C kaynaklarının yönetimi için desteklenir.

Aşağıdaki bölümlerdeki her bağlantı, bu işlem için Microsoft Graph API başvurusu içindeki ilgili sayfayı hedefler.

## <a name="user-management"></a>Kullanıcı yönetimi

- [Kullanıcıları listele](https://docs.microsoft.com/graph/api/user-list)
- [Tüketici kullanıcısı oluşturma](https://docs.microsoft.com/graph/api/user-post-users)
- [Bir kullanıcı alma](https://docs.microsoft.com/graph/api/user-get)
- [Kullanıcıyı güncelleştirme](https://docs.microsoft.com/graph/api/user-update)
- [Kullanıcı sil](https://docs.microsoft.com/graph/api/user-delete)

Microsoft Graph API ile Azure AD B2C kullanıcı hesaplarını yönetme hakkında daha fazla bilgi için [bkz.](manage-user-accounts-graph-api.md)

## <a name="identity-providers-user-flow"></a>Kimlik sağlayıcıları (kullanıcı akışı)

Azure AD B2C kiracınızda kullanıcı akışlarınızın kullanabileceği kimlik sağlayıcılarını yönetin.

- [Azure AD B2C kiracısında kayıtlı kimlik sağlayıcılarını listele](https://docs.microsoft.com/graph/api/identityprovider-list)
- [Kimlik sağlayıcısı oluşturma](https://docs.microsoft.com/graph/api/identityprovider-post-identityproviders)
- [Bir kimlik sağlayıcısı alma](https://docs.microsoft.com/graph/api/identityprovider-get)
- [Kimlik sağlayıcısını güncelleştirme](https://docs.microsoft.com/graph/api/identityprovider-update)
- [Kimlik sağlayıcısını silme](https://docs.microsoft.com/graph/api/identityprovider-delete)

## <a name="user-flow"></a>Kullanıcı akışı

Kaydolma, kaydolma, birleştirilmiş kayıt ve oturum açma, parola sıfırlama ve profil güncelleştirmesi için önceden oluşturulmuş ilkeleri yapılandırın.

- [Kullanıcı akışlarını listele](https://docs.microsoft.com/graph/api/identityuserflow-list)
- [Kullanıcı akışı oluşturma](https://docs.microsoft.com/graph/api/identityuserflow-post-userflows)
- [Kullanıcı akışı elde edin](https://docs.microsoft.com/graph/api/identityuserflow-get)
- [Kullanıcı akışını silme](https://docs.microsoft.com/graph/api/identityuserflow-delete)

## <a name="custom-policies"></a>Özel ilkeler

Aşağıdaki işlemler, [özel ilkeler](custom-policy-overview.md)olarak bilinen Azure AD B2C Güven Çerçevesi ilkelerinizi yönetmenize olanak tanır.

- [Kiracıda yapılandırılan tüm güven çerçevesi ilkelerini listele](https://docs.microsoft.com/graph/api/trustframework-list-trustframeworkpolicies)
- [Güven çerçevesi ilkesi oluşturma](https://docs.microsoft.com/graph/api/trustframework-post-trustframeworkpolicy)
- [Varolan bir güven çerçeve ilkesinin özelliklerini okuma](https://docs.microsoft.com/graph/api/trustframeworkpolicy-get)
- [Güven çerçevesi ilkesini güncelleştirin veya oluşturun.](https://docs.microsoft.com/graph/api/trustframework-put-trustframeworkpolicy)
- [Varolan bir güven çerçeve ilkesini silme](https://docs.microsoft.com/graph/api/trustframeworkpolicy-delete)

## <a name="policy-keys"></a>İlke anahtarları

Kimlik Deneyimi Çerçevesi, bileşenler arasında güven oluşturmak için özel bir ilkede başvurulan sırları depolar. Bu sırlar simetrik veya asimetrik anahtarlar/değerler olabilir. Azure portalında, bu varlıklar **İlke anahtarları**olarak gösterilir.

Microsoft Graph API'deki ilke anahtarları için üst düzey [kaynak, Güvenilen Framework Keyset'tir.](https://docs.microsoft.com/graph/api/resources/trustframeworkkeyset) Her **Anahtar kümesi** en az bir **Anahtar**içerir. Bir anahtar oluşturmak için önce boş bir anahtar kümesi oluşturun ve ardından anahtar kümesinde bir anahtar oluşturun. El ile gizli bir sır oluşturabilir, sertifika yükleyebilir veya PKCS12 tuşu oluşturabilirsiniz. Anahtar oluşturulan bir sır, tanımladığınız bir dize (Facebook uygulama sırrı gibi) veya yüklediğiniz bir sertifika olabilir. Bir anahtar kümesinde birden çok anahtar varsa, anahtarlardan yalnızca biri etkindir.

### <a name="trust-framework-policy-keyset"></a>Güven Çerçevesi ilkesi anahtar kümesi

- [Güven çerçevesi anahtar kümelerini listele](https://docs.microsoft.com/graph/api/trustframework-list-keysets)
- [Güven çerçevesi anahtar kümeleri oluşturma](https://docs.microsoft.com/graph/api/trustframework-post-keysets)
- [Bir tuş seti alın](https://docs.microsoft.com/graph/api/trustframeworkkeyset-get)
- [Güven çerçevesi anahtar kümelerini güncelleştirme](https://docs.microsoft.com/graph/api/trustframeworkkeyset-update)
- [Güven çerçevesi anahtar kümelerini silme](https://docs.microsoft.com/graph/api/trustframeworkkeyset-delete)

### <a name="trust-framework-policy-key"></a>Çerçeve ilkesine güven ilkesi anahtarı

- [Tuş setinde şu anda etkin anahtar alın](https://docs.microsoft.com/graph/api/trustframeworkkeyset-getactivekey)
- [Tuş setinde anahtar oluşturma](https://docs.microsoft.com/graph/api/trustframeworkkeyset-generatekey)
- [Dize tabanlı gizli yükleme](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadsecret)
- [X.509 sertifikası yükleme](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadcertificate)
- [PKCS12 biçim sertifikası yükleme](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadpkcs12)

## <a name="applications"></a>Uygulamalar

- [Uygulamaları listeleme](https://docs.microsoft.com/graph/api/application-list)
- [Uygulama oluşturma](https://docs.microsoft.com/graph/api/resources/application)
- [Uygulamayı güncelleştirme](https://docs.microsoft.com/graph/api/application-update)
- [Hizmet OluşturmaPrincipal](https://docs.microsoft.com/graph/api/resources/serviceprincipal)
- [Oauth2Permission Grant oluşturma](https://docs.microsoft.com/graph/api/resources/oauth2permissiongrant)
- [Uygulamayı silme](https://docs.microsoft.com/graph/api/application-delete)

## <a name="application-extension-properties"></a>Uygulama uzantısı özellikleri

- [Liste uzantısı özellikleri](https://docs.microsoft.com/graph/api/application-list-extensionproperty)

Azure AD B2C, kullanıcı başına 100 özel öznitelik tutabilen bir dizin sağlar. Kullanıcı akışları için bu uzantı özellikleri [Azure portalı kullanılarak yönetilir.](custom-policy-custom-attributes.md) Özel ilkeler için Azure AD B2C, ilke uzantısı özelliğine ilk kez bir değer yazdığında sizin için bir özellik oluşturur.

## <a name="audit-logs"></a>Denetim günlükleri

- [Denetim günlüklerini listele](https://docs.microsoft.com/graph/api/directoryaudit-list)

Microsoft Graph API ile Azure AD B2C denetim günlüklerine erişim hakkında daha fazla bilgi için [bkz.](view-audit-logs.md)
