---
title: Sanal ağ oluşturma-hızlı başlangıç-Azure portal
titleSuffix: Azure Virtual Network
description: "Hızlı başlangıç: Azure portal bir sanal ağ oluşturun. Bu ağlar VM 'Ler gibi Azure kaynaklarının birbirleriyle ve internet ile güvenli bir şekilde iletişim kurmasına olanak tanır."
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
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128490"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma

Bu hızlı başlangıçta, Azure portal kullanarak bir sanal ağ oluşturmayı öğreneceksiniz. İki sanal makine (VM) dağıtırsınız. Daha sonra, VM 'Ler arasında güvenli bir şekilde iletişim kurabilir ve internet 'ten VM 'lere bağlanırsınız. Bir sanal ağ, Azure 'daki özel ağınız için temel yapı taşdır. VM 'Ler gibi Azure kaynaklarının birbirleriyle ve internet ile güvenli bir şekilde iletişim kurmasına olanak sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Azure portal menüsünde **kaynak oluştur**' u seçin. Azure Marketi 'nden **ağ** > **sanal ağ**' ı seçin.

1. **Sanal ağ oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni oluştur**' u seçin, *myresourcegroup*yazın ve ardından **Tamam**' ı seçin. |
    | Adı | *MyVirtualNetwork*girin. |
    | Konum | **Doğu ABD**’yi seçin.|

1. Ileri ' yi seçin **: IP adresleri**ve **IPv4 adres alanı**için *10.1.0.0/16*girin.

1. Alt ağ **Ekle**' yi seçin, ardından alt ağ **adı** için *myvirtualsubnet* ve **alt ağ adres aralığı**için *10.1.0.0/24* girin.

1. **Ekle**' yi ve ardından **gözden geçir + oluştur**' u seçin. Rest 'i varsayılan olarak bırakın ve **Oluştur**' u seçin.

1. **Sanal ağ oluştur**' da **Oluştur**' u seçin.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun:

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

1. Azure portal menüsünde **kaynak oluştur**' u seçin.

1. Azure Marketi 'nden **Windows Server 2019 Datacenter** > **işlem** ' ı seçin. **Oluştur**’u seçin.

1. **Sanal makine oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu kaynak grubunu önceki bölümde oluşturdunuz. |
    | **Örnek ayrıntıları** |  |
    | Sanal makine adı | *MyVm1*girin. |
    | Bölge | **Doğu ABD**’yi seçin. |
    | Kullanılabilirlik seçenekleri | **Altyapı yedekliliği**için varsayılan değer gerekmez. |
    | Görüntü | Varsayılan olarak **Windows Server 2019 Datacenter**. |
    | Boyut | **Standart DS1 v2**için varsayılan. |
    | **Yönetici hesabı** |  |
    | Kullanıcı adı | Seçmekten bir Kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **Gelen bağlantı noktası kuralları** |  |
    | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına Izin ver**' i seçin. |
    | Gelen bağlantı noktalarını seçin | *Http (80)* ve *RDP (3389)* girin. |
    | **Tasarruf edin** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan olarak **Hayır**. |

1. **İleri ' yi seçin: diskler**.

1. **Sanal makine oluşturma-diskler**' de, Varsayılanları tutun ve Ileri ' **yi seçin: ağ**.

1. **Sanal makine oluşturma-ağ oluşturma**bölümünde şu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan olarak **myVirtualNetwork**. |
    | Alt ağ | Varsayılan olarak **Myvirtualsubnet (10.1.0.0/24)** . |
    | Genel IP | Varsayılan değer **(yeni) myVm-ip**. |
    | NIC ağ güvenlik grubu | Varsayılan olarak **temel**. |
    | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin**vermek için varsayılan. |
    | Gelen bağlantı noktalarını seçin | Varsayılan olarak **http** ve **RDP**.

1. Ileri 'yi seçin **: yönetim**.

1. **Sanal makine yönetimi oluştur**bölümünde, **Tanılama depolama hesabı**için **Yeni oluştur**' u seçin.

1. **Depolama hesabı oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *Myvmstorageaccount*girin. Bu ad alındıysanız, benzersiz bir ad oluşturun.|
    | Hesap türü | Varsayılan **depolama alanı (genel amaçlı v1)** . |
    | Performans | Varsayılan değer **Standart**. |
    | Çoğaltma | Varsayılan **olarak yerel olarak yedekli depolama (LRS)** . |

1. **Tamam**' ı ve ardından **gözden geçir + oluştur**' u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Başka bir sanal makine oluşturmak için önceki bölümde bulunan yordamı tekrarlayın.

> [!IMPORTANT]
> **Sanal makine adı**için *myVm2*girin.
>
> **Tanılama depolama hesabı**için, bir tane oluşturmak yerine **myvmstorageaccount**' ı seçtiğinizden emin olun.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

*MyVm1*oluşturduktan sonra internet 'e bağlanın.

1. Azure portal için arama yapın ve *myVm1*seçin.

1. **Bağlan**' ı ve ardından **RDP**' yi seçin.

    ![Sanal makineye bağlanma](./media/quick-create-portal/connect-to-virtual-machine.png)

    **Bağlan** sayfası açılır.

1. **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü ( *. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

1. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

    > [!NOTE]
    > VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için **farklı bir hesap kullanmak** > **daha fazla seçenek** belirlemeniz gerekebilir.

1. **Tamam**’ı seçin.

1. Oturum açtığınızda bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. *MyVm1*uzak masaüstünde PowerShell ' i açın.

1. `ping myVm2` yazın.

    Bu çıktıya benzer bir ileti alacaksınız:

    ```output
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    `ping` Internet Denetim Iletisi Protokolü 'Nü (ıCMP) kullandığından `ping` başarısız olur. Varsayılan olarak, Windows Güvenlik Duvarı üzerinden ıCMP 'ye izin verilmez.

1. *MyVm2* 'in daha sonraki bir *adımda ping yapmasına* izin vermek için şu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Bu komut Windows Güvenlik Duvarı üzerinden gelen ıCMP 'ye izin verir:

1. *myVm1* ile uzak masaüstü bağlantısını kapatın.

1. [İnternet'ten bir sanal makineye bağlanma](#connect-to-a-vm-from-the-internet) bölümündeki adımları tekrar tamamlayın, ancak *myVm2*’ye bağlanın.

1. Bir komut isteminden `ping myvm1` komutunu girin.

    Şu ileti gibi bir şey geri alacaksınız:

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

    Adım 3 ' teki *myVm1* VM 'de Windows Güvenlik DUVARı üzerinden ICMP 'ye Izin verildiğinden *myVm1*adresinden yanıt alırsınız.

1. *myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet 'ten bir sanal makineye bağlanırsınız ve iki VM arasında güvenli bir şekilde iletişim kurdınız.

Sanal ağ ve VM 'Leri kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. *Myresourcegroup*öğesini arayın ve seçin.

1. **Kaynak grubunu sil**'i seçin.

1. **Kaynak grubu adını yazın** ve **Sil**' i seçmek için *myresourcegroup* girin.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağ ayarları hakkında daha fazla bilgi edinmek için bkz. [sanal ağ oluşturma, değiştirme veya silme](manage-virtual-network.md).

Azure, varsayılan olarak VM 'Ler arasında güvenli iletişim sağlar. Azure, yalnızca Internet 'ten gelen Windows VM 'lerine gelen Uzak Masaüstü bağlantılarına izin verir. VM ağı iletişimleri türleri hakkında daha fazla bilgi edinmek için bkz. [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md).
