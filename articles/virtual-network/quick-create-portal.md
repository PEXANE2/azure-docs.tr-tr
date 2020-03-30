---
title: Sanal ağ oluşturma - quickstart - Azure portalı
titleSuffix: Azure Virtual Network
description: "Hızlı başlangıç: Azure portalında sanal ağ oluşturun. Bu ağlar, VM'ler gibi Azure kaynaklarının birbirleriyle ve internetle güvenli bir şekilde iletişim kurmasına izin tanır."
author: KumudD
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2020
ms.author: kumud
ms.openlocfilehash: 1a1593566b8bdb72f322d64c1ee99c7018f49329
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240075"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma

Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğrenirsiniz. İki sanal makine (VM) dağıtmışsınız. Ardından, VM'ler arasında güvenli bir şekilde iletişim kurar ve Internet'ten VM'lere bağlanırsınız. Sanal ağ, Azure'daki özel ağınızın temel yapı taşıdır. VM'ler gibi Azure kaynaklarının birbirleriyle ve internetle güvenli bir şekilde iletişim kurmasını sağlar.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Azure portalı menüsünden **kaynak oluştur'u**seçin. Azure Marketi'nden Ağ**Sanal ağı'nı** **Networking** > seçin.

1. **Sanal ağ oluştur'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni Oluştur'u**seçin, *myResourceGroup'u*girin, ardından **Tamam'ı**seçin. |
    | Adı | *myVirtualNetwork*girin. |
    | Konum | **Doğu ABD**’yi seçin.|

1. **Sonraki'ni seçin: IP Adresleri**ve **IPv4 adres alanı**için *10.1.0.0/16*girin.

1. **Alt ağ ekle'yi**seçin, ardından Subnet adı için *myVirtualSubnet'i* ve **Subnet** **adres aralığı**için *10.1.0.0/24* girin.

1. **Ekle'yi**seçin, ardından **Gözden Geçir + oluştur'u**seçin. Geri sini varsayılan olarak bırakın ve **Oluştur'u**seçin.

1. **Sanal ağ oluştur'da** **Oluştur'u**seçin.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun:

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

1. Azure portalı menüsünden **kaynak oluştur'u**seçin.

1. Azure Marketi'nden, Windows > **Server 2019 Veri** **Merkezi'ni İşlem'i**seçin. **Oluştur'u**seçin.

1. Sanal makine Oluştur 'da **- Temel bilgiler,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bu kaynak grubunu önceki bölümde oluşturdunuz. |
    | **Örnek ayrıntıları** |  |
    | Sanal makine adı | *myVm1*girin. |
    | Bölge | **Doğu ABD**’yi seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan altyapı **artıklığı gerekli değildir.** |
    | Görüntü | Windows **Server 2019 Datacenter**varsayılan . |
    | Boyut | Varsayılan **standart DS1 v2**. |
    | **Yönetici hesabı** |  |
    | Kullanıcı adı | Seçtiğiniz bir kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **Gelen bağlantı noktası kuralları** |  |
    | Genel gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin ver'i**seçin. |
    | Gelen bağlantı noktalarını seçme | *HTTP (80)* ve *RDP (3389)* girin. |
    | **Tasarruf edin** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan olarak **Hayır.** |

1. **Sonraki'ni seçin: Diskler.**

1. **Sanal makine Oluştur 'da - Diskler**, varsayılanları tutun ve **Sonraki: Ağ'** ı seçin.

1. **Sanal makine Oluştur 'da - Ağ oluşturma**, bu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | **MyVirtualNetwork**varsayılan. |
    | Alt ağ | **MyVirtualSubnet varsayılan (10.1.0.0/24)**. |
    | Genel IP | Varsayılan **(yeni) myVm-ip**. |
    | NIC ağ güvenlik grubu | Varsayılan **temel**. |
    | Genel gelen bağlantı noktaları | Varsayılan **olarak seçili bağlantı noktalarına izin ver.** |
    | Gelen bağlantı noktalarını seçme | Varsayılan **OLARAK HTTP** ve **RDP'dir.**

