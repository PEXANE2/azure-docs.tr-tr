---
title: Azure Active Directory hizmet hesaplarının güvenliğini sağlama konusuna giriş
description: Azure Active Directory ' de kullanılabilen hizmet hesabı türlerinin açıklaması.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 3/1/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 049025a5d871f1dd26e5dab498756aa44d2ebfe2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693286"
---
# <a name="introduction-to-securing-azure-service-accounts"></a>Azure hizmet hesaplarının güvenliğini sağlama konusuna giriş

Azure Active Directory için yerel olarak üç tür hizmet hesabı vardır: Yönetilen kimlikler, hizmet sorumluları ve Kullanıcı tabanlı hizmet hesapları. Hizmet hesapları, bir uygulama, API veya başka bir hizmet gibi insan dışı bir varlığı temsil etmesi amaçlanan özel bir hesap türüdür. Bu varlıklar, hizmet hesabı tarafından sunulan güvenlik bağlamı içinde çalışır. 

## <a name="types-of-azure-active-directory-service-accounts"></a>Azure Active Directory hizmet hesabı türleri

Azure 'da barındırılan hizmetler için mümkünse yönetilen bir kimlik ve hizmet sorumlusu kullanılması önerilir. Yönetilen kimlikler, Azure dışında barındırılan hizmetler için kullanılamaz. Bu durumda, bir hizmet sorumlusu önerilir. Yönetilen bir kimlik veya hizmet sorumlusu kullanabilmeniz için bunu yapın. Hizmet sorumlusu olarak Azure Active Directory bir kullanıcı hesabı kullanmanızı öneririz. Özet için aşağıdaki tabloya bakın.
 

| Hizmet barındırma| Yönetilen kimlik| Hizmet sorumlusu| Azure Kullanıcı hesabı |
| - | - | - | - |
|Hizmet Azure 'da barındırılıyor.| Evet. <br>Hizmet ise önerilir <br>Yönetilen bir kimliği destekler.| Evet.| Önerilmez. |
| Hizmet Azure 'da barındırılmıyor.| Hayır| Evet. Önerilen.| Önerilmez. |
| Hizmet çok kiracılı| Hayır| Evet. Önerilen.| Hayır. |


## <a name="managed-identities"></a>Yönetilen kimlikler

Yönetilen kimlikler, Azure kaynakları için kimlikler sağlamak üzere oluşturulan güvenli Azure Active Directory (Azure AD) kimlikleridir. [İki tür yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#managed-identity-types)vardır: 
 
* Sistem tarafından atanan Yönetilen kimlikler, bir hizmetin örneğine doğrudan atanabilir. 

* Kullanıcı tarafından atanan Yönetilen kimlikler, tek başına bir kaynak olarak oluşturulabilir. 

Daha fazla bilgi için bkz. [yönetilen kimliklerin güvenliğini sağlama](service-accounts-managed-identities.md). Yönetilen kimlikler hakkında genel bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

## <a name="service-principals"></a>Hizmet sorumluları

Uygulamanızı temsil etmek için yönetilen bir kimlik kullanamıyoruz, bir hizmet sorumlusu kullanın. Hizmet sorumluları, tek kiracılı ve çok kiracılı uygulamalarla birlikte kullanılabilir. 

Hizmet sorumlusu, tek bir Azure AD kiracısında bir uygulama nesnesinin yerel gösterimidir. Uygulama örneğinin kimliği olarak çalışır, uygulamaya kimlerin erişebileceğini ve uygulamanın erişebileceği kaynakları tanımlar. Bir hizmet sorumlusu, uygulamanın kullanıldığı her kiracıda (yerel olarak) oluşturulur ve genel olarak benzersiz uygulama nesnesine başvurur. Kiracı, hizmet sorumlusunun oturum açma ve kaynaklara erişme güvenliğini sağlar.

Hizmet sorumlularını kullanarak kimlik doğrulama için iki mekanizma vardır: istemci sertifikaları ve istemci gizli dizileri. Sertifikalar daha güvenlidir: mümkünse istemci sertifikalarını kullanın. İstemci gizliliklerinden farklı olarak, istemci sertifikaları yanlışlıkla koda Katıştırılamaz.

Hizmet sorumlularının güvenliğini sağlama hakkında bilgi için bkz. hizmet sorumlularını güvenli hale getirme.

 
## <a name="next-steps"></a>Sonraki adımlar


Azure hizmet hesaplarının güvenliğini sağlama hakkında daha fazla bilgi için bkz.

[Yönetilen kimliklerin güvenliğini sağlama](service-accounts-managed-identities.md)

[Hizmet sorumlularının güvenliğini sağlama](service-accounts-principal.md)

[Azure hizmet hesaplarını yöneten](service-accounts-governing-azure.md)



