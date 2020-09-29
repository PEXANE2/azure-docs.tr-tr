---
title: Yönetici birimine Kullanıcı ekleme, kaldırma ve listeleme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory bir yönetim biriminde kullanıcıları ve rol izinlerini yönetme
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75e518a66ae2eedd952f521e0a67769b6e08de87
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91450427"
---
# <a name="add-and-manage-users-in-an-administrative-unit-in-azure-active-directory"></a>Azure Active Directory bir yönetim biriminde Kullanıcı ekleme ve yönetme

Azure Active Directory (Azure AD) ' de, denetimin daha ayrıntılı yönetim kapsamı için yönetici birimine (AU) Kullanıcı ekleyebilirsiniz.

PowerShell 'i kullanmaya hazırlanma ve yönetim birimi yönetimi için Microsoft Graph adımlar için bkz. [kullanmaya başlayın](roles-admin-units-manage.md#get-started).

## <a name="add-users-to-an-au"></a>AU 'ya Kullanıcı ekleme

### <a name="azure-portal"></a>Azure portal

Kullanıcıları, yönetim birimlerine iki şekilde atayabilirsiniz.

1. Bireysel atama

    1. Portalda Azure AD 'ye gidebilir ve kullanıcılar ' ı seçip bir yönetim birimine atanacak kullanıcıyı seçebilirsiniz. Daha sonra sol panelde yönetim birimleri ' ni seçebilirsiniz. Kullanıcı, yönetim birimine ata ' ya tıklayarak ve kullanıcının atanacağı yönetim birimlerini seçerek bir veya daha fazla yönetim birimine atanabilir.

       ![Ekle ' yi seçin ve ardından yönetim birimi için bir ad girin](./media/roles-admin-units-add-manage-users/assign-users-individually.png)

    1. Portalda Azure AD 'ye gidebilir ve sol bölmedeki yönetim birimleri ' ni seçip kullanıcıların atanacağı yönetim birimini seçebilirsiniz. **Tüm kullanıcılar** ' ı seçin ve ardından **üye Ekle**' yi seçin. Daha sonra, sağ bölmeden yönetim birimine atanacak bir veya daha fazla kullanıcı seçebilirsiniz.

        ![bir yönetim birimi seçip üye Ekle ' yi seçin.](./media/roles-admin-units-add-manage-users/assign-to-admin-unit.png)

1. Toplu atama

    Portalda Azure AD 'ye gidin ve yönetim birimleri ' ni seçin. Kullanıcıların ekleneceği yönetim birimini seçin. Tüm kullanıcılar ' a tıklayarak devam edin. csv dosyasından üye Ekle >. Daha sonra CSV şablonunu indirebilir ve dosyayı düzenleyebilirsiniz. Biçim basittir ve her satıra tek bir UPN eklenmesi gerekir. Dosya hazırlanıyor, uygun bir konuma kaydedin ve ardından anlık görüntüde vurgulanan şekilde adım 3 ' te karşıya yükleyin.

    ![kullanıcıları bir yönetim birimine toplu ata](./media/roles-admin-units-add-manage-users/bulk-assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrativeunitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
$UserObj = Get-AzureADUser -Filter "UserPrincipalName eq 'billjohn@fabidentity.onmicrosoft.com'"
Add-AzureADMSAdministrativeUnitMember -Id $administrativeunitObj.ObjectId -RefObjectId $UserObj.ObjectId
```

Yukarıdaki örnekte, kullanıcıyı yönetim birimine eklemek için Add-AzureADAdministrativeUnitMember cmdlet 'i kullanılır. Kullanıcının ekleneceği yönetim biriminin nesne KIMLIĞI ve eklenecek kullanıcının nesne KIMLIĞI bağımsız değişken olarak alınır. Vurgulanan bölüm, belirli bir ortam için gerektiği şekilde değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /administrativeUnits/{Admin Unit id}/members/$ref
Request body
{
  "@odata.id":"https://graph.microsoft.com/v1.0/users/{id}"
}
```

Örnek:

```http
{
  "@odata.id":"https://graph.microsoft.com/v1.0/users/johndoe@fabidentity.com"
}
```

## <a name="list-administrative-units-for-a-user"></a>Bir kullanıcı için yönetim birimlerini listeleme

### <a name="azure-portal"></a>Azure portal

Azure portal bir kullanıcının profilini şu şekilde açabilirsiniz:

1. **Azure AD**  >  **kullanıcıları**açılıyor.

1. Kullanıcının profilini açmak için kullanıcıyı seçin.

1. Kullanıcının atandığı yönetim birimlerinin listesini görmek için **yönetim birimleri** ' ni seçin.

   ![Bir kullanıcı için yönetim birimlerini listeleme](./media/roles-admin-units-add-manage-users/list-user-admin-units.png)

### <a name="powershell"></a>PowerShell

```powershell
Get-AzureADMSAdministrativeUnit | where { Get-AzureADMSAdministrativeUnitMember -Id $_.ObjectId | where {$_.RefObjectId -eq $userObjId} }
```

### <a name="microsoft-graph"></a>Microsoft Graph

```http
https://graph.microsoft.com/v1.0/users/{id}/memberOf/$/Microsoft.Graph.AdministrativeUnit
```

## <a name="remove-a-single-user-from-an-au"></a>Tek bir kullanıcıyı AU 'dan kaldırma

### <a name="azure-portal"></a>Azure portal

Bir kullanıcıyı yönetim biriminden kaldırabilmeniz için iki yol vardır. Azure Portal **Azure AD**kullanıcılarına giderek bir kullanıcının profilini açabilirsiniz  >  **Users**. Kullanıcının profilini açmak için kullanıcıyı seçin. Kullanıcının kaldırılmasını istediğiniz yönetim birimini seçin ve **Yönetim biriminden kaldır**' ı seçin.

![Kullanıcı profilinden bir yönetim biriminden kullanıcı kaldırma](./media/roles-admin-units-add-manage-users/user-remove-admin-units.png)

Ayrıca, kullanıcıları kaldırmak istediğiniz yönetim birimini seçerek **Azure AD**  >  **Yönetim birimlerindeki** bir kullanıcıyı da kaldırabilirsiniz. Kullanıcıyı seçin ve **üyeyi kaldır**' ı seçin.
  
![Yönetim birimi düzeyinde Kullanıcı kaldırma](./media/roles-admin-units-add-manage-users/admin-units-remove-user.png)

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureADMSAdministrativeUnitMember -Id $auId -MemberId $memberUserObjId
```

### <a name="microsoft-graph"></a>Microsoft Graph

   https://graph.microsoft.com/v1.0/directory/administrativeUnits/{adminunit-id}/members/{user-id}/$ref

## <a name="bulk-remove-more-than-one-user"></a>Birden fazla kullanıcıyı toplu kaldırma

Azure AD > yönetim birimlerine giderek kullanıcıları kaldırmak istediğiniz yönetim birimini seçebilirsiniz. Üyeyi toplu Kaldır ' a tıklayın. Kaldırılacak kullanıcıların listesini sağlamak için CSV şablonunu indirin.

İndirilen CSV şablonunu ilgili Kullanıcı girişleriyle düzenleyin. Şablonun ilk iki satırını kaldırmayın. Her satıra bir Kullanıcı UPN 'si ekleyin.

![Yönetim birimlerinden toplu Kullanıcı kaldırma için CSV dosyasını düzenleme](./media/roles-admin-units-add-manage-users/bulk-user-entries.png)

Girdileri dosyaya kaydettikten sonra dosyayı karşıya yükleyin, **Gönder**' i seçin.

![Toplu karşıya yükleme dosyasını gönder](./media/roles-admin-units-add-manage-users/bulk-user-remove.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetim birimine rol atama](roles-admin-units-assign-roles.md)
- [Yönetim birimine gruplar ekleme](roles-admin-units-add-manage-groups.md)
