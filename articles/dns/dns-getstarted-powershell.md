---
title: 'Hızlı başlangıç: Azure DNS bölgesi ve kaydı oluşturma-Azure PowerShell'
titleSuffix: Azure DNS
description: Azure DNS'te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu hızlı başlangıçta, Azure PowerShell kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım yönergeler sağlanır.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: quickstart
ms.date: 10/20/2020
ms.author: rohink
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f2563c33d02490732f73fcf9d1a78f548ec2d3e2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92282212"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell’i kullanarak Azure DNS bölgesi ve kaydı oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu hızlı başlangıçta PowerShell kullanarak ilk DNS bölgenizi ve kaydını oluşturursunuz. Ayrıca, [Azure portal](dns-getstarted-portal.md) veya [Azure CLI](dns-getstarted-cli.md)'sini kullanarak aşağıdaki adımları gerçekleştirebilirsiniz. 

DNS bölgesi, belirli bir etki alanına ait DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Azure DNS ayrıca özel etki alanları oluşturmayı da destekler. İlk özel DNS bölgenizi ve kaydınızı oluşturma hakkında adım adım yönergeler için [PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama](private-dns-getstarted-powershell.md) konusunu inceleyin.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure PowerShell yerel olarak veya Azure Cloud Shell yüklendi

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek kaynak grubunu oluşturun.

```powershell
New-AzResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzDnsZone` cmdlet’i kullanılarak oluşturulur. Aşağıdaki örnek, *Myresourcegroup* adlı kaynak grubunda *contoso.xyz* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```powershell
New-AzDnsZone -Name contoso.xyz -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

`New-AzDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Aşağıdaki örnek, "MyResourceGroup" kaynak grubunda "contoso.xyz" DNS bölgesinde "www" göreli adına sahip bir kayıt oluşturur. Kayıt kümesinin tam nitelikli adı "www.contoso.xyz" dir. Kayıt türü "A", IP adresi "10.10.10.10" ve TTL 3600 saniyedir.

```powershell
New-AzDnsRecordSet -Name www -RecordType A -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "10.10.10.10")
```

## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu seçenekleri kullanın:

```powershell
Get-AzDnsRecordSet -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup
```

## <a name="test-the-name-resolution"></a>Ad çözümlemesini test etme

Test ' A ' kaydıyla bir test DNS bölgesine sahip olduğunuza göre, ad çözümlemesini *nslookup* adlı bir araçla test edebilirsiniz. 

**DNS ad çözümlemesini test etmek için:**

1. Bölgenizin ad sunucularının listesini almak için aşağıdaki cmdlet 'i çalıştırın:

   ```azurepowershell
   Get-AzDnsRecordSet -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup -RecordType ns
   ```

1. Ad sunucusu adlarından birini, önceki adımın çıktısından kopyalayın.

1. Bir komut istemi açın ve aşağıdaki komutu çalıştırın:

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Örnek:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Aşağıdaki ekrana benzer bir şey görmeniz gerekir:

   ![Ekran görüntüsü, bir n s arama komutu ve sunucu, adres, ad ve adres değerlerini içeren bir komut istemi penceresi gösterir.](media/dns-getstarted-portal/nslookup.PNG)

**Www \. contoso.xyz** ana bilgisayar adı, yalnızca yapılandırdığınız gibi **10.10.10.10** olarak çözümlenir. Bu sonuç, ad çözümlemenin doğru çalıştığını doğrular.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu silerek bu hızlı başlangıçta oluşturulan tüm kaynakları silebilirsiniz:

```powershell
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell kullanarak ilk DNS bölgenizi ve kaydı oluşturduğunuza göre artık özel etki alanında bulunan bir web uygulaması için kayıt oluşturabilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)

