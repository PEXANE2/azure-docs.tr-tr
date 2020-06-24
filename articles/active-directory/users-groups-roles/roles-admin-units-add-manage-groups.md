---
title: Yönetim biriminde gruplar ekleme, kaldırma ve listeleme (Önizleme)-Azure Active Directory | Microsoft Docs
description: Azure Active Directory bir yönetim biriminde grupları ve rol izinlerini yönetme
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: d549d64dc2729b5d6280089e42d456043fb18393
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84732506"
---
# <a name="add-and-manage-groups-in-administrative-units-in-azure-active-directory"></a>Azure Active Directory içindeki yönetim birimlerindeki grupları ekleme ve yönetme

Azure Active Directory (Azure AD) içinde, denetimin daha ayrıntılı yönetim kapsamı için yönetim birimine (AU) gruplar ekleyebilirsiniz.

PowerShell 'i kullanmaya hazırlanma ve yönetim birimi yönetimi için Microsoft Graph adımlar için bkz. [kullanmaya başlayın](roles-admin-units-manage.md#get-started).

## <a name="add-groups-to-an-au"></a>AU 'ya gruplar ekleme

### <a name="azure-portal"></a>Azure portal

Önizlemede, grupları yalnızca bir yönetim birimine tek tek atayabilirsiniz. Yönetim birimine grupların toplu atama seçeneği yoktur. Portal 'da iki farklı şekilde yönetim birimine bir grup atayabilirsiniz:

1. **Azure AD > grupları** sayfasından

    Azure AD 'de gruplara genel bakış sayfasını açın ve yönetim birimine atanması gereken grubu seçin. Sol tarafta, grubun atandığı yönetim birimlerini listelemek için **yönetim birimleri** ' ni seçin. En üstte, yönetim birimine ata seçeneğini bulacak ve üzerine tıkladığınızda, yönetim birimini seçmek için sağ tarafa bir panel verecektir.

    ![bir yönetim birimine tek bir grup atama](./media/roles-admin-units-add-manage-groups/assign-to-group-1.png)

1. **Tüm Gruplar sayfasında > Azure AD > yönetim birimleri** ' nden

    Azure AD > yönetim birimlerindeki tüm gruplar dikey penceresini açın. Yönetim birimine zaten atanmış gruplar varsa, bu gruplar sağ tarafta görüntülenir. Üstteki **Ekle** ' yi seçin ve sağ panel, Azure AD kuruluşunuzda mevcut olan grupları listelemek Için slayt ekler. Yönetim birimlerine atanacak bir veya daha fazla grup seçin.

    ![bir yönetim birimi seçip üye Ekle ' yi seçin.](./media/roles-admin-units-add-manage-groups/assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

    $administrative unitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    $GroupObj = Get-AzureADGroup -Filter "displayname eq 'TestGroup'"
    Add-AzureADAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId -RefObjectId $GroupObj.ObjectId

Bu örnekte, grubu yönetim birimine eklemek için Add-AzureADAdministrativeUnitMember cmdlet 'i kullanılır. Yönetim biriminin nesne KIMLIĞI ve eklenecek grubun nesne KIMLIĞI bağımsız değişken olarak alınır. Vurgulanan bölüm, belirli bir ortam için gerektiği şekilde değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

    Http request
    POST /administrativeUnits/{Admin Unit id}/members/$ref

    Request body
    {
      "@odata.id":"https://graph.microsoft.com/beta/groups/{id}"
    }

Örnek:

    {
      "@odata.id":"https://graph.microsoft.com/beta/users/ 871d21ab-6b4e-4d56-b257-ba27827628f3"
    }

## <a name="list-groups-in-an-au"></a>AU 'daki liste grupları

### <a name="azure-portal"></a>Azure portal

Portalda **Azure AD > yönetim birimleri** ' ne gidin. Kullanıcıları listelemek istediğiniz yönetim birimini seçin. Varsayılan olarak, **tüm kullanıcılar** zaten sol panelde seçilidir. **Tüm grupları** seçin ve sağ tarafta seçili yönetim biriminin üyesi olan grupların listesini bulabilirsiniz.

![Yönetim birimindeki grupları listeleme](./media/roles-admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

    $administrative unitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    Get-AzureADAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId

Bu, yönetim biriminin tüm üyelerini almanıza yardımcı olur. Yönetim biriminin üyesi olan tüm grupları göstermek istiyorsanız aşağıdaki kod parçacığını kullanabilirsiniz:

    foreach ($member in (Get-AzureADAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId)) 
    {
    if($member.ObjectType -eq "Group")
    {
    Get-AzureADGroup -ObjectId $member.ObjectId
    }
    }

### <a name="microsoft-graph"></a>Microsoft Graph

    HTTP request
    GET /administrativeUnits/{Admin id}/members/$/microsoft.graph.group
    Request body
    {}

## <a name="list-aus-for-a-group"></a>Bir grup için au listesini listeleyin

### <a name="azure-portal"></a>Azure portal

Azure AD portalında, **gruplar**' ı açarak bir grubun ayrıntılarını açabilirsiniz. Grubun profilini açmak için bir grup seçin. Grubun üye olduğu tüm yönetim birimlerini listelemek için **yönetim birimleri** ' ni seçin.

![Bir grup için yönetim birimlerini listeleme](./media/roles-admin-units-add-manage-groups/list-group-au.png)

### <a name="powershell"></a>PowerShell

    Get-AzureADAdministrativeUnit | where { Get-AzureADAdministrativeUnitMember -ObjectId $_.ObjectId | where {$_.ObjectId -eq $groupObjId} }

### <a name="microsoft-graph"></a>Microsoft Graph

    https://graph.microsoft.com/beta/groups/<group-id>/memberOf/$/Microsoft.Graph.AdministrativeUnit

## <a name="remove-a-group-from-an-au"></a>AU 'dan bir grubu kaldırma

### <a name="azure-portal"></a>Azure portal

Azure portal bir grubu bir yönetim biriminden kaldırabilmeniz için iki yol vardır.

**Azure AD**  >  **gruplarını** açın ve yönetim biriminden kaldırmak istediğiniz grubun profilini açın. Grubun üye olduğu tüm yönetim birimlerini listelemek için sol panelde **yönetim birimleri** ' ni seçin. Grubu kaldırmak istediğiniz yönetim birimini seçin ve ardından **Yönetim biriminden kaldır**' ı seçin.

![Yönetim biriminden bir grubu kaldırma](./media/roles-admin-units-add-manage-groups/group-au-remove.png)

Alternatif olarak, **Azure AD**  >  **yönetim birimlerine** gidebilir ve grubun üye olduğu yönetim birimini seçebilirsiniz. Üye gruplarını listelemek için sol paneldeki **gruplar** ' ı seçin. Yönetim biriminden kaldırılacak grubu seçin ve ardından **grupları kaldır**' ı seçin.

![Yönetim birimindeki grupları listeleme](./media/roles-admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

    Remove-AzureADAdministrativeUnitMember -ObjectId $auId -MemberId $memberGroupObjId

### <a name="microsoft-graph"></a>Microsoft Graph

    https://graph.microsoft.com/beta/administrativeUnits/<adminunit-id>/members/<group-id>/$ref

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetim birimine rol atama](roles-admin-units-assign-roles.md)
- [Yönetici birimindeki kullanıcıları yönetme](roles-admin-units-add-manage-users.md)
