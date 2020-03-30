---
title: Quickstart - Azure portalını kullanarak azure özel DNS bölgesi oluşturma
description: Bu hızlı başlangıçta, azure DNS'de özel bir DNS bölgesi oluşturup sınayıp kaydedebilirsiniz. Bu, Azure portalını kullanarak ilk özel DNS bölgenizi oluşturmak ve yönetmek ve kaydetmek için adım adım bir kılavuzdur.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: quickstart
ms.date: 10/11/2019
ms.author: rohink
ms.openlocfilehash: 52bf9e061eb57c7ce6ea698b7468b5ba5e11b4e8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78244963"
---
# <a name="quickstart-create-an-azure-private-dns-zone-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak bir Azure özel DNS bölgesi oluşturma

Bu hızlı başlangıç, ilk özel DNS bölgenizi oluşturmak ve Azure portalını kullanarak kayıt yapmak için size adımlardan geçer.

DNS bölgesi, belirli bir etki alanına ait DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Bunlara *bağlantılı* sanal ağlar denir. Otomatik kayıt etkinleştirildiğinde, Azure DNS sanal bir makine oluşturulduğunda, IP adresini değiştirdiğinde veya silindiğinde bölge kayıtlarını da güncelleştirir.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Özel bir DNS bölgesi oluşturma
> * Sanal ağ oluşturma
> * Sanal ağı birbirine bağla
> * Test amaçlı sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

İsterseniz, [Azure PowerShell](private-dns-getstarted-powershell.md) veya [Azure CLI](private-dns-getstarted-cli.md)kullanarak bu hızlı başlatmayı tamamlayabilirsiniz.

## <a name="create-a-private-dns-zone"></a>Özel bir DNS bölgesi oluşturma

Aşağıdaki örnek, **MyAzureResourceGroup**adlı bir kaynak grubunda **private.contoso.com** adlı bir DNS bölgesi oluşturur.

DNS bölgesi, bir etki alanı için DNS girişlerini içerir. Etki alanınızı Azure DNS'de barındırmaya başlamak için, bu etki alanı adı için bir DNS bölgesi oluşturursunuz.

![Özel DNS bölgeleri arama](media/private-dns-portal/search-private-dns.png)

1. Portal arama çubuğunda, arama metin kutusuna **özel dns bölgeleri** yazın ve **Enter**tuşuna basın.
1. **Özel DNS bölgesini**seçin.
2. **Özel dns bölgesi oluştur'u**seçin.

1. Özel **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri yazın veya seçin:

   - **Kaynak grubu**: **Yeni Oluştur'u**seçin, *MyAzureResourceGroup'u*girin ve **Tamam'ı**seçin. Kaynak grubu adı Azure aboneliği nde benzersiz olmalıdır.
   -  **Ad**: Bu örnek için *private.contoso.com* yazın.
1. **Kaynak grubu konumu** **için, Batı Orta ABD'yi**seçin.

1. **Gözden Geçir + Oluştur'u**seçin.

1. **Oluştur'u**seçin.

Bölgenin oluşturulması birkaç dakika sürebilir.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | MyAzureResourceGroup (Varolan kaynak grubunu seçin) |
| **\<sanal ağ adı>** | MyAzureVNet          |
| **\<bölge adı>**          | Orta Batı ABD      |
| **\<IPv4 adres-boşluk>**   | 10.2.0.0\16          |
| **\<subnet-name>**          | MyAzureSubnet        |
| **\<alt net-adres aralığı>** | 10.2.0.0\24          |


[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="link-the-virtual-network"></a>Sanal ağı birbirine bağla

Özel DNS bölgesini sanal ağa bağlamak için sanal ağ bağlantısı oluşturursunuz.

![Sanal ağ bağlantısı ekleme](media/private-dns-portal/dns-add-virtual-network-link.png)

1. **MyAzureResourceGroup** kaynak grubunu açın ve özel bölge **private.contoso.com** seçin.
2. Sol bölmede Sanal **ağ bağlantılarını**seçin.
3. **Ekle'yi**seçin.
4. **Bağlantı adı**için **myLink** yazın.
5. **Sanal ağ**için **myAzureVNet'i**seçin.
6. Otomatik **kaydı etkinleştir onay** kutusunu seçin.
7. **Tamam'ı**seçin.

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

Şimdi özel DNS bölgenizi test etmek için iki sanal makine oluşturun:

1. Sol üstteki portal sayfasında **kaynak oluştur'u**seçin ve ardından **Windows Server 2016 Datacenter'ı**seçin.
1. Kaynak grubu için **MyAzureResourceGroup'u** seçin.
1. **MyVM01** yazın - sanal makinenin adı için.
1. **Bölge**için **Batı Orta ABD'yi** seçin.
1. Yönetici kullanıcı adı için bir ad girin.
2. Bir parola girin ve parolayı onaylayın.
5. **Ortak gelen bağlantı noktaları**için, **seçili bağlantı noktalarına izin ver'i**seçin ve ardından gelen bağlantı **noktalarını seçmek**için **RDP (3389)** seçeneğini belirleyin.
10. Sayfanın diğer varsayılanlarını kabul edin ve sonra **İleri:Diskler >. **
11. **Diskler** sayfasındavarsayılan ları kabul edin, ardından **İleri: Ağ >' **yi tıklatın.
1. **MyAzureVNet'in** sanal ağ için seçildiğinden emin olun.
1. Sayfanın diğer varsayılanlarını kabul edin ve sonra **İleri: Yönetim >' **yi tıklatın.
2. **Önyükleme tanılama için,** **Kapalı'yı**seçin, diğer varsayılanları kabul edin ve ardından **Gözden Geçir + oluştur'u**seçin.
1. Ayarları gözden geçirin ve ardından **Oluştur'u**tıklatın.

Bu adımları tekrarlayın ve **myVM02**adlı başka bir sanal makine oluşturun.

Her iki sanal makinenin tamamlanması birkaç dakika sürer.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

 Aşağıdaki örnek, **MyAzureResourceGroup**kaynak grubunda dns bölgesi **private.contoso.com,** göreli adı **db** ile bir kayıt oluşturur. Kayıt kümesinin tam nitelikli adı **db.private.contoso.com.** Kayıt türü **myVM01**IP adresi ile "A".

1. **MyAzureResourceGroup** kaynak grubunu açın ve özel bölge **private.contoso.com** seçin.
2. **+ Kayıt kümesi**’ni seçin.
3. **Ad**için , **db**yazın .
4. **IP Adresi**için, **myVM01**için gördüğünüz IP adresini yazın. Sanal makine başlatıldığında bu otomatik olarak kaydedilmiş olmalıdır.
5. **Tamam'ı**seçin.

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


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure DNS Özel Bölgeleri senaryoları](private-dns-scenarios.md)

