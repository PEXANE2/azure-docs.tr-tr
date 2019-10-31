---
title: Sanal ağ oluşturma-hızlı başlangıç-Azure portal
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Sanal ağ, sanal makineler gibi Azure kaynaklarının birbirleriyle ve internet ile güvenli bir şekilde iletişim kurmasına olanak tanır
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can securely communicate with each other and with the internet.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 07/08/2019
ms.author: kumud
ms.openlocfilehash: f95174efbffedd09946f5910a39285e7bec062c1
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73161488"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma

Bir sanal ağ, Azure 'daki özel ağınız için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının birbirleriyle ve internet ile güvenli bir şekilde iletişim kurmasına olanak sağlar. Bu hızlı başlangıçta, Azure portal kullanarak bir sanal ağ oluşturmayı öğreneceksiniz. Daha sonra, iki VM 'yi sanal ağa dağıtabilir, iki VM arasında güvenli bir şekilde iletişim kurabilir ve internet 'ten VM 'lere bağlanabilirsiniz.


Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturun

1. Ekranın sol üst kısmında, **kaynak oluştur** > **ağ** > **sanal ağ**' ı seçin.

1. **Sanal ağ oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *MyVirtualNetwork*girin. |
    | Adres alanı | *10.1.0.0/16*girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni oluştur**' u seçin, *myresourcegroup*yazın ve ardından **Tamam**' ı seçin. |
    | Konum | **Doğu ABD**’yi seçin.|
    | Alt ağ adı | *Myvirtualsubnet*girin. |
    | Alt Ağ - Adres aralığı | *10.1.0.0/24*girin. |

1. Rest 'i varsayılan olarak bırakın ve **Oluştur**' u seçin.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun:

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

1. Ekranın sol üst kısmında **Windows Server 2019 Datacenter** > **Işlem** > **kaynak oluştur** ' u seçin.

1. **Sanal makine oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRıNTıLARı** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz. |
    | **ÖRNEK AYRıNTıLARı** |  |
    | Sanal makine adı | *MyVm1*girin. |
    | Bölge | **Doğu ABD**’yi seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan **altyapı yedekliliği gerekli değildir**. |
    | Görüntü | Varsayılan **Windows Server 2019 veri merkezini**bırakın. |
    | Boyut | Varsayılan **Standart DS1 v2**' i bırakın. |
    | **YÖNETICI HESABı** |  |
    | Kullanıcı adı | Seçmekten bir Kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTı NOKTASı KURALLARı** |  |
    | Ortak gelen bağlantı noktaları | Varsayılanı **yok**olarak bırakın. |
    | **TASARRUF EDIN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan **Hayır**olarak bırakın. |

1. **İleri ' yi seçin: diskler**.

1. **Sanal makine oluşturma-diskler**' de, varsayılan değerleri bırakın ve **İleri ' yi seçin: ağ**.

1. **Sanal makine oluşturma-ağ oluşturma**bölümünde şu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan **myVirtualNetwork**bırakın. |
    | Alt ağ | Varsayılan **Myvirtualsubnet (10.1.0.0/24)** olarak bırakın. |
    | Genel IP | Varsayılan **(yeni) myVm-ip**' i bırakın. |
    | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına Izin ver**' i seçin. |
    | Gelen bağlantı noktalarını seçin | **Http** ve **RDP**' yi seçin.

1. Ileri 'yi seçin **: yönetim**.

1. **Sanal makine yönetimi oluştur**bölümünde, **Tanılama depolama hesabı**için **Yeni oluştur**' u seçin.

1. **Depolama hesabı oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *Myvmstorageaccount*girin. Bu ad alındıysanız, benzersiz bir ad oluşturun.|
    | Hesap türü | Varsayılan **depolama alanını (genel amaçlı v1)** bırakın. |
    | Performans | Varsayılan **Standart**bırakın. |
    | Çoğaltma | Varsayılan **yerel olarak yedekli depolamayı (LRS)** bırakın. |

1. **Tamam**’ı seçin

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

1. Yukarıdaki 1. ve 9. adımları uygulayın.

    > [!NOTE]
    > 2\. adımda, **sanal makine adı**için *myVm2*girin.
    >
    > Adım 7 ' de, **Tanılama depolama hesabı**için **myvmstorageaccount**' ı seçtiğinizden emin olun.

1. **İncele ve oluştur**’u seçin. **Gözden geçir + oluştur** sayfasına götürülürsünüz ve Azure yapılandırmanızı doğrular.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

*MyVm1*oluşturduktan sonra internet 'e bağlanın.

1. Portalın arama çubuğunda *myVm1*girin.

1. **Bağlan** düğmesini seçin.

    ![Sanal makineye bağlanma](./media/quick-create-portal/connect-to-virtual-machine.png)

    **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü ( *. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen *. rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

        > [!NOTE]
        > VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için **farklı bir hesap kullanmak** > **daha fazla seçenek** belirlemeniz gerekebilir.

1. **Tamam**’ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. *MyVm1*uzak masaüstünde PowerShell ' i açın.

1. `ping myVm2` yazın.

    Şuna benzer bir ileti alacaksınız:

    ```powershell
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

    ```powershell
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

Sanal ağ ve VM 'Leri kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **arama** kutusuna *myresourcegroup* yazın ve arama sonuçlarından **myresourcegroup** öğesini seçin.

1. **Kaynak grubunu sil**'i seçin.

1. **Kaynak grubu adını yazın** ve **Sil**' i seçmek için *myresourcegroup* girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki VM oluşturdunuz. İnternet 'ten bir sanal makineye bağlanırsınız ve iki VM arasında güvenli bir şekilde iletişim kurdınız. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Azure, varsayılan olarak VM 'Ler arasında sınırsız güvenli iletişim sağlar. Buna karşılık, yalnızca Internet 'ten gelen Windows VM 'lerine gelen Uzak Masaüstü bağlantılarına izin verir. Farklı türlerde VM ağı iletişimleri yapılandırma hakkında daha fazla bilgi edinmek için [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğreticisine gidin.
