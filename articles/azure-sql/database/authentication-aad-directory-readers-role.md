---
title: Azure SQL için Azure Active Directory Directory okuyucuları rolü
description: Azure AD uygulamaları (hizmet sorumluları) Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics 'te Azure AD Kullanıcı oluşturmayı destekler
ms.service: sql-db-mi
ms.subservice: security
ms.custom: azure-synapse
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 08/14/2020
ms.openlocfilehash: a517a4b14a64be2b9bf12270eef3d08e14d8f3c8
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88556330"
---
# <a name="directory-readers-role-in-azure-active-directory-for-azure-sql"></a>Azure SQL için Azure Active Directory Directory okuyucuları rolü

[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

> [!NOTE]
> Bu makaledeki bu özellik **genel önizlemede**.

Azure Active Directory (Azure AD), [Azure Active Directory (Önizleme) içindeki rol atamalarını yönetmek için bulut grupları kullanarak](../../active-directory/users-groups-roles/roles-groups-concept.md)tanıtılmıştır. Bu, Azure AD rollerinin gruplara atanmasını sağlar.

Azure SQL veritabanı, Azure SQL yönetilen örneği veya Azure SYNAPSE Analytics için [yönetilen bir kimlik](../../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) etkinleştirilirken, Azure AD [**Dizin okuyucuları**](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) rolü, [Azure AD Graph API](../../active-directory/develop/active-directory-graph-api.md)okuma erişimine izin vermek için kimliğe atanmalıdır. SQL Database ve Azure SYNAPSE yönetilen kimliği sunucu kimliği olarak adlandırılır. SQL yönetilen örneğinin yönetilen kimliği, yönetilen örnek kimliği olarak adlandırılır ve örnek oluşturulduğunda otomatik olarak atanır. SQL veritabanı veya Azure SYNAPSE 'a sunucu kimliği atama hakkında daha fazla bilgi için bkz. [Azure AD kullanıcıları oluşturmak için hizmet sorumlularını etkinleştirme](authentication-aad-service-principal.md#enable-service-principals-to-create-azure-ad-users).

**Dizin okuyucuları** rolü şunları yapmak için gereklidir:

- SQL yönetilen örneği için Azure AD oturum açmaları oluşturma
- Azure SQL 'de Azure AD kullanıcılarını taklit et
- Windows kimlik doğrulaması ( [alter User (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-user-transact-sql?view=azuresqldb-mi-current#d-map-the-user-in-the-database-to-an-azure-ad-login-after-migration) komutu kullanılarak) Ile SQL yönetilen örneği kullanan SQL Server kullanıcıları geçirme
- SQL yönetilen örneği için Azure AD yöneticisini değiştirme
- [Hizmet sorumlularına (uygulamalar)](authentication-aad-service-principal.md) Azure SQL 'de Azure AD kullanıcıları oluşturmalarına izin verme

## <a name="assigning-the-directory-readers-role"></a>Dizin okuyucuları rolünü atama

[**Dizin okuyucuları**](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) rolünü bir kimliğe atamak Için, [genel yönetici](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) veya [ayrıcalıklı rol yöneticisi](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) izinlerine sahip bir kullanıcı gereklidir. Genellikle SQL veritabanı, SQL yönetilen örneği veya Azure SYNAPSE 'ı yöneten veya dağıtan kullanıcıların bu yüksek ayrıcalıklı rollere erişimi olmayabilir. Bu, genellikle planlanmamış Azure SQL kaynakları oluşturan kullanıcılar için karmaşıklıklara neden olabilir veya büyük kuruluşlarda genellikle erişilemeyen son derece ayrıcalıklı rol üyelerinden yardım almanız gerekir.

[Yönetilen örnek için bir Azure AD yöneticisi](authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance)ayarlamadan önce, SQL yönetilen örneği Için, **Dizin okuyucular** rolü yönetilen örnek kimliğine atanmalıdır. 

Mantıksal sunucu için bir Azure AD yöneticisi ayarlanırken SQL veritabanı veya Azure SYNAPSE için **Dizin okuyucuları** rolünü sunucu kimliğine atama gerekli değildir. Ancak, bir Azure AD uygulaması adına SQL veritabanı veya Azure SYNAPSE 'de Azure AD nesne oluşturmayı etkinleştirmek için **Dizin okuyucuları** rolü gereklidir. Rol SQL mantıksal sunucu kimliğine atanmamışsa Azure SQL 'de Azure AD kullanıcıları oluşturmak başarısız olur. Daha fazla bilgi için bkz. [Azure SQL ile Azure Active Directory hizmet sorumlusu](authentication-aad-service-principal.md).

## <a name="granting-the-directory-readers-role-to-an-azure-ad-group"></a>Dizin okuyucuları rolünü bir Azure AD grubuna verme

Şu anda **genel önizlemede**, artık [genel yönetici](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) veya [AYRıCALıKLı rol yöneticisi](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) bir Azure AD grubu oluşturabilir ve [**Dizin okuyucuları**](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) iznini gruba atayabilir. Bu, bu grubun üyeleri için Azure AD Graph API erişimine izin verir. Bunlara ek olarak, bu grubun sahibi olan Azure AD kullanıcıları, Azure SQL mantıksal sunucularının kimlikleri dahil olmak üzere bu grup için yeni üyeler atamaya izin verilir.

Bu çözüm, bir grup oluşturmak ve kullanıcıları tek seferlik bir etkinlik olarak atamak için yüksek ayrıcalıklı bir Kullanıcı (genel yönetici veya ayrıcalıklı rol yöneticisi) gerektirir, ancak Azure AD grubu sahipleri ileri doğru Üyeler atayabilecektir. Bu, gelecekte Azure AD kiracısındaki tüm SQL veritabanlarını, SQL yönetilen örnekleri veya Azure SYNAPSE sunucularını yapılandırmak için yüksek ayrıcalıklı bir kullanıcının dahil olması gereğini ortadan kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure AD grubuna Dizin okuyucuları rolü atama ve rol atamalarını yönetme](authentication-aad-directory-readers-role-tutorial.md)
