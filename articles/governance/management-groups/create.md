---
title: Kaynakları düzenlemek için yönetim grupları oluşturma - Azure Yönetimi
description: Portalı, Azure PowerShell'i ve Azure CLI'yi kullanarak birden çok kaynağı yönetmek için Azure yönetim gruplarını nasıl oluşturup oluşturabilirsiniz öğrenin.
ms.date: 12/18/2019
ms.topic: conceptual
ms.openlocfilehash: a06679432d795b5b7854af8dc66b468841978e9c
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273197"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Kaynak organizasyonu ve yönetimi için yönetim grupları oluşturma

Yönetim grupları, birden çok abonelik te erişimi, ilkeyi ve uyumluluğu yönetmenize yardımcı olan kapsayıcılardır. [Azure İlkesi](../policy/overview.md) ve [Azure Role Tabanlı Erişim Denetimleri](../../role-based-access-control/overview.md)ile kullanılabilecek etkili ve verimli bir hiyerarşi oluşturmak için bu kapsayıcıları oluşturun. Yönetim grupları hakkında daha fazla bilgi için [bkz.](overview.md)

Dizinde oluşturulan ilk yönetim grubunun tamamlanması 15 dakika kadar sürebilir. Dizininiz için Azure'da yönetim grupları hizmetini ilk kez ayarlayan işlemler vardır. İşlem tamamlandığında bir bildirim alırsınız. Daha fazla bilgi için [yönetim gruplarının ilk kurulumuna](./overview.md#initial-setup-of-management-groups)bakın. 

## <a name="create-a-management-group"></a>Yönetim grubu oluşturma

Kiracıdaki herhangi bir Azure REKLAM kullanıcısı, yönetim grubu yazma izni olmadan bir yönetim grubu oluşturabilir.  Bu yeni yönetim grubu Kök Yönetim Grubu'nun bir çocuğu olacak ve yaratıcısına bir "Sahip" rol ataması verilecektir. Yönetim grubu hizmeti, rol atamalarının kök düzeyinde gerekli olmaması için bu yeteneğe izin verir. Oluşturulduğunda hiçbir kullanıcının Kök Yönetim Grubu'na erişimi yoktur.  Azure AD Global Yöneticilerinin yönetim gruplarını kullanmaya başlamasını engellememek için, ilk yönetim gruplarının kök düzeyinde oluşturulmasına izin veririz.      

Portal, [Kaynak Yöneticisi şablonu,](../../azure-resource-manager/templates/deploy-to-tenant.md#create-management-group)PowerShell veya Azure CLI'yi kullanarak yönetim grubu oluşturabilirsiniz.

### <a name="create-in-portal"></a>Portalda oluştur

1. [Azure portalına](https://portal.azure.com)giriş yapın.

1. **Tüm hizmet** > **yönetimi + yönetişim**seçin.

1. **Maliyet Yönetimi + Faturalama'yı** seçin

1. Maliyet Yönetimi + Faturalama - Yönetim grupları sayfasında **Yönetim Grupları'nı** seçin

1. + **Yönetim grubu ekle' yi**seçin.

   ![Yönetim gruplarıyla çalışmak için sayfa](./media/main.png)

1. Yönetim grubu kimliği alanını doldurun.

   - **Yönetim Grubu Kimliği,** bu yönetim grubunda komut göndermek için kullanılan dizin benzersiz tanımlayıcısındır. Bu tanımlayıcı, bu grubu tanımlamak için Azure sisteminde kullanıldığından, oluşturulduktan sonra düzenlenemez. [Kök yönetim grubu,](overview.md#root-management-group-for-each-directory) Azure Etkin Dizin Kimliği olan bir kimlikle otomatik olarak oluşturulur. Diğer tüm yönetim grupları için benzersiz bir kimlik atayın.
   - Görüntü adı alanı, Azure portalında görüntülenen addır. Yönetim grubu oluşturulurken ayrı bir görüntü adı isteğe bağlı bir alandır ve herhangi bir zamanda değiştirilebilir.  

   ![Yeni bir yönetim grubu oluşturmak için seçenekler bölmesi](./media/create_context_menu.png)  

1. **Kaydet'i**seçin.

### <a name="create-in-powershell"></a>PowerShell'de Oluştur

PowerShell için yeni bir yönetim grubu oluşturmak için [New-AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) cmdlet'i kullanın.

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** oluşturulmakta olan benzersiz bir tanımlayıcıdır. Bu kimlik, bu gruba başvurmak için diğer komutlar tarafından kullanılır ve daha sonra değiştirilemez.

Yönetim grubunun Azure portalında farklı bir ad göstermesini **istiyorsanız, DisplayName** parametresini ekleyin. Örneğin, Contoso GroupName ve "Contoso Group" ekran adı içeren bir yönetim grubu oluşturmak için aşağıdaki cmdlet'i kullanın:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

Önceki örneklerde, yeni yönetim grubu kök yönetim grubu altında oluşturulur. Üst öğe olarak farklı bir yönetim grubu belirtmek için **ParentId** parametresini kullanın.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>Azure CLI'de oluşturma

Azure CLI için, yeni bir yönetim grubu oluşturmak için [az hesap yönetim grubu oluşturma](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create) komutunu kullanın.

```azurecli-interactive
az account management-group create --name Contoso
```

**Ad,** oluşturulmakta olan benzersiz bir tanımlayıcıdır. Bu kimlik, bu gruba başvurmak için diğer komutlar tarafından kullanılır ve daha sonra değiştirilemez.

Yönetim grubunun Azure portalında farklı bir ad göstermesini istiyorsanız, **görüntü adı** parametresini ekleyin. Örneğin, Contoso GroupName ve "Contoso Group" ekran adı içeren bir yönetim grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

Önceki örneklerde, yeni yönetim grubu kök yönetim grubu altında oluşturulur. Üst öğe olarak farklı bir yönetim grubu belirtmek için **üst** parametreyi kullanın ve üst grubun adını sağlayın.

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](manage.md)
- [Azure PowerShell Kaynak Modülünde yönetim gruplarını gözden geçirme](/powershell/module/az.resources#resources)
- [REST API'de yönetim gruplarını gözden geçirme](/rest/api/resources/managementgroups)
- [Azure CLI'de yönetim gruplarını gözden geçirme](/cli/azure/account/management-group)
