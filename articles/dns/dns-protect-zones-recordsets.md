---
title: DNS Bölgelerini ve Kayıtlarını Koruma - Azure DNS
description: Bu öğrenme yolunda, Microsoft Azure DNS'de DNS bölgelerini ve kayıt kümelerini korumaya başlayın.
services: dns
author: asudbring
ms.service: dns
ms.topic: article
ms.date: 2/20/2020
ms.author: allensu
ms.openlocfilehash: 89a945f146601084795b2e12a721a03a1b96aaea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371486"
---
# <a name="how-to-protect-dns-zones-and-records"></a>DNS bölgelerini ve kayıtlarını koruma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

DNS bölgeleri ve kayıtları kritik kaynaklardır. Bir DNS bölgesinin veya tek bir DNS kaydının silinmesi bir hizmet kesintisine neden olabilir. DNS bölgelerinin ve kayıtlarının yetkisiz veya yanlışlıkla yapılan değişikliklere karşı korunması önemlidir.

Bu makalede, Azure DNS'nin özel DNS bölgelerinizi ve kayıtlarınızı bu tür değişikliklere karşı korumanızı nasıl sağladığı açıklanmaktadır.  Azure Kaynak Yöneticisi tarafından sağlanan iki güçlü güvenlik özelliğini uyguluyoruz: [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) ve kaynak [kilitleri.](../azure-resource-manager/management/lock-resources.md)

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure Role Tabanlı Erişim Denetimi (RBAC), Azure kullanıcıları, grupları ve kaynakları için ince taneli erişim yönetimi sağlar. RBAC ile, kullanıcıların ihtiyaç duyduğu erişim düzeyini verebilirsiniz. RBAC'ın erişimi yönetmenize nasıl yardımcı olduğu hakkında daha fazla bilgi için [Rol Tabanlı Erişim Denetimi nedir'e](../role-based-access-control/overview.md)bakın.

### <a name="the-dns-zone-contributor-role"></a>DNS Bölgesi Katılımcısıfatı Rolü

DNS Bölgesi Katılımcısı rolü, özel DNS kaynaklarını yönetmek için yerleşik bir roldür. Bir kullanıcıya veya gruba uygulanan bu rol, dns kaynaklarını yönetmelerini sağlar.

Kaynak grubu *myResourceGroup,* Contoso Corporation için beş bölge içerir. DNS yöneticisi DNS Bölgesi Katılımcısı izinlerinin bu kaynak grubuna verilmesi, bu DNS bölgeleri üzerinde tam denetim sağlar. Gereksiz izinler vermekten kaçınır. DNS yöneticisi sanal makineler oluşturamaz veya durduramaz.

RBAC izinleri atamanın en basit yolu [Azure portalı üzerindendir.](../role-based-access-control/role-assignments-portal.md)  

Kaynak grubu için **Erişim denetimini (IAM)** açın, ardından **Ekle'yi**seçin ve **ardından DNS Bölgesi Katılımcısı** rolünü seçin. İzin vermek için gerekli kullanıcıları veya grupları seçin.

![Azure portalı üzerinden kaynak grubu düzeyi RBAC](./media/dns-protect-zones-recordsets/rbac1.png)

İzinler Azure [PowerShell kullanılarak](../role-based-access-control/role-assignments-powershell.md)da verilebilir:

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group

$usr = "<user email address>"
$rol = "DNS Zone Contributor"
$rsg = "<resource group name>"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg
```

Eşdeğer komut, [Azure CLI üzerinden](../role-based-access-control/role-assignments-cli.md)de kullanılabilir:

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group

az role assignment create \
--assignee "<user email address>" \
--role "DNS Zone Contributor" \
--resource-group "<resource group name>"
```

### <a name="zone-level-rbac"></a>Bölge seviyesi RBAC

Azure RBAC kuralları bir aboneye, kaynak grubuna veya tek bir kaynağa uygulanabilir. Bu kaynak tek bir DNS bölgesi veya tek bir kayıt kümesi olabilir.

