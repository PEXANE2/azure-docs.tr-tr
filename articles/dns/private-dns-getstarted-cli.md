---
title: Quickstart - Azure CLI'yi kullanarak Azure özel DNS bölgesi oluşturma
description: Bu hızlı başlangıçta, azure DNS'de özel bir DNS bölgesi oluşturup sınayıp kaydedebilirsiniz. Bu kılavuzda, Azure CLI kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: quickstart
ms.date: 10/05/2019
ms.author: rohink
ms.openlocfilehash: 34a9858ffbec2360a347b8c765d53551fd2d9fc0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76935420"
---
# <a name="quickstart-create-an-azure-private-dns-zone-using-the-azure-cli"></a>Hızlı başlatma: Azure CLI'yi kullanarak bir Azure özel DNS bölgesi oluşturun

Bu hızlı başlangıç, ilk özel DNS bölgenizi oluşturmak ve Azure CLI'yi kullanarak kaydetmek için size adımlardan geçer.

DNS bölgesi, belirli bir etki alanına ait DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Bunlara *bağlantılı* sanal ağlar denir. Otomatik kayıt etkinleştirildiğinde, Azure DNS sanal bir makine oluşturulduğunda, IP adresini değiştirdiğinde veya silindiğinde bölge kayıtlarını da güncelleştirir.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Özel bir DNS bölgesi oluşturma
> * Test amaçlı sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

İsterseniz, [Azure PowerShell'i](private-dns-getstarted-powershell.md)kullanarak bu hızlı başlangıcı tamamlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Öncelikle DNS bölgesini içerecek kaynak grubunu oluşturun: 

```azurecli
az group create --name MyAzureResourceGroup --location "East US"
```

## <a name="create-a-private-dns-zone"></a>Özel bir DNS bölgesi oluşturma

Aşağıdaki **örnekmyAzureVNet**adlı bir sanal ağ oluşturur. Daha sonra **MyAzureResourceGroup** kaynak grubunda **private.contoso.com** adında bir DNS bölgesi oluşturur, DNS bölgesini **MyAzureVnet** sanal ağına bağlar ve otomatik kayıt sağlar.

```azurecli
az network vnet create \
  --name myAzureVNet \
  --resource-group MyAzureResourceGroup \
  --location eastus \
  --address-prefix 10.2.0.0/16 \
  --subnet-name backendSubnet \
  --subnet-prefixes 10.2.0.0/24

az network private-dns zone create -g MyAzureResourceGroup \
   -n private.contoso.com

az network private-dns link vnet create -g MyAzureResourceGroup -n MyDNSLink \
   -z private.contoso.com -v myAzureVNet -e true
```

Sadece ad çözümlemesi için bir bölge oluşturmak istiyorsanız (otomatik ana `-e false` bilgisayar adı kaydı yok), parametreyi kullanabilirsiniz.

### <a name="list-dns-private-zones"></a>DNS özel bölgelerini listeleme

DNS bölgelerini numaralandırmak için `az network private-dns zone list` komutunu kullanın. Yardım için bkz. `az network dns zone list --help`.

Kaynak grubu belirtildiğinde yalnızca kaynak grubu içindeki bölgeler listelenir:

```azurecli
az network private-dns zone list \
  -g MyAzureResourceGroup
```

Kaynak grubu atıldığında, abonelikteki tüm bölgeler listelenir:

```azurecli
az network private-dns zone list 
```

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

Şimdi özel DNS bölgenizi test etmek için iki sanal makine oluşturun:

```azurecli
az vm create \
 -n myVM01 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter

az vm create \
 -n myVM02 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter
```

İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network private-dns record-set [record type] add-record` komutunu kullanın. A kaydı ekleme konusunda yardım almak için bkz. `az network private-dns record-set A add-record --help`.

 Aşağıdaki örnek, **MyAzureResourceGroup**kaynak grubunda dns bölgesi **private.contoso.com,** göreli adı **db** ile bir kayıt oluşturur. Kayıt kümesinin tam nitelikli adı **db.private.contoso.com.** Kayıt türü "A" ve IP adresi "10.2.0.4" olarak belirlenmiştir.

```azurecli
az network private-dns record-set a add-record \
  -g MyAzureResourceGroup \
  -z private.contoso.com \
  -n db \
  -a 10.2.0.4
```

### <a name="view-dns-records"></a>DNS kayıtlarını görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu komutu çalıştırın:

```azurecli
az network private-dns record-set list \
  -g MyAzureResourceGroup \
  -z private.contoso.com
```

## <a name="test-the-private-zone"></a>Özel bölgeyi test etme

Artık **private.contoso.com** özel bölgenizin ad çözümlemesi test edebilirsiniz.

### <a name="configure-vms-to-allow-inbound-icmp"></a>Sanal makineleri gelen ICMP paketlerine izin verecek şekilde yapılandırma

Ad çözümlemesini test etmek için ping komutunu kullanabilirsiniz. Bunun için iki sanal makinedeki güvenlik duvarını da gelen ICMP paketlerine izin verecek şekilde yapılandırmanız gerekir.

1. myVM01 adlı sanal makineye bağlanın, yönetici ayrıcalıklarıyla bir Windows PowerShell penceresi açın.
2. Şu komutu çalıştırın:

   ```powershell
   New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
   ```

myVM02 için yineleyin.

### <a name="ping-the-vms-by-name"></a>Sanal makinelere ada göre ping gönderme

1. myVM02 Windows PowerShell komut isteminden otomatik olarak kaydedilen ana bilgisayar adını kullanarak myVM01 adlı makineye ping gönderin:

   ```
   ping myVM01.private.contoso.com
   ```

   Şuna benzer bir çıkışla karşılaşmanız gerekir:

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

2. Şimdi önceden oluşturduğunuz **db** adına ping gönderin:

   ```
   ping db.private.contoso.com
   ```

   Şuna benzer bir çıkışla karşılaşmanız gerekir:

   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Artık ihtiyaç duyulmadığında, bu hızlı başlangıçta oluşturulan kaynakları silmek için **MyAzureResourceGroup** kaynak grubunu silin.

```azurecli
az group delete --name MyAzureResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure DNS Özel Bölgeleri senaryoları](private-dns-scenarios.md)

