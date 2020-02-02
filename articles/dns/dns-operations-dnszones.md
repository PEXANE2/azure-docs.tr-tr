---
title: Azure DNS-PowerShell 'de DNS bölgelerini yönetme | Microsoft Docs
description: Azure PowerShell 'i kullanarak DNS bölgelerini yönetebilirsiniz. Bu makalede, Azure DNS üzerinde DNS bölgelerini güncelleştirme, silme ve oluşturma işlemlerinin nasıl yapılacağı açıklanmaktadır
services: dns
documentationcenter: na
author: rohinkoul
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: rohink
ms.openlocfilehash: 0120501aab7f0a63721126bfb5b3d04d9deb42fb
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76936812"
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>PowerShell kullanarak DNS Bölgeleri Yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure klasik CLI](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI](dns-operations-dnszones-cli.md)

Bu makalede, Azure PowerShell kullanarak DNS bölgelerinizi nasıl yöneteceğiniz gösterilmektedir. Ayrıca, platformlar arası [Azure CLI](dns-operations-dnszones-cli.md) veya Azure Portal kullanarak DNS bölgelerinizi yönetebilirsiniz.

Bu kılavuz özellikle ortak DNS bölgeleriyle ilgilidir. Azure DNS özel bölgeleri yönetmek için Azure PowerShell kullanma hakkında daha fazla bilgi için bkz. [Azure PowerShell kullanarak Azure DNS özel bölgeleri kullanmaya başlama](private-dns-getstarted-powershell.md).

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzureRmDnsZone` cmdlet’i kullanılarak oluşturulur.

Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

Aşağıdaki örnek, *Proje = demo* ve *env = test*olmak üzere ıkı [Azure Resource Manager etiketleriyle](dns-zones-records.md#tags)bir DNS bölgesinin nasıl oluşturulacağını gösterir:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

Ayrıca, özel DNS bölgelerini de destekler Azure DNS.  Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md). Özel bir DNS bölgesi oluşturma örneği için bkz. [PowerShell kullanarak Azure DNS özel bölgeleriyle çalışmaya başlama](./private-dns-getstarted-powershell.md).

## <a name="get-a-dns-zone"></a>DNS bölgesi al

Bir DNS bölgesi almak için `Get-AzureRmDnsZone` cmdlet 'ini kullanın. Bu işlem, Azure DNS var olan bir bölgeye karşılık gelen bir DNS bölgesi nesnesi döndürür. Nesnesi, bölge hakkında (kayıt kümesi sayısı gibi) veriler içerir, ancak kayıt kümelerini içermez (bkz. `Get-AzureRmDnsRecordSet`).

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

`Set-AzureRmDnsZone` kullanılarak bir DNS bölgesi kaynağı üzerinde değişiklikler yapılabilir. Bu cmdlet, bölge içindeki DNS kayıt kümelerinin herhangi birini güncelleştirmez (bkz. [DNS kayıtlarını yönetme](dns-operations-recordsets.md)). Yalnızca bölge kaynağının özelliklerini güncelleştirmek için kullanılır. Yazılabilir bölge özellikleri şu anda [bölge kaynağı için Azure Resource Manager ' Tags '](dns-zones-records.md#tags)ile sınırlıdır.

DNS bölgesini güncelleştirmek için aşağıdaki iki yöntemi kullanın:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Bölge adını ve kaynak grubunu kullanarak bölgeyi belirtin

Bu yaklaşım, var olan bölge etiketlerinin yerine belirtilen değerleri koyar.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>$zone nesnesini kullanarak bölgeyi belirtin

Bu yaklaşım, var olan bölge nesnesini alır, etiketleri değiştirir ve ardından değişiklikleri kaydeder. Bu şekilde, varolan Etiketler korunabilir.

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

$Zone nesnesiyle `Set-AzureRmDnsZone` kullanırken, eş zamanlı değişikliklerin üzerine yazılmadığından emin olmak için [ETag denetimleri](dns-zones-records.md#etags) kullanılır. Bu denetimleri bastırmak için isteğe bağlı `-Overwrite` anahtarını kullanabilirsiniz.

## <a name="delete-a-dns-zone"></a>DNS bölgesini silme

DNS bölgeleri `Remove-AzureRmDnsZone` cmdlet 'i kullanılarak silinebilir.

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

`Set-AzureRmDnsZone`olduğu gibi, `$zone` bir nesne kullanarak bölgenin belirtilmesi, eş zamanlı değişikliklerin silinmediğinden emin olmak için ETag denetimleri sağlar. Bu denetimleri bastırmak için `-Overwrite` anahtarını kullanın.

## <a name="confirmation-prompts"></a>Onay istemleri

`New-AzureRmDnsZone`, `Set-AzureRmDnsZone` ve `Remove-AzureRmDnsZone` cmdlet’lerinin tamamı onay istemlerini destekler.

`$ConfirmPreference` PowerShell tercih değişkeni, `Medium` veya daha küçük bir değere sahipse hem `New-AzureRmDnsZone` hem de `Set-AzureRmDnsZone` onay istemi görüntüler. DNS bölgesini silmenin olası kritik etkisi nedeniyle, `$ConfirmPreference` PowerShell değişkeninin `None` dışında bir değeri varsa `Remove-AzureRmDnsZone` cmdlet’i, onay istemi görüntüler.

`$ConfirmPreference` için varsayılan değer `High` olduğundan yalnızca `Remove-AzureRmDnsZone` varsayılan olarak onay istemi görüntüler.

`-Confirm` parametresini kullanarak geçerli `$ConfirmPreference` ayarını geçersiz kılabilirsiniz. `-Confirm` veya `-Confirm:$True` belirtirseniz cmdlet, çalıştırılmadan önce size onay istemi görüntüler. `-Confirm:$False` belirtirseniz cmdlet size onay istemi görüntülemez.

`-Confirm` ve `$ConfirmPreference` hakkında daha fazla bilgi için bkz. [Tercih Değişkenleri Hakkında](/powershell/module/microsoft.powershell.core/about/about_preference_variables).

## <a name="next-steps"></a>Sonraki adımlar

DNS bölgesindeki [kayıt kümelerini ve kayıtları yönetmeyi](dns-operations-recordsets.md) öğrenin.
<br>
[Azure DNS için etki alanınızı nasıl atayacağınızı](dns-domain-delegation.md)öğrenin.
<br>
[PowerShell başvuru belgelerini Azure DNS](/powershell/module/azurerm.dns)inceleyin.