Örneğin, kaynak grubu *myResourceGroup* bölge *contoso.com* ve *bir*alt bölge customers.contoso.com içerir. CNAME kayıtları her müşteri hesabı için oluşturulur. CNAME kayıtlarını yönetmek için kullanılan yönetici hesabına *customers.contoso.com* bölgesinde kayıt oluşturmak için izinler atanır. Hesap yalnızca *customers.contoso.com* yönetebilir.

Bölge düzeyinde KiRSa izinleri Azure portalı üzerinden verilebilir.  Bölge için **Erişim denetimini (IAM)** açın, **Ekle'yi**seçin, ardından **DNS Bölgesi Katılımcısı** rolünü seçin ve izin vermek için gerekli kullanıcıları veya grupları seçin.

![Azure portalı üzerinden DNS Zone düzeyi RBAC](./media/dns-protect-zones-recordsets/rbac2.png)

İzinler Azure [PowerShell kullanılarak](../role-based-access-control/role-assignments-powershell.md)da verilebilir:

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to a specific zone

$usr = "<user email address>"
$rol = "DNS Zone Contributor"
$rsg = "<resource group name>"
$zon = "<zone name>"
$typ = "Microsoft.Network/DNSZones"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg -ResourceName $zon -ResourceType $typ
```

Eşdeğer komut, [Azure CLI üzerinden](../role-based-access-control/role-assignments-cli.md)de kullanılabilir:

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone

az role assignment create \
--assignee <user email address> \
--role "DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/DnsZones/<zone name>/"
```

### <a name="record-set-level-rbac"></a>Rekor seviye RBAC

İzinler kayıt kümesi düzeyinde uygulanır.  Kullanıcıya gereksinim duydukları girişlere denetim verilir ve başka değişiklik yapamaz.

Kayıt düzeyindeki RBAC izinleri, kayıt kümesi sayfasındaki **Erişim Denetimi (IAM)** düğmesi kullanılarak Azure portalı üzerinden yapılandırılabilir:

![Azure portalı üzerinden rekor seviye RBAC](./media/dns-protect-zones-recordsets/rbac3.png)

Kayıt düzeyinde ki RBAC izinleri [Azure PowerShell kullanılarak](../role-based-access-control/role-assignments-powershell.md)da verilebilir:

```azurepowershell
# Grant permissions to a specific record set

$usr = "<user email address>"
$rol = "DNS Zone Contributor"
$sco = 
"/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -Scope $sco
```

Eşdeğer komut, [Azure CLI üzerinden](../role-based-access-control/role-assignments-cli.md)de kullanılabilir:

```azurecli
# Grant permissions to a specific record set

az role assignment create \
--assignee "<user email address>" \
--role "DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Özel roller

Yerleşik DNS Bölgesi Katılımcısı rolü, bir DNS kaynağı üzerinde tam denetim sağlar. Daha ince grenli denetim sağlamak için kendi özel Azure rollerinizi oluşturmak mümkündür.

CNAM'ları yönetmek için kullanılan hesaba yalnızca CNAME kayıtlarını yönetme izni verilir. Hesap diğer türlerin kayıtlarını değiştiremiyor. Hesap, bölge silme gibi bölge düzeyinde işlemleri yapamaz.

Aşağıdaki örnek, yalnızca CNAME kayıtlarını yönetmek için özel bir rol tanımı gösterir:

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
        "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

Eylemler özelliği aşağıdaki DNS'ye özgü izinleri tanımlar:

* `Microsoft.Network/dnsZones/CNAME/*`CNAME kayıtları üzerinde tam kontrol sağlar
* `Microsoft.Network/dnsZones/read`DNS bölgelerini okumak için izin verir, ancak bunları değiştirmez, böylece CNAME oluşturulduğu bölgeyi görmenizi sağlar.

Kalan [Eylemler, DNS Bölgesi Katılımcısı yerleşik rolünden](../role-based-access-control/built-in-roles.md#dns-zone-contributor)kopyalanır.

> [!NOTE]
> Kayıt kümelerinin güncelleştirilebilmesine izin verirken, özel bir RBAC rolünün kullanılması etkili bir denetim değildir. Kayıt kümelerinin silinmesini engeller, ancak bunların değiştirilmesini engellemez.  İzin verilen değişiklikler, boş bir kayıt kümesi bırakmak için tüm kayıtları kaldırmak da dahil olmak üzere, kayıt kümesinden kayıt ekleme yi ve kaldırmayı içerir. Bu, DNS çözünürlük bakış açısından ayarlanan kaydı silmeyle aynı etkiye sahiptir.

Özel rol tanımları şu anda Azure portalı üzerinden tanımlanamaz. Bu rol tanımına dayalı özel bir rol Azure PowerShell kullanılarak oluşturulabilir:

```azurepowershell
# Create new role definition based on input file
New-AzRoleDefinition -InputFile <file path>
```

Azure CLI üzerinden de oluşturulabilir:

```azurecli
# Create new role definition based on input file
az role create -inputfile <file path>
```

Bu makalede daha önce açıklandığı gibi, rol daha sonra yerleşik rollerle aynı şekilde atanabilir.

Özel rollerin nasıl oluşturulup yönetilen ve atayınız hakkında daha fazla bilgi için [Azure RBAC'da Özel Roller'e](../role-based-access-control/custom-roles.md)bakın.

## <a name="resource-locks"></a>Kaynak kilitleri

Azure Kaynak Yöneticisi, kaynakları kilitleme özelliği olan başka bir tür güvenlik denetimini destekler. Kaynak kilitleri kaynağa uygulanır ve tüm kullanıcılar ve roller arasında etkilidir. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/management/lock-resources.md).

İki tür kaynak kilidi vardır: **CanNotDelete** ve **ReadOnly**. Bu kilit türleri özel bir DNS bölgesine veya tek bir kayıt kümesine uygulanabilir. Aşağıdaki bölümlerde birkaç yaygın senaryo ve kaynak kilitlerini kullanarak bunları nasıl destekleyeceğiaçıklanmıştır.

### <a name="protecting-against-all-changes"></a>Tüm değişikliklere karşı koruma

Değişikliklerin yapılmasını önlemek için bölgeye ReadOnly kilidi uygulayın. Bu kilit, yeni kayıt kümelerinin oluşturulmasını ve varolan kayıt kümelerinin değiştirilmesini veya silinmesini önler.

Bölge düzeyi kaynak kilitleri Azure portalı üzerinden oluşturulabilir.  DNS bölge sayfasından **Kilitler'i**seçin ve **+Ekle'yi**seçin:

![Azure portalı üzerinden bölge düzeyinde kaynak kilitleri](./media/dns-protect-zones-recordsets/locks1.png)

Bölge düzeyinde kaynak kilitleri Azure [PowerShell](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcelock?view=latest)üzerinden de oluşturulabilir:

```azurepowershell
# Lock a DNS zone

$lvl = "<lock level>"
$lnm = "<lock name>"
$rsc = "<zone name>"
$rty = "Microsoft.Network/DNSZones"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

Eşdeğer komut, [Azure CLI üzerinden](https://docs.microsoft.com/cli/azure/lock?view=azure-cli-latest#az-lock-create)de kullanılabilir:

```azurecli
# Lock a DNS zone

az lock create \
--lock-type "<lock level>" \
--name "<lock name>" \
--resource-name "<zone name>" \
--namespace "Microsoft.Network" \
--resource-type "DnsZones" \
--resource-group "<resource group name>"
```

### <a name="protecting-individual-records"></a>Tek tek kayıtları koruma

Değişiklik yapılmasına karşı ayarlanmış varolan bir DNS kaydını önlemek için, kayıt kümesine ReadOnly kilidi uygulayın.

> [!NOTE]
> CanNotDelete kilidibir kayıt kümesine uygulamak etkili bir denetim değildir. Ayarlanan kaydın silinmesini engeller, ancak değiştirilmesini engellemez.  İzin verilen değişiklikler, boş bir kayıt kümesi bırakmak için tüm kayıtları kaldırmak da dahil olmak üzere, kayıt kümesinden kayıt ekleme yi ve kaldırmayı içerir. Bu, DNS çözünürlük bakış açısından ayarlanan kaydı silmeyle aynı etkiye sahiptir.

Kayıt kümesi düzeyi kaynak kilitleri şu anda yalnızca Azure PowerShell kullanılarak yapılandırılabilir.  Azure portalında veya Azure CLI'de desteklenmez.

```azurepowershell
# Lock a DNS record set

$lvl = "<lock level>"
$lnm = "<lock name>"
$rsc = "<zone name>/<record set name>"
$rty = "Microsoft.Network/DNSZones/<record type>"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

### <a name="protecting-against-zone-deletion"></a>Bölge silmeye karşı koruma

Azure DNS'de bir bölge silindiğinde, bölgedeki tüm kayıt kümeleri silinir.  Bu işlem geri alınamaz. Kritik bir bölgenin yanlışlıkla silmesi, önemli bir iş etkisi yaratma potansiyeline sahiptir.  Kazara bölge silmeye karşı korunmak önemlidir.

Bir bölgeye CanNotDelete kilidi uygulamak, bölgenin silinmesini engeller. Kilitler alt kaynaklardan devralınır. Kilit, bölgede bulunan kayıt kümelerinin silinmesini önler. Yukarıdaki notta açıklandığı gibi, kayıtlar hala varolan kayıt kümelerinden kaldırılabilir beri etkisizdir.

Alternatif olarak, SOA kayıt kümesi gibi bölgede ayarlanan bir kayda CanNotDelete kilidi uygulayın. Bölge, kayıt kümelerini de silmeden silinmez. Bu kilit bölge silmeye karşı korurken, bölge içindeki kayıt kümelerinin serbestçe değiştirilmesine izin verir. Bölgeyi silme girişiminde bulunulması durumunda, Azure Kaynak Yöneticisi bu kaldırma işlemini algılar. Kaldırma işlemi SOA kayıt kümesini de siler, Azure Kaynak Yöneticisi SOA kilitli olduğu için aramayı engeller.  Hiçbir kayıt kümesi silinmez.

Aşağıdaki PowerShell komutu verilen bölgenin SOA kaydına karşı cannotdelete kilidi oluşturur:

```azurepowershell
# Protect against zone delete with CanNotDelete lock on the record set

$lvl = "CanNotDelete"
$lnm = "<lock name>"
$rsc = "<zone name>/@"
$rty = "Microsoft.Network/DNSZones/SOA"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

Yanlışlıkla bölge silmeönlemek için başka bir seçenek özel bir rol kullanarak. Bu rol, bölgelerinizi yönetmek için kullanılan hesapların bölge silme izinleri olmamasını sağlar. 

Bir bölgeyi silmeniz gerektiğinde, iki adımlı silme yi zorlayabilirsiniz:

 - İlk olarak, hibe bölgesi silme izinleri
 - İkinci olarak, bölgeyi silmek için izin ler ver.

Özel rol, bu hesaplar tarafından erişilen tüm bölgeler için çalışır. Abonelik sahibi gibi bölge silme izinleri olan hesaplar yine de yanlışlıkla bir bölgeyi silebilir.

Her iki yaklaşımı da -kaynak kilitleri ve özel roller- aynı anda, DNS bölge korumasına yönelik derinlemesine bir savunma yaklaşımı olarak kullanmak mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

* RBAC ile çalışma hakkında daha fazla bilgi için Azure [portalındaki erişim yönetimine başlayın'a](../role-based-access-control/overview.md)bakın.
* Kaynak kilitleriyle çalışma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi ile kaynakları kilitle'ye](../azure-resource-manager/management/lock-resources.md)bakın.
