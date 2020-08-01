---
title: Azure sanal makineli teknik varlıklarınızı oluşturun
description: Azure Marketi için bir sanal makine (VM) teklifiyle ilgili teknik varlıkları oluşturma ve yapılandırma hakkında bilgi edinin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 04/13/2020
ms.openlocfilehash: afc012329d0d9e337dfca93a88615ba7c28f1754
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460380"
---
# <a name="create-your-azure-virtual-machine-technical-assets"></a>Azure sanal makineli teknik varlıklarınızı oluşturun

Bu makalede, Azure Marketi için bir sanal makine (VM) teklifi için teknik varlıkların nasıl oluşturulacağı ve yapılandırılacağı açıklanmaktadır. VM iki bileşen içerir: işletim sistemi sanal sabit diski (VHD) ve isteğe bağlı ilişkili veri diskleri VHD 'leri:

* **İşletim sistemi VHD 'si** : teklifinizle birlikte dağıtılan işletim sistemini ve çözümü içerir. VHD 'yi hazırlama işlemi, Linux tabanlı, Windows tabanlı veya özel tabanlı bir VM olmasına bağlı olarak farklılık gösterir.
* **Veri diskleri VHD** 'ler, bir VM için ayrılmış, kalıcı depolama. Kalıcı bilgileri depolamak için işletim sistemi VHD 'sini (örneğin, C: sürücüsü) kullanmayın.

Bir VM görüntüsü, bir işletim sistemi diski ve en fazla 16 veri diski içerir. Disk boş olsa bile, veri diski başına bir VHD kullanın.

> [!NOTE]
> Kullandığınız işletim sisteminden bağımsız olarak yalnızca çözüm için gereken en az sayıda veri diski ekleyin. Müşteriler, dağıtım sırasında bir görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler.

> [!IMPORTANT]
> Bir plandaki her sanal makine görüntüsünün aynı sayıda veri diski olmalıdır.

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkların tasarlanmasına, oluşturulmasına ve test edilmesine zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir. Çözüm etki alanınız ' ne ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

