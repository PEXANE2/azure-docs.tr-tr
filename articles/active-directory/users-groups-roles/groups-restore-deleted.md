---
title: Silinen Office 365 grubunu geri yükleme - Azure AD | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde silinen bir grubu geri yükleme, geri yüklenebilir grupları görüntüleme ve kalıcı olarak silme
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74422381"
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Azure Active Directory’de silinmiş bir Office 365 grubunu geri yükleme

Azure Active Directory’de (Azure AD) bir Office 365 grubunu sildiğinizde, silinen grup görüntülenmez ancak silindiği tarihten itibaren 30 gün boyunca tutulur. Bu davranış sayesinde, gerekirse grup ve içeriği geri yüklenebilir. Bu işlev yalnızca Azure AD’deki Office 365 gruplarına özgüdür. Güvenlik grupları ve dağıtım grupları için kullanılamaz. 30 günlük grup restorasyon süresinin özelleştirilemeyeceğini lütfen unutmayın.

> [!NOTE]
> `Remove-MsolGroup` kullanmayın çünkü bu komut grubu kalıcı olarak siler. Office `Remove-AzureADMSGroup` 365 grubunu silmek için her zaman kullanın.

Grubu geri yüklemek için gerekli izinler aşağıdakilerden herhangi biri olabilir:

Rol | İzinler
--------- | ---------
Global yönetici, Grup yöneticisi, İş Ortağı Tier2 desteği ve Intune yöneticisi | Silinen herhangi bir Office 365 grubunu geri yükleyebilir
Kullanıcı yöneticisi ve İş Ortağı Tier1 desteği | Şirket Yöneticisi rolüne atanan gruplar dışında silinen office 365 grubunu geri yükleyebilir
Kullanıcı | Sahip oldukları silinen Office 365 grubunu geri yükleyebilir

## <a name="view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore"></a>Geri yüklemek için kullanılabilen silinmiş Office 365 gruplarını görüntüleme ve yönetme

1. Kullanıcı yöneticisi hesabıyla [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.

2. **Gruplar'ı**seçin, ardından geri yüklemeye hazır silinmiş grupları görüntülemek için **Silinmiş grupları** seçin.

    ![geri yüklemeye hazır grupları görüntüleme](media/groups-lifecycle/deleted-groups3.png)

3. **Silinen gruplar** bıçak, şunları yapabilirsiniz:

   - Geri Yükle grubunu seçerek silinen grubu ve içeriğini geri **yükleyin.**
   - Silin'i kalıcı olarak seçerek silinen grubu **kalıcı olarak**kaldırın. Bir grubu kalıcı olarak kaldırmak için yönetici olmalısınız.

## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore-using-powershell"></a>Powershell'i kullanarak geri yüklemeye hazır silinmiş Office 365 gruplarını görüntüleme

İlgilendiğiniz bir veya birden çok silinmiş grubun henüz kalıcı olarak temizlenmediğini doğrulamak için, aşağıdaki cmdlet'ler kullanılarak silinmiş gruplar görüntülenebilir. Bu cmdlet’ler [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureAD/) parçasıdır. Bu modülle ilgili daha fazla bilgi [Azure Active Directory PowerShell Sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0) makalesinde bulunabilir.

1.  Kiracınızda yer alan tüm geri yüklenmeye uygun, silinmiş Office 365 gruplarını görüntülemek için bu cmdlet’i çalıştırın.
   

    ```powershell
    Get-AzureADMSDeletedGroup
    ```

2.  Alternatif olarak, belirli bir grubun objectID değerini biliyorsanız (bunu 1. adımdaki cmdlet ile alabilirsiniz), silinmiş belirli bir grubun henüz kalıcı olarak silinmediğini doğrulamak için aşağıdaki cmdlet’i çalıştırın.

    ```
    Get-AzureADMSDeletedGroup –Id <objectId>
    ```

## <a name="how-to-restore-your-deleted-office-365-group-using-powershell"></a>Powershell'i kullanarak silinen Office 365 grubunuz nasıl geri yüklenir?

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
- Herhangi bir SharePoint sitesi ve tüm içeriği kullanılabilir olacak
- Gruba, herhangi bir Exchange uç noktasından ve Office 365 gruplarını destekleyen diğer Office 365 iş yüklerinden erişilebilir

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory gruplarıyla ilgili ek bilgi sağlanmıştır.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
