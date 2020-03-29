---
title: Active Directory kimlik doğrulaması - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL için Azure Veritabanı ile Azure Active Directory ile kimlik doğrulama kavramları hakkında bilgi edinin - Tek Sunucu
author: lfittl
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: ec853657d6dd1f3b019d8a414cfa28edc1083b29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74769923"
---
# <a name="use-azure-active-directory-for-authenticating-with-postgresql"></a>PostgreSQL ile kimlik doğrulaması için Azure Etkin Dizini'ni kullanın

Microsoft Azure Active Directory (Azure AD) kimlik doğrulaması, Azure AD'de tanımlanan kimlikleri kullanarak PostgreSQL için Azure Veritabanı'na bağlanma mekanizmasıdır.
Azure AD kimlik doğrulaması ile veritabanı kullanıcı kimliklerini ve diğer Microsoft hizmetlerini merkezi bir konumda yönetebilirsiniz ve bu da izin yönetimini kolaylaştırır.

> [!IMPORTANT]
> PostgreSQL için Azure Veritabanı için Azure AD kimlik doğrulaması şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure AD kullanmanın yararları şunlardır:

- Azure Hizmetleri'ndeki kullanıcıların özgünasyonu tek tip bir şekilde
- Parola ilkelerinin yönetimi ve parola döndürme tek bir yerde
- Parolaları depolama gereksinimini ortadan kaldırabilecek Azure Active Directory tarafından desteklenen birden çok kimlik doğrulama biçimi
- Müşteriler veritabanı izinlerini harici (Azure AD) gruplarını kullanarak yönetebilir.
- Azure AD kimlik doğrulaması, veritabanı düzeyindekimlikleri doğrulamak için PostgreSQL veritabanı rollerini kullanır
- PostgreSQL için Azure Veritabanı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulama desteği

Azure Active Directory kimlik doğrulamasını yapılandırmak ve kullanmak için aşağıdaki işlemi kullanın:

1. Azure Active Directory'yi gerektiğinde kullanıcı kimlikleriyle oluşturun ve doldurun.
2. Azure aboneliğinizle ilişkili olan Active Directory'yi isteğe bağlı olarak ilişkilendirin veya değiştirin.
3. PostgreSQL sunucusu için Azure Veritabanı için bir Azure AD yöneticisi oluşturun.
4. Veritabanınızda Azure AD kimliklerine eşlenen veritabanı kullanıcıları oluşturun.
5. Azure AD kimliği için bir belirteç alarak ve oturum açarak veritabanınıza bağlanın.

> [!NOTE]
> Azure AD'yi nasıl oluşturup doldurup doldurmayı öğrenmek ve ardından Azure AD'yi PostgreSQL için Azure Veritabanı ile yapılandırmak [için, PostgreSQL için Azure Veritabanı için Azure AD'yi yapılandır'a](howto-configure-sign-in-aad-authentication.md)bakın ve oturum açın.

## <a name="architecture"></a>Mimari

Aşağıdaki üst düzey diyagram, PostgreSQL için Azure Veritabanı ile Azure AD kimlik doğrulamasını kullanarak kimlik doğrulamanın nasıl çalıştığını özetler. Oklar iletişim yollarını gösteriyor.

![kimlik doğrulama akışı][1]

## <a name="administrator-structure"></a>Yönetici yapısı

Azure AD kimlik doğrulaması kullanırken, PostgreSQL sunucusu için iki Yönetici hesabı vardır; özgün PostgreSQL yöneticisi ve Azure AD yöneticisi. Yalnızca bir Azure AD hesabına dayalı yönetici, bir kullanıcı veritabanında ilk Azure AD içerdiği veritabanı kullanıcısını oluşturabilir. Azure AD yöneticisi girişi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetici bir grup hesabı olduğunda, herhangi bir grup üyesi tarafından kullanılabilir ve PostgreSQL sunucusu için birden çok Azure AD yöneticisi etkinleştirilebilir. Bir grup hesabını yönetici olarak kullanmak, PostgreSQL sunucusundaki kullanıcıları veya izinleri değiştirmeden Azure AD'deki grup üyelerini merkezi olarak eklemenize ve kaldırmanıza izin vererek yönetilebilirliği artırır. İstediğiniz zaman yalnızca bir Azure AD yöneticisi (kullanıcı veya grup) yapılandırılabilir.

![yönetici yapısı][2]

## <a name="permissions"></a>İzinler

Azure AD ile kimlik doğrulaması yapabilen yeni `azure_ad_admin` kullanıcılar oluşturmak için veritabanında rol sahibi olmalısınız. Bu rol, PostgreSQL sunucusu için belirli bir Azure Veritabanı için Azure AD Yöneticisi hesabını yapılandırarak atanır.

