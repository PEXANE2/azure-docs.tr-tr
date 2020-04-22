---
title: Yönetim biriminde gruplar ekleme, kaldırma ve listeleme (önizleme) - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'ndeki bir yönetim biriminde grupları ve rol izinlerini yönetme
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f0b0c0a7b666aed56ad24625c80c0a57683b998
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683265"
---
# <a name="add-and-manage-groups-in-administrative-units-in-azure-active-directory"></a>Azure Etkin Dizini'nde yönetim birimlerinde gruplar ekleme ve yönetme

Azure Etkin Dizini'nde (Azure AD), daha ayrıntılı denetim kapsamı için bir yönetim birimine (AU) gruplar ekleyebilirsiniz.

Yönetim birimi yönetimi için PowerShell ve Microsoft Graph'ı kullanmaya hazırlanma adımları [için](roles-admin-units-manage.md#get-started)bkz.

## <a name="add-groups-to-an-au"></a>AU'ya grup ekleme

### <a name="azure-portal"></a>Azure portal

Önizlemede, grupları yalnızca tek tek bir yönetim birimine atayabilirsiniz. Grupların yönetim birimine toplu atama seçeneği yoktur. Bir grubu portaldaki iki şekildebir yönetim birimine atayabilirsiniz:

1. Azure **AD > Grupları** sayfasından

    Azure AD'de Gruplara genel bakış sayfasını açın ve yönetim birimine atanması gereken grubu seçin. Sol tarafta, grubun atandığı yönetim birimlerini listelemek için **Yönetim birimlerini** seçin. Üstte yönetim birimine atama seçeneğini bulacaksınız ve üzerine tıklayarak yönetim birimi seçmek için sağ tarafta bir panel verecektir.

    ![bir grubu tek tek bir yönetim birimine atamak](./media/roles-admin-units-add-manage-groups/assign-to-group-1.png)

1. Azure **AD > Yönetim birimlerinden Tüm Gruplar** sayfasını >

    Azure AD > Yönetim Birimlerinde Tüm Gruplar bıçağını açın. Yönetim birimine atanmış gruplar varsa, bunlar sağ tarafta görüntülenir. Üstte **Ekle'yi** seçin ve sağ panel Azure REKLAM kuruluşunuzdaki grupları listelerken slayt olur. Yönetim birimlerine atanacak bir veya daha fazla grup seçin.

    ![bir yönetim birimi seçin ve sonra üye ekle'yi seçin](./media/roles-admin-units-add-manage-groups/assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

    $administrative unitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    $GroupObj = Get-AzureADGroup -Filter "displayname eq 'TestGroup'"
    Add-AzureADAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId -RefObjectId $GroupObj.ObjectId

Bu örnekte, cmdlet Add-AzureADAdministrativeUnitMember, grubu yönetim birimine eklemek için kullanılır. İdari birimin nesne kimliği ve eklenecek grubun nesne kimliği bağımsız değişken olarak alınır. Vurgulanan bölüm, belirli bir ortam için gerektiği gibi değiştirilebilir.

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

## <a name="list-groups-in-an-au"></a>AU'daki grupları listele

### <a name="azure-portal"></a>Azure portal

Portaldaki **Azure AD > Yönetim birimlerine** gidin. Kullanıcıları listelemek istediğiniz yönetim birimini seçin. Varsayılan olarak, **tüm kullanıcılar** sol panelde zaten seçilir. **Tüm grupları** seçin ve sağda seçilen yönetim biriminin üyesi grupların listesini bulacaksınız.

![Silmek için bir yönetim birimi seçin](./media/roles-admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

    $administrative unitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    Get-AzureADAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId

Bu, idari birimin tüm üyelerini almanıza yardımcı olacaktır. Yönetim biriminin üyesi olan tüm grupları görüntülemek istiyorsanız, aşağıdaki kod parçacıklarını kullanabilirsiniz:

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

## <a name="list-aus-for-a-group"></a>Bir grup için AUs'ları listele

### <a name="azure-portal"></a>Azure portal

Azure AD portalında, **Gruplar'ı**açarak bir grubun ayrıntılarını açabilirsiniz. Grubun profilini açmak için bir grup seçin. Grubun üyesi olduğu tüm yönetim birimlerini listelemek için **Yönetim birimlerini** seçin.

![Bir grup için yönetim birimlerini listele](./media/roles-admin-units-add-manage-groups/list-group-au.png)

### <a name="powershell"></a>PowerShell

    Get-AzureADAdministrativeUnit | where { Get-AzureADAdministrativeUnitMember -ObjectId $_.ObjectId | where {$_.ObjectId -eq $groupObjId} }

### <a name="microsoft-graph"></a>Microsoft Graph

    https://graph.microsoft.com/beta/groups/<group-id>/memberOf/$/Microsoft.Graph.AdministrativeUnit

## <a name="remove-a-group-from-an-au"></a>Grubu AU'dan kaldırma

### <a name="azure-portal"></a>Azure portal

Azure portalındaki bir yönetim biriminden bir grubu kaldırmanın iki yolu vardır.

**Azure REKLAM** > **Grupları'nı** açın ve yönetim biriminden kaldırmak istediğiniz grup için profili açın. Grubun üyesi olduğu tüm yönetim birimlerini listelemek için sol panelde **Yönetim birimlerini** seçin. Grubu kaldırmak istediğiniz yönetim birimini seçin ve ardından **yönetim biriminden kaldır'ı**seçin.

![Bir grubu yönetim biriminden kaldırma](./media/roles-admin-units-add-manage-groups/group-au-remove.png)

Alternatif olarak, Azure **AD** > **Yönetim birimlerine** gidebilir ve grubun üyesi olduğu yönetim birimini seçebilirsiniz. Üye grupları listelemek için sol paneldeki **Gruplar'ı** seçin. Yönetim biriminden kaldırılacak grubu seçin ve ardından **Grupları Kaldır'ı**seçin.

![Yönetim birimindeki grupları listele](./media/roles-admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

    Remove-AzureADAdministrativeUnitMember -ObjectId $auId -MemberId $memberGroupObjId

### <a name="microsoft-graph"></a>Microsoft Graph

    https://graph.microsoft.com/beta/administrativeUnits/<adminunit-id>/members/<group-id>/$ref

## <a name="next-steps"></a>Sonraki adımlar

- [İdari bir birime rol atama](roles-admin-units-assign-roles.md)
- [Yönetim birimindeki kullanıcıları yönetme](roles-admin-units-add-manage-users.md)
