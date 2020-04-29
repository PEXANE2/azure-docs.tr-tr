---
title: Azure Analysis Services | sunucu yöneticilerini yönetme | Microsoft Docs
description: Bu makalede, Azure portal, PowerShell veya REST API 'Lerini kullanarak bir Azure Analysis Services sunucusu için sunucu yöneticilerinin nasıl yönetileceği açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 944a84405bd7e03b72b2610278f9f0e4d3cfaf38
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81454246"
---
# <a name="manage-server-administrators"></a>Sunucu yöneticilerini yönetme

Sunucu yöneticileri, sunucunun bulunduğu kiracı için Azure Active Directory (Azure AD) içinde geçerli bir kullanıcı veya güvenlik grubu olmalıdır. Sunucu yöneticileri 'ni yönetmek için sunucu Azure portal, SSMS, PowerShell veya REST API sunucu özellikleri ' nde sunucunuz için **Analysis Services yöneticileri** kullanabilirsiniz. 

Bir **güvenlik grubu**eklerken kullanın `obj:groupid@tenantid`.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Azure portal kullanarak sunucu yöneticileri eklemek için

1. Portalda, sunucunuz için **Analysis Services Yöneticiler**' e tıklayın.
2. ** \<ServerName>-Analysis Services Admins**bölümünde **Ekle**' ye tıklayın.
3. **Sunucu yöneticileri Ekle**' de Azure AD 'nizden Kullanıcı hesapları ' nı seçin veya dış kullanıcıları e-posta adresine davet edin.

    ![Azure portal 'de sunucu yöneticileri](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>SSMS kullanarak sunucu yöneticileri eklemek için

1. Sunucu > **Özellikler**' e sağ tıklayın.
2. **Analiz sunucusu Özellikler**' de **güvenlik**' e tıklayın.
3. **Ekle**' ye tıklayın ve ardından Azure AD 'nize bir kullanıcı veya grubun e-posta adresini girin.
   
    ![SSMS 'de sunucu yöneticileri ekleme](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Yeni bir sunucu oluştururken yönetici parametresini belirtmek için [New-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) cmdlet 'ini kullanın. <br>
Var olan bir sunucunun yönetici parametresini değiştirmek için [set-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver) cmdlet 'ini kullanın.

## <a name="rest-api"></a>REST API

Yeni bir sunucu oluştururken asAdministrator özelliğini belirtmek için [Create](https://docs.microsoft.com/rest/api/analysisservices/servers/create) komutunu kullanın. <br>
Var olan bir sunucuyu değiştirirken asAdministrator özelliğini belirtmek için [Güncelleştir](https://docs.microsoft.com/rest/api/analysisservices/servers/update) ' i kullanın. <br>



## <a name="next-steps"></a>Sonraki adımlar 

[Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md)  
[Veritabanı rollerini ve kullanıcılarını yönetme](analysis-services-database-users.md)  
[Rol tabanlı Access Control](../role-based-access-control/overview.md)  