Yeni bir Azure AD veritabanı kullanıcısı oluşturmak için Azure AD yöneticisi olarak bağlanmanız gerekir. Bu, [PostgreSQL için Azure Veritabanı için Azure AD ile Yapılandırma ve Oturum Açma'da](howto-configure-sign-in-aad-authentication.md)gösterilmiştir.

Azure AD kimlik doğrulaması yalnızca Azure AD yöneticisi PostgreSQL için Azure Veritabanı için oluşturulduğunda mümkündür. Azure Etkin Dizin yöneticisi sunucudan kaldırıldıysa, daha önce oluşturulan mevcut Azure Active Directory kullanıcıları Artık Azure Etkin Dizin kimlik bilgilerini kullanarak veritabanına bağlanabilir.

## <a name="connecting-using-azure-ad-identities"></a>Azure AD kimliklerini kullanarak bağlanma

Azure Etkin Dizin kimlik doğrulaması, Azure AD kimliklerini kullanarak bir veritabanına bağlanma nın aşağıdaki yöntemlerini destekler:

- Azure Active Directory Password
- Azure Active Directory Entegre
- MFA ile Azure Active Directory Universal
- Active Directory Application sertifikalarını veya istemci sırlarını kullanma

Etkin Dizin'e karşı kimlik doğrulaması yaptıktan sonra bir belirteç alırsınız. Bu belirteç, oturum açmak için parolanızdır.

> [!NOTE]
> Active Directory belirteciyle nasıl bağlanılabağlanınız hakkında daha fazla bilgi için, [PostgreSQL için Azure Veritabanı için Azure REKLAM'ı Yapılandırma ve oturum açma](howto-configure-sign-in-aad-authentication.md)hakkında bilgi edinin.

## <a name="additional-considerations"></a>Diğer konular

- Yönetilebilirliği artırmak için, yönetici olarak özel bir Azure REKLAM grubu sağlamanızı öneririz.
- Yalnızca bir Azure AD yöneticisi (kullanıcı veya grup) herhangi bir zamanda PostgreSQL sunucusu için bir Azure Veritabanı için yapılandırılabilir.
- Yalnızca PostgreSQL için bir Azure AD yöneticisi, başlangıçta Bir Azure Active Directory hesabı kullanarak PostgreSQL için Azure Veritabanı'na bağlanabilir. Etkin Dizin yöneticisi sonraki Azure AD veritabanı kullanıcılarını yapılandırabilir.
- Bir kullanıcı Azure AD'den silinirse, bu kullanıcı artık Azure AD ile kimlik doğrulaması yapamayacaktır ve bu nedenle bu kullanıcı için bir erişim jetonu elde etmek artık mümkün olmayacaktır. Bu durumda, eşleşen rol yine de veritabanında olsa da, bu role sahip sunucuya bağlanmak mümkün olmayacaktır.
> [!NOTE]
> Silinen Azure REKLAM kullanıcısıyla giriş, belirteç süresi dolana kadar (belirteç verme tarihinden 60 dakikaya kadar) yine de yapılabilir.  Kullanıcıyı PostgreSQL için Azure Veritabanı'ndan da kaldırırsanız, bu erişim hemen iptal edilecektir.
- Azure AD yöneticisi sunucudan kaldırılırsa, sunucu artık bir Azure AD kiracısıyla ilişkilendirilmez ve bu nedenle tüm Azure AD girişleri sunucu için devre dışı bırakılır. Aynı kiracıdan yeni bir Azure AD yöneticisi eklemek, Azure AD oturumlarını yeniden etkinleştirir.
- PostgreSQL için Azure Veritabanı, kullanıcı adını kullanmanın aksine, kullanıcının benzersiz Azure AD kullanıcı kimliğini kullanarak PostgreSQL rolü için Azure Veritabanı'na erişim belirteçleri ile eşleşir. Bu, Azure AD'de bir Azure REKLAM kullanıcısı silinirse ve aynı adla oluşturulan yeni bir kullanıcı ysa, PostgreSQL için Azure Veritabanı bunu farklı bir kullanıcı olarak kabul eder. Bu nedenle, bir kullanıcı Azure AD'den silinir ve ardından aynı ada sahip yeni bir kullanıcı eklenirse, yeni kullanıcı varolan role bağlanamaz. Buna izin vermek için, PostgreSQL Azure AD yöneticisi için Azure Veritabanı'nın iptal etmesi ve ardından Azure REKLAM kullanıcı kimliğini yenilemesi için kullanıcıya "azure_ad_user" rolünü vermesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD'yi nasıl oluşturup doldurup doldurmayı öğrenmek ve ardından Azure AD'yi PostgreSQL için Azure Veritabanı ile yapılandırmak [için, PostgreSQL için Azure Veritabanı için Azure AD'yi yapılandır'a](howto-configure-sign-in-aad-authentication.md)bakın ve oturum açın.
- PostgreSQL için girişlere, kullanıcılara ve veritabanı rollerine genel bakış için, [PostgreSQL için Azure Veritabanı'nda kullanıcıları oluşturma - Tek Sunucu](howto-create-users.md)bölümüne bakın.

<!--Image references-->

[1]: ./media/concepts-aad-authentication/authentication-flow.png
[2]: ./media/concepts-aad-authentication/admin-structure.png