* [Azure hizmetlerini](https://azure.microsoft.com/services/) temel olarak anlama
* [Azure uygulamalarını tasarlama ve mimarın](https://azure.microsoft.com/solutions/architecture/)
* [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage) ve [Azure ağı](https://azure.microsoft.com/services/?filter=networking) ile çalışma hakkında bilgi
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) çalışma bilgileri
* [JSON](https://www.json.org/) hakkında çalışma bilgisi

## <a name="suggested-tools--optional"></a>Önerilen araçlar – isteğe bağlı

VM 'Lerin ve VHD 'lerin yönetilmesine yardımcı olmak için aşağıdaki komut dosyası ortamlarının birini kullanmayı göz önünde bulundurun:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)
* [Azure CLI](https://code.visualstudio.com/)

Ayrıca, aşağıdaki araçları geliştirme ortamınıza eklemeyi göz önünde bulundurun:

* [Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Visual Studio Code](https://code.visualstudio.com/)
  * Uzantı: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  * Uzantı: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  * Uzantı: [prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

[Azure Geliştirici Araçları](https://azure.microsoft.com/product-categories/developer-tools/) sayfasındaki kullanılabilir araçları gözden geçirin ve Visual Studio kullanıyorsanız, [Visual Studio Market](https://marketplace.visualstudio.com/).

## <a name="create-a-vm-image-using-an-approved-base"></a>Onaylanan bir taban kullanarak VM görüntüsü oluşturma

> [!NOTE]
> Kendi şirket içi oluşturduğunuz bir görüntüyü kullanarak sanal makine teknik varlıklarınızı oluşturmak için [kendi görüntünüzü kullanarak VM oluşturma](#create-a-vm-using-your-own-image)bölümüne gidin.

Bu bölümde, Uzak Masaüstü Protokolü (RDP), sanal makine için boyut seçme, en son Windows güncelleştirmelerini yükleme ve VHD görüntüsünü Genelleştirme gibi, onaylı bir temel kullanmanın çeşitli yönleri açıklanmaktadır.

Aşağıdaki bölümler, temel olarak Windows tabanlı VHD 'lere odaklanmaktadır. Linux tabanlı VHD 'ler oluşturma hakkında daha fazla bilgi için bkz. [Azure tarafından onaylanan dağıtımlara Linux](../../virtual-machines/linux/endorsed-distros.md).

> [!WARNING]
> Önceden yapılandırılmış, onaylı bir işletim sistemi içeren bir VM oluşturmak için Azure 'u kullanmak üzere bu konudaki yönergeleri izleyin. Çözümünüz ile uyumlu değilse, onaylanmış bir işletim sistemi kullanarak şirket içi bir VM oluşturmak ve yapılandırmak mümkündür. Daha sonra, [Azure 'a yüklemek üzere bir WINDOWS VHD veya vhdx hazırlama](../../virtual-machines/windows/prepare-for-upload-vhd-image.md)bölümünde açıklandığı gibi karşıya yükleme için bu uygulamayı yapılandırabilir ve hazırlarsınız.

### <a name="select-an-approved-base"></a>Onaylanan bir taban seçin

Tabanınız olarak Windows işletim sistemi veya Linux ' u seçin.

#### <a name="windows"></a>Windows

Windows tabanlı VM Görüntünüzün işletim sistemi VHD 'SI, Windows Server veya SQL Server içeren bir Azure onaylı temel görüntüye dayalı olmalıdır. Başlamak için Azure portal aşağıdaki görüntülerden birinden bir VM oluşturun:

* Windows Server ([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://www.microsoft.com/cloud-platform/windows-server-pricing), [2012 Datacenter](https://www.microsoft.com/cloud-platform/windows-server-pricing), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
* [Windows 10 IoT Enterprise](/windows/iot-core/windows-iot-enterprise)
* [SQL Server 2014](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance.md) (Enterprise, Standard, Web)
* [SQL Server 2012 SP2](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance.md) (Enterprise, Standard, Web)

> [!NOTE]
> Geçerli Azure portal veya Azure PowerShell kullanıyorsanız, 8 Eylül 2014 ' de ve sonrasında yayımlanan Windows Server görüntüleri onaylanır.

#### <a name="linux"></a>Linux

Azure, onaylı bir Linux dağıtımları yelpazesi sunar. Geçerli bir liste için bkz. [Azure tarafından onaylanan dağıtımlara Linux](../../virtual-machines/linux/endorsed-distros.md).

### <a name="create-vm-in-the-azure-portal"></a>Azure portal VM oluşturma

[Azure Portal](https://ms.portal.azure.com/)temel VM görüntüsünü oluşturmak için aşağıdaki adımları izleyin:

1. VM teklifinizi yayımlamak için kullanmak istediğiniz Azure aboneliğiyle ilişkili Microsoft hesabı [Azure Portal](https://ms.portal.azure.com/) oturum açın.
2. Yeni bir kaynak grubu oluşturun ve **kaynak grubu adınızı**, **aboneliğinizi**ve **kaynak grubu konumunu**belirtin. Ayrıntılar için bkz. [kaynakları yönetme](../../azure-resource-manager/resource-group-portal.md).
3. Sanal makineler Ayrıntılar sayfasını göstermek için sol taraftaki **sanal makineler** ' i seçin.
4. **+ Ekle** ' yi seçerek **sanal makine oluşturma deneyimini**açın.
5. Açılan listeden görüntüyü seçin veya tüm kullanılabilir sanal makine görüntülerini aramak veya taramak için **tüm ortak ve özel görüntülere gözatıp** ' ye tıklayın.
6. Aşağıdaki önerileri kullanarak dağıtılacak VM 'nin boyutunu seçin:
    * VHD 'yi şirket içinde geliştirmeyi planlıyorsanız, boyut önemi yoktur. Küçük VM 'lerden birini kullanmayı göz önünde bulundurun.
    * Görüntüyü Azure 'da geliştirmeyi planlıyorsanız, seçilen görüntü için önerilen VM boyutlarından birini kullanmayı göz önünde bulundurun.

7. **Diskler** bölümünde, **Gelişmiş** bölümünü genişletin ve **yönetilen diskleri kullan** seçeneğini **Hayır**olarak ayarlayın.
8. VM oluşturmak için gereken diğer ayrıntıları sağlayın.
9. Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

Azure, belirttiğiniz sanal makineyi sağlamaya başlar. Sol taraftaki **sanal makineler** sekmesini seçerek ilerlemesini izleyebilirsiniz. Oluşturulduktan sonra durum **çalışıyor**olarak değişir.

Yeni Azure tabanlı VHD 'nizi oluştururken zorlukla karşılaşırsanız, bkz. [VHD oluşturma (SSS) sırasında yaygın sorunlar](common-issues-during-vhd-creation.md).

### <a name="connect-to-your-azure-vm"></a>Azure VM 'nize bağlanma

Bu bölümde, Azure 'da oluşturduğunuz sanal makineye bağlanma ve oturum açma işlemlerinin nasıl yapılacağı açıklanmaktadır. Başarıyla bağlandıktan sonra, ana bilgisayar sunucusunda yerel olarak oturum açmış olduğunuz gibi VM ile çalışabilirsiniz.

#### <a name="connect-to-a-windows-based-vm"></a>Windows tabanlı bir VM 'ye bağlanma

Azure 'da barındırılan Windows tabanlı VM 'ye bağlanmak için uzak masaüstü istemcisini kullanın. Windows 'un çoğu sürümü yerel olarak Uzak Masaüstü Protokolü (RDP) desteği içerir. Diğer işletim sistemleri için, [Uzak Masaüstü istemcilerindeki](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)istemciler hakkında daha fazla bilgi edinebilirsiniz.

Bu makalede, yerleşik Windows RDP desteğinin sanal makinenize bağlanmak için nasıl kullanılacağı açıklanır: [Windows çalıştıran bir Azure sanal makinesine bağlanma ve oturum açma](../../virtual-machines/windows/connect-logon.md).

> [!TIP]
> İşlem sırasında güvenlik uyarıları alabilirsiniz. Örneğin, ". rdp dosyası bilinmeyen bir yayımcıdan" veya "Kullanıcı kimlik bilgileriniz doğrulanamıyor" gibi uyarılar. Bu uyarıları yoksaymak güvenlidir.

#### <a name="connect-to-a-linux-based-vm"></a>Linux tabanlı bir VM 'ye bağlanma

Linux tabanlı bir VM 'ye bağlanmak için güvenli bir kabuk Protokolü (SSH) istemcisine ihtiyacınız vardır. Aşağıdaki adımlarda ücretsiz [Putty](https://www.ssh.com/ssh/putty/) Shh terminali kullanılır.

1. [Azure portalına](https://ms.portal.azure.com/) gidin.
2. **Sanal makineleri**arayın ve seçin.
3. Bağlanmak istediğiniz VM 'yi seçin.
4. Zaten çalışmıyorsa VM 'yi başlatın.
5. **Genel bakış** sayfasını açmak için VM 'nin adını seçin.
6. VM 'nizin genel IP adresi ve DNS adı ' nı not edin (Bu değerler ayarlanmamışsa, [bir ağ arabirimi oluşturmanız](../../virtual-network/virtual-network-network-interface.md#create-a-network-interface)gerekir)).
7. PuTTY uygulamasını açın.
8. PuTTY yapılandırması iletişim kutusunda, sanal makinenizin IP adresini veya DNS adını girin.

    :::image type="content" source="media/avm-putty.png" alt-text="PuTTY terminal ayarlarını gösterir. Ana bilgisayar adı veya IP adresi ve bağlantı noktası kutuları vurgulanır.":::

9. PuTTY terminali açmak için **Aç** ' ı seçin.
10. İstendiğinde, Linux VM hesabınızın hesap adını ve parolasını girin.

Bağlantı sorunlarınız varsa, SSH istemcinizin belgelerine bakın. Örneğin, [Bölüm 10: ortak hata iletileri](https://www.ssh.com/ssh/putty/putty-manuals).

Sağlanan bir Linux sanal makinesine masaüstü ekleme gibi ayrıntılar için bkz. [Azure 'da bir LINUX VM 'sine bağlanmak Için uzak masaüstü 'Nü yükleyip yapılandırma](../../virtual-machines/linux/use-remote-desktop.md).

## <a name="create-a-vm-using-your-own-image"></a>Kendi görüntünüzü kullanarak VM oluşturma

Bu bölümde, Kullanıcı tarafından belirtilen bir sanal makine (VM) görüntüsünün nasıl oluşturulacağı ve dağıtılacağı açıklanmaktadır. Bunu, Azure tarafından dağıtılan bir sanal sabit diskten (VHD) işletim sistemi ve veri diski VHD görüntüleri sağlayarak yapabilirsiniz.

> [!NOTE]
> İsteğe bağlı olarak onaylanmış bir temel görüntü kullanmak için, onaylı bir [taban kullanarak VM görüntüsü oluşturma](#create-a-vm-image-using-an-approved-base)bölümündeki yönergeleri izleyin.

1. Görüntülerinizi Azure Storage hesabına yükleyin.
2. VM görüntüsünü dağıtın.
3. VM görüntüsünü yakalayın.

### <a name="upload-your-images-to-an-azure-storage-account"></a>Görüntülerinizi bir Azure depolama hesabına yükleyin

1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. Genelleştirilmiş işletim sistemi VHD 'nizi ve veri diski VHD 'nizi Azure depolama hesabınıza yükleyin.

### <a name="deploy-your-image"></a>Görüntünüzü dağıtın

Azure portal ya da Azure PowerShell kullanarak görüntünüzü oluşturun.

#### <a name="deploy-using-the-azure-portal"></a>Azure portalını kullanarak dağıtma

1. Giriş sayfasında, **kaynak oluştur**' u seçin, "şablon dağıtımı" ifadesini arayın ve **Oluştur**' u seçin.
2. **Düzenleyicide kendi şablonunuzu oluşturun**öğesini seçin.

    :::image type="content" source="media/avm-custom-deployment.png" alt-text="Özel dağıtım sayfasını gösterir.":::

3. Bu [JSON şablonunu](../partner-center-portal/azure-vm-image-certification.md) düzenleyiciye yapıştırın ve **Kaydet**' i seçin.
4. Görüntülenmiş **özel dağıtım** özelliği sayfaları için parametre değerlerini belirtin.

    | Parametre | Açıklama |
    | ------------ | ------------- |
    | Kullanıcı depolama hesabı adı | Hücreden içerik 2 |
    | Kullanıcı depolama kapsayıcısı adı | Genelleştirilmiş VHD 'nin bulunduğu depolama hesabı adı |
    | Genel IP için DNS adı | Genel IP DNS adı. Teklif dağıtıldıktan sonra Azure portal genel IP adresi için DNS adını tanımlayın. |
    | Yönetici Kullanıcı adı | Yeni VM için yönetici hesabının Kullanıcı adı |
    | Yönetici Parolası | Yeni VM için yönetici hesabının parolası |
    | İşletim Sistemi Türü | VM işletim sistemi: Windows veya Linux |
    | Abonelik Kimliği | Seçili aboneliğin tanımlayıcısı |
    | Konum | Dağıtımın coğrafi konumu |
    | VM Boyutu | [Azure VM boyutu](../../virtual-machines/windows/sizes.md), örneğin Standard_A2 |
    | Genel IP adresi adı | Genel IP adresinizin adı |
    | VM Adı | Yeni VM 'nin adı |
    | Sanal ağ adı | VM tarafından kullanılan sanal ağın adı |
    | NIC adı | Sanal ağı çalıştıran ağ arabirimi kartının adı |
    | VHD URL 'SI | İşletim sistemi diski VHD URL 'SI |
    |  |  |

5. Bu değerleri sağlamadıktan sonra, **satın al**' ı seçin.

Azure, dağıtıma başlayacak. Belirtilen depolama hesabı yolunda belirtilen yönetilmeyen VHD 'ye sahip yeni bir VM oluşturur. Azure portal ilerlemeyi, portalın sol tarafındaki **sanal makineler** ' i seçerek izleyebilirsiniz. VM oluşturulduğunda, durum başlangıç durumundan çalışmaya başlayacak şekilde değişir.

#### <a name="deploy-using-azure-powershell"></a>Azure PowerShell’i kullanarak dağıtma

```powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

### <a name="capture-the-vm-image"></a>VM görüntüsünü yakala

Yaklaşımınıza karşılık gelen aşağıdaki yönergeleri kullanın:

* Azure PowerShell: [Azure VM 'den YÖNETILMEYEN VM görüntüsü oluşturma](../../virtual-machines/windows/capture-image-resource.md)
* Azure CLı: [bir sanal makinenin veya VHD 'nin görüntüsünü oluşturma](../../virtual-machines/linux/capture-image.md)
* API: [sanal makineler-yakala](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

## <a name="configure-the-virtual-machine"></a>Sanal makineyi yapılandırma

Bu bölümde bir Azure VM 'nin nasıl boyutlandırılacağını, güncelleştirileceğini ve genelleştirirsiniz. Bu adımlar, VM 'nizi Azure Marketi 'nde dağıtılacak şekilde hazırlamak için gereklidir.

### <a name="sizing-the-vhds"></a>VHD 'leri boyutlandırma

Bir işletim sistemi (ve isteğe bağlı olarak ek hizmetler) ile önceden yapılandırılmış VM 'lerden birini seçtiyseniz, zaten standart bir Azure VM boyutu seçmiş olursunuz. Çözümünüzü önceden yapılandırılmış bir işletim sistemi ile başlatmak önerilen yaklaşımdır. Ancak, bir işletim sistemini el ile yüklüyorsanız, birincil VHD 'nizi VM yansımanıza göre boyutlandırmanız gerekir:

* Windows için, işletim sistemi VHD 'si 127-128 GB sabit biçimli VHD olarak oluşturulmalıdır.
* Linux için bu VHD, 30 – 50 GB sabit biçimli VHD olarak oluşturulmalıdır.

Fiziksel boyut 127 – 128 GB 'den küçükse, VHD genişletilebilir (seyrek/dinamik) olmalıdır. Belirtilen temel pencereler ve SQL Server görüntüler bu gereksinimleri zaten karşılamış olduğundan, VHD 'nin biçimini veya boyutunu değiştirmeyin.

Veri diskleri 1 TB kadar büyük olabilir. Boyut ' a karar verirken, müşterilerin dağıtım sırasında bir görüntü içindeki VHD 'leri yeniden boyutlandıramayacağını unutmayın. Veri diski VHD 'leri sabit biçimli VHD 'ler olarak oluşturulmalıdır. Ayrıca, genişletilebilir (seyrek/dinamik) olmalıdır. Veri diskleri başlangıçta boş olabilir veya veri içerebilir.

### <a name="install-the-most-current-updates"></a>En güncel güncelleştirmeleri yükler

İşletim sistemi VM 'lerinin temel görüntülerinin yayımlanan tarihleri en son güncelleştirmeleri içermesi gerekir. Oluşturduğunuz işletim sistemi VHD 'sini yayımlamadan önce, IŞLETIM sistemini ve yüklü tüm hizmetleri en son güvenlik ve bakım düzeltme ekleriyle güncelleştirdiğinizden emin olun.

Windows Server için, **Güncelleştirmeleri denetle** komutunu çalıştırın.

Linux dağıtımları için güncelleştirmeler genellikle bir komut satırı aracı veya bir grafik yardımcı programıyla indirilir ve yüklenir. Örneğin Ubuntu Linux, işletim sistemini güncelleştirmek için [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) komutunu ve [Güncelleştirme Yöneticisi](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) aracını sağlar.

### <a name="perform-additional-security-checks"></a>Ek güvenlik denetimleri gerçekleştirme

Azure Marketi 'nde çözüm görüntüleriniz için yüksek düzeyde güvenlik koruyun. Aşağıdaki makalede size yardımcı olacak güvenlik yapılandırmalarının ve yordamların bir listesini bulabilirsiniz: [Azure Market görüntüleri Için güvenlik önerileri](../../security/security-recommendations-azure-marketplace-images.md). Bu önerilerin bazıları Linux tabanlı görüntülere özgüdür, ancak çoğu sanal makine görüntüsü için geçerlidir.

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Özel yapılandırma ve zamanlanmış görevler gerçekleştirme

Ek yapılandırma gerekiyorsa, sanal makinede dağıtıldıktan sonra son değişiklikler yapmak için başlangıçta çalışan zamanlanmış bir görev kullanın. Ayrıca aşağıdaki önerileri göz önünde bulundurun:

* Bir kez çalıştır görevi varsa, görev başarıyla tamamlandıktan sonra görevin kendisini silmesi gerekir.
* Yapılandırma C veya D dışındaki sürücülere güvenmemelidir, çünkü yalnızca bu iki sürücünün var olduğu garanti edilir (sürücü C, işletim sistemi diski ve D sürücüsü geçici yerel disktir).

Linux özelleştirmeleri hakkında daha fazla bilgi için bkz. [Linux Için sanal makine uzantıları ve özellikleri](../../virtual-machines/extensions/features-linux.md).

## <a name="generalize-the-image"></a>Görüntüyü genelleştirin

Azure Marketi 'ndeki tüm görüntülerin genel bir biçimde yeniden kullanılabilir olması gerekir. Bunu başarmak için, işletim sistemi VHD 'SI, bir VM 'den örneğe özgü tüm tanımlayıcıları ve yazılım sürücülerini kaldıran bir işlem olan genelleştirilmelidir.

### <a name="windows"></a>Windows

Windows işletim sistemi diskleri, [Sysprep aracıyla](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)genelleştirilmiştir. İşletim sistemini daha sonra güncelleştirip yeniden yapılandırırsanız, Sysprep 'i yeniden çalıştırmanız gerekir.

> [!WARNING]
> Güncelleştirmeler otomatik olarak çalıştırılabileceğinden, Sysprep 'i çalıştırdıktan sonra VM 'yi dağıtıldıktan sonra kapatın. Bu kapanıyor, sonraki güncelleştirmelerin işletim sisteminde veya yüklü hizmetlerde örneğe özgü değişiklikler yapmasını önler. Sysprep 'i çalıştırma hakkında daha fazla bilgi için bkz. [BIR VHD Genelleştirme adımları](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep).

### <a name="linux"></a>Linux

Aşağıdaki işlem bir Linux sanal makinesini genelleştirir ve ayrı bir VM olarak yeniden dağıtır. Ayrıntılar için bkz. [sanal makine veya VHD 'nin görüntüsünü oluşturma](../../virtual-machines/linux/capture-image.md). "Yakalanan görüntüden sanal makine oluşturma" bölümüne ulaştığınızda durulabiliyor olabilirsiniz.

1. **Azure Linux aracısını kaldırma**

    1. SSH istemcisi kullanarak Linux sanal makinenize bağlanın.
    2. SSH penceresinde şu komutu girin: `sudo waagent -deprovision+user` .
    3. Devam etmek için **Y** yazın (onay adımını önlemek için önceki komuta **-zorlama** parametresini ekleyebilirsiniz).
    d. Komut tamamlandıktan sonra SSH istemcisini kapatmak için **Exit** yazın.

2. **Sanal makineyi durdurma**

    1. Azure portal, kaynak grubunuzu (RG) seçin ve VM 'yi serbest olarak ayırın.
    2. VHD 'niz artık genelleştirilmiştir ve bu VHD 'YI kullanarak yeni bir VM oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yeni Azure tabanlı VHD 'nizi oluştururken zorluk yaşıyorsanız, bkz. [VHD oluşturma sırasında yaygın sorunlar](common-issues-during-vhd-creation.md).

Aksi durumda:.

* [VHD 'den dağıtılan sınama sanal makinesi (VM)](azure-vm-image-certification.md) , Azure *sertifikalı aracı Için sertifika test aracı* 'Nı nereden alacağınız ve VM görüntünüzü onaylamak için nasıl kullanılacağı dahil olmak üzere Azure MARKETI sertifikası için bir VM görüntüsünü test etme ve gönderme işlemlerinin nasıl yapılacağını açıklar.
