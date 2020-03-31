---
title: Azure Active Directory B2C'deki kullanıcı hesaplarına genel bakış
description: Azure Active Directory B2C'de kullanılabilecek kullanıcı hesabı türleri hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9630e1f23b6595ca690ecafcf0c4b9bfff603f2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185667"
---
# <a name="overview-of-user-accounts-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'deki kullanıcı hesaplarına genel bakış

Azure Etkin Dizin B2C'de (Azure AD B2C) oluşturulabilecek çeşitli hesap türleri vardır. Azure Etkin Dizini, Active Directory B2B ve Active Directory B2C, kullanılabilecek kullanıcı hesabı türlerinde paylaşır.

Aşağıdaki hesap türleri kullanılabilir:

- **Çalışma hesabı** - Bir iş hesabı kiracıdaki kaynaklara erişebilir ve yönetici rolüyle kiracıları yönetebilir.
- **Konuk hesabı** - Konuk hesabı yalnızca bir Microsoft hesabı veya uygulamalara erişmek veya kiracıları yönetmek için kullanılabilecek bir Azure Etkin Dizin kullanıcısı olabilir.
- **Tüketici hesabı** - Azure AD B2C'ye kaydettiğiniz uygulamaların kullanıcısı tarafından bir tüketici hesabı kullanılır. Tüketici hesapları tarafından oluşturulabilir:
  - Azure AD B2C uygulamasında kaydolma kullanıcı akışından geçen kullanıcı
  - Microsoft Grafik API'yi kullanma
  - Azure portalını kullanma

## <a name="work-account"></a>Çalışma hesabı

Azure AD'yi temel alan tüm kiracılar için aynı şekilde bir iş hesabı oluşturulur. Bir iş hesabı oluşturmak için [Quickstart: Azure Etkin Dizin'e yeni kullanıcılar ekleme](../active-directory/fundamentals/add-users-azure-active-directory.md)'de bu bilgileri kullanabilirsiniz. Azure portalındaki Yeni **kullanıcı** seçimi kullanılarak bir iş hesabı oluşturulur.

Yeni bir iş hesabı eklediğinizde, aşağıdaki yapılandırma ayarlarını göz önünde bulundurmanız gerekir:

