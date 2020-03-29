---
title: Yönetim gruplarınızla nasıl çalışılabilen - Azure Yönetimi
description: Yönetim grubu hiyerarşinizi nasıl görüntüleyecek, koruyacağınızı, güncelleştirecek ve sileceklerinizi öğrenin.
ms.date: 12/18/2019
ms.topic: conceptual
ms.openlocfilehash: 94df67888c0ed0ea532844a92a362a181621d3d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78267931"
---
# <a name="manage-your-resources-with-management-groups"></a>Kaynaklarınızı yönetim gruplarıyla yönetin

Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure yönetim grupları, aboneliklerin üzerinde bir kapsam düzeyi sunar. Abonelikleri "yönetim grupları" adlı kapsayıcılarla düzenler ve idare koşullarınızı bu yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan koşulları devralır.

Yönetim grupları, sahip olabileceğiniz abonelik türüne bakılmaksızın kurumsal düzeyde yönetimi büyük ölçekte sunar.  Yönetim grupları hakkında daha fazla bilgi edinmek için, [azure yönetim gruplarıyla kaynaklarınızı](overview.md)düzenleyin'e bakın.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

>[!IMPORTANT]
>Azure Kaynak Yöneticisi kullanıcı belirteçleri ve yönetim grubu önbelleği, yenilemek zorunda kalmadan önce 30 dakika dayanır.  Bir yönetim grubunu veya aboneliği taşımak gibi herhangi bir eylem yaptıktan sonra, göstermek 30 dakika kadar sürebilir.  
>Güncelleştirmeleri daha erken görmek için tarayıcıyı yenileyerek, oturum unuzu ve dışarıda oturum unuzu yenileyerek veya yeni bir belirteç isteyerek belirtecinizi güncelleştirmeniz gerekir.  



## <a name="change-the-name-of-a-management-group"></a>Yönetim grubunun adını değiştirme

Portal, PowerShell veya Azure CLI'yi kullanarak yönetim grubunun adını değiştirebilirsiniz.

### <a name="change-the-name-in-the-portal"></a>Portaldaki adı değiştirme