1. **Sonraki Seçiniz: Yönetim**.

1. **Sanal bir makine oluştur - Yönetim**, **Tanılama depolama hesabı**için Yeni **Oluştur'u**seçin.

1. **Depolama hesabı oluştur'da,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *Myvmstorageaccount*girin. Bu ad alınırsa, benzersiz bir ad oluşturun.|
    | Hesap türü | Varsayılan **Depolama (genel amaçlı v1)**. |
    | Performans | Varsayılan **standart**. |
    | Çoğaltma | Varsayılan **olarak Yerel olarak yedekli depolama (LRS)**. |

1. **Tamam'ı**seçin, ardından **Gözden Geçir + oluştur'u**seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz.

1. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin.

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Başka bir sanal makine oluşturmak için önceki bölümde yordamı yineleyin.

> [!IMPORTANT]
> Sanal **makine adı için,** *myVm2*girin.
>
> **Tanı depolama hesabı**için, bir tane oluşturmak yerine **myvmstorage account'u**seçtiğinizden emin olun.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

*MyVm1*oluşturduktan sonra internete bağlanın.

1. Azure portalında *myVm1'i*arayın ve seçin.

1. **Connect'i**seçin, ardından **RDP'yi.**

    ![Sanal makineye bağlanma](./media/quick-create-portal/connect-to-virtual-machine.png)

    **Bağlan** sayfası açılır.

1. **RDP Dosyasını İndir'i**seçin. Azure uzak masaüstü protokolü (*.rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

1. VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

    > [!NOTE]
    > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullanın,** **daha fazla seçenek** > seçmeniz gerekebilir.

1. **Tamam'ı**seçin.

1. Oturum açtığınızda bir sertifika uyarısı alabilirsiniz. Sertifika uyarısı alırsanız **Evet** veya **Devam et'i**seçin.

1. VM masaüstü göründükten sonra, yerel masaüstünüze geri dönmek için en aza indirin.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. *myVm1*uzak masaüstünde, PowerShell açın.

1. `ping myVm2` yazın.

    Bu çıktıya benzer bir ileti alırsınız:

    ```output
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Internet `ping` Denetim `ping` İleti protokolü (ICMP) kullandığıiçin başarısız olur. Varsayılan olarak, ICMP'nin Windows güvenlik duvarından geçmesine izin verilmez.

1. *myVm2'nin* daha sonraki bir adımda *myVm1* pingini atamasını sağlamak için şu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Bu komut, ICMP'nin Windows güvenlik duvarından içeri sinisin:

1. *myVm1* ile uzak masaüstü bağlantısını kapatın.

1. [İnternet'ten bir sanal makineye bağlanma](#connect-to-a-vm-from-the-internet) bölümündeki adımları tekrar tamamlayın, ancak *myVm2*’ye bağlanın.

1. Bir komut isteminden `ping myvm1` komutunu girin.

    Bu mesaj gibi bir şey geri alırsınız:

    ```output
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    *MyVm1'den*yanıtlar alırsınız, çünkü adım 3'teki *myVm1* VM'deki Windows güvenlik duvarından ICMP'ye izin verdiniz.

1. *myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternetten bir VM'ye bağlandınız ve iki VM arasında güvenli bir şekilde iletişim kurdunuz.

Sanal ağı ve VM'leri kullanmayı bitirdiğinizde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. *MyResourceGroup'u*arayın ve seçin.

1. **Kaynak grubunu sil**'i seçin.

1. **KAYNAK GRUBU ADINI YAZIN** ve **Sil'i**seçmek için *MyResourceGroup'u* girin.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağ ayarları hakkında daha fazla bilgi edinmek için [bkz.](manage-virtual-network.md)

Varsayılan olarak, Azure VM'ler arasında güvenli iletişim sağlar. Azure, yalnızca Internet'ten Windows VM'lere gelen uzak masaüstü bağlantılarına izin verir. VM ağ iletişimi türleri hakkında daha fazla bilgi edinmek için filtre [ağ trafiği](tutorial-filter-network-traffic.md)bölümüne bakın.
