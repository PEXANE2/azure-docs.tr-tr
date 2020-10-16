---
title: API önkoşulları-Azure Marketi
description: Bulut İş Ortağı Portalı API 'Leri kullanma önkoşulları.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 09/23/2020
ms.openlocfilehash: 374cc3398d1037b19ceddcbdaafd333f643e44ff
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91261167"
---
# <a name="api-prerequisites"></a>API önkoşulları

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. Yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için CPP API 'Lerini kullanın; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Bulut İş Ortağı Portalı API 'Leri kullanmak için iki zorunlu program varlığı gerekir: bir hizmet sorumlusu ve bir Azure Active Directory (Azure AD) erişim belirteci.

## <a name="create-service-principal-in-azure-active-directory-tenant"></a>Azure Active Directory kiracısında hizmet sorumlusu oluşturma

İlk olarak, Azure AD kiracınızda bir hizmet sorumlusu oluşturmanız gerekir. Bu kiracıya, Bulut İş Ortağı Portalı kendi izin kümesi atanır. Kodunuz, kişisel kimlik bilgileriniz yerine bu kiracıyı kullanarak API 'Leri çağırır. Hizmet sorumlusu oluşturma hakkında tam bir açıklama için bkz. [nasıl yapılır: portalı kullanarak kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="add-service-principal-to-your-account"></a>Hesabınıza hizmet sorumlusu ekleyin

Kiracınızda hizmet sorumlusunu oluşturduğunuza göre, bunu Iş Ortağı Merkezi portalı hesabınıza bir kullanıcı olarak ekleyebilirsiniz. Bir kullanıcı gibi, hizmet sorumlusu de Portal için bir sahip veya katılımcı olabilir. Ayrıntılar için aşağıdaki **sonraki adımlara** bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Azure AD uygulamalarını yönetme](partner-center-portal/manage-account.md#manage-azure-ad-applications).