1. [Azure portalına](https://portal.azure.com)giriş yapın.

1. **Tüm hizmet** > **Yönetimi gruplarını**seçin.

1. Yeniden adlandırmak istediğiniz yönetim grubunu seçin.

1. **Ayrıntıları**seçin.

1. Sayfanın üst kısmındaki **Yeniden Adlandır grubu** seçeneğini seçin.

   ![Yönetim grubu sayfasında Grup'u yeniden adlandır](./media/detail_action_small.png)

1. Menü açıldığında, görüntülenmek istediğiniz yeni adı girin.

   ![Yönetim grubunu yeniden adlandırmak için Grup bölmesini yeniden adlandırın](./media/rename_context.png)

1. **Kaydet'i**seçin.

### <a name="change-the-name-in-powershell"></a>PowerShell'de adı değiştirme

Görüntü adını güncelleştirmek için **Update-AzManagementGroup**kullanın. Örneğin, bir yönetim grubu görüntüleminin adını "Contoso BT"den "Contoso Group" olarak değiştirmek için aşağıdaki komutu çalıştırAbilirsiniz:

```azurepowershell-interactive
Update-AzManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Azure CLI'de adı değiştirme

Azure CLI için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Yönetim grubunu silme

Bir yönetim grubunu silmek için aşağıdaki gereksinimlerin karşılanması gerekir:

1. Yönetim grubu altında alt yönetim grubu veya abonelik yoktur.

   - Bir abonelik veya yönetim grubunu başka bir yönetim grubuna taşımak için [bkz.](#moving-management-groups-and-subscriptions)

1. Yönetim grubu ("Sahibi", "Katkıda Bulunan", veya "Yönetim Grubu Katılımcısı") hakkında yazma izinleri almanız gerekir. Hangi izinlere sahip olduğunuzu görmek için yönetim grubunu seçin ve ardından **IAM'yi**seçin. RBAC Rolleri hakkında daha fazla bilgi edinmek için [rbac ile erişimi ve izinleri yönet'e](../../role-based-access-control/overview.md)bakın.  

### <a name="delete-in-the-portal"></a>Portalda silme

1. [Azure portalına](https://portal.azure.com)giriş yapın.

1. **Tüm hizmet** > **Yönetimi gruplarını**seçin.

1. Silmek istediğiniz yönetim grubunu seçin.

1. **Ayrıntıları**seçin.

1. **Sil'i** Seçin

    > [!TIP]
    > Simge devre dışı bırakılırsa, fare seçicinizi simgenin üzerinde gezinmek size nedenini gösterir.

   ![Grup seçeneğini silme](./media/delete.png)

1. Yönetim grubunu silmek istediğinizi onaylayan bir pencere açılır.

   ![Grup onay pencereni silme](./media/delete_confirm.png)

1. **Evet'i**seçin.

### <a name="delete-in-powershell"></a>PowerShell içinde silme

Yönetim gruplarını silmek için PowerShell içindeki **Kaldır-AzManagementGroup** komutunu kullanın.

```azurepowershell-interactive
Remove-AzManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Azure CLI ile silme

Azure CLI ile az hesap yönetimi grubu silme komutunu kullanın.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Yönetim gruplarını görüntüleme

Doğrudan veya devralınan Bir RBAC rolüne sahip olduğunuz herhangi bir yönetim grubunu görüntüleyebilirsiniz.  

### <a name="view-in-the-portal"></a>Portalda görüntüle

1. [Azure portalına](https://portal.azure.com)giriş yapın.

1. **Tüm hizmet** > **Yönetimi gruplarını**seçin.

1. Yönetim grubu hiyerarşisi sayfası yüklenir. Bu sayfa, erişebildiğiniz tüm yönetim gruplarını ve abonelikleri keşfedebileceğiniz yerdir. Grup adını seçmek sizi hiyerarşideki bir düzeye götürür. Gezinti, dosya gezgini ile aynı şekilde çalışır.

1. Yönetim grubunun ayrıntılarını görmek için, yönetim grubunun başlığının yanındaki **(ayrıntılar)** bağlantısını seçin. Bu bağlantı kullanılamıyorsa, bu yönetim grubunu görüntüleme izniniz yoktur.

   ![Ana](./media/main.png)

### <a name="view-in-powershell"></a>PowerShell'de Görüntüle

Tüm grupları almak için Get-AzManagementGroup komutunu kullanırsınız.  Yönetim grubu GET PowerShell komutlarının tam listesi için [Az.Resources](/powershell/module/az.resources/Get-AzManagementGroup) modüllerine bakın.  

```azurepowershell-interactive
Get-AzManagementGroup
```

Tek bir yönetim grubunun bilgileri için -GroupName parametresini kullanın

```azurepowershell-interactive
Get-AzManagementGroup -GroupName 'Contoso'
```

Belirli bir yönetim grubunu ve altındaki hiyerarşinin tüm düzeylerini döndürmek için **-Genişlet** ve **-Yeniden Lanetleme** parametrelerini kullanın.  

```azurepowershell-interactive
PS C:\> $response = Get-AzManagementGroup -GroupName TestGroupParent -Expand -Recurse
PS C:\> $response

Id                : /providers/Microsoft.Management/managementGroups/TestGroupParent
Type              : /providers/Microsoft.Management/managementGroups
Name              : TestGroupParent
TenantId          : 00000000-0000-0000-0000-000000000000
DisplayName       : TestGroupParent
UpdatedTime       : 2/1/2018 11:15:46 AM
UpdatedBy         : 00000000-0000-0000-0000-000000000000
ParentId          : /providers/Microsoft.Management/managementGroups/00000000-0000-0000-0000-000000000000
ParentName        : 00000000-0000-0000-0000-000000000000
ParentDisplayName : 00000000-0000-0000-0000-000000000000
Children          : {TestGroup1DisplayName, TestGroup2DisplayName}

PS C:\> $response.Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestGroup1
Name        : TestGroup1
DisplayName : TestGroup1DisplayName
Children    : {TestRecurseChild}

PS C:\> $response.Children[0].Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestRecurseChild
Name        : TestRecurseChild
DisplayName : TestRecurseChild
Children    :
```

### <a name="view-in-azure-cli"></a>Azure CLI'de Görünüm

Tüm grupları almak için liste komutunu kullanırsınız.  

```azurecli-interactive
az account management-group list
```

Tek bir yönetim grubunun bilgileri için, göster komutunu kullanın

```azurecli-interactive
az account management-group show --name 'Contoso'
```

Belirli bir yönetim grubunu ve altındaki hiyerarşinin tüm düzeylerini döndürmek için **-Genişlet** ve **-Yeniden Lanetleme** parametrelerini kullanın.

```azurecli-interactive
az account management-group show --name 'Contoso' -e -r
```

## <a name="moving-management-groups-and-subscriptions"></a>Yönetim gruplarını ve abonelikleri taşıma   

Bir yönetim grubu oluşturmanın bir nedeni, abonelikleri bir araya getirmektir. Yalnızca yönetim grupları ve abonelikler başka bir yönetim grubunun çocukları yapılabilir. Yönetim grubuna taşınan bir abonelik, tüm kullanıcı erişimini ve ilkeleri üst yönetim grubundan devralır

Bir yönetim grubunu veya aboneliği başka bir yönetim grubunun çocuğu olarak taşınırken üç kuralın doğru olarak değerlendirilmesi gerekir.

Taşıma eylemini yapıyorsanız, şunları yapmanız gerekir: 

-  Yönetim grubu yazma ve Rol Atama çocuk abonelik veya yönetim grubu üzerinde izinleri yazmak.
    - Yerleşik rol örneği **Sahibi**
- Yönetim grubu hedef üst yönetim grubuna erişim yazar.
    - Yerleşik rol örneği: **Sahibi**, **Katılımcısı**, **Yönetim Grubu Katılımcısı**
- Yönetim grubu, varolan üst yönetim grubuna erişim yazar.
    - Yerleşik rol örneği: **Sahibi**, **Katılımcısı**, **Yönetim Grubu Katılımcısı**

**Özel Durum**: Hedef veya varolan üst yönetim grubu Root yönetim grubuysa, izin gereksinimleri geçerli değildir. Root yönetim grubu tüm yeni yönetim grupları ve abonelikler için varsayılan iniş noktası olduğundan, bir öğeyi taşımak için izinlere ihtiyacınız yoktur.

Abonelikteki Sahip rolü geçerli yönetim grubundan devralılırsa, hareket hedefleriniz sınırlıdır. Aboneliği yalnızca Sahip rolüne sahip olduğunuz başka bir yönetim grubuna taşıyabilirsiniz. Aboneliğin sahipliğini kaybedeceğiniz için katılımcı olduğunuz bir yönetim grubuna taşıyamazsınız. Abonelik için Doğrudan Sahip rolüne atandıysanız (yönetim grubundan devralınmadı), bu görevi katkıda bulunan olduğunuz herhangi bir yönetim grubuna taşıyabilirsiniz. 

Azure portalında hangi izinlere sahip olduğunuzu görmek için yönetim grubunu seçin ve ardından **IAM'yi**seçin. RBAC Rolleri hakkında daha fazla bilgi edinmek için [rbac ile erişimi ve izinleri yönet'e](../../role-based-access-control/overview.md)bakın.


## <a name="move-subscriptions"></a>Abonelikleri taşıma 

#### <a name="add-an-existing-subscription-to-a-management-group-in-the-portal"></a>Portaldaki bir yönetim grubuna varolan bir Abonelik ekleme

1. [Azure portalına](https://portal.azure.com)giriş yapın.

1. **Tüm hizmet** > **Yönetimi gruplarını**seçin.

1. Ebeveyn olmayı planladığınız yönetim grubunu seçin.

1. Sayfanın üst kısmında **abonelik ekle'yi**seçin.

1. Listede doğru kimlikle aboneliği seçin.

   ![Yönetim grubuna eklemek için kullanılabilir abonelikler](./media/add_context_sub.png)

1. "Kaydet"i seçin.

#### <a name="remove-a-subscription-from-a-management-group-in-the-portal"></a>Portaldaki bir yönetim grubundan abonelik kaldırma

1. [Azure portalına](https://portal.azure.com)giriş yapın.

1. **Tüm hizmet** > **Yönetimi gruplarını**seçin.

1. Planlamanız gereken yönetim grubunu seçin.  

1. Taşımak istediğiniz listede abonelik için satırın sonundaki elipsi seçin.

   ![Yönetim grubunda seçeneği taşıma](./media/move_small.png)

1. **Taşı'yı**seçin.

1. Açılan menüde **Üst yönetim grubunu**seçin.

   ![Üst grubu değiştirmek için bölmeyi taşıma](./media/move_small_context.png)

1. **Kaydet'i**seçin.

### <a name="move-subscriptions-in-powershell"></a>PowerShell'deki abonelikleri taşıma

PowerShell'de aboneliği taşımak için Yeni YönetimGroupSubscription komutunu kullanırsınız.  

```azurepowershell-interactive
New-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Abonelik ve yönetim grubu arasındaki bağlantıyı kaldırmak için Remove-AzManagementGroupSubscription komutunu kullanın.

```azurepowershell-interactive
Remove-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Azure CLI'de abonelikleri taşıma

CLI'de bir aboneliği taşımak için ekle komutunu kullanırsınız.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Aboneliği yönetim grubundan kaldırmak için abonelik kaldırma komutunu kullanın.  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups"></a>Yönetim gruplarını taşıma 

### <a name="move-management-groups-in-the-portal"></a>Portaldaki yönetim gruplarını taşıma

1. [Azure portalına](https://portal.azure.com)giriş yapın.

1. **Tüm hizmet** > **Yönetimi gruplarını**seçin.

1. Ebeveyn olmayı planladığınız yönetim grubunu seçin.

1. Sayfanın üst kısmında **yönetim grubu ekle'yi**seçin.

1. Açılan menüde, yeni bir yönetim grubu isteyip istemediğiniz veya varolan bir yönetim grubu kullanıp kullanmadığınızı seçin.

   - Yeni seçiseçmek yeni bir yönetim grubu oluşturur.
   - Varolan bir seçim, bu yönetim grubuna taşıyabileceğiniz tüm yönetim gruplarının bir açılır düşüşünü sunar.  

   ![Yönetim grubunu yeni veya varolan gruba taşıma](./media/add_context_MG.png)

1. **Kaydet'i**seçin.

### <a name="move-management-groups-in-powershell"></a>PowerShell'deki yönetim gruplarını taşıyın

Bir yönetim grubunu farklı bir gruba taşımak için PowerShell'deki Update-AzManagementGroup komutunu kullanın.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName ContosoIT
Update-AzManagementGroup -GroupName 'Contoso' -ParentId $parentGroup.id
```  

### <a name="move-management-groups-in-azure-cli"></a>Azure CLI'de yönetim gruplarını taşıma

Azure CLI ile bir yönetim grubunu taşımak için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent ContosoIT
```

## <a name="audit-management-groups-using-activity-logs"></a>Etkinlik günlüklerini kullanarak yönetim gruplarını denetleme

Yönetim grupları [Azure Etkinlik Günlüğü](../../azure-monitor/platform/platform-logs-overview.md)'nde desteklenir. Bir yönetim grubunun başına gelen tüm olayları diğer Azure kaynaklarıyla aynı merkezi konumda sorgulayabilirsiniz.  Örneğin, belirli bir yönetim grubunda yapılan tüm Rol Atamalarını veya İlke Ataması değişikliklerini görebilirsiniz.

![Yönetim gruplarıyla Etkinlik Günlükleri](media/al-mg.png)

Azure portalının dışında Yönetim Gruplarını sorgulamak istediğinizde, yönetim gruplarının hedef kapsamı **"/providers/Microsoft.Management/managementGroups/{yourMgID}"** gibi görünür.

## <a name="referencing-management-groups-from-other-resource-providers"></a>Diğer Kaynak Sağlayıcılardan yönetim gruplarına başvurma

Diğer Kaynak Sağlayıcı'nın eylemlerinden yönetim gruplarına başvururken, kapsam olarak aşağıdaki yolu kullanın. Bu yol PowerShell, Azure CLI ve REST API'leri kullanılırken kullanılır.  

>"/providers/Microsoft.Management/managementGroups/{yourMgID}"

PowerShell'deki bir yönetim grubuna yeni bir rol ataması atamak, bu yolu kullanmaya örnektir

```azurepowershell-interactive
New-AzRoleAssignment -Scope "/providers/Microsoft.Management/managementGroups/Contoso"
```

Bir yönetim grubunda bir ilke tanımı alırken aynı kapsam yolu kullanılır.

```http
GET https://management.azure.com/providers/Microsoft.Management/managementgroups/MyManagementGroup/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming?api-version=2018-05-01
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](manage.md)
- [Azure PowerShell Kaynak Modülünde yönetim gruplarını gözden geçirme](/powershell/module/az.resources#resources)
- [REST API'de yönetim gruplarını gözden geçirme](/rest/api/resources/managementgroups)
- [Azure CLI'de yönetim gruplarını gözden geçirme](/cli/azure/account/management-group)