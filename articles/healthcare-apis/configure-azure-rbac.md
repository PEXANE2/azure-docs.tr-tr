---
title: FHıR için Azure API 'SI için Azure rol tabanlı Access Control (RBAC) yapılandırma
description: Bu makalede, FHıR veri düzlemi için Azure API 'SI için Azure RBAC 'nin nasıl yapılandırılacağı açıklanır.
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 03/15/2020
ms.author: mihansen
ms.openlocfilehash: bf05ede858c86fa82e199c800ea42747745c9c19
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "84871920"
---
# <a name="configure-azure-rbac-for-fhir"></a>FHıR için Azure RBAC 'yi yapılandırma 

Bu makalede, FHıR veri düzlemi için Azure API 'sine erişim atamak üzere [Azure rol tabanlı Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/) kullanmayı öğreneceksiniz. Azure RBAC, veri düzlemi kullanıcıları Azure aboneliğinizle ilişkili Azure Active Directory kiracısında yönetildiğinde veri düzlemi erişimi atamaya yönelik tercih edilen yöntemlerdir. Dış Azure Active Directory kiracı kullanıyorsanız, [Yerel RBAC atama başvurusuna](configure-local-rbac.md)bakın.

## <a name="confirm-azure-rbac-mode"></a>Azure RBAC modunu onaylama

Azure RBAC 'yi kullanmak için, FHıR için Azure API 'nizin, veri düzlemi için Azure abonelik kiracınızı kullanacak şekilde yapılandırılması ve atanmış kimlik nesne kimlikleri olmaması gerekir. FHıR için Azure API 'nizin **kimlik doğrulama** dikey penceresini inceleyerek ayarlarınızı doğrulayabilirsiniz:

:::image type="content" source="media/rbac/confirm-azure-rbac-mode.png" alt-text="Azure RBAC modunu onaylama":::

**Yetkili** , aboneliğinizle Ilişkili Azure Active Directory kiracısına ayarlanmalıdır ve **Izin verilen nesne kimlikleri**etiketli kutuda GUID olmamalıdır. Ayrıca, kutunun devre dışı olduğunu ve bir etiketin veri düzlemi rollerini atamak için Azure RBAC 'nin kullanılması gerektiğini belirten bir etiket olduğunu fark edeceksiniz.

## <a name="assign-roles"></a>Rol atama

Kullanıcıları, hizmet sorumlularını veya grupları FHıR veri düzlemine erişim izni vermek için **erişim denetimi (IAM)** öğesine tıklayın, ardından **rol atamaları** ' na tıklayın ve **+ Ekle**' ye tıklayın:

:::image type="content" source="media/rbac/add-azure-rbac-role-assignment.png" alt-text="Azure RBAC rol ataması ekleme":::

**Rol** seçiminde, fhır veri düzlemi için yerleşik rollerden birini arayın:

:::image type="content" source="media/rbac/built-in-fhir-data-roles.png" alt-text="Yerleşik FHıR veri rolleri":::

Arasından seçim yapabilirsiniz:

* FHıR veri okuyucu: FHıR verilerini okuyabilir (ve arayabilir).
* FHıR veri yazıcı: FHıR verilerini okuyabilir, yazabilir ve geçici olarak silebilir.
* FHıR veri verme programı: verileri okuyabilir ve dışarı aktarabilir ( `$export` işleç).
* FHıR verileri katılımcısı: tüm veri düzlemi işlemlerini gerçekleştirebilir.

Bu roller ihtiyacınız olması için yeterli değilse [özel roller de oluşturabilirsiniz](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-powershell).

**Seç** kutusunda, rolü atamak istediğiniz kullanıcı, hizmet sorumlusu veya grup için arama yapın.

## <a name="caching-behavior"></a>Önbelleğe alma davranışı

FHıR için Azure API 'SI, kararları 5 dakikaya kadar önbelleğe alacak. İzin verilen nesne kimlikleri listesine ekleyerek FHıR sunucusuna bir kullanıcı erişimi verirseniz veya onları listeden kaldırırsanız, bu, en fazla beş dakika sonra yayarak yayma izinleridir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR veri düzlemi için Azure RBAC rolleri atamayı öğrendiniz. Daha sonra FHıR için Azure API 'SI için ek ayarlar hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Ek ayarlar FHıR için Azure API](azure-api-for-fhir-additional-settings.md)

