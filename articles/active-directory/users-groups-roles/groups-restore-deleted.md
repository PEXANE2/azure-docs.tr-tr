---
title: Restore a deleted Office 365 group - Azure AD | Microsoft Docs
description: How to restore a deleted group, view restorable groups, and permanently delete a group in Azure Active Directory
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96d212df51a58125e3b959a18f5cf2ac9d391d30
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422381"
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Azure Active Directory’de silinmiş bir Office 365 grubunu geri yükleme

Azure Active Directory’de (Azure AD) bir Office 365 grubunu sildiğinizde, silinen grup görüntülenmez ancak silindiği tarihten itibaren 30 gün boyunca tutulur. Bu davranış sayesinde, gerekirse grup ve içeriği geri yüklenebilir. Bu işlev yalnızca Azure AD’deki Office 365 gruplarına özgüdür. Güvenlik grupları ve dağıtım grupları için kullanılamaz. Please note that the 30-day group restoration period is not customizable.

> [!NOTE]
> `Remove-MsolGroup` kullanmayın çünkü bu komut grubu kalıcı olarak siler. Always use `Remove-AzureADMSGroup` to delete an Office 365 group.

Grubu geri yüklemek için gerekli izinler aşağıdakilerden herhangi biri olabilir:

Rol | İzinler
--------- | ---------
Global administrator, Group administrator, Partner Tier2 support, and Intune administrator | Silinen herhangi bir Office 365 grubunu geri yükleyebilir
User administrator and Partner Tier1 support | Can restore any deleted Office 365 group except those groups assigned to the Company Administrator role
Kullanıcı | Can restore any deleted Office 365 group that they own

## <a name="view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore"></a>View and manage the deleted Office 365 groups that are available to restore

1. Sign in to the [Azure AD admin center](https://aad.portal.azure.com) with a User administrator account.

2. Select **Groups**, then select **Deleted groups** to view the deleted groups that are available to restore.

    ![view groups that are available to restore](media/groups-lifecycle/deleted-groups3.png)

3. On the **Deleted groups** blade, you can:

   - Restore the deleted group and its contents by selecting **Restore group**.
   - Permanently remove the deleted group by selecting **Delete permanently**. To permanently remove a group, you must be an administrator.

## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore-using-powershell"></a>View the deleted Office 365 groups that are available to restore using Powershell

İlgilendiğiniz bir veya birden çok silinmiş grubun henüz kalıcı olarak temizlenmediğini doğrulamak için, aşağıdaki cmdlet'ler kullanılarak silinmiş gruplar görüntülenebilir. Bu cmdlet’ler [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureAD/) parçasıdır. Bu modülle ilgili daha fazla bilgi [Azure Active Directory PowerShell Sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0) makalesinde bulunabilir.

1.  Kiracınızda yer alan tüm geri yüklenmeye uygun, silinmiş Office 365 gruplarını görüntülemek için bu cmdlet’i çalıştırın.
   

    ```powershell
    Get-AzureADMSDeletedGroup
    ```

2.  Alternatif olarak, belirli bir grubun objectID değerini biliyorsanız (bunu 1. adımdaki cmdlet ile alabilirsiniz), silinmiş belirli bir grubun henüz kalıcı olarak silinmediğini doğrulamak için aşağıdaki cmdlet’i çalıştırın.

    ```
    Get-AzureADMSDeletedGroup –Id <objectId>
    ```

## <a name="how-to-restore-your-deleted-office-365-group-using-powershell"></a>How to restore your deleted Office 365 group using Powershell

Grubun hala geri yüklemeye uygun olduğunu doğruladıktan sonra aşağıdaki adımlardan birini kullanarak silinmiş grubu geri yükleyin. Grup belgeler, SP siteleri veya diğer kalıcı nesneler içeriyorsa, grubu ve tüm içeriğini tümüyle geri yüklemek 24 saat kadar sürebilir.

1. Grubu ve içeriğini geri yüklemek için aşağıdaki cmdlet’i çalıştırın.
 

   ```
    Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
    ``` 

2. Alternatif olarak, silinmiş grubu kalıcı olarak silmek için aşağıdaki cmdlet kullanılabilir.
    

    ```
    Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
    ```

## <a name="how-do-you-know-this-worked"></a>İşe yaradığını nasıl anlarsınız?

Office 365 grubunu başarıyla geri yüklediğinizi doğrulamak için, grup hakkındaki bilgileri görüntüleyen `Get-AzureADGroup –ObjectId <objectId>` cmdlet’ini çalıştırın. Geri yükleme isteği tamamlandıktan sonra:

- Grup, Exchange’de Sol gezinti çubuğunda görünür
- Grubun planı Planner’da gösterilir
- Any SharePoint sites and all of their contents will be available
- Gruba, herhangi bir Exchange uç noktasından ve Office 365 gruplarını destekleyen diğer Office 365 iş yüklerinden erişilebilir

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory gruplarıyla ilgili ek bilgi sağlanmıştır.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
