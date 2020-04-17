---
title: Bir yönetim birimindeki kullanıcıları ekleme, kaldırma ve listele (önizleme) - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde bir yönetim biriminde kullanıcıları ve rol izinlerini yönetme
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
ms.openlocfilehash: 2b2b901f652564c47ca35cb0f75a69f26fa2fa71
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81533233"
---
# <a name="add-and-manage-users-in-an-administrative-unit-in-azure-active-directory"></a>Azure Etkin Dizini'nde bir yönetim birimindeki kullanıcıları ekleme ve yönetme

Azure Etkin Dizini'nde (Azure AD), daha ayrıntılı denetim kapsamı için kullanıcıları bir yönetim birimine (AU) ekleyebilirsiniz.

Yönetim birimi yönetimi için PowerShell ve Microsoft Graph'ı kullanmaya hazırlanma adımları [için](roles-admin-units-manage.md#getting-started)bkz.

## <a name="add-users-to-an-au"></a>Kullanıcıları AU'ya ekleme

### <a name="azure-portal"></a>Azure portal

Kullanıcıları yönetim birimlerine iki şekilde atayabilirsiniz.

1. Bireysel atama

    1. Portaldaki Azure AD'ye gidebilir ve Kullanıcılar'ı seçebilir ve yönetim birimine atanacak kullanıcıyı seçebilirsiniz. Daha sonra sol panelde Yönetim birimleri seçebilirsiniz. Kullanıcı, yönetim birimine atamak ve kullanıcının atanacağı yönetim birimlerini seçerek bir veya daha fazla yönetim birimine atanabilir.

       ![ekle'yi seçin ve ardından yönetim birimi için bir ad girin](./media/roles-admin-units-add-manage-users/assign-users-individually.png)

    1. Portaldaki Azure AD'ye gidebilir ve sol bölmedeki Yönetim birimlerini seçebilir ve ardından kullanıcıların atanacağı yönetim birimini seçebilirsiniz. Sol bölmedeki Tüm kullanıcıları seçin ve ardından Üye Ekle'yi seçin. Daha sonra devam edebilir ve sağ bölmeden yönetim birimine atanacak bir veya daha fazla kullanıcı seçebilirsiniz.

        ![bir yönetim birimi seçin ve sonra üye ekle'yi seçin](./media/roles-admin-units-add-manage-users/assign-to-admin-unit.png)

1. Toplu atama

    Portaldaki Azure AD'ye gidin ve Yönetim birimlerini seçin. Kullanıcıların eklenecek olduğu yönetim birimini seçin. Tüm kullanıcılara tıklayarak devam edin -> .csv dosyasından üye ekleyin. Daha sonra CSV şablonu indirebilir ve dosyayı edinebilirsiniz. Biçim basittir ve her satıra eklenmesi için tek bir UPN gerekir. Dosya hazır olduğunda, uygun bir konuma kaydedin ve anlık görüntüde vurgulandığı gibi adım 3'e yükleyin.

    ![kullanıcıları bir yönetim birimine toplu olarak atama](./media/roles-admin-units-add-manage-users/bulk-assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

    $administrativeunitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    $UserObj = Get-AzureADUser -Filter "UserPrincipalName eq 'billjohn@fabidentity.onmicrosoft.com'"
    Add-AzureADAdministrativeUnitMember -ObjectId $administrativeunitObj.ObjectId -RefObjectId $UserObj.ObjectId

Yukarıdaki örnekte, cmdlet Add-AzureADAdministrativeUnitMember, kullanıcıyı yönetim birimine eklemek için kullanılır. Kullanıcının eklenecek olan yönetim biriminin nesne kimliği ve eklenecek kullanıcının nesne kimliği bağımsız değişken olarak alınır. Vurgulanan bölüm, belirli bir ortam için gerektiği gibi değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

    Http request
    POST /administrativeUnits/{Admin Unit id}/members/$ref
    Request body
    {
      "@odata.id":"https://graph.microsoft.com/beta/users/{id}"
    }

Örnek:

    {
      "@odata.id":"https://graph.microsoft.com/beta/users/johndoe@fabidentity.com"
    }

## <a name="list-administrative-units-for-a-user"></a>Bir kullanıcı için yönetim birimlerini listele

### <a name="azure-portal"></a>Azure portal

Azure portalında, Kullanıcılar > Azure AD'ye giderek bir kullanıcının profilini açabilirsiniz. Kullanıcının profilini açmak için kullanıcının üzerine tıklayın.

![Azure Active Directory'de bir kullanıcı profili açma](./media/roles-admin-units-add-manage-users/user-profile-admin-units.png)

Kullanıcının atandığı yönetim birimlerinin listesini görmek için sol panelde **Yönetim birimlerini** seçin.

![Bir kullanıcıiçin yönetim birimlerini listele](./media/roles-admin-units-add-manage-users/list-user-admin-units.png)

### <a name="powershell"></a>PowerShell

    Get-AzureADAdministrativeUnit | where { Get-AzureADAdministrativeUnitMember -ObjectId $_.ObjectId | where {$_.ObjectId -eq $userObjId} }

### <a name="microsoft-graph"></a>Microsoft Graph

    https://graph.microsoft.com/beta/users//memberOf/$/Microsoft.Graph.AdministrativeUnit

## <a name="remove-a-single-user-from-an-au"></a>Tek bir kullanıcıyı AU'dan kaldırma

### <a name="azure-portal"></a>Azure portal

Bir kullanıcıyı yönetim biriminden kaldırmanın iki yolu vardır. Azure portalında **Azure AD** > **Kullanıcıları'na**giderek bir kullanıcının profilini açabilirsiniz. Kullanıcının profilini açmak için kullanıcıyı seçin. Kullanıcının kaldırılmasını istediğiniz yönetim birimini seçin ve **yönetim biriminden kaldır'ı**seçin.

![Kullanıcı profilinden bir yönetim biriminden kullanıcıyı kaldırma](./media/roles-admin-units-add-manage-users/user-remove-admin-units.png)

Ayrıca, kullanıcıları kaldırmak istediğiniz yönetim birimini seçerek **Azure AD** > **Yönetim birimlerindeki** bir kullanıcıyı da kaldırabilirsiniz. Kullanıcıyı seçin ve **Üyeyi Kaldır'ı**seçin.
  
![Bir kullanıcıyı yönetim birimi düzeyinde kaldırma](./media/roles-admin-units-add-manage-users/admin-units-remove-user.png)

### <a name="powershell"></a>PowerShell

    Remove-AzureADAdministrativeUnitMember -ObjectId $auId -MemberId $memberUserObjId

### <a name="microsoft-graph"></a>Microsoft Graph

   https://graph.microsoft.com/beta/administrativeUnits/<adminunit-id>/members/<user-id>/$ref

## <a name="bulk-remove-more-than-one-user"></a>Toplu olarak birden fazla kullanıcıyı kaldırın

Azure AD > Yönetim birimlerine gidebilir ve kullanıcıları kaldırmak istediğiniz yönetim birimini seçebilirsiniz. Toplu kaldır üye'ye tıklayın. Kaldırılacak kullanıcı listesini sağlamak için CSV şablonuna indirin.

İndirilen CSV şablonunu ilgili kullanıcı girişleriyle birlikte edin. Şablonun ilk iki satırını kaldırmayın. Her satıra bir kullanıcı UPN ekleyin.

![Yönetim birimlerinden toplu kullanıcı kaldırma için CSV dosyasını edin](./media/roles-admin-units-add-manage-users/bulk-user-entries.png)

Girişleri dosyaya kaydettikten sonra dosyayı yükleyin ve **Gönder'i**seçin.

![Toplu yükleme dosyasını gönderme](./media/roles-admin-units-add-manage-users/bulk-user-remove.png)

## <a name="next-steps"></a>Sonraki adımlar

- [İdari bir birime rol atama](roles-admin-units-assign-roles.md)
- [Yönetim birimine gruplar ekleme](roles-admin-units-add-manage-groups.md)
