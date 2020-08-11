---
title: Kaynakları düzenlemek için yönetim grupları oluşturma-Azure Idare
description: Portal, Azure PowerShell ve Azure CLı kullanarak birden çok kaynağı yönetmek için Azure Yönetim grupları oluşturmayı öğrenin.
ms.date: 08/10/2020
ms.topic: conceptual
ms.openlocfilehash: 0739eafdf4cf41195b189a8496c331ab470bbec0
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056644"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Kaynak organizasyonu ve yönetimi için yönetim grupları oluşturma

Yönetim grupları, birden çok abonelik üzerinde erişimi, ilkeyi ve uyumluluğu yönetmenize yardımcı olan kapsayıcılardır. [Azure ilkesi](../policy/overview.md) ve [Azure rol tabanlı erişim denetimleriyle](../../role-based-access-control/overview.md)kullanılabilecek etkili ve verimli bir hiyerarşi oluşturmak için bu kapsayıcıları oluşturun. Yönetim grupları hakkında daha fazla bilgi için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](overview.md).

Dizinde oluşturulan ilk yönetim grubunun tamamlanması 15 dakika sürebilir. Dizininiz için Azure 'da yönetim grupları hizmetini ayarlamak için ilk kez çalışan süreçler vardır. İşlem tamamlandığında bir bildirim alırsınız. Daha fazla bilgi için bkz. [yönetim gruplarının ilk kurulumu](./overview.md#initial-setup-of-management-groups).

## <a name="create-a-management-group"></a>Yönetim grubu oluşturma

Kiracıdaki herhangi bir Azure AD kullanıcısı, bu kullanıcıya atanan yönetim grubu yazma izni olmadan bir yönetim grubu oluşturabilir. Bu yeni yönetim grubu kök yönetim grubunun bir alt öğesi olacak ve oluşturucuya bir "Owner" rol ataması verilecek. Yönetim grubu hizmeti, bu becerisine, rol atamalarının kök düzeyinde gerekmemesi için izin verir. Kök yönetim grubuna, bir kullanıcı oluşturulduğunda erişemez. Yönetim grupları 'nı kullanmaya başlamak üzere Azure AD Genel yöneticilerini bulmanın önüne geçmek için, kökte ilk yönetim gruplarının oluşturulmasına izin veriyoruz  
düzeyde.

Portal, [Azure Resource Manager şablonu](../../azure-resource-manager/templates/deploy-to-tenant.md#create-management-group), PowerShell veya Azure CLI kullanarak yönetim grubu oluşturabilirsiniz.

### <a name="create-in-portal"></a>Portalda oluştur

1. [Azure Portal](https://portal.azure.com)oturum açın.

1. **Tüm hizmetler**  >  **yönetimi + idare**' ı seçin.

1. **Yönetim grupları**seçin.

1. **+ Yönetim grubu Ekle**' yi seçin.

   :::image type="content" source="./media/main.png" alt-text="Yönetim gruplarıyla çalışma sayfası" border="false":::

1. Yönetim grubu KIMLIĞI alanını girin.

   - **Yönetim grubu kimliği** , bu yönetim grubundaki komutları göndermek için kullanılan dizin benzersiz tanımlayıcısıdır. Bu tanımlayıcı, bu grubu tanımlamak için Azure sistem genelinde kullanıldığından oluşturulduktan sonra düzenlenebilir değildir. [Kök yönetim grubu](overview.md#root-management-group-for-each-directory) , Azure Active Directory kimliği olan bir kimlikle otomatik olarak oluşturulur. Diğer tüm yönetim grupları için benzersiz bir KIMLIK atayın.
   - Görünen ad alanı Azure portal içinde görüntülenen addır. Ayrı bir görünen ad, yönetim grubu oluşturulurken isteğe bağlı bir alandır ve herhangi bir  
     ışınızda.

   :::image type="content" source="./media/create_context_menu.png" alt-text="Yeni yönetim grubu oluşturmak için seçenekler bölmesi" border="false":::

1. **Kaydet**’i seçin.

### <a name="create-in-powershell"></a>PowerShell 'de oluştur

PowerShell için, New [-AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) cmdlet 'ini kullanarak yeni bir yönetim grubu oluşturun.

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** , oluşturulmakta olan benzersiz bir tanımlayıcıdır. Bu KIMLIK, bu gruba başvurmak için diğer komutlar tarafından kullanılır ve daha sonra değiştirilemez.

Yönetim grubunun Azure portal içinde farklı bir ad göstermesini istiyorsanız **DisplayName** parametresini ekleyin. Örneğin, contoso GroupName ve "contoso Group" görünen adı ile bir yönetim grubu oluşturmak için aşağıdaki cmdlet 'i kullanın:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

Yukarıdaki örneklerde, yeni yönetim grubu kök yönetim grubu altında oluşturulur. Üst öğe olarak farklı bir yönetim grubu belirtmek için **parentID** parametresini kullanın.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>Azure CLı 'de oluşturma

Azure CLı için, yeni bir yönetim grubu oluşturmak için [az Account Management-Group Create](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create) komutunu kullanın.

```azurecli-interactive
az account management-group create --name Contoso
```

**Ad** , oluşturulmakta olan benzersiz bir tanımlayıcıdır. Bu KIMLIK, bu gruba başvurmak için diğer komutlar tarafından kullanılır ve daha sonra değiştirilemez.

Yönetim grubunun Azure portal içinde farklı bir ad göstermesini istiyorsanız, **görünen ad** parametresini ekleyin. Örneğin, contoso GroupName ve "contoso Group" görünen adı ile bir yönetim grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

Yukarıdaki örneklerde, yeni yönetim grubu kök yönetim grubu altında oluşturulur. Üst öğe olarak farklı bir yönetim grubu belirtmek için **üst** parametreyi kullanın ve üst grubun adını sağlayın.

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](./create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](./manage.md)
- [Azure PowerShell Kaynak Modülünde yönetim gruplarını gözden geçirme](/powershell/module/az.resources#resources)
- [REST API'de yönetim gruplarını gözden geçirme](/rest/api/resources/managementgroups)
- [Azure CLI'de yönetim gruplarını gözden geçirme](/cli/azure/account/management-group)
