---
title: Yönlendirme ağ trafiği-öğretici-Azure portal
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
ms.openlocfilehash: d630a41f9b83a852605ffad2a85ad6dd14bbac73
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86079658"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme

Azure, varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasındaki trafiği yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Özel yollar, örneğin bir ağ sanal gereci (NVA) ile alt ağlar arasında trafiği yönlendirmek istediğinizde yararlıdır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Trafiği yönlendiren bir NVA oluşturma
> * Yönlendirme tablosu oluşturma
> * Yönlendirme oluşturma
> * Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
> * Sanal makineleri (VM) farklı alt ağlara dağıtma
> * NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Bu öğretici [Azure Portal](https://portal.azure.com)kullanır. [Azure CLI](tutorial-create-route-table-cli.md) veya [Azure PowerShell](tutorial-create-route-table-powershell.md)de kullanabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-nva"></a>NVA oluşturma

Ağ sanal gereçleri (NVA 'lar), Yönlendirme ve güvenlik duvarı iyileştirmesi gibi ağ işlevleriyle yardımcı olan sanal makinelerdir. Bu öğretici, **Windows Server 2016 Datacenter**kullandığınızı varsayar. İsterseniz farklı bir işletim sistemi seçebilirsiniz.

1. [Azure Portal](https://portal.azure.com) menüsünde veya **giriş** sayfasında, **kaynak oluştur**' u seçin.

1. **Güvenlik**  >  **Windows Server 2016 Datacenter**' ı seçin.

    ![Windows Server 2016 Datacenter, VM oluşturma, Azure portal](./media/tutorial-create-route-table-portal/vm-ws2016-datacenter.png)

1. **Sanal makine oluştur** sayfasında, **temel**bilgiler altında bu bilgileri girin veya seçin:

    | Section | Ayar | Eylem |
    | ------- | ------- | ----- |
    | **Proje ayrıntıları** | Abonelik | Aboneliğinizi seçin. |
    | | Kaynak grubu | **Yeni oluştur**' u seçin, *Myresourcegroup*girin ve **Tamam**' ı seçin. |
    | **Örnek ayrıntıları** | Sanal makine adı | *Myvmnva*girin. |
    | | Bölge | **Doğu ABD (ABD)** seçin. |
    | | Kullanılabilirlik seçenekleri | **Altyapı yedekliliği gerekli değil**' i seçin. |
    | | Görüntü | **Windows Server 2016 Datacenter**' ı seçin. |
    | | Boyut | Varsayılan **Standart DS1 v2**'yi koruyun. |
    | **Yönetici hesabı** | Kullanıcı adı | Seçtiğiniz bir kullanıcı adını girin. |
    | | Parola | Seçtiğiniz bir parolayı girin, en az 12 karakter uzunluğunda olmalıdır ve [tanımlanan karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm)karşılayın. |
    | | Parolayı Onayla | Parolayı yeniden girin. |
    | **Gelen bağlantı noktası kuralları** | Ortak gelen bağlantı noktaları | **Hiçbiri**seçin. |
    | **Tasarruf edin** | Zaten bir Windows Server lisansınız var mı? | Seçim **No**. |

    ![Temel bilgiler, sanal makine oluşturma Azure portal](./media/tutorial-create-route-table-portal/basics-create-virtual-machine.png)

    Ardından Ileri ' yi seçin **: diskler >**.

1. **Diskler**bölümünde, gereksinimlerinize uygun olan ayarları seçin ve ardından İleri ' yi seçin **: ağ >**.

1. **Ağ iletişimi**:

    1. **Sanal ağ**Için **Yeni oluştur**' u seçin.
    
    1. **Sanal ağ oluştur** iletişim kutusunda, **ad**' ın altında, *myVirtualNetwork*girin.

    1. **Adres alanı**' nda, var olan adres aralığını *10.0.0.0/16*ile değiştirin.

    1. **Alt ağlarda**, mevcut alt ağı silmek için **Sil** simgesini seçin ve ardından aşağıdaki **alt ağ adı** ve **adres aralığı**birleşimlerini girin. Geçerli bir ad ve Aralık girildikten sonra, altında yeni bir boş satır görüntülenir.

        | Alt ağ adı | Adres aralığı |
        | ----------- | ------------- |
        | *Geneldir* | *10.0.0.0/24* |
        | *Özelleştirme* | *10.0.1.0/24* |
        | *DMZ* | *10.0.2.0/24* |

    1. İletişim kutusundan çıkmak için **Tamam ' ı** seçin.

    1. **Alt ağda** **DMZ (10.0.2.0/24)** öğesini seçin.

    1. **Genel IP**' de, bu VM internet üzerinden bağlanmadığından **yok**' u seçin.

    1. **İleri ' yi seçin: yönetim >**.

1. **Yönetim**altında:

    1. **Tanılama depolama hesabı**' nda **Yeni oluştur**' u seçin.
    
    1. **Depolama hesabı oluştur** iletişim kutusunda şu bilgileri girin veya seçin:

        | Ayar | Değer |
        | ------- | ----- |
        | Adı | *mynvastorageaccount* |
        | Hesap türü | **Depolama (genel amaçlı v1)** |
        | Performans | **Standart** |
        | Çoğaltma | **Yerel olarak yedekli depolama (LRS)** |
    
    1. İletişim kutusundan çıkmak için **Tamam ' ı** seçin.

    1. **İncele ve oluştur**’u seçin. **Gözden geçir + oluştur** sayfasına götürülürsünüz ve Azure yapılandırmanızı doğrular.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

    Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM oluşturmayı bitirene kadar bekleyin. **Dağıtım çalışıyor** sayfasında dağıtım ayrıntıları gösterilir.

1. VM 'niz hazırsanız **Kaynağa Git**' i seçin.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

1. [Azure Portal](https://portal.azure.com) menüsünde veya **giriş** sayfasında, **kaynak oluştur**' u seçin.

2. Arama kutusuna *yol tablosu*girin. Arama sonuçlarında **yol tablosu** göründüğünde, bunu seçin.

3. **Yol tablosu** sayfasında **Oluştur**' u seçin.

4. **Yol tablosu oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *myRouteTablePublic* |
    | Abonelik | Aboneliğiniz |
    | Kaynak grubu | **myResourceGroup** |
    | Konum | **ABD Doğu ABD** |
    | Sanal ağ geçidi yol yayma | **Etkin** |

    ![Yol tablosu oluşturma, Azure portal](./media/tutorial-create-route-table-portal/create-route-table.png)

5. **Oluştur**'u seçin.

## <a name="create-a-route"></a>Yönlendirme oluşturma

1. Yol tablonuzu yönetmek için [Azure Portal](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Yol tablonuzun adını (**Myroutetablepublic**) seçin.

1. **Rotalar**  >  **Ekle**' yi seçin.

    ![Rota, yol tablosu, Azure portal ekleyin](./media/tutorial-create-route-table-portal/add-route.png)

1. **Yol Ekle**' de bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Yönlendirme adı | *ToPrivateSubnet* |
    | Adres ön eki | *10.0.1.0/24* (daha önce oluşturulan *özel* alt ağın adres aralığı) |
    | Sonraki atlama türü | **Sanal gereç** |
    | Sonraki atlama adresi | *10.0.2.4* ( *DMZ* alt ağının adres aralığı içindeki bir adres) |

1. **Tamam**’ı seçin.

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

1. Sanal ağınızı yönetmek için [Azure Portal](https://portal.azure.com) gidin. **Sanal ağları**arayın ve seçin.

1. Sanal ağınızın adını (**myVirtualNetwork**) seçin.

1. Sanal ağın menü çubuğunda **alt ağlar**' ı seçin.

1. Sanal ağın alt ağ listesinde **genel**' i seçin.

1. **Yol tablosu**' nda oluşturduğunuz yol tablosunu (**Myroutetablepublic**) seçin ve ardından **Kaydet** ' i seçerek rota tablonuzu *genel* alt ağıyla ilişkilendirin.

    ![Rota tablosunu, alt ağ listesini, sanal ağı, Azure portal ilişkilendir](./media/tutorial-create-route-table-portal/associate-route-table.png)

## <a name="turn-on-ip-forwarding"></a>IP iletmeyi aç

Ardından, *Myvmnva adlı*yeni NVA sanal MAKINENIZ için IP iletmeyi açın. Azure, *Myvmnva*'ya ağ trafiği gönderdiğinde, trafik farklı bir IP adresine gidiyor Ise, IP iletimi trafiği doğru konuma gönderir.

1. VM 'nizi yönetmek için [Azure Portal](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

1. VM 'nizin adını (**Myvmnva**) seçin.

1. NVA sanal makinenizin menü çubuğunda **ağ iletişimi**' ni seçin.

1. **Myvmnva123**öğesini seçin. Bu, VM 'niz için oluşturulan Azure ağ arabirimidir. Azure, benzersiz bir ad sağlamak için numaralar ekler.

    ![Ağ iletişimi, ağ sanal gereci (NVA) sanal makinesi (VM), Azure portal](./media/tutorial-create-route-table-portal/virtual-machine-networking.png)

1. Ağ arabirimi menü çubuğunda **IP yapılandırması**' nı seçin.

1. **IP yapılandırması** sayfasında, **IP iletmeyi** **etkin**olarak ayarlayın ve **Kaydet**' i seçin.

    ![IP iletmeyi, IP yapılandırmasını, ağ arabirimini, ağ sanal gereç (NVA) sanal makinesini (VM), Azure portal etkinleştir](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png)

## <a name="create-public-and-private-virtual-machines"></a>Genel ve özel sanal makineleri oluşturma

Sanal ağda ortak bir VM ve özel bir VM oluşturun. Daha sonra, Azure 'un *genel* alt ağ trafiğini NVA aracılığıyla *özel* alt ağa yönlendirdiğini görmek için bu uygulamaları kullanacaksınız.

Ortak VM ve özel VM oluşturmak için daha önce [BIR NVA oluşturma](#create-an-nva) adımlarını izleyin. Dağıtımın bitmesini beklemeniz veya VM kaynağına gitmeniz gerekmez. Aşağıda açıklananlar dışında, aynı ayarların çoğunu kullanacaksınız.

Ortak veya özel VM oluşturmak için **Oluştur** ' u seçmeden önce, farklı olması gereken değerleri gösteren aşağıdaki iki alt bölümüne ([genel VM](#public-vm) ve [özel VM](#private-vm)) gidin. Azure her iki sanal makineyi de dağıtmaya başladıktan sonra bir sonraki bölüme ([trafiği BIR NVA üzerinden yönlendirin](#route-traffic-through-an-nva)) devam edebilirsiniz.

### <a name="public-vm"></a>Ortak VM

| Tab | Ayar | Değer |
| --- | ------- | ----- |
| Temel Bilgiler | Kaynak grubu | **myResourceGroup** |
| | Sanal makine adı | *myVmPublic* |
| | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin ver** |
| | Gelen bağlantı noktalarını seçin | **RDP** |
| Ağ | Sanal ağ | **myVirtualNetwork** |
| | Alt ağ | **Ortak (10.0.0.0/24)** |
| | Genel IP adresi | Varsayılan değer |
| Yönetim | Tanılama depolama hesabı | **mynvastorageaccount** |

### <a name="private-vm"></a>Özel VM

| Tab | Ayar | Değer |
| --- | ------- | ----- |
| Temel Bilgiler | Kaynak grubu | **myResourceGroup** |
| | Sanal makine adı | *myVmPrivate* |
| | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin ver** |
| | Gelen bağlantı noktalarını seçin | **RDP** |
| Ağ | Sanal ağ | **myVirtualNetwork** |
| | Alt ağ | **Özel (10.0.1.0/24)** |
| | Genel IP adresi | Varsayılan değer |
| Yönetim | Tanılama depolama hesabı | **mynvastorageaccount** |

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

### <a name="sign-in-to-myvmprivate-over-remote-desktop"></a>Uzak Masaüstü üzerinden myVmPrivate 'de oturum açın

1. Özel VM 'nizi yönetmek için [Azure Portal](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

1. Özel sanal makinenizin adını (**myVmPrivate**) seçin.

1. VM menü çubuğunda, özel VM 'ye Uzak Masaüstü bağlantısı oluşturmak için **Bağlan** ' ı seçin.

1. **RDP Ile Bağlan** SAYFASıNDA, **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü (*. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen *. rdp* dosyasını açın. İstendiğinde **Bağlan**’ı seçin. **Diğer seçimler**  >  ' i seçin,**farklı bir hesap kullanın**ve ardından özel VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

1. **Tamam**’ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alırsanız, sanal makineye bağlanmak için **Evet** ' i seçin.

### <a name="enable-icmp-through-the-windows-firewall"></a>Windows Güvenlik Duvarı üzerinden ıCMP 'yi etkinleştirme

Sonraki bir adımda, yönlendirmeyi test etmek için izleme yönlendirme aracını kullanacaksınız. İzleme yolu, Windows güvenlik duvarının varsayılan olarak engellediği Internet Denetim Iletisi Protokolü 'Nü (ıCMP) kullanır. Windows Güvenlik Duvarı üzerinden ıCMP 'yi etkinleştirin.

1. *MyVmPrivate*uzak masaüstünde PowerShell ' i açın.

1. Şu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Bu öğreticide yönlendirmeyi test etmek için izleme rotası kullanıyorsunuz. Üretim ortamları için Windows Güvenlik Duvarı üzerinden ıCMP 'ye izin vermeyi önermiyoruz.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>MyVmNva içinde IP iletmeyi aç

Azure kullanarak VM 'nin ağ arabirimi için [IP iletmeyi açtınız](#turn-on-ip-forwarding) . VM 'nin işletim sistemi de ağ trafiğini iletmektir. Bu komutlarla *Myvmnva* VM 'nin işletim SISTEMI için IP iletmeyi açın.

1. *MyVmPrivate* VM 'deki bir komut Isteminden, *Myvmnva* VM 'ye bir Uzak Masaüstü açın:

    ```cmd
    mstsc /v:myvmnva
    ```

1. *Myvmnva* VM üzerindeki POWERSHELL 'den IP iletmeyi açmak için şu komutu girin:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

1. *Myvmnva* VM 'sini yeniden başlatın: görev çubuğundan **Başlangıç**  >  **gücü**' ni, **diğer (planlı)**  >  **devam et**' i seçin.

    Bu, uzak masaüstü oturumunun bağlantısını da keser.

1. *Myvmnva* VM yeniden başlatıldıktan sonra, *myVmPublic* VM 'ye bir Uzak Masaüstü oturumu oluşturun. *MyVmPrivate* VM 'sine hala bağlıyken, bir komut istemi açın ve şu komutu çalıştırın:

    ```cmd
    mstsc /v:myVmPublic
    ```
1. *MyVmPublic*uzak masaüstünde PowerShell ' i açın.

1. Şu komutu girerek Windows Güvenlik Duvarı üzerinden ıCMP 'yi etkinleştirin:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Ağ trafiğinin yönlendirilmesini test etme

İlk olarak, ağ trafiğinin *myVmPublic* VM 'den *myVmPrivate* VM 'ye yönlendirilmesini test edelim.

1. *MyVmPublic* VM 'deki PowerShell 'den şu komutu girin:

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

    İlk atlamanın, NVA 'nın özel IP adresi olan 10.0.2.4 'e olduğunu görebilirsiniz. İkinci atlama *myVmPrivate* VM 'nın özel IP adresidir: 10.0.1.4. Daha önce, yolu *Myroutetablepublic* yol tablosuna eklediniz ve bunu *ortak* alt ağla ilişkilendirdiniz. Sonuç olarak, Azure trafiği NVA üzerinden gönderdi ve doğrudan *özel* alt ağa değil.

1. *myVmPublic* VM ile uzak masaüstü oturumunu kapatın. *myVmPrivate* VM bağlantınız hala açıktır.

1. *MyVmPrivate* VM 'deki bir komut isteminden şu komutu girin:

    ```cmd
    tracert myVmPublic
    ```

    Bu komut, ağ trafiğinin *myVmPrivate* VM 'den *myVmPublic* VM 'ye yönlendirilmesini sınar. Yanıt bu örneğe benzer:

    ```cmd
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

    1     1 ms     1 ms     1 ms  10.0.0.4

    Trace complete.
    ```

    Azure 'un trafiği doğrudan *myVmPrivate* VM 'den *myVmPublic* VM 'ye yönlendirdiğini görebilirsiniz. Varsayılan olarak Azure, trafiği doğrudan alt ağlar arasında yönlendirir.

1. *myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubuna artık ihtiyaç duyulmadığında *Myresourcegroup* ve içerdiği tüm kaynakları silin:

1. Kaynak grubunuzu yönetmek için [Azure Portal](https://portal.azure.com) gidin. **Kaynak gruplarını**arayın ve seçin.

1. Kaynak grubunuzun adını (**Myresourcegroup**) seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Onay iletişim kutusunda, **kaynak grubu adını yazmak**Için *myresourcegroup* yazın ve ardından **Sil**' i seçin. Azure, yol tablolarınız, depolama hesaplarınız, sanal ağlar, VM 'Ler, ağ arabirimleriniz ve genel IP adresleri dahil olmak üzere *Myresourcegroup* ve bu kaynak grubuna bağlı tüm kaynakları siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir yönlendirme tablosu oluşturup bir alt ağ ile ilişkilendirdiniz. Bir genel alt ağdan özel alt ağa trafiği yönlendiren basit bir NVA oluşturdunuz. Artık çok sayıda kullanışlı ağ işlevi sağlayan [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking)'nden farklı önceden yapılandırılmış NVA 'lar dağıtabilirsiniz. Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Bir sanal ağ içinde birçok Azure kaynağı dağıtabilmeniz sırasında Azure, bazı PaaS hizmetleri için bir sanal ağa kaynak dağıtamaz. Bazı Azure PaaS hizmetlerinin kaynaklarına erişimi kısıtlamak mümkündür, ancak kısıtlama yalnızca bir sanal ağ alt ağından gelen trafik olmalıdır. Azure PaaS kaynaklarına ağ erişimini kısıtlama hakkında bilgi edinmek için sonraki öğreticiye bakın.

> [!div class="nextstepaction"]
> [PaaS kaynaklarına ağ erişimini kısıtlama](tutorial-restrict-network-access-to-resources.md)

> [!NOTE] 
> Azure hizmetleri maliyet parayı. Azure maliyet yönetimi, bütçeleri ayarlamanıza ve denetim altına harcamalarını sağlamak için uyarıları yapılandırmanıza yardımcı olur. Maliyet yönetimi ile Azure maliyetlerinizi çözümleyin, yönetin ve iyileştirin. Daha fazla bilgi edinmek için [maliyetlerinizi çözümlemeye yönelik hızlı başlangıç](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)bölümüne bakın.