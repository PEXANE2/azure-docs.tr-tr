---
title: Rota ağ trafiği - öğretici - Azure portalı
titlesuffix: Azure Virtual Network
description: Bu öğreticide, Azure portalını kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme hakkında bilgi edineceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2020
ms.author: kumud
ms.openlocfilehash: a565aba12f1b10f215d8f6cc7fc0b7247a0441d2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80066332"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme

Azure, varsayılan olarak sanal ağdaki tüm alt ağlar arasındaki trafiği yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Özel rotalar, örneğin, bir ağ sanal cihaz (NVA) üzerinden alt ağlar arasındaki trafiği yönlendirmek istediğinizde yararlıdır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Trafiği yönlendiren bir NVA oluşturma
> * Yönlendirme tablosu oluşturma
> * Yönlendirme oluşturma
> * Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
> * Sanal makineleri (VM) farklı alt ağlara dağıtma
> * NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Bu öğretici [Azure portalını](https://portal.azure.com)kullanır. [Azure CLI](tutorial-create-route-table-cli.md) veya [Azure PowerShell'i](tutorial-create-route-table-powershell.md)de kullanabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-nva"></a>NVA oluşturma

Ağ sanal cihazları (NV'ler), yönlendirme ve güvenlik duvarı optimizasyonu gibi ağ işlevlerine yardımcı olan sanal makinelerdir. Bu öğretici, **Windows Server 2016 Datacenter'ı**kullandığınızı varsayar. İsterseniz farklı bir işletim sistemi seçebilirsiniz.

1. Azure [portalı](https://portal.azure.com) menüsünde veya **Ana** sayfadan **kaynak oluştur'u**seçin.

1. **Security** > **Windows Server 2016 Datacenter'ı**seçin.

    ![Windows Server 2016 Datacenter, VM, Azure portalı oluşturma](./media/tutorial-create-route-table-portal/vm-ws2016-datacenter.png)

1. **Temel Bilgiler**altında sanal **makine oluştur** sayfasında şu bilgileri girin veya seçin:

    | Section | Ayar | Eylem |
    | ------- | ------- | ----- |
    | **Proje ayrıntıları** | Abonelik | Aboneliğinizi seçin. |
    | | Kaynak grubu | **Yeni Oluştur'u**seçin , *myResourceGroup'u*girin ve **Tamam'ı**seçin. |
    | **Örnek ayrıntıları** | Sanal makine adı | *myVmNva*girin. |
    | | Bölge | Seçin **(ABD) Doğu ABD**. |
    | | Kullanılabilirlik seçenekleri | **Altyapı artıklığı gerekmemektedir'** seçeneğini belirleyin. |
    | | Görüntü | **Windows Server 2016 Datacenter'ı**seçin. |
    | | Boyut | Varsayılan, **Standart DS1 v2**tutun. |
    | **Yönetici hesabı** | Kullanıcı adı | Seçtiğiniz bir kullanıcı adını girin. |
    | | Parola | En az 12 karakter uzunluğunda ve [tanımlanan karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm)karşılaması gereken seçtiğiniz parolayı girin. |
    | | Parolayı Onayla | Parolayı yeniden girin. |
    | **Gelen bağlantı noktası kuralları** | Genel gelen bağlantı noktaları | **Hiçbirini**seç. |
    | **Tasarruf edin** | Zaten Windows Server lisansınız var mı? | **Hayır'ı**seçin. |

    ![Temel bilgiler, Sanal makine oluşturma, Azure portalı](./media/tutorial-create-route-table-portal/basics-create-virtual-machine.png)

    Sonra **İleri seçin : Diskler >**.

1. **Diskler**altında, ihtiyaçlarınız için doğru ayarları seçin ve sonra Sonraki seçin **: Ağ >. **

1. **Ağ**Altında:

    1. **Sanal ağ**için **yeni oluştur'u**seçin.
    
    1. Sanal **ağ** oluştur iletişim kutusunda, **Ad**altında *myVirtualNetwork'e*girin.

    1. **Adres alanında,** varolan adres aralığını *10.0.0.0/16*ile değiştirin.

    1. **Alt ağlarda,** varolan alt ağı silmek için **Sil** simgesini seçin ve ardından **Aşağıdaki Altnet adı** ve Adres **aralığı**kombinasyonlarını girin. Geçerli bir ad ve aralık girilen bir kez, altında yeni bir boş satır görüntülenir.

        | Alt ağ adı | Adres aralığı |
        | ----------- | ------------- |
        | *Kamu* | *10.0.0.0/24* |
        | *Özel* | *10.0.1.0/24* |
        | *DMZ* | *10.0.2.0/24* |

    1. İletişim kutusundan çıkmak için **Tamam'ı** seçin.

    1. **Subnet'te** **DMZ (10.0.2.0/24)** seçeneğini belirleyin.

    1. Bu VM internet üzerinden bağlanmaz beri **Kamu IP**, **Hiçbiri**seçin.

    1. Sonraki ni Seçin **: Yönetim >**.

1. **Yönetim**Altında :

    1. **Tanılama depolama hesabında**Yeni **Oluştur'u**seçin.
    
    1. Depolama **hesabı oluştur** iletişim kutusuna şu bilgileri girin veya seçin:

        | Ayar | Değer |
        | ------- | ----- |
        | Adı | *mynvastoragehesabı* |
        | Hesap türü | **Depolama (genel amaçlı v1)** |
        | Performans | **Standart** |
        | Çoğaltma | **Yerel olarak yedekli depolama (LRS)** |
    
    1. İletişim kutusundan çıkmak için **Tamam'ı** seçin.

    1. **İncele ve oluştur**’u seçin. Gözden Geçir + **oluşturma** sayfasına alınırsınız ve Azure yapılandırmanızı doğrular.

1. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin.

    Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM'yi oluşturmayı bitirene kadar bekleyin. **Dağıtımınız devam ediyor** sayfası dağıtım ayrıntılarını gösterir.

1. VM'niz hazır olduğunda **kaynağa git'i**seçin.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

1. Azure [portalı](https://portal.azure.com) menüsünde veya **Ana** sayfadan **kaynak oluştur'u**seçin.

2. Arama kutusuna *Rota tablosunu*girin. Arama sonuçlarında **Rota tablosu** göründüğünde, onu seçin.

3. Rota **tablosu** sayfasında **Oluştur'u**seçin.

4. **Rota Oluştur tablosunda,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *myRouteTablePublic* |
    | Abonelik | Aboneliğiniz |
    | Kaynak grubu | **myResourceGroup** |
    | Konum | **(Abd) Doğu ABD** |
    | Sanal ağ ağ geçidi rotası yayılımı | **Etkin** |

    ![Rota tablosu oluşturma, Azure portalı](./media/tutorial-create-route-table-portal/create-route-table.png)

5. **Oluştur'u**seçin.

## <a name="create-a-route"></a>Yönlendirme oluşturma

1. Rota tablonuzu yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablonuzun adını seçin **(myRouteTablePublic).**

1. **Rotalar** > **Ekle'yi**Seçin.

    ![Rota ekleme, rota tablosu, Azure portalı](./media/tutorial-create-route-table-portal/add-route.png)

1. **Rota**Ekle'de, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Yönlendirme adı | *ToPrivateSubnet* |
    | Adres ön eki | *10.0.1.0/24* (daha önce oluşturulan *Özel* alt netin adres aralığı) |
    | Sonraki atlama türü | **Sanal gereç** |
    | Sonraki atlama adresi | *10.0.2.4* *(DMZ* alt netinin adres aralığındaki bir adres) |

1. **Tamam'ı**seçin.

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

1. Sanal ağınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Sanal ağları**arayın ve seçin.

1. Sanal ağınızın **(myVirtualNetwork)** adını seçin.

1. Sanal ağın menü çubuğunda **Alt Ağlar'ı**seçin.

1. Sanal ağın alt net listesinde **Genel'i**seçin.

1. **Rota tablosunda,** oluşturduğunuz rota tablosunu **(myRouteTablePublic)** seçin ve ardından rota tablonuzu *Ortak* alt ağla ilişkilendirmek için **Kaydet'i** seçin.

    ![Ortak rota tablosu, alt net listesi, sanal ağ, Azure portalı](./media/tutorial-create-route-table-portal/associate-route-table.png)

## <a name="turn-on-ip-forwarding"></a>IP yönlendirmeyi açma

Ardından, yeni NVA sanal makineniz *myVmNva*için IP yönlendirmeyi açın. Azure ağ *trafiğini myVmNva'ya*gönderdiğinde , trafik farklı bir IP adresine yönelikse, IP yönlendirme trafiği doğru konuma gönderir.

1. VM'nizi yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

1. VM **(myVmNva)** adını seçin.

1. NVA sanal makinenizin menü çubuğunda **Ağ'ı**seçin.

1. **myvmnva123'ü**seçin. Bu, Azure'un VM'iniz için oluşturduğu ağ arabirimidir. Azure, benzersiz bir ad sağlamak için sayılar ekler.

    ![Ağ, ağ sanal cihaz (NVA) sanal makine (VM), Azure portalı](./media/tutorial-create-route-table-portal/virtual-machine-networking.png)

1. Ağ arabirimi menü çubuğunda **IP yapılandırmalarını**seçin.

1. IP **yapılandırmaları** sayfasında IP **yönlendirmeyi** **Enabled'e**ayarlayın ve **Kaydet'i**seçin.

    ![IP iletme, IP yapılandırmaları, ağ arabirimi, ağ sanal cihaz (NVA) sanal makine (VM), Azure portalı etkinleştirme](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png)

## <a name="create-public-and-private-virtual-machines"></a>Genel ve özel sanal makineleri oluşturma

Sanal ağda ortak bir VM ve özel bir VM oluşturun. Daha sonra, Azure'un *Genel* alt ağ trafiğini NVA üzerinden *Özel* alt ağa yönlendiririni görmek için bunları kullanırsınız.

Ortak VM'yi ve özel VM'yi oluşturmak için, daha önce [NVA oluşturma](#create-an-nva) adımlarını izleyin. Dağıtımın tamamlanmasını veya VM kaynağına gitmesini beklemeniz gerekmez. Aşağıda açıklandığı gibi dışında, aynı ayarların çoğunu kullanırsınız.

Ortak veya özel VM oluşturmak için **Oluştur'u** seçmeden önce, farklı olması gereken değerleri gösteren aşağıdaki iki alt bölüme[(Genel VM](#public-vm) ve [Özel VM)](#private-vm)gidin. Azure her iki VM'yi de dağıtmayı tamamladıktan sonra bir sonraki bölüme[(NVA üzerinden yönlendirme trafiği)](#route-traffic-through-an-nva)devam edebilirsiniz.

### <a name="public-vm"></a>Kamu VM

| Tab | Ayar | Değer |
| --- | ------- | ----- |
| Temel Bilgiler | Kaynak grubu | **myResourceGroup** |
| | Sanal makine adı | *myVmPublic* |
| | Genel gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin ver** |
| | Gelen bağlantı noktalarını seçme | **HTTP** ve **RDP** |
| Ağ Oluşturma | Sanal ağ | **myVirtualNetwork** |
| | Alt ağ | **Genel (10.0.0.0/24)** |
| | Genel IP adresi | Varsayılan değer |
| Yönetim | Tanılama depolama hesabı | **mynvastoragehesabı** |

### <a name="private-vm"></a>Özel VM

| Tab | Ayar | Değer |
| --- | ------- | ----- |
| Temel Bilgiler | Kaynak grubu | **myResourceGroup** |
| | Sanal makine adı | *myVmPrivate* |
| | Genel gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin ver** |
| | Gelen bağlantı noktalarını seçme | **HTTP** ve **RDP** |
| Ağ Oluşturma | Sanal ağ | **myVirtualNetwork** |
| | Alt ağ | **Özel (10.0.1.0/24)** |
| | Genel IP adresi | Varsayılan değer |
| Yönetim | Tanılama depolama hesabı | **mynvastoragehesabı** |

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

### <a name="sign-in-to-myvmprivate-over-remote-desktop"></a>Uzak masaüstü üzerinden myVmPrivate'da oturum açın

1. Özel VM'nizi yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

1. Özel VM **(myVmPrivate)** adını seçin.

1. VM menü çubuğunda, özel VM'ye uzak bir masaüstü bağlantısı oluşturmak için **Bağlan'ı** seçin.

1. **RDP ile Bağlan** sayfasında **RDP Dosyasını İndir'i**seçin. Azure uzak masaüstü protokolü (*.rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen *.rdp* dosyasını açın. İstendiğinde **Bağlan**’ı seçin. **Daha fazla seçenek** > seçin**Farklı bir hesap kullanın**ve ardından özel VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

1. **Tamam'ı**seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alırsanız, VM'ye bağlanmak için **Evet'i** seçin.

### <a name="enable-icmp-through-the-windows-firewall"></a>Windows güvenlik duvarı aracılığıyla ICMP'yi etkinleştirme

Daha sonraki bir adımda, yönlendirmeyi test etmek için izleme rotası aracını kullanırsınız. İzleme yolu, Windows Güvenlik Duvarı'nın varsayılan olarak reddettiği Internet Denetim İletiSi Protokolü'nü (ICMP) kullanır. ICMP'yi Windows güvenlik duvarı üzerinden etkinleştirin.

1. *myVmPrivate*uzak masaüstünde , Açık PowerShell.

1. Bu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Bu öğreticide yönlendirmeyi test etmek için izleme rotası kullanıyor olacaksınız. Üretim ortamları için, ICMP'nin Windows Güvenlik Duvarı'ndan geçmesine izin vermenizi önermiyoruz.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>myVmNva içinde IP yönlendirmeyi açma

Azure'u kullanarak VM'nin ağ arabirimi için [IP yönlendirmeyi açtınız.](#turn-on-ip-forwarding) VM işletim sistemi de ağ trafiğini iletmek zorundadır. Bu komutlarla *myVmNva* VM işletim sistemi için IP yönlendirmeyi açın.

1. *myVmPrivate* VM'deki komut isteminden uzak bir masaüstünü *myVmNva* VM'ye açın:

    ```cmd
    mstsc /v:myvmnva
    ```

1. *MyVmNva* VM'deki PowerShell'den IP yönlendirmeyi açmak için bu komutu girin:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

1. *myVmNva* VM'i yeniden başlatın: Görev çubuğundan **Başlat** > **Gücü' nü**seçin, Diğer **(Planlanmış)** > Devam**edin.**

    Bu, uzak masaüstü oturumunun bağlantısını da keser.

1. *myVmNva* VM yeniden başlatıldıktan sonra *myVmPublic* VM'ye uzak bir masaüstü oturumu oluşturun. *MyVmPrivate* VM'ye bağlı yken, bir komut istemi açın ve şu komutu çalıştırın:

    ```cmd
    mstsc /v:myVmPublic
    ```
1. *myVmPublic*uzak masaüstünde, PowerShell açın.

1. Bu komutu girerek Windows güvenlik duvarı üzerinden ICMP'yi etkinleştirin:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Ağ trafiğinin yönlendirmesini test edin

İlk olarak, *myVmPublic* VM'den *myVmPrivate* VM'ye ağ trafiğinin yönlendirmesini test edelim.

1. *MyVmPublic* VM'deki PowerShell'den şu komutu girin:

    ```powershell
    tracert myVmPrivate
    ```

    Yanıt bu örneğe benzer:

    ```powershell
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:

    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4

    Trace complete.
    ```

    İlk atlamanın NVA'nın özel IP adresi olan 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama *myVmPrivate* VM özel IP adresi: 10.0.1.4. Daha önce, rotayı *myRouteTablePublic* rota tablosuna eklediniz ve *genel* alt ağına ilişkilendirdiniz. Sonuç olarak Azure trafiği Doğrudan *Özel* alt ağına değil, NVA üzerinden gönderdi.

1. *myVmPublic* VM ile uzak masaüstü oturumunu kapatın. *myVmPrivate* VM bağlantınız hala açıktır.

1. *myVmPrivate* VM'deki komut isteminden şu komutu girin:

    ```cmd
    tracert myVmPublic
    ```

    Bu komut, *myVmPrivate* VM'den *myVmPublic* VM'ye ağ trafiğinin yönlendirmesini test ediyor. Yanıt bu örneğe benzer:

    ```cmd
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

    1     1 ms     1 ms     1 ms  10.0.0.4

    Trace complete.
    ```

    Azure'un trafiği doğrudan *myVmPrivate* VM'den *myVmPublic* VM'ye yönlendirir. Varsayılan olarak Azure, trafiği doğrudan alt ağlar arasında yönlendirir.

1. *myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubuna artık ihtiyaç duyulmadığında, *myResourceGroup'u* ve sahip olduğu tüm kaynakları silin:

1. Kaynak grubunuzu yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Kaynak gruplarını**arayın ve seçin.

1. Kaynak grubunuzun **(myResourceGroup)** adını seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Onay iletişim kutusunda, **KAYNAK GRUBU ADINI YAZIN**için *myResourceGroup'u* girin ve sonra **Sil'i**seçin. Azure, yol tablolarınız, depolama hesaplarınız, sanal ağlar, Sanal Ağlar, AĞ arabirimleri ve genel IP adresleri de dahil olmak üzere *myResourceGroup'u* ve bu kaynak grubuna bağlı tüm kaynakları siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir yönlendirme tablosu oluşturup bir alt ağ ile ilişkilendirdiniz. Bir genel alt ağdan özel alt ağa trafiği yönlendiren basit bir NVA oluşturdunuz. Artık, birçok kullanışlı ağ işlevi sağlayan [Azure](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking)Marketi'nden önceden yapılandırılmış farklı NVA'ları dağıtabilirsiniz. Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Sanal ağda birçok Azure kaynağı dağıtabiliyor ken, Azure bazı PaaS hizmetleri için kaynakları sanal ağa dağıtamaz. Kısıtlama yalnızca sanal ağ alt ağındaki trafik olsa da, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi kısıtlamak mümkündür. Ağ erişimini Azure PaaS kaynaklarına nasıl kısıtlayacağımı öğrenmek için sonraki öğreticiye bakın.

> [!div class="nextstepaction"]
> [PaaS kaynaklarına ağ erişimini kısıtlama](tutorial-restrict-network-access-to-resources.md)
