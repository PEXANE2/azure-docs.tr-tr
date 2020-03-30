---
title: Active Directory kimlik doğrulaması - MySQL için Azure Veritabanı
description: MySQL için Azure Veritabanı ile kimlik doğrulama için Azure Active Directory kavramları hakkında bilgi edinin
author: lfittl-msft
ms.author: lufittl
ms.service: mysql
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 960536c3f80aa7870d6f2056d8e95cd1a4338dfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299031"
---
# <a name="use-azure-active-directory-for-authenticating-with-mysql"></a>MySQL ile kimlik doğrulaması için Azure Etkin Dizini'ni kullanma

Microsoft Azure Active Directory (Azure AD) kimlik doğrulaması, Azure AD'de tanımlanan kimlikleri kullanarak MySQL için Azure Veritabanı'na bağlanma mekanizmasıdır.
Azure AD kimlik doğrulaması ile veritabanı kullanıcı kimliklerini ve diğer Microsoft hizmetlerini merkezi bir konumda yönetebilirsiniz ve bu da izin yönetimini kolaylaştırır.

> [!IMPORTANT]
> MySQL için Azure Veritabanı için Azure AD kimlik doğrulaması şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure AD kullanmanın yararları şunlardır:

- Azure Hizmetleri'ndeki kullanıcıların özgünasyonu tek tip bir şekilde
- Parola ilkelerinin yönetimi ve parola döndürme tek bir yerde
- Parolaları depolama gereksinimini ortadan kaldırabilecek Azure Active Directory tarafından desteklenen birden çok kimlik doğrulama biçimi
- Müşteriler veritabanı izinlerini harici (Azure AD) gruplarını kullanarak yönetebilir.
- Azure AD kimlik doğrulaması, veritabanı düzeyindekimlikleri doğrulamak için MySQL veritabanı kullanıcılarını kullanır
- MySQL için Azure Veritabanı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulama desteği

Azure Active Directory kimlik doğrulamasını yapılandırmak ve kullanmak için aşağıdaki işlemi kullanın:

1. Azure Active Directory'yi gerektiğinde kullanıcı kimlikleriyle oluşturun ve doldurun.
2. Azure aboneliğinizle ilişkili olan Active Directory'yi isteğe bağlı olarak ilişkilendirin veya değiştirin.
3. MySQL sunucusu için Azure Veritabanı için bir Azure AD yöneticisi oluşturun.
4. Veritabanınızda Azure AD kimliklerine eşlenen veritabanı kullanıcıları oluşturun.
5. Azure AD kimliği için bir belirteç alarak ve oturum açarak veritabanınıza bağlanın.

