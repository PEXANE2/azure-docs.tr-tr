---
title: Statik grup üyeliğini dinamik olarak değiştirme - Azure AD | Microsoft Dokümanlar
description: Grupları otomatik olarak doldurmak için üyelik kuralları ve kural başvurusu oluşturma.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34451fcc4597f77464e5e9566613e21e9fecdbc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74027308"
---
# <a name="change-static-group-membership-to-dynamic-in-azure-active-directory"></a>Azure Etkin Dizini'nde statik grup üyeliğini dinamik olarak değiştirme

Azure Etkin Dizini'nde (Azure AD) bir grubun üyeliğini statikten dinamik (veya tam tersi) olarak değiştirebilirsiniz. Azure AD, sistemde aynı grup adını ve kimliğini tutar, bu nedenle gruba yapılan tüm varolan başvurular hala geçerlidir. Bunun yerine yeni bir grup oluşturursanız, bu başvuruları güncelleştirmeniz gerekir. Dinamik grup üyeliği, yönetim ek yükü eklemeyi ve kullanıcıları kaldırmayı ortadan kaldırır. Bu makalede, Azure AD Yönetici merkezi veya PowerShell cmdlets kullanarak varolan grupları statikten dinamik üyeliğe nasıl dönüştürebilirsiniz.

> [!WARNING]
> Varolan statik bir grubu dinamik bir gruba değiştirirken, varolan tüm üyeler gruptan kaldırılır ve yeni üyeler eklemek için üyelik kuralı işlenir. Grup uygulamalara veya kaynaklara erişimi denetlemek için kullanılırsa, üyelik kuralı tam olarak işlenene kadar orijinal üyelerin erişimi kaybedebileceğini unutmayın.
>
> Gruptaki yeni üyeliğin beklendiği gibi olduğundan emin olmak için yeni üyelik kuralını önceden test etmenizi öneririz.

## <a name="change-the-membership-type-for-a-group"></a>Bir grubun üyelik türünü değiştirme

1. Azure AD [yönetici merkezinde,](https://aad.portal.azure.com) kiracınızda küresel yönetici veya kullanıcı yöneticisi olan bir hesapla oturum açın.
2. **Grupları**seçin.
3. Tüm **gruplar** listesinden, değiştirmek istediğiniz grubu açın.
4. **Özellikleri**seçin.
5. Grubun **Özellikleri** sayfasında, istediğiniz üyelik türüne bağlı olarak Atanmış (statik), Dinamik Kullanıcı veya Dinamik **Aygıt'ın Üyelik türünü** seçin. Dinamik üyelik için, basit bir kural için seçenekleri seçmek veya bir üyelik kuralını kendiniz yazmak için kural oluşturucuyu kullanabilirsiniz. 

Aşağıdaki adımlar, bir grup kullanıcı için bir grubu statikten dinamik üyeliğe değiştirmeye örnektir.

1. Seçtiğiniz grubun **Özellikleri** sayfasında, Dinamik **Kullanıcı'nın Üyelik türünü** seçin ve ardından devam etmek üzere grup üyeliğindeki değişiklikleri açıklayan iletişim kutusunda Evet'i seçin. **Dynamic User** 
  
   ![dinamik kullanıcının üyelik türünü seçin](./media/groups-change-type/select-group-to-convert.png)
  
2. **Dinamik sorgu ekle'yi**seçin ve ardından kuralı sağlayın.
  
   ![dinamik grup için kural girin](./media/groups-change-type/enter-rule.png)
  
3. Kuralı oluşturduktan sonra sayfanın altındaki **sorgu ekle'yi** seçin.
4. Değişikliklerinizi kaydetmek için grubun **Özellikler** sayfasında **Kaydet'i** seçin. Grubun **Üyelik türü** hemen grup listesinde güncelleştirilir.

> [!TIP]
> Girdiğiniz üyelik kuralı yanlışsa grup dönüştürme başarısız olabilir. Portalın sağ üst köşesinde, kuralın sistem tarafından neden kabul edilemediğinin bir açıklamasını içeren bir bildirim görüntülenir. Kuralı geçerli kılmak için nasıl ayarlayabileceğinizi anlamak için dikkatle okuyun. Kural sözdizimi örnekleri ve üyelik kuralı için desteklenen özelliklerin, işleçlerin ve değerlerin tam listesi için Azure [Etkin Dizini'ndeki gruplar için Dinamik üyelik kurallarına](groups-dynamic-membership.md)bakın.

## <a name="change-membership-type-for-a-group-powershell"></a>Bir grup için üyelik türünü değiştirme (PowerShell)

> [!NOTE]
> Dinamik grup özelliklerini değiştirmek için [Azure AD PowerShell Sürüm 2'nin](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) **önizleme sürümünden** cmdlets kullanmanız gerekir. Önizlemeyi [PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzureADPreview)yükleyebilirsiniz.

Burada, varolan bir grupta üyelik yönetimini değiştiren işlevlere bir örnek verilmiştir. Bu örnekte, GroupTypes özelliğini doğru bir şekilde işlemek ve dinamik üyelikle ilgisi olmayan değerleri korumak için dikkatli olmak gerekir.

```powershell
#The moniker for dynamic groups as used in the GroupTypes property of a group object
$dynamicGroupTypeString = "DynamicMembership"

function ConvertDynamicGroupToStatic
{
    Param([string]$groupId)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -eq $null -or !$groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a static group. Aborting conversion.";
    }


    #remove the type for dynamic groups, but keep the other type values
    $groupTypes.Remove($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to remove the dynamic type, ii) pause execution of the current rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "Paused"
}

function ConvertStaticGroupToDynamic
{
    Param([string]$groupId, [string]$dynamicMembershipRule)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -ne $null -and $groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a dynamic group. Aborting conversion.";
    }
    #add the dynamic group type to existing types
    $groupTypes.Add($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to add the dynamic type, ii) start execution of the rule, iii) set the rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "On" -MembershipRule $dynamicMembershipRule
}
```
Bir grubu statik yapmak için:

```powershell
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```

Grubu dinamik hale getirmek için:

```powershell
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure Etkin Dizini'ndeki gruplar hakkında ek bilgiler sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
