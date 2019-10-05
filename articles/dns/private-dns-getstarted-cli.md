---
title: Hızlı başlangıç-Azure CLı kullanarak Azure özel DNS bölgesi oluşturma
description: Bu hızlı başlangıçta, Azure DNS bir özel DNS bölgesi ve kaydı oluşturup test edersiniz. Bu, Azure CLı kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmek için adım adım kılavuzudur.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 10/05/2019
ms.author: victorh
ms.openlocfilehash: 6f5f2263736e1317ca40e6c27a909be2aa106270
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960438"
---
# <a name="quickstart-create-an-azure-private-dns-zone-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure özel DNS bölgesi oluşturma

Bu hızlı başlangıç, Azure CLı kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturma adımlarında size yol gösterir.

DNS bölgesi, belirli bir etki alanı için DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS barındırılmaya başlamak için, bu etki alanı adı için bir DNS bölgesi oluşturmanız gerekir. Etki alanınız için her bir DNS kaydı daha sonra bu DNS bölgesi içinde oluşturulur. Sanal ağınıza özel bir DNS bölgesi yayımlamak için, bölgedeki kayıtları çözümlemek için izin verilen sanal ağların listesini belirtirsiniz.  Bunlara *bağlı* sanal ağlar denir. Oto kayıt etkinleştirildiğinde Azure DNS, bir sanal makine oluşturulduğunda bölge kayıtlarını da güncelleştirir, ' IP adresini değiştirir veya silinir.

Bu hızlı başlangıçta şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Özel bir DNS bölgesi oluşturma
> * Test sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

İsterseniz [Azure PowerShell](private-dns-getstarted-powershell.md)kullanarak bu hızlı başlangıcı tamamlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Kaynak grubu oluşturma

İlk olarak, DNS bölgesini içerecek bir kaynak grubu oluşturun: 

```azurecli
az group create --name MyAzureResourceGroup --location "East US"
```

## <a name="create-a-private-dns-zone"></a>Özel bir DNS bölgesi oluşturma

Aşağıdaki örnek, **Myazurevnet**adlı bir sanal ağ oluşturur. Daha sonra, **MyAzureResourceGroup** kaynak grubunda **PRIVATE.contoso.com** adlı bir DNS bölgesi oluşturur, DNS bölgesini **myazurevnet** sanal ağına bağlar ve otomatik kaydı sağlar.

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

Yalnızca ad çözümlemesi için bir bölge oluşturmak istiyorsanız (otomatik konak adı kaydı olmadan) `-e false` parametresini kullanabilirsiniz.

### <a name="list-dns-private-zones"></a>DNS özel bölgelerini listeleme

DNS bölgelerini numaralandırmak için `az network private-dns zone list` kullanın. Yardım için bkz. `az network dns zone list --help`.

Kaynak grubu belirtildiğinde yalnızca kaynak grubu içindeki bölgeler listelenir:

```azurecli
az network private-dns zone list \
  -g MyAzureResourceGroup
```

Kaynak grubunun atlanması, abonelikteki tüm bölgeleri listeler:

```azurecli
az network private-dns zone list 
```

## <a name="create-the-test-virtual-machines"></a>Test sanal makineleri oluşturma

Şimdi, özel DNS bölgenizi sınayabilmeniz için iki sanal makine oluşturun:

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

Bu işlem birkaç dakika sürer.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network private-dns record-set [record type] add-record` komutunu kullanın. Örneğin, bir kayıt eklemeye yönelik yardım için bkz. `az network private-dns record-set A add-record --help`.

 Aşağıdaki örnek, **Private.contoso.com**kaynak GRUBUNDAKI **MyAzureResourceGroup**DNS bölgesinde göreli ad **DB** ile bir kayıt oluşturur. Kayıt kümesinin tam nitelikli adı **DB.Private.contoso.com**' dir. Kayıt türü, "10.2.0.4" IP adresine sahip "A" dır.

```azurecli
az network private-dns record-set a add-record \
  -g MyAzureResourceGroup \
  -z private.contoso.com \
  -n db \
  -a 10.2.0.4
```

### <a name="view-dns-records"></a>DNS kayıtlarını görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şunu çalıştırın:

```azurecli
az network private-dns record-set list \
  -g MyAzureResourceGroup \
  -z private.contoso.com
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
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Tüm kaynakları Sil

Artık gerekli değilse, bu hızlı başlangıçta oluşturulan kaynakları silmek için **MyAzureResourceGroup** kaynak grubunu silin.

```azurecli
az group delete --name MyAzureResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure DNS Özel Bölgeleri senaryolar](private-dns-scenarios.md)

