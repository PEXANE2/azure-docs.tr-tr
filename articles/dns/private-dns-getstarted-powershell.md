---
title: Hızlı başlangıç-Azure PowerShell kullanarak bir Azure özel DNS bölgesi oluşturma
description: Bu makalede, Azure DNS özel bir DNS bölgesi ve kaydı oluşturup test edersiniz. Bu, Azure PowerShell kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmek için adım adım kılavuzudur.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 10/05/2019
ms.author: victorh
ms.openlocfilehash: fb39042e53795057a3404ba1e8cb5903188966f7
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960453"
---
# <a name="quickstart-create-an-azure-private-dns-zone-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak Azure özel DNS bölgesi oluşturma

Bu makalede, Azure PowerShell kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturma adımlarında size kılavuzluk eder.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

DNS bölgesi, belirli bir etki alanı için DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS barındırılmaya başlamak için, bu etki alanı adı için bir DNS bölgesi oluşturmanız gerekir. Etki alanınız için her bir DNS kaydı daha sonra bu DNS bölgesi içinde oluşturulur. Sanal ağınıza özel bir DNS bölgesi yayımlamak için, bölgedeki kayıtları çözümlemek için izin verilen sanal ağların listesini belirtirsiniz.  Bunlara *bağlı* sanal ağlar denir. Oto kayıt etkinleştirildiğinde Azure DNS, bir sanal makine oluşturulduğunda bölge kayıtlarını da güncelleştirir, ' IP adresini değiştirir veya silinir.

Bu makalede, aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Özel bir DNS bölgesi oluşturma
> * Test sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Tercih ederseniz, [Azure CLI](private-dns-getstarted-cli.md)kullanarak bu hızlı başlangıcı tamamlayabilirsiniz.

## <a name="create-the-resource-group"></a>Kaynak grubu oluşturma

İlk olarak, DNS bölgesini içerecek bir kaynak grubu oluşturun: 

```azurepowershell
New-AzResourceGroup -name MyAzureResourceGroup -location "eastus"
```

## <a name="create-a-private-dns-zone"></a>Özel bir DNS bölgesi oluşturma

Bir DNS bölgesi `New-AzPrivateDnsZone` cmdlet 'i kullanılarak oluşturulur.

Aşağıdaki örnek, **Myazurevnet**adlı bir sanal ağ oluşturur. Daha sonra, **MyAzureResourceGroup** kaynak grubunda **PRIVATE.contoso.com** adlı bir DNS bölgesi oluşturur, DNS bölgesini **myazurevnet** sanal ağına bağlar ve otomatik kaydı sağlar.

```azurepowershell
Install-Module -Name Az.PrivateDns -force

$backendSubnet = New-AzVirtualNetworkSubnetConfig -Name backendSubnet -AddressPrefix "10.2.0.0/24"
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName MyAzureResourceGroup `
  -Location eastus `
  -Name myAzureVNet `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $backendSubnet

$zone = New-AzPrivateDnsZone -Name private.contoso.com -ResourceGroupName MyAzureResourceGroup

$link = New-AzPrivateDnsVirtualNetworkLink -ZoneName private.contoso.com `
  -ResourceGroupName MyAzureResourceGroup -Name "mylink" `
  -VirtualNetworkId $vnet.id -EnableRegistration
```

Yalnızca ad çözümlemesi için bir bölge oluşturmak istiyorsanız (otomatik konak adı kaydı olmadan) `-EnableRegistration` parametresini atlayabilirsiniz.

### <a name="list-dns-private-zones"></a>DNS özel bölgelerini listeleme

@No__t-0 ' dan bölge adını atlayarak, bir kaynak grubundaki tüm bölgeleri sıralayabilirsiniz. Bu işlem, bölge nesnelerinin bir dizisini döndürür.

```azurepowershell
$zones = Get-AzPrivateDnsZone -ResourceGroupName MyAzureResourceGroup
$zones
```

Hem bölge adını hem de kaynak grubu adını `Get-AzPrivateDnsZone` ' dan atlayarak Azure aboneliğindeki tüm bölgeleri sıralayabilirsiniz.

```azurepowershell
$zones = Get-AzPrivateDnsZone
$zones
```

## <a name="create-the-test-virtual-machines"></a>Test sanal makineleri oluşturma

Şimdi, özel DNS bölgenizi sınayabilmeniz için iki sanal makine oluşturun:

```azurepowershell
New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM01" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389

New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM02" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389
```

Bu işlem birkaç dakika sürer.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

@No__t-0 cmdlet 'ini kullanarak kayıt kümeleri oluşturursunuz. Aşağıdaki örnek, **Private.contoso.com**kaynak GRUBUNDAKI **MyAzureResourceGroup**DNS bölgesinde göreli ad **DB** ile bir kayıt oluşturur. Kayıt kümesinin tam nitelikli adı **DB.Private.contoso.com**' dir. Kayıt türü "A", IP adresi "10.2.0.4" ve TTL 3600 saniyedir.

```azurepowershell
New-AzPrivateDnsRecordSet -Name db -RecordType A -ZoneName private.contoso.com `
   -ResourceGroupName MyAzureResourceGroup -Ttl 3600 `
   -PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address "10.2.0.4")
```

### <a name="view-dns-records"></a>DNS kayıtlarını görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şunu çalıştırın:

```azurepowershell
Get-AzPrivateDnsRecordSet -ZoneName private.contoso.com -ResourceGroupName MyAzureResourceGroup
```

## <a name="test-the-private-zone"></a>Özel bölgeyi test etme

Artık **Private.contoso.com** özel bölgeniz için ad çözümlemesini test edebilirsiniz.

### <a name="configure-vms-to-allow-inbound-icmp"></a>VM 'Leri gelen ıCMP 'ye izin verecek şekilde yapılandırma

Ad çözümlemesini sınamak için ping komutunu kullanabilirsiniz. Bu nedenle, gelen ıCMP paketlerine izin vermek için her iki sanal makinede güvenlik duvarını yapılandırın.

1. MyVM01 'e bağlanın ve yönetici ayrıcalıklarına sahip bir Windows PowerShell penceresi açın.
2. Şu komutu çalıştırın:

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

MyVM02 için yineleyin.

### <a name="ping-the-vms-by-name"></a>VM 'Lere ada göre ping gönder

1. MyVM02 Windows PowerShell komut isteminde, otomatik olarak kaydettirilen ana bilgisayar adını kullanarak ping myVM01:

   ```
   ping myVM01.private.contoso.com
   ```

   Şuna benzer bir çıktı görmeniz gerekir:

   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```

2. Daha önce oluşturduğunuz **veritabanı** adına ping gönderin:

   ```
   ping db.private.contoso.com
   ```

   Şuna benzer bir çıktı görmeniz gerekir:

   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milliseconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Tüm kaynakları Sil

Artık gerekli değilse, bu makalede oluşturulan kaynakları silmek için **MyAzureResourceGroup** kaynak grubunu silin.

```azurepowershell
Remove-AzResourceGroup -Name MyAzureResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure DNS Özel Bölgeleri senaryolar](private-dns-scenarios.md)
