---
title: DNS Bölgeleri ve kayıtları koruma-Azure DNS
description: Bu öğrenme yolunda, DNS bölgelerini ve kayıt kümelerini Microsoft Azure DNS ' de korumaya başlayın.
services: dns
author: asudbring
ms.service: dns
ms.topic: article
ms.date: 12/4/2018
ms.author: allensu
ms.openlocfilehash: b84ba055dd8214ae18e76004671e3922e6f3b878
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74211455"
---
# <a name="how-to-protect-dns-zones-and-records"></a>DNS bölgelerini ve kayıtlarını koruma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

DNS bölgeleri ve kayıtları kritik kaynaklardır. Bir DNS bölgesinin silinmesi, hatta yalnızca tek bir DNS kaydının toplam hizmet kesintisi oluşmasına neden olabilir.  Bu nedenle, kritik DNS bölgelerinin ve kayıtlarının yetkisiz veya yanlışlıkla yapılan değişikliklere karşı korunması önemlidir.

Bu makalede Azure DNS, DNS bölgelerinizi ve kayıtlarınızı bu değişikliklere karşı korumanıza nasıl olanak sağladığını açıklamaktadır.  Azure Resource Manager tarafından sunulan iki güçlü güvenlik özelliği uyguladık: [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) ve [kaynak kilitleri](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure rol tabanlı Access Control (RBAC), Azure kullanıcıları, grupları ve kaynakları için ayrıntılı erişim yönetimine izin verebilir. RBAC kullanarak, kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarına tam olarak izin verebilirsiniz. RBAC 'nin erişimi yönetmenize nasıl yardımcı olduğu hakkında daha fazla bilgi için bkz. [rol tabanlı Access Control nedir?](../role-based-access-control/overview.md).

### <a name="the-dns-zone-contributor-role"></a>DNS bölgesi katılımcısı rolü

DNS bölgesi katılımcısı rolü, Azure tarafından DNS kaynaklarını yönetmek için sunulan yerleşik bir roldür.  Bir kullanıcıya veya gruba DNS bölgesi katkıda bulunan izinleri atamak, bu grubun DNS kaynaklarını yönetmesini, ancak başka bir türdeki kaynakları almasını sağlar.

Örneğin, *myzones* kaynak grubunun contoso Corporation için beş bölge içerdiğini varsayalım. DNS Yöneticisi DNS bölgesine katkıda bulunan izinleri bu kaynak grubuna vermek, bu DNS bölgeleri üzerinde tam denetim sağlar. Ayrıca, DNS Yöneticisi sanal makineleri oluşturamaz veya durduramaz.

RBAC izinlerini atamak için en kolay yol [Azure Portal](../role-based-access-control/role-assignments-portal.md)kullanmaktır.  Kaynak grubu için **erişim denetimi (IAM)** açın, ardından **Ekle**' yi seçin, sonra **DNS bölgesi katılımcısı** rolünü seçin ve izin vermek için gerekli kullanıcıları veya grupları seçin.

![Azure portal aracılığıyla kaynak grubu düzeyi RBAC](./media/dns-protect-zones-recordsets/rbac1.png)

İzinler, [Azure PowerShell kullanılarak da verilebilir](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

Eşdeğer komut [Azure CLI aracılığıyla da kullanılabilir](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Bölge düzeyi RBAC

Azure RBAC kuralları bir aboneliğe, bir kaynak grubuna veya tek bir kaynağa uygulanabilir. Azure DNS durumda, bu kaynak tek bir DNS bölgesi veya hatta tek bir kayıt kümesi olabilir.

Örneğin, *myzones* kaynak grubunun bölge *contoso.com* ve her müşteri hesabı için CNAME kayıtlarının oluşturulduğu bir alt bölge *Customers.contoso.com* içerdiğini varsayalım.  Bu CNAME kayıtlarını yönetmek için kullanılan hesaba, yalnızca *Customers.contoso.com* bölgesinde kayıt oluşturmak için izinler atanmalıdır, diğer bölgelere erişimi olmamalıdır.

Bölge düzeyinde RBAC izinleri Azure portal aracılığıyla verilebilir.  Bölge için **erişim denetimi (IAM)** açın, ardından **Ekle**' yi seçin, sonra **DNS bölgesi katılımcısı** rolünü seçin ve izin vermek için gerekli kullanıcıları veya grupları seçin.

![Azure portal ile DNS bölge düzeyi RBAC](./media/dns-protect-zones-recordsets/rbac2.png)

İzinler, [Azure PowerShell kullanılarak da verilebilir](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

Eşdeğer komut [Azure CLI aracılığıyla da kullanılabilir](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Kayıt kümesi düzeyi RBAC

Bir adım daha ilerleyebiliriz. Contoso.com bölgesinin tepesinde MX ve TXT kayıtlarına erişmesi gereken contoso Corporation için posta yöneticisini göz önünde bulundurun.  Diğer herhangi bir MX veya TXT kaydına veya başka bir türdeki herhangi bir kayda erişim gereksinimi yoktur.  Azure DNS, kayıt kümesi düzeyindeki izinleri, posta yöneticisinin erişmesi gereken kayıtları tam olarak atamanıza izin verir.  Posta yöneticisine tam olarak gereken denetim verilir ve başka herhangi bir değişiklik yapamaz.

Kayıt kümesi düzeyi RBAC izinleri, kayıt kümesi sayfasındaki **Kullanıcılar** düğmesi kullanılarak Azure Portal aracılığıyla yapılandırılabilir:

![Azure portal aracılığıyla küme düzeyi RBAC 'yi Kaydet](./media/dns-protect-zones-recordsets/rbac3.png)

Kayıt kümesi düzeyi RBAC izinleri, [Azure PowerShell kullanılarak da verilebilir](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant permissions to a specific record set
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

Eşdeğer komut [Azure CLI aracılığıyla da kullanılabilir](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Özel roller

Yerleşik DNS bölgesi katılımcısı rolü bir DNS kaynağı üzerinde tam denetim olanağı sunar. Ayrıca, daha ayrıntılı denetim sağlamak için kendi müşteri Azure rollerinizi oluşturmak da mümkündür.

Her contoso Corporation müşteri hesabı için *Customers.contoso.com* BÖLGESINDE bir CNAME kaydının oluşturulduğu örneği yeniden deneyin.  Bu CNAMEs 'leri yönetmek için kullanılan hesaba yalnızca CNAME kayıtlarını yönetmek için izin verilmelidir.  Daha sonra diğer türlerin (MX kayıtlarını değiştirme gibi) kayıtlarını değiştiremez veya bölge silme gibi bölge düzeyindeki işlemleri gerçekleştirebilir.

Aşağıdaki örnekte yalnızca CNAME kayıtlarını yönetmek için özel bir rol tanımı gösterilmektedir:

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/ c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

Actions özelliği, aşağıdaki DNS 'e özgü izinleri tanımlar:

* `Microsoft.Network/dnsZones/CNAME/*` CNAME kayıtları üzerinde tam denetim verir
* `Microsoft.Network/dnsZones/read`, DNS bölgelerini okuma izni verir, ancak bunları değiştirmemelidir, CNAME 'in oluşturulduğu bölgeyi görmenizi sağlar.

Kalan eylemler, [DNS bölgesi katılımcısı yerleşik rolünden](../role-based-access-control/built-in-roles.md#dns-zone-contributor)kopyalanır.

> [!NOTE]
> Kayıt kümelerinin güncelleştirilmesini engellemek için özel bir RBAC rolü kullanmak, etkili bir denetim değildir. Kayıt kümelerinin silinmesini engeller, ancak değiştirilmesini engellemez.  İzin verilen değişiklikler, kayıt kümesinden kayıtları ekleme ve kaldırma, boş bir kayıt kümesi bırakmak için tüm kayıtları kaldırma dahil. Bu, kayıt kümesini bir DNS çözünürlüğü bakış açısından silme ile aynı etkiye sahiptir.

Özel rol tanımları Şu anda Azure portal aracılığıyla tanımlanamaz. Bu rol tanımına dayalı özel bir rol, Azure PowerShell kullanılarak oluşturulabilir:

```azurepowershell
# Create new role definition based on input file
New-AzRoleDefinition -InputFile <file path>
```

Azure CLı aracılığıyla da oluşturulabilir:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

Daha sonra bu makalede açıklandığı gibi, rol yerleşik rollerle aynı şekilde atanabilir.

Özel roller oluşturma, yönetme ve atama hakkında daha fazla bilgi için bkz. [Azure RBAC 'de özel roller](../role-based-access-control/custom-roles.md).

## <a name="resource-locks"></a>Kaynak kilitleri

RBAC 'in yanı sıra, kaynakları kilitleme özelliği olan Azure Resource Manager başka türde güvenlik denetimini destekler. RBAC kuralları belirli kullanıcı ve grupların eylemlerini denetlemenize izin verince, kaynak kilitleri kaynağa uygulanır ve tüm kullanıcılar ve roller arasında etkilidir. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

İki tür kaynak kilidi vardır: **Cannotdelete** ve **ReadOnly**. Bunlar bir DNS bölgesine ya da tek bir kayıt kümesine uygulanabilir.  Aşağıdaki bölümlerde bazı yaygın senaryolar ve kaynak kilitleri kullanılarak bunların nasıl destekleyeceği açıklanır.

### <a name="protecting-against-all-changes"></a>Tüm değişikliklere karşı koruma

Yapılan değişikliklerin oluşmasını engellemek için, bölgeye salt okunur bir kilit uygulayın.  Bu, yeni kayıt kümelerinin oluşturulmasını ve varolan kayıt kümelerinin değiştirilmesini veya silinmesini engeller.

Bölge düzeyi kaynak kilitleri Azure portal aracılığıyla oluşturulabilir.  DNS bölgesi sayfasında, **kilitler**' ı seçin ve **+ Ekle**' yi seçin:

![Azure portal aracılığıyla bölge düzeyi kaynak kilitleri](./media/dns-protect-zones-recordsets/locks1.png)

Bölge düzeyi kaynak kilitleri Azure PowerShell aracılığıyla da oluşturulabilir:

```azurepowershell
# Lock a DNS zone
New-AzResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Azure kaynak kilitlerini yapılandırma Şu anda Azure CLı aracılığıyla desteklenmemektedir.

### <a name="protecting-individual-records"></a>Bireysel kayıtları koruma

Varolan bir DNS kaydının değişikliğe karşı yapılmasını engellemek için kayıt kümesine salt okunur bir kilit uygulayın.

> [!NOTE]
> Kayıt kümesine CanNotDelete kilidi uygulamak, etkili bir denetim değildir. Kayıt kümesinin silinmesini engeller, ancak değiştirilmesini engellemez.  İzin verilen değişiklikler, kayıt kümesinden kayıtları ekleme ve kaldırma, boş bir kayıt kümesi bırakmak için tüm kayıtları kaldırma dahil. Bu, kayıt kümesini bir DNS çözünürlüğü bakış açısından silme ile aynı etkiye sahiptir.

Kayıt kümesi düzeyi kaynak kilitleri, şu anda yalnızca Azure PowerShell kullanılarak yapılandırılabilir.  Azure portal veya Azure CLı 'de desteklenmez.

```azurepowershell
# Lock a DNS record set
New-AzResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Bölge silmeye karşı koruma

Azure DNS bir bölge silindiğinde bölgedeki tüm kayıt kümeleri de silinir.  Bu işlem geri alınamaz.  Yanlışlıkla kritik bir bölgeyi silmenin önemli bir iş etkisi olması olası olur.  Bu nedenle, yanlışlıkla bölge silmeye karşı korunması çok önemlidir.

Bir bölgeye CanNotDelete kilidi uygulamak bölgenin silinmesini engeller.  Ancak, kilitler alt kaynaklar tarafından devralındığından, bölgedeki herhangi bir kayıt kümesinin silinmesini önler, bu da istenmeyen bir işlem olabilir.  Ayrıca, yukarıdaki notta açıklandığı gibi, kayıtlar mevcut kayıt kümelerinden hala kaldırılabileceği için de bu da verimsiz değildir.

Alternatif olarak, bölgenin SOA kayıt kümesi gibi bir kayıt kümesine bir CanNotDelete kilidi uygulamayı düşünün.  Bölge, kayıt kümelerini de silmeden silinemediği için bu, bölge silmeye karşı koruma sağlarken bölge içindeki kayıt kümelerinin serbestçe değiştirilmesine izin vermeye devam eder. Bölgeyi silmeye yönelik bir girişim yapılırsa Azure Resource Manager, bu da SOA kayıt kümesini siler ve SOA kilitli olduğu için çağrıyı engeller.  Hiçbir kayıt kümesi silinmez.

Aşağıdaki PowerShell komutu, belirtilen bölgenin SOA kaydında bir CanNotDelete kilidi oluşturur:

```azurepowershell
# Protect against zone delete with CanNotDelete lock on the record set
New-AzResourceLock -LockLevel CanNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Yanlışlıkla bölge silmeyi önlemenin bir diğer yolu da, bölgelerinizi yönetmek için kullanılan operatör ve hizmet hesaplarının bölge silme izinlerine sahip olmamasını sağlamak için özel bir rol kullanmaktır. Bir bölgeyi silmeniz gerektiğinde, bir iki adımlı silme işlemini uygulayabilir, önce bölge silme izinleri (bölge kapsamında, yanlış bölgeyi silmeyi önler) ve bölgeyi silmek için ikinci adımı zorunlu kılabilirsiniz.

Bu ikinci yaklaşım, herhangi bir kilit oluşturmayı anımsamak zorunda kalmadan, bu hesapların eriştiği tüm bölgeler için çalışma avantajına sahiptir. Abonelik sahibi gibi, bölge silme izinlerine sahip tüm hesapların yanlışlıkla hala kritik bir bölgeyi silmesine karşı olumsuz bir öneme sahiptir.

Aynı anda, DNS bölgesi korumasına yönelik derinlemesine savunma yaklaşımı olarak hem yaklaşımların kaynak kilitleri hem de özel roller kullanmak mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

* RBAC ile çalışma hakkında daha fazla bilgi için, [Azure Portal erişim yönetimiyle çalışmaya başlama](../role-based-access-control/overview.md)bölümüne bakın.
* Kaynak kilitleri ile çalışma hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).
