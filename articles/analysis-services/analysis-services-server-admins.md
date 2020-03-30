---
title: Azure Analiz Hizmetleri'nde sunucu yöneticilerini yönetme | Microsoft Dokümanlar
description: Bu makalede, Azure portalı, PowerShell veya REST API'lerini kullanarak bir Azure Çözümleme Hizmetleri sunucusunun sunucu yöneticilerinin nasıl yönetilenolduğu açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f7c57a5751f2ff34abb26b7653070ce4ee5010fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572617"
---
# <a name="manage-server-administrators"></a>Sunucu yöneticilerini yönetme

Sunucu yöneticileri, sunucunun bulunduğu kiracı için Azure Etkin Dizini'nde (Azure AD) geçerli bir kullanıcı veya güvenlik grubu olmalıdır. Sunucu yöneticilerini yönetmek için Azure portalında sunucunuz için **Analiz Hizmetleri Yöneticileri,** SSMS, PowerShell veya REST API'deki Sunucu Özellikleri'ni kullanabilirsiniz. 

**Güvenlik grupları,** `MailEnabled` '' olarak `True`ayarlanmış özellikle posta yla [etkinleştirilmelidir.](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-enabled-security-groups) E-posta adresine göre `obj:groupid@tenantid`bir grup belirtirken kullanın.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Azure portalını kullanarak sunucu yöneticileri eklemek için

1. Portalda, sunucunuz için **Analiz Hizmetleri Yöneticileri'ni**tıklatın.
2. **Servername> - Analiz Hizmetleri Yöneticileri , Ekle'yi tıklatın. \<** **Add**
3. **Sunucu Yöneticileri Ekle'de,** Azure REKLAM'ınızdan kullanıcı hesaplarını seçin veya e-posta adresine göre harici kullanıcıları davet edin.

    ![Azure portalında Sunucu Yöneticileri](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>SSMS kullanarak sunucu yöneticileri eklemek için

1. Sunucuya sağ tıklayın > **Özellikler.**
2. **Analysis Server Properties'de** **Güvenlik'i**tıklatın.
3. **Ekle'yi**tıklatın ve ardından Azure AD'ınızda bir kullanıcı veya grubun e-posta adresini girin.
   
    ![SSMS'de sunucu yöneticileri ekleme](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Yeni bir sunucu oluştururken Yönetici parametresini belirtmek için [Yeni-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) cmdlet kullanın. <br>
Varolan bir sunucu için Yönetici parametresini değiştirmek için [Set-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver) cmdlet'i kullanın.

## <a name="rest-api"></a>REST API

Yeni bir sunucu oluştururken asAdministrator özelliğini belirtmek için [Oluştur'u](https://docs.microsoft.com/rest/api/analysisservices/servers/create) kullanın. <br>
Varolan bir sunucuyu değiştirirken asAdministrator özelliğini belirtmek için [Güncelleştirme'yi](https://docs.microsoft.com/rest/api/analysisservices/servers/update) kullanın. <br>



## <a name="next-steps"></a>Sonraki adımlar 

[Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md)  
[Veritabanı rollerini ve kullanıcılarını yönetme](analysis-services-database-users.md)  
[Rol Tabanlı Erişim Kontrolü](../role-based-access-control/overview.md)  

