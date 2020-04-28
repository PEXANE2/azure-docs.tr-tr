---
title: Azure AD 'de tek ve çok kiracılı uygulamalar
titleSuffix: Microsoft identity platform
description: Azure AD 'de tek kiracılı ve çok kiracılı uygulamalar arasındaki özellikler ve farklılıklar hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: justhu
ms.custom: aaddev
ms.openlocfilehash: ec59383f9ca2b71ec9f4b6df3ab2e24c6b52473b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80881375"
---
# <a name="tenancy-in-azure-active-directory"></a>Azure Active Directory kiracı

Azure Active Directory (Azure AD), kullanıcılar ve uygulamalar gibi nesneleri *kiracılar*adlı gruplar halinde düzenler. Kiracılar, yöneticinin kuruluş içindeki kullanıcılar ve kuruluşun güvenlik ve işletimsel ilkelerini karşılamak için sahip olduğu uygulamalar üzerinde ilkeler ayarlamanıza olanak sağlar. 

## <a name="who-can-sign-in-to-your-app"></a>Uygulamanızda kimler oturum açabilir?

Uygulamalar geliştirilirken, geliştiriciler [Azure Portal](https://portal.azure.com)uygulama kaydı sırasında uygulamayı tek kiracılı veya çok kiracılı olacak şekilde yapılandırmayı seçebilirler.
* Tek kiracılı uygulamalar, yalnızca kayıtlı oldukları kiracıda kullanılabilir ve kendi ev kiracının olarak da bilinir.
* Çok kiracılı uygulamalar, hem ev kiracılarında hem de diğer kiracılarda bulunan kullanıcılar tarafından kullanılabilir.

Azure portal, hedef kitleyi aşağıdaki gibi ayarlayarak uygulamanızı tek kiracılı veya çok kiracılı olacak şekilde yapılandırabilirsiniz.

| Hedef kitle | Tek/çok kiracılı | Kimler oturum açabilir | 
|----------|--------| ---------|
| Yalnızca bu dizindeki hesaplar | Tek kiracılı | Dizininizdeki tüm kullanıcılar ve Konuk hesapları, uygulamanızı veya API 'nizi kullanabilir.<br>*Hedef hedef kitlesi kuruluşunuzda dahili ise bu seçeneği kullanın.* |
| Herhangi bir Azure AD dizinindeki hesaplar | Çok Kiracılı | Microsoft 'un iş veya okul hesabına sahip tüm kullanıcılar ve konukları, uygulamanızı veya API 'nizi kullanabilir. Bu, Office 365 kullanan okullar ve işletmeler içerir.<br>*Hedef kitleniz iş veya Eğitim müşterileri ise bu seçeneği kullanın.* |
| Herhangi bir Azure AD dizinindeki hesaplar ve kişisel Microsoft hesapları (Skype, Xbox, Outlook.com gibi) | Çok Kiracılı | İş veya okul veya kişisel Microsoft hesabı olan tüm kullanıcılar uygulamanızı veya API 'nizi kullanabilir. Bu, Xbox ve Skype gibi hizmetlerde oturum açmak için kullanılan kişisel hesapların yanı sıra Office 365 ' i kullanan okullar ve işletmeler içerir.<br>*En geniş Microsoft hesabı kümesini hedeflemek için bu seçeneği kullanın.* | 

## <a name="best-practices-for-multi-tenant-apps"></a>Çok kiracılı uygulamalar için en iyi uygulamalar

Harika çok kiracılı uygulamalar oluşturmak, BT yöneticilerinin kiracılarında ayarlayabildiğinden farklı ilkelerin sayısı nedeniyle zor olabilir. Çok kiracılı bir uygulama oluşturmayı seçerseniz, bu en iyi yöntemleri izleyin:

* Uygulamanızı [koşullu erişim ilkelerini](../azuread-dev/conditional-access-dev-guide.md)yapılandıran bir kiracıda test edin.
* Uygulamanızın yalnızca gerçekten gereken izinleri istediğinden emin olmak için en az Kullanıcı erişimi ilkesini izleyin. Yönetici onayı gerektiren izinler istememeye özen gösterin. Bu, kullanıcıların uygulamanızı bazı kuruluşlarda almasını önleyebilmesine engel olabilir. 
* Uygulamanızın bir parçası olarak kullanıma sunabileceğiniz tüm izinler için uygun adlar ve açıklamalar sağlayın. Bu, kullanıcıların ve yöneticilerin uygulamanızın API 'Lerini kullanmaya çalıştıklarında ne kabul etmiş olduklarını bilmesini sağlar. Daha fazla bilgi için, [izinler kılavuzundaki](v2-permissions-and-consent.md)en iyi uygulamalar bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir uygulamayı çok kiracılı olarak dönüştürme](howto-convert-app-to-be-multi-tenant.md)
