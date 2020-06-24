---
title: Bir alt etki alanı temsilcisi-Azure PowerShell Azure DNS
description: Bu öğrenme yoluyla, Azure PowerShell kullanarak bir Azure DNS alt etki alanı için temsilci seçmeye başlayın.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: how-to
ms.date: 2/7/2019
ms.author: rohink
ms.openlocfilehash: 9b37d313aa5d8c2255b4e3be69831dfcb50238ea
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84712556"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Azure PowerShell kullanarak bir Azure DNS alt etki alanı temsilcisi seçme

DNS alt etki alanı atamak için Azure PowerShell kullanabilirsiniz. Örneğin, contoso.com etki alanına sahipseniz, *mühendislik* adlı bir alt etki alanını contoso.com bölgesinden ayrı olarak yönetebilmeniz için farklı bir bölgeye atayabilirsiniz.

İsterseniz, [Azure portalını](delegate-subdomain.md)kullanarak bir alt etki alanı için temsilci seçebilirsiniz.

> [!NOTE]
> Bu makalenin tamamında bir örnek olarak Contoso.com kullanılır. contoso.com yerine kendi etki alanı adınızı yazın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure DNS bir alt etki alanı atamak için, önce genel etki alanınızı Azure DNS olarak vermelisiniz. Ad sunucularınızı temsilci olarak yapılandırma hakkında yönergeler için bkz. [Azure DNS etki alanı verme](./dns-delegate-domain-azure-dns.md) . Etki alanınız Azure DNS bölgenize atandıktan sonra, alt etki alanınızı temsil edebilirsiniz.

## <a name="create-a-zone-for-your-subdomain"></a>Alt etki alanı için bir bölge oluşturun

İlk olarak, **mühendislik** alt etki alanı için bölgeyi oluşturun.

`New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com`

## <a name="note-the-name-servers"></a>Ad sunucularına göz önünde

Ardından, mühendislik alt etki alanı için dört ad sunucusu ' nu aklınızda yapın.

`Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS`

## <a name="create-a-test-record"></a>Test kaydı oluşturma

Test için kullanılacak mühendislik bölgesinde **bir** kayıt oluşturun.

   `New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)`.

## <a name="create-an-ns-record"></a>NS kaydı oluşturma

Sonra, contoso.com bölgesindeki **mühendislik** bölgesi için bir ad sunucusu (NS) kaydı oluşturun.

```azurepowershell
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>Temsilciyi test etme

Temsilciyi test etmek için nslookup 'ı kullanın.

1. PowerShell penceresini açın.
2. Komut isteminde şunu yazın:`nslookup www.engineering.contoso.com.`
3. **10.10.10.10**adresini gösteren yetkili olmayan bir yanıt almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure 'da barındırılan hizmetler için ters DNS yapılandırmayı](dns-reverse-dns-for-azure-services.md)öğrenin.