---
title: Manage DNS zones in Azure DNS - Azure CLI | Microsoft Docs
description: You can manage DNS zones using Azure CLI. This article shows how to update, delete and create DNS zones on Azure DNS.
services: dns
documentationcenter: na
author: asudbring
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: allensu
ms.openlocfilehash: e1a3c401de32beb9757011ac306443334da8b867
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74211917"
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli"></a>How to manage DNS Zones in Azure DNS using the Azure CLI

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI](dns-operations-dnszones-cli.md)


This guide shows how to manage your DNS zones by using the cross-platform Azure CLI, which is available for Windows, Mac and Linux. You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or the Azure portal.

This guide specifically deals with Public DNS zones. For information on using Azure CLI to manage Private Zones in Azure DNS, see [Get started with Azure DNS Private Zones using Azure CLI](private-dns-getstarted-cli.md).

## <a name="introduction"></a>Tanıtım

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-for-azure-dns"></a>Azure DNS için Azure CLI'yi ayarlama

### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

* Windows, Linux veya Mac için Azure CLI'nin son sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-az-cli2).

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Bir konsol penceresi açın ve kimlik bilgilerinizi girerek kimliğinizi doğrulayın. Daha fazla bilgi için bkz. [Azure CLI üzerinden Azure’da oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

```
az login
```

### <a name="select-the-subscription"></a>Aboneliği seçme

Hesapla ilişkili abonelikleri kontrol edin.

```
az account list
```

Hangi Azure aboneliğinizin kullanılacağını seçin.

```azurecli
az account set --subscription "subscription name"
```

### <a name="optional-to-installuse-azure-dns-private-zones-feature"></a>Optional: To install/use Azure DNS Private Zones feature
The Azure DNS Private Zone feature is available via an extension to the Azure CLI. "Dns" Azure CLI uzantısını yükleme 
```
az extension add --name dns
``` 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Yardım alma

All Azure CLI commands relating to Azure DNS start with `az network dns`. Help is available for each command using the `--help` option (short form `-h`).  Örnek:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `az network dns zone create` komutu kullanılarak oluşturulur. Yardım için bkz. `az network dns zone create -h`.

The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>To create a DNS zone with tags

The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using the `--tags` parameter (short form `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Get a DNS zone

To retrieve a DNS zone, use `az network dns zone show`. Yardım için bkz. `az network dns zone show --help`.

The following example returns the DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*. 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

Aşağıdaki örnek, yanıttır.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

`az network dns zone show` tarafından DNS kayıtlarının döndürülmediğini unutmayın. DNS kayıtlarını listelemek için `az network dns record-set list` komutunu kullanın.


## <a name="list-dns-zones"></a>DNS bölgelerini listeleme

DNS bölgelerini numaralandırmak için `az network dns zone list` komutunu kullanın. Yardım için bkz. `az network dns zone list --help`.

Kaynak grubu belirtildiğinde yalnızca kaynak grubu içindeki bölgeler listelenir:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Kaynak grubu atıldığında, abonelikteki tüm bölgeler listelenir:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>DNS bölgesini güncelleştirme

`az network dns zone update` komutu kullanılarak bir DNS bölgesi kaynağı üzerinde değişiklikler yapılabilir. Yardım için bkz. `az network dns zone update --help`.

Bu komut, bölgedeki DNS kayıt kümelerinin herhangi birini güncelleştirmez (bkz. [DNS kayıtlarını yönetme](dns-operations-recordsets-cli.md)). Yalnızca bölge kaynağının özelliklerini güncelleştirmek için kullanılır. These properties are currently limited to the [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for the zone resource.

The following example shows how to update the tags on a DNS zone. The existing tags are replaced by the value specified.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>DNS bölgesini silme

`az network dns zone delete` komutu kullanılarak DNS bölgeleri silinebilir. Yardım için bkz. `az network dns zone delete --help`.

> [!NOTE]
> Bir DNS bölgesi silindiğinde, bölge içindeki tüm DNS kayıtları da silinir. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölgeyi kullanan hizmetler başarısız olur.
>
>Bölgenin yanlışlıkla silinmesine karşı koruma oluşturmak için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md).

Bu komut, onay ister. İsteğe bağlı `--yes` anahtarı, bu onay istemini gizler.

The following example shows how to delete the zone *contoso.com* from resource group *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Sonraki adımlar

Learn how to [manage record sets and records](dns-getstarted-create-recordset-cli.md) in your DNS zone.

Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).

