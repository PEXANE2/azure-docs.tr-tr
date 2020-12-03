---
title: Silinen Microsoft 365 grubunu geri yükleme-Azure AD | Microsoft Docs
description: Silinen bir grubu geri yükleme, yeniden yüklenebilen grupları görüntüleme ve Azure Active Directory bir grubu kalıcı olarak silme
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: quickstart
ms.date: 12/02/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: d8a2aed6bf4a6b327ca7dd026f615a5d00af0bd8
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96546276"
---
# <a name="restore-a-deleted-microsoft-365-group-in-azure-active-directory"></a>Silinen bir Microsoft 365 grubunu Azure Active Directory geri yükleme

Azure Active Directory (Azure AD) içinde bir Microsoft 365 grubunu sildiğinizde, silinen grup korunur ancak silme tarihinden itibaren 30 gün boyunca görünmez. Bu davranış sayesinde, gerekirse grup ve içeriği geri yüklenebilir. Bu işlev, Azure AD 'de Microsoft 365 gruplara özel olarak kısıtlıdır. Güvenlik grupları ve dağıtım grupları için kullanılamaz. 30 günlük grup geri yükleme döneminin özelleştirilemez olduğunu lütfen unutmayın.

> [!NOTE]
> `Remove-MsolGroup` kullanmayın çünkü bu komut grubu kalıcı olarak siler. `Remove-AzureADMSGroup`Microsoft 365 grubunu silmek için her zaman kullanın.

Grubu geri yüklemek için gerekli izinler aşağıdakilerden herhangi biri olabilir:

Role | İzinler
--------- | ---------
Genel yönetici, Grup Yöneticisi, Iş ortağı Katman2 desteği ve Intune Yöneticisi | Silinen Microsoft 365 grubunu geri yükleyebilir
Kullanıcı Yöneticisi ve Iş ortağı Katman1 desteği | Şirket Yöneticisi rolüne atanan gruplar hariç silinen Microsoft 365 grubunu geri yükleyebilir
User | Sahip oldukları silinen Microsoft 365 grubunu geri yükleyebilir

## <a name="view-and-manage-the-deleted-microsoft-365-groups-that-are-available-to-restore"></a>Geri yüklenebilecek silinen Microsoft 365 gruplarını görüntüleyin ve yönetin

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) Kullanıcı yönetici hesabıyla oturum açın.

2. **Gruplar**' ı seçin ve ardından **silinen gruplar** ' ı seçerek geri yükleme için kullanılabilir olan grupları görüntüleyin.

    ![geri yüklenebilecek grupları görüntüle](./media/groups-restore-deleted/deleted-groups3.png)

3. **Silinen gruplar** dikey penceresinde şunları yapabilirsiniz:

   - **Geri yükleme grubu**' nu seçerek silinen grubu ve içeriğini geri yükleyin.
   - **Kalıcı olarak sil**' i seçerek silinen grubu kalıcı olarak kaldırın. Bir grubu kalıcı olarak kaldırmak için yönetici olmanız gerekir.

## <a name="view-the-deleted-microsoft-365-groups-that-are-available-to-restore-using-powershell"></a>PowerShell kullanarak geri yüklenebilecek silinen Microsoft 365 gruplarını görüntüle

İlgilendiğiniz bir veya birden çok silinmiş grubun henüz kalıcı olarak temizlenmediğini doğrulamak için, aşağıdaki cmdlet'ler kullanılarak silinmiş gruplar görüntülenebilir. Bu cmdlet’ler [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureAD/) parçasıdır. Bu modülle ilgili daha fazla bilgi [Azure Active Directory PowerShell Sürüm 2](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) makalesinde bulunabilir.

1.  Azure AD kuruluşunuzda, geri yükleme için kullanılabilir olan tüm silinen Microsoft 365 gruplarını göstermek için aşağıdaki cmdlet 'i çalıştırın.
   

    ```powershell
    Get-AzureADMSDeletedGroup
    ```

2.  Alternatif olarak, belirli bir grubun objectID değerini biliyorsanız (bunu 1. adımdaki cmdlet ile alabilirsiniz), silinmiş belirli bir grubun henüz kalıcı olarak silinmediğini doğrulamak için aşağıdaki cmdlet’i çalıştırın.

    ```
    Get-AzureADMSDeletedGroup –Id <objectId>
    ```

## <a name="how-to-restore-your-deleted-microsoft-365-group-using-powershell"></a>Silinen Microsoft 365 grubunuzu PowerShell kullanarak geri yükleme

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

Bir Microsoft 365 grubunu başarıyla geri yüklediğinizi doğrulamak için `Get-AzureADGroup –ObjectId <objectId>` cmdlet 'ini çalıştırarak grupla ilgili bilgileri görüntüleyin. Geri yükleme isteği tamamlandıktan sonra:

- Grup, Exchange’de Sol gezinti çubuğunda görünür
- Grubun planı Planner’da gösterilir
- Tüm SharePoint siteleri ve tüm içerikleri kullanılabilir olacaktır
- Gruba, Exchange uç noktalarından ve Microsoft 365 grupları destekleyen diğer Microsoft365 iş yüklerinden erişilebilir

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory gruplarıyla ilgili ek bilgi sağlanmıştır.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
