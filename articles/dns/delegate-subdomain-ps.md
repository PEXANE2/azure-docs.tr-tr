---
title: Bir alt etki alanını temsilci - Azure PowerShell - Azure DNS
description: Bu öğrenme yolu ile Azure PowerShell'i kullanarak bir Azure DNS alt etki alanını atamaya başlayın.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: rohink
ms.openlocfilehash: 7e019afaae98422b8d5a3c8fa7a5f79e26c6a149
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937700"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Azure PowerShell'i kullanarak bir Azure DNS alt etki alanı temsilci

Bir DNS alt etki alanını devretmek için Azure PowerShell'i kullanabilirsiniz. Örneğin, contoso.com etki alanına sahipseniz, *mühendislik* adı verilen bir alt etki alanını, contoso.com bölgesinden ayrı olarak yönetebileceğiniz başka bir ayrı bölgeye devredebilirsiniz.

İsterseniz, [Azure Portalı'nı](delegate-subdomain.md)kullanarak bir alt etki alanı devralabilirsiniz.

> [!NOTE]
> Contoso.com bu makale boyunca örnek olarak kullanılır. contoso.com yerine kendi etki alanı adınızı yazın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bir Azure DNS alt etki alanını devretmek için öncelikle ortak etki alanınızı Azure DNS'ye devretmeniz gerekir. Ad sunucularınızı temsilci olarak nasıl yapılandıracağınıza ilişkin talimatlar için [azure DNS'ye bir etki alanı devretme](./dns-delegate-domain-azure-dns.md) bilgisini görün. Etki alanınız Azure DNS bölgenize devredildikten sonra alt etki alanınızı devredebilirsiniz.

## <a name="create-a-zone-for-your-subdomain"></a>Alt etki alanınız için bir bölge oluşturma

İlk olarak, **mühendislik** alt etki alanı için bölge oluşturun.

`New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com`

## <a name="note-the-name-servers"></a>Ad sunucularını not edin

Ardından, mühendislik alt etki alanı için dört ad sunucularını not edin.

`Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS`

## <a name="create-a-test-record"></a>Test kaydı oluşturma

Test için kullanılacak mühendislik bölgesinde bir **A** kaydı oluşturun.

   `New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)`.

## <a name="create-an-ns-record"></a>NS kaydı oluşturma

Ardından, contoso.com bölgesindeki **mühendislik** bölgesi için bir ad sunucusu (NS) kaydı oluşturun.

```azurepowershell
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>Delegasyonu test edin

Delegasyonu test etmek için nslookup'u kullanın.

1. PowerShell penceresini açın.
2. Komut isteminde, yazın`nslookup www.engineering.contoso.com.`
3. Adresi gösteren **10.10.10.10'u**gösteren yetkili olmayan bir yanıt almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

[Azure'da barındırılan hizmetler için ters DNS'yi nasıl yapılandırabileceğinizi](dns-reverse-dns-for-azure-services.md)öğrenin.