- **Ad** ve **Kullanıcı Adı** - **Ad** özelliği, kullanıcının verilen ve soyadını içerir. **Kullanıcı adı,** kullanıcının oturum açmaiçin girdiği tanımlayıcıdır. Kullanıcı adı tam etki alanını içerir. Kullanıcı adının alan adı bölümü, *your-domain.onmicrosoft.com*ilk varsayılan etki alanı adı veya *contoso.com*gibi doğrulanmış, federe olmayan [özel alan](../active-directory/fundamentals/add-custom-domain.md) adı olmalıdır.
- **Profil** - Hesap, kullanıcı verilerinin profili ile ayarlanır. Ad, soyad, iş unvanı ve bölüm adı girme fırsatınız var. Hesap oluşturulduktan sonra profili edinebilirsiniz.
- **Gruplar** - Aynı anda bir dizi kullanıcıya veya cihaza lisans veya izin atama gibi yönetim görevlerini gerçekleştirmek için bir grup kullanın. Yeni hesabı kiracınızdaki varolan bir [gruba](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) koyabilirsiniz.
- **Dizin rolü** - Kullanıcı hesabının kiracınızdaki kaynaklara sahip olduğu erişim düzeyini belirtmeniz gerekir. Aşağıdaki izin düzeyleri kullanılabilir:

    - **Kullanıcı** - Kullanıcılar atanan kaynaklara erişebilir, ancak kiracı kaynaklarının çoğunu yönetemez.
    - **Global yönetici** - Global yöneticiler tüm kiracı kaynakları üzerinde tam denetime sahiptir.
    - **Sınırlı yönetici** - Kullanıcının yönetim rolünü veya rollerini seçin. Seçilebilen roller hakkında daha fazla bilgi için Azure [Etkin Dizini'nde yönetici rolleri atama](../active-directory/users-groups-roles/directory-assign-admin-roles.md)'ya bakın.

### <a name="create-a-work-account"></a>İş hesabı oluşturma

Yeni bir iş hesabı oluşturmak için aşağıdaki bilgileri kullanabilirsiniz:

- [Azure portalında](../active-directory/fundamentals/add-users-azure-active-directory.md)
- [Microsoft Graph](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0)

### <a name="update-a-user-profile"></a>Kullanıcı profilini güncelleştirme

Bir kullanıcının profilini güncelleştirmek için aşağıdaki bilgileri kullanabilirsiniz:

- [Azure portalında](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
- [Microsoft Graph](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0)

### <a name="reset-a-password-for-a-user"></a>Kullanıcı için parolayı sıfırlama

Bir kullanıcının parolasını sıfırlamak için aşağıdaki bilgileri kullanabilirsiniz:

- [Azure portalında](../active-directory/fundamentals/active-directory-users-reset-password-azure-portal.md)
- [Microsoft Graph](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0)

## <a name="guest-user"></a>Konuk kullanıcı

Harici kullanıcıları konuk kullanıcı olarak kiracınıza davet edebilirsiniz. Konuk kullanıcıyı Azure AD B2C kiracınıza davet etmek için tipik bir senaryo, yönetim sorumluluklarını paylaşmaktır. Konuk hesabı kullanma örneği için, [Azure Etkin Dizin B2B işbirliği kullanıcısının Özellikleri](../active-directory/b2b/user-properties.md)bölümüne bakın.

Bir konuk kullanıcıyı kiracınıza davet ettiğinizde, alıcının e-posta adresini ve daveti açıklayan bir iletiyi sağlarsınız. Davet bağlantısı, kullanıcıyı **Başlat düğmesinin** seçildiği ve izinlerin gözden geçirilmesinin kabul edildiği onay sayfasına götürür. Gelen kutusu e-posta adresine eklenmemişse, kullanıcı davet edilen kimlik bilgilerini kullanarak bir Microsoft sayfasına giderek onay sayfasına gidebilir. Kullanıcı daha sonra daveti e-postadaki bağlantıyı tıklatarak kullanmak zorunda kalır. Örneğin: `https://myapps.microsoft.com/B2CTENANTNAME`.

Konuk kullanıcıyı davet etmek için [Microsoft Graph API'yi](https://docs.microsoft.com/graph/api/invitation-post?view=graph-rest-beta) de kullanabilirsiniz.

## <a name="consumer-user"></a>Tüketici kullanıcısı

Tüketici kullanıcı, Azure AD B2C tarafından güvenli hale gelen uygulamalarda oturum açabilir, ancak Azure portalı gibi Azure kaynaklarına erişemez. Tüketici kullanıcı, Facebook veya Twitter gibi yerel bir hesabı veya federe hesapları kullanabilir. Bir tüketici hesabı, [Kaydolma veya Kaydolma kullanıcı akışı](user-flow-overview.md)kullanılarak, Microsoft Graph API'sını kullanarak veya Azure portalını kullanarak oluşturulur.

Özel kullanıcı öznitelikleri kullanılarak bir tüketici kullanıcı hesabı oluşturulduğunda toplanan verileri belirtebilirsiniz. Daha fazla bilgi için Azure [Etkin Dizin B2C'de özel öznitelikleri tanımlayın'](user-flow-custom-attributes.md)a bakın.

Tüketici hesaplarını yönetme hakkında daha fazla bilgi için [bkz.](manage-user-accounts-graph-api.md)

### <a name="migrate-consumer-user-accounts"></a>Tüketici kullanıcı hesaplarını geçirme

Varolan tüketici kullanıcı hesaplarını herhangi bir kimlik sağlayıcısından Azure AD B2C'ye geçirmeniz gerekebilir. Daha fazla bilgi için [bkz.](user-migration.md)