> [!NOTE]
> Azure AD'yi nasıl oluşturup doldurup doldurmayı öğrenmek ve ardından Azure AD'yi MySQL için Azure Veritabanı ile yapılandırmak [için Yapılve MySQL için Azure Veritabanı için Azure AD ile oturum açın'ı](howto-configure-sign-in-azure-ad-authentication.md)görün.

## <a name="architecture"></a>Mimari

Aşağıdaki üst düzey diyagram, MySQL için Azure Veritabanı ile Azure AD kimlik doğrulamasını kullanarak kimlik doğrulamanın nasıl çalıştığını özetler. Oklar iletişim yollarını gösteriyor.

![kimlik doğrulama akışı][1]

## <a name="administrator-structure"></a>Yönetici yapısı

Azure AD kimlik doğrulaması kullanırken, MySQL sunucusu için iki Yönetici hesabı vardır; orijinal MySQL yöneticisi ve Azure AD yöneticisi. Yalnızca bir Azure AD hesabına dayalı yönetici, bir kullanıcı veritabanında ilk Azure AD içerdiği veritabanı kullanıcısını oluşturabilir. Azure AD yöneticisi girişi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetici bir grup hesabı olduğunda, mysql sunucusu için birden çok Azure AD yöneticisine olanak sağlayarak herhangi bir grup üyesi tarafından kullanılabilir. Bir grup hesabını yönetici olarak kullanmak, MySQL sunucusundaki kullanıcıları veya izinleri değiştirmeden Azure AD'deki grup üyelerini merkezi olarak eklemenize ve kaldırmanıza izin vererek yönetilebilirliği artırır. İstediğiniz zaman yalnızca bir Azure AD yöneticisi (kullanıcı veya grup) yapılandırılabilir.

![yönetici yapısı][2]

## <a name="permissions"></a>İzinler

Azure AD ile kimlik doğrulaması yapabilen yeni kullanıcılar oluşturmak için, tasarlanmış Azure AD yöneticisi olmalısınız. Bu kullanıcı, MySQL sunucusu için belirli bir Azure Veritabanı için Azure AD Yöneticisi hesabını yapılandırarak atanır.

Yeni bir Azure AD veritabanı kullanıcısı oluşturmak için Azure AD yöneticisi olarak bağlanmanız gerekir. Bu, [MySQL için Azure Veritabanı için Azure AD ile Yapılandırma ve Oturum Açma'da](howto-configure-sign-in-azure-ad-authentication.md)gösterilmiştir.

Azure AD kimlik doğrulaması yalnızca Azure AD yöneticisi MySQL için Azure Veritabanı için oluşturulduğunda mümkündür. Azure Etkin Dizin yöneticisi sunucudan kaldırıldıysa, daha önce oluşturulan mevcut Azure Active Directory kullanıcıları Artık Azure Etkin Dizin kimlik bilgilerini kullanarak veritabanına bağlanabilir.

## <a name="connecting-using-azure-ad-identities"></a>Azure AD kimliklerini kullanarak bağlanma

Azure Etkin Dizin kimlik doğrulaması, Azure AD kimliklerini kullanarak bir veritabanına bağlanma nın aşağıdaki yöntemlerini destekler:

- Azure Active Directory Password
- Azure Active Directory Entegre
- MFA ile Azure Active Directory Universal
- Active Directory Application sertifikalarını veya istemci sırlarını kullanma

Etkin Dizin'e karşı kimlik doğrulaması yaptıktan sonra bir belirteç alırsınız. Bu belirteç, oturum açmak için parolanızdır.

> [!NOTE]
> Active Directory belirteciyle nasıl bağlanılabağlanınız hakkında daha fazla bilgi için, [MySQL için Azure Veritabanı için Azure AD'yi Yapılandırma ve oturum açma](howto-configure-sign-in-azure-ad-authentication.md)hakkında bilgi edinin.

## <a name="additional-considerations"></a>Diğer konular

- MySQL sunucusu için bir Azure Veritabanı için herhangi bir zamanda yalnızca bir Azure AD yöneticisi yapılandırılabilir.
- Yalnızca MySQL için bir Azure AD yöneticisi, başlangıçta Bir Azure Active Directory hesabı kullanarak MySQL için Azure Veritabanı'na bağlanabilir. Etkin Dizin yöneticisi sonraki Azure AD veritabanı kullanıcılarını yapılandırabilir.
- Bir kullanıcı Azure AD'den silinirse, bu kullanıcı artık Azure AD ile kimlik doğrulaması yapamayacaktır ve bu nedenle bu kullanıcı için bir erişim jetonu elde etmek artık mümkün olmayacaktır. Bu durumda, eşleşen kullanıcı veritabanında olmaya devam etse de, sunucuya bu kullanıcıyla bağlanmak mümkün olmayacaktır.
> [!NOTE]
> Silinen Azure REKLAM kullanıcısıyla giriş, belirteç süresi dolana kadar (belirteç verme tarihinden 60 dakikaya kadar) yine de yapılabilir.  Kullanıcıyı MySQL için Azure Veritabanı'ndan da kaldırırsanız, bu erişim hemen iptal edilecektir.
- Azure AD yöneticisi sunucudan kaldırılırsa, sunucu artık bir Azure AD kiracısıyla ilişkilendirilmez ve bu nedenle tüm Azure AD girişleri sunucu için devre dışı bırakılır. Aynı kiracıdan yeni bir Azure AD yöneticisi eklemek, Azure AD oturum larını yeniden etkinleştirir.
- MySQL için Azure Veritabanı, kullanıcı adını kullanmanın aksine, kullanıcının benzersiz Azure AD kullanıcı kimliğini kullanarak MySQL kullanıcısı için Azure Veritabanı'na erişim belirteçlerini eşler. Bu, Azure AD kullanıcısı Azure AD'de silinirse ve aynı adla oluşturulan yeni bir kullanıcı ysa, MySQL için Azure Veritabanı farklı bir kullanıcının bunu dikkate aldığı anlamına gelir. Bu nedenle, bir kullanıcı Azure AD'den silinirse ve aynı ada sahip yeni bir kullanıcı eklenirse, yeni kullanıcı varolan kullanıcıyla bağlantı kuramaz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD'yi nasıl oluşturup doldurup doldurmayı öğrenmek ve ardından Azure AD'yi MySQL için Azure Veritabanı ile yapılandırmak [için Yapılve MySQL için Azure Veritabanı için Azure AD ile oturum açın'ı](howto-configure-sign-in-azure-ad-authentication.md)görün.
- MySQL için Azure Veritabanı için girişlere ve veritabanı [kullanıcılarına](howto-create-users.md)genel bakış için bkz.

<!--Image references-->

[1]: ./media/concepts-azure-ad-authentication/authentication-flow.png
[2]: ./media/concepts-azure-ad-authentication/admin-structure.png
