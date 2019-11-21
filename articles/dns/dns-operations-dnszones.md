---
title: Manage DNS zones in Azure DNS - PowerShell | Microsoft Docs
description: You can manage DNS zones using Azure Powershell. This article describes how to update, delete and create DNS zones on Azure DNS
services: dns
documentationcenter: na
author: asudbring
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: allensu
ms.openlocfilehash: 7a838f10d44f3be0af79bba5d303467884159cbf
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74211711"
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>How to manage DNS Zones using PowerShell

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure klasik CLI](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI](dns-operations-dnszones-cli.md)

This article shows you how to manage your DNS zones by using Azure PowerShell. You can also manage your DNS zones using the cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or the Azure portal.

This guide specifically deals with Public DNS zones. For information on using Azure PowerShell to manage Private Zones in Azure DNS, see [Get started with Azure DNS Private Zones using Azure PowerShell](private-dns-getstarted-powershell.md).

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzureRmDnsZone` cmdlet’i kullanılarak oluşturulur.

The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

Azure DNS also supports private DNS zones.  Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md). Özel bir DNS bölgesi oluşturma örneği için bkz. [PowerShell kullanarak Azure DNS özel bölgeleriyle çalışmaya başlama](./private-dns-getstarted-powershell.md).

## <a name="get-a-dns-zone"></a>Get a DNS zone

To retrieve a DNS zone, use the `Get-AzureRmDnsZone` cmdlet. This operation returns a DNS zone object corresponding to an existing zone in Azure DNS. The object contains data about the zone (such as the number of record sets), but does not contain the record sets themselves (see `Get-AzureRmDnsRecordSet`).

```powershell
Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-8ec2-f4879750d201
Tags                  : {project, env}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org.,
                        ns4-01.azure-dns.info.}
NumberOfRecordSets    : 2
MaxNumberOfRecordSets : 5000
```

## <a name="list-dns-zones"></a>DNS bölgelerini listeleme

`Get-AzureRmDnsZone` içinden bölge adını atarak bir kaynak grubundaki tüm bölgeleri numaralandırabilirsiniz. Bu işlem, bölge nesnelerinin bir dizisini döndürür.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

`Get-AzureRmDnsZone` içinden hem bölge adını hem de kaynak grubunu atarak, Azure aboneliğindeki tüm bölgeleri numaralandırabilirsiniz.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>DNS bölgesini güncelleştirme

`Set-AzureRmDnsZone` kullanılarak bir DNS bölgesi kaynağı üzerinde değişiklikler yapılabilir. Bu cmdlet, bölge içindeki DNS kayıt kümelerinin herhangi birini güncelleştirmez (bkz. [DNS kayıtlarını yönetme](dns-operations-recordsets.md)). Yalnızca bölge kaynağının özelliklerini güncelleştirmek için kullanılır. The writable zone properties are currently limited to the [Azure Resource Manager ‘tags’ for the zone resource](dns-zones-records.md#tags).

Use one of the following two ways to update a DNS zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Specify the zone using the zone name and resource group

This approach replaces the existing zone tags with the values specified.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>$zone nesnesini kullanarak bölgeyi belirtin

This approach retrieves the existing zone object, modifies the tags, and then commits the changes. In this way, existing tags can be preserved.

```powershell
# Get the zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

When using `Set-AzureRmDnsZone` with a $zone object, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten. You can use the optional `-Overwrite` switch to suppress these checks.

## <a name="delete-a-dns-zone"></a>Delete a DNS Zone

DNS zones can be deleted using the `Remove-AzureRmDnsZone` cmdlet.

> [!NOTE]
> Bir DNS bölgesi silindiğinde, bölge içindeki tüm DNS kayıtları da silinir. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölgeyi kullanan hizmetler başarısız olur.
>
>Bölgenin yanlışlıkla silinmesine karşı koruma oluşturmak için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md).


DNS bölgesini silmek için aşağıdaki iki yöntemden birini uygulayın:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Bölge adını ve kaynak grubu adını kullanarak bölgeyi belirtin

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>$zone nesnesini kullanarak bölgeyi belirtin

`Get-AzureRmDnsZone` tarafından döndürülen bir `$zone` nesnesini kullanarak silinecek bölgeyi belirtebilirsiniz.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Bölge nesnesi, parametre olarak geçirilmek yerine yöneltilebilir:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

As with `Set-AzureRmDnsZone`, specifying the zone using a `$zone` object enables Etag checks to ensure concurrent changes are not deleted. Use the `-Overwrite` switch to suppress these checks.

## <a name="confirmation-prompts"></a>Onay istemleri

`New-AzureRmDnsZone`, `Set-AzureRmDnsZone` ve `Remove-AzureRmDnsZone` cmdlet’lerinin tamamı onay istemlerini destekler.

`$ConfirmPreference` PowerShell tercih değişkeni, `Medium` veya daha küçük bir değere sahipse hem `New-AzureRmDnsZone` hem de `Set-AzureRmDnsZone` onay istemi görüntüler. DNS bölgesini silmenin olası kritik etkisi nedeniyle, `$ConfirmPreference` PowerShell değişkeninin `None` dışında bir değeri varsa `Remove-AzureRmDnsZone` cmdlet’i, onay istemi görüntüler.

`$ConfirmPreference` için varsayılan değer `High` olduğundan yalnızca `Remove-AzureRmDnsZone` varsayılan olarak onay istemi görüntüler.

`-Confirm` parametresini kullanarak geçerli `$ConfirmPreference` ayarını geçersiz kılabilirsiniz. `-Confirm` veya `-Confirm:$True` belirtirseniz cmdlet, çalıştırılmadan önce size onay istemi görüntüler. `-Confirm:$False` belirtirseniz cmdlet size onay istemi görüntülemez.

`-Confirm` ve `$ConfirmPreference` hakkında daha fazla bilgi için bkz. [Tercih Değişkenleri Hakkında](/powershell/module/microsoft.powershell.core/about/about_preference_variables).

## <a name="next-steps"></a>Sonraki adımlar

Learn how to [manage record sets and records](dns-operations-recordsets.md) in your DNS zone.
<br>
Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).
<br>
Review the [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).

