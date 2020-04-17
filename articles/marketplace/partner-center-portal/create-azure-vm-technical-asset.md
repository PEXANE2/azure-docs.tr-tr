---
title: Azure Sanal Makine teknik varlıklarınızı oluşturun
description: Azure Marketi için sanal makine (VM) teklifi için teknik varlıkları nasıl oluşturup yapılandıracaklarını öğrenin.
author: dannyevers
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/13/2020
ms.openlocfilehash: 46c59e8b4e60fbe17887ea0fc375b6da758ebf50
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457408"
---
# <a name="create-your-azure-virtual-machine-technical-assets"></a>Azure Sanal Makine teknik varlıklarınızı oluşturun

> [!IMPORTANT]
> Azure Sanal Makine tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izne gelene kadar, bulut iş ortağı portalının tekliflerinizi yönetmesi [için sanal bir makine teklifi için teknik varlıklar oluştur'daki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-technical-assets) yönergeleri izleyin.

Bu makalede, Azure Marketi için sanal makine (VM) teklifi için teknik varlıkların nasıl oluşturulup yapılandırılabildiğini açıklanmaktadır. VM iki bileşen içerir: işletim sistemi sanal sabit disk (VHD) ve isteğe bağlı ilişkili veri diskleri VHDs:

* **İşletim sistemi VHD** – Teklifinizle birlikte dağıtılan işletim sistemini ve çözümü içerir. VHD'yi hazırlama işlemi, Linux tabanlı, Windows tabanlı veya özel tabanlı VM olmasına bağlı olarak değişir.
* **Veri diskleri VHDs** - Bir VM için özel, kalıcı depolama. Kalıcı bilgileri depolamak için VHD işletim sistemini (örneğin C: sürücü) kullanmayın.

VM görüntüsü nde bir işletim sistemi diski ve en fazla 16 veri diski bulunur. Disk boş olsa bile, veri diski başına bir VHD kullanın.

> [!NOTE]
> Hangi işletim sistemini kullandığınızdan bağımsız olarak, çözümün gerektirdiği en az sayıda veri diski ekleyin. Müşteriler dağıtım sırasında görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler.

> [!IMPORTANT]
> Plandaki her VM Görüntüsü aynı sayıda veri diski olmalıdır.

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkların tasarlanması, oluşturulması ve test edilmesi zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir. Çözüm etki alanınıza ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

* [Azure Hizmetlerinin](https://azure.microsoft.com/services/) temel anlayışı
* Azure uygulamaları nasıl [tasarlar ve tasarlar](https://azure.microsoft.com/solutions/architecture/)
* [Azure Sanal Makineler,](https://azure.microsoft.com/services/virtual-machines/) [Azure Depolama](https://azure.microsoft.com/services/?filter=storage) ve [Azure Ağı](https://azure.microsoft.com/services/?filter=networking) çalışma bilgisi
* [Azure Kaynak Yöneticisi'nin](https://azure.microsoft.com/features/resource-manager/) çalışma bilgisi
* [JSON](https://www.json.org/) Çalışma Bilgisi

## <a name="suggested-tools--optional"></a>Önerilen araçlar – isteğe bağlı

VM'leri ve VH'leri yönetmenize yardımcı olmak için aşağıdaki komut dosyası ortamlarından birini kullanmayı düşünün:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
* [Azure CLI](https://code.visualstudio.com/)

Ayrıca, geliştirme ortamınıza aşağıdaki araçları eklemeyi düşünün:

* [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Visual Studio Code](https://code.visualstudio.com/)
  * Uzantı: [Azure Kaynak Yöneticisi Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  * Uzantısı: [Güzelleştirmek](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  * Uzatma: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

[Azure Geliştirici Araçları](https://azure.microsoft.com/product-categories/developer-tools/) sayfasındaki kullanılabilir araçları gözden geçirin ve Visual Studio kullanıyorsanız Visual Studio [Marketplace'i](https://marketplace.visualstudio.com/)inceleyin.

## <a name="create-a-vm-image-using-an-approved-base"></a>Onaylı bir taban kullanarak VM görüntüsü oluşturma

> [!NOTE]
> Kendi binanızda oluşturduğunuz bir görüntüyü kullanarak sanal makine teknik varlıklarınızı oluşturmak için [kendi resminizi kullanarak Bir VM Oluşturun'e](#create-a-vm-using-your-own-image)gidin.

Bu bölümde, Uzak Masaüstü Protokolü 'nü (RDP) kullanma, VM için bir boyut seçme, en son Windows güncelleştirmelerini yükleme ve VHD görüntüsünü genelleme gibi onaylanmış bir tabanı kullanmanın çeşitli yönleri açıklanmaktadır.

Aşağıdaki bölümlerde daha çok windows tabanlı VHD'ler ele alınalmaktadır. Linux tabanlı VHD'ler oluşturma hakkında daha fazla bilgi için [Azure tarafından onaylanan dağıtımlar hakkında Linux'a](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.

> [!WARNING]
> Önceden yapılandırılmış, onaylanmış bir işletim sistemi içeren bir VM oluşturmak için Azure'u kullanmak için bu konuyla ilgili kılavuzu izleyin. Bu, çözümünüzle uyumlu değilse, onaylanmış bir işletim sistemi kullanarak şirket içi bir VM oluşturmak ve yapılandırmak mümkündür. Daha sonra, Azure'a yüklemek için [Windows VHD veya VHDX'te](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)açıklandığı gibi yapılandırAbilir ve yüklemeye hazırlayabilirsiniz.

### <a name="select-an-approved-base"></a>Onaylı bir taban seçin

Temel olarak Windows işletim sistemini veya Linux'u seçin.

#### <a name="windows"></a>Windows

Windows tabanlı VM görüntünüz için işletim sistemi VHD, Windows Server veya SQL Server içeren Azure onaylı bir temel görüntüye dayanmalıdır. Başlamak için, Azure portalından aşağıdaki resimlerden birinden bir VM oluşturun:

* Windows Server ([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
* [SQL Server 2014](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Kurumsal, Standart, Web)
* [SQL Server 2012 SP2](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Kurumsal, Standart, Web)

> [!NOTE]
> Geçerli Azure portalını veya Azure PowerShell'i kullanıyorsanız, 8 Eylül 2014 tarihinde yayınlanan ve daha sonra onaylanan Windows Server görüntüleri onaylanır.

#### <a name="linux"></a>Linux

Azure, bir dizi onaylı Linux dağıtımı sunar. Geçerli bir liste için Azure [tarafından onaylanan dağıtımlar hakkında Linux'a](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.

### <a name="create-vm-in-the-azure-portal"></a>Azure portalında VM oluşturma

[Azure portalında](https://ms.portal.azure.com/)temel VM görüntüsünü oluşturmak için aşağıdaki adımları izleyin:

1. VM teklifinizi yayınlamak için kullanmak istediğiniz Azure aboneliğiyle ilişkili Microsoft hesabıyla [Azure portalında](https://ms.portal.azure.com/) oturum açın.
2. Yeni bir kaynak grubu oluşturun ve **Kaynak grup adınızı,** **Aboneliğinizi**ve **Kaynak grup konumunuzu**sağlayın. Ayrıntılar için [kaynakları yönet'e](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)bakın.
3. Sanal makineler ayrıntıları sayfasını görüntülemek için soldaki **Sanal makineleri** seçin.
4. Sanal makine deneyimi **oluşturmak**için + **Ekle'yi** seçin.
5. Açılan listeden resmi seçin veya kullanılabilir tüm sanal makine görsellerini aramak veya göz atmak için **tüm genel ve özel resimlere göz at'ı** tıklatın.
6. Aşağıdaki önerileri kullanarak dağıtmak için VM boyutunu seçin:
    * Eğer şirket içinde VHD geliştirmeyi planlıyorsanız, boyutu önemli değildir. Küçük VM'lerden birini kullanmayı düşünün.
    * Görüntüyü Azure'da geliştirmeyi planlıyorsanız, seçili görüntü için önerilen VM boyutlarından birini kullanmayı düşünün.

7. **Diskler** bölümünde, **Gelişmiş** bölümü genişletin ve **Kullanım yönetilen diskler** seçeneğini **No**olarak ayarlayın.
8. VM'yi oluşturmak için gerekli diğer ayrıntıları sağlayın.
9. Seçimlerinizi gözden geçirmek için **Gözden Geçir + oluştur'u** seçin. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin.

Azure, belirttiğiniz sanal makineyi sağlamabaşlar. Soldaki **Sanal Makineler** sekmesini seçerek ilerlemesini izleyebilirsiniz. Oluşturulduktan sonra durum **Çalışan**olarak değişecektir.

Yeni Azure tabanlı VHD'nizi oluştururken güçlük çekiyorsanız, [VHD oluşturma (SSS) sırasında sık karşılaşılan sorunlara](https://aka.ms/VHDcreationIssues)bakın.

### <a name="connect-to-your-azure-vm"></a>Azure VM'nize bağlanın

Bu bölümde, Azure'da oluşturduğunuz VM'ye nasıl bağlanıp oturum açabileceğiniz açıklanmaktadır. Başarıyla bağlandıktan sonra, VM ile ana bilgisayar sunucusunda yerel olarak oturum açmış gibi çalışabilirsiniz.

#### <a name="connect-to-a-windows-based-vm"></a>Windows tabanlı bir VM'ye bağlanma

Azure'da barındırılan Windows tabanlı VM'ye bağlanmak için uzak masaüstü istemcisini kullanın. Windows'un çoğu sürümü, uzak masaüstü protokolü (RDP) için yerel olarak destek içerir. Diğer işletim sistemleri için, [Uzak Masaüstü istemcilerinde](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)istemciler hakkında daha fazla bilgi bulabilirsiniz.

Bu makalede, VM'nize bağlanmak için yerleşik Windows RDP desteğinin nasıl kullanılacağı ayrıntılı olarak anlatılıyor: [Windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum açabilirsiniz.](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)

> [!TIP]
> İşlem sırasında güvenlik uyarıları alabilirsiniz. Örneğin, ".rdp dosyası bilinmeyen bir yayımcıdan geliyor" veya "Kullanıcı kimliğiniz doğrulanamıyor" gibi uyarılar. Bu uyarıları yoksaymak güvenlidir.

#### <a name="connect-to-a-linux-based-vm"></a>Linux tabanlı bir VM'ye bağlanma

Linux tabanlı bir VM'ye bağlanmak için güvenli bir kabuk protokolü (SSH) istemcisine ihtiyacınız vardır. Aşağıdaki adımlarücretsiz [PuTTY](https://www.ssh.com/ssh/putty/) SHH terminalini kullanır.

1. [Azure portalına](https://ms.portal.azure.com/)gidin.
2. **Sanal makineleri**arayın ve seçin.
3. Bağlanmak istediğiniz VM'yi seçin.
4. Zaten çalışmıyorsa VM'yi başlatın.
5. **Genel Bakış** sayfasını açmak için VM'nin adını seçin.
6. VM'nizin Genel IP adresine ve DNS adını not edin (bu değerler ayarlı değilse, [bir ağ arabirimi oluşturmanız](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface)gerekir).
7. PuTTY uygulamasını açın.
8. PuTTY Configuration iletişim kutusunda, VM'nizin IP adresini veya DNS adını girin.

    :::image type="content" source="media/avm-putty.png" alt-text="PuTTY terminal ayarlarını gösterir. Ana Bilgisayar Adı veya IP adresi ve Bağlantı Noktası kutuları vurgulanır.":::

9. PuTTY terminalini açmak için **Aç'ı** seçin.
10. İstendiğinde, Linux VM hesabınızın hesap adını ve parolasını girin.

Bağlantı sorunlarınız varsa, SSH istemcinizin belgelerine bakın. Örneğin, [Bölüm 10: Yaygın hata iletileri](https://www.ssh.com/ssh/putty/putty-manuals).

Sağlanan bir Linux VM'sine masaüstü nasıl ekleyeceğiniz gibi ayrıntılar için, [Azure'daki bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü Yükle ve yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/use-remote-desktop)konusuna bakın.

## <a name="create-a-vm-using-your-own-image"></a>Kendi resminizi kullanarak bir VM oluşturun

Bu bölümde, kullanıcı tarafından sağlanan sanal makine (VM) görüntüsünün nasıl oluşturulup dağıtılanın. Bunu, Azure tarafından dağıtılan sanal sabit diskten (VHD) işletim sistemi ve veri diski VHD görüntüleri sağlayarak yapabilirsiniz.

> [!NOTE]
> İsteğe bağlı olarak onaylanmış bir temel görüntü kullanmak için, [onaylanmış bir taban kullanarak VM görüntüsü oluştur'daki](#create-a-vm-image-using-an-approved-base)yönergeleri izleyin.

1. Resimlerinizi Azure Depolama hesabına yükleyin.
2. VM görüntüsünü dağıtın.
3. VM görüntüsünü yakalayın.

### <a name="upload-your-images-to-an-azure-storage-account"></a>Resimlerinizi bir Azure depolama hesabına yükleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Genelleştirilmiş işletim sisteminiz VHD'nizi ve veri diski VHD'lerinizi Azure depolama hesabınıza yükleyin.

### <a name="deploy-your-image"></a>Resminizi dağıtma

Azure portalını veya Azure PowerShell'i kullanarak resminizi oluşturun.

#### <a name="deploy-using-the-azure-portal"></a>Azure portalı kullanarak dağıtma

1. Ana sayfada kaynak **oluştur'u,**"Şablon Dağıtımı"nı arayın'ı ve **Oluştur'u**seçin.
2. **Editörde kendi şablonunuzu oluştur'u**seçin.

    :::image type="content" source="media/avm-custom-deployment.png" alt-text="Özel dağıtım sayfasını gösterir.":::

3. Bu [JSON şablonunu](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-deploy-json-template) editöre yapıştırın ve **Kaydet'i**seçin.
4. Görüntülenen **Özel dağıtım** özelliği sayfaları için parametre değerlerini sağlayın.

    | Parametre | Açıklama |
    | ------------ | ------------- |
    | Kullanıcı Depolama Hesap Adı | Hücre 2'den gelen içerik |
    | Kullanıcı Depolama Konteyner Adı | Genelleştirilmiş VHD'nin bulunduğu depolama hesabı adı |
    | Genel IP için DNS Adı | Genel IP DNS adı. Teklif dağıtıldıktan sonra Azure portalındaki genel IP adresinin DNS adını tanımlayın. |
    | Yönetici Kullanıcı Adı | Yeni VM için yönetici hesabının kullanıcı adı |
    | Yönetici Parolası | Yeni VM için yönetici hesabının parolası |
    | İşletim Sistemi Türü | VM işletim sistemi: Windows veya Linux |
    | Abonelik Kimliği | Seçili aboneliğin tanımlayıcısı |
    | Konum | Dağıtımın coğrafi konumu |
    | VM Boyutu | [Azure VM boyutu](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), örneğin Standard_A2 |
    | Genel IP Adresi Adı | Herkese açık IP adresinizin adı |
    | VM Adı | Yeni VM'nin adı |
    | Sanal Ağ Adı | VM tarafından kullanılan sanal ağın adı |
    | NIC Adı | Sanal ağı çalıştıran ağ arabirim kartının adı |
    | VHD URL | Komple Işletim Sistemi Disk VHD URL |
    |  |  |

5. Bu değerleri tedarik ettikten sonra **Satın al'ı**seçin.

Azure dağıtımına başlar. Belirtilen depolama hesabı yolunda belirtilen yönetilmeyen VHD ile yeni bir VM oluşturur. Portalın sol tarafındaki **Sanal Makineler'i** seçerek Azure portalındaki ilerlemeyi izleyebilirsiniz. VM oluşturulduğunda, durum Çalıştırmaya Başlamak'tan itibaren değişir.

#### <a name="deploy-using-azure-powershell"></a>Azure PowerShell’i kullanarak dağıtma

```powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

### <a name="capture-the-vm-image"></a>VM görüntüsünü yakalama

Yaklaşımınıza karşılık gelen aşağıdaki yönergeleri kullanın:

* Azure PowerShell: [Azure VM'den yönetilmeyen bir VM görüntüsü nasıl oluşturulur?](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)
* Azure CLI: [Sanal makine veya VHD görüntüsü nasıl oluşturulur?](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image)
* API: [Sanal Makineler - Yakalama](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

## <a name="configure-the-virtual-machine"></a>Sanal makineyi yapılandırma

Bu bölümde, azure VM'yi nasıl boyutlandırıp güncelleştireceğimize ve genelleştireceğimize açıklanmaktadır. Bu adımlar, VM'nizi Azure Marketi'nde dağıtılacak şekilde hazırlamak için gereklidir.

### <a name="sizing-the-vhds"></a>VHD'lerin boyutlandırılması

Bir işletim sistemiyle (ve isteğe bağlı olarak ek hizmetlerle) önceden yapılandırılmış VM'lerden birini seçtiyseniz, standart bir Azure VM boyutu seçtiniz. Çözümünüzü önceden yapılandırılmış bir işletim sistemiyle başlatmak önerilen yaklaşımdır. Ancak, bir işletim sistemi'ni el ile yüklüyorsanız, birincil VHD'nizi VM görüntünüzde boyutlandırmanız gerekir:

* Windows için, işletim sistemi VHD 127-128 GB sabit formatlı VHD olarak oluşturulmalıdır.
* Linux için bu VHD 30-50 GB sabit formatlı VHD olarak oluşturulmalıdır.

Fiziksel boyutu 127-128 GB'dan küçükse, VHD Genişletilebilir (seyrek/dinamik) olmalıdır. Sağlanan temel Windows ve SQL Server görüntüleri zaten bu gereksinimleri karşılar, bu nedenle VHD'nin biçimini veya boyutunu değiştirmeyin.

Veri diskleri 1 TB kadar büyük olabilir. Boyutuna karar verirken, müşterilerin dağıtım sırasında görüntü içindeki VHD'leri yeniden boyutlandıramayacağını unutmayın. Veri diski VHD'leri sabit biçimli VHD'ler olarak oluşturulmalıdır. Onlar da Genişletilebilir (seyrek / dinamik) olmalıdır. Veri diskleri başlangıçta boş olabilir veya veri içerebilir.

### <a name="install-the-most-current-updates"></a>En güncel güncelleştirmeleri yükleme

İşletim sistemi VM'lerinin temel görüntüleri, yayımlandığı tarihe kadar en son güncelleştirmeleri içermelidir. Oluşturduğunuz vhd işletim sistemini yayımlamadan önce, işletim sistemini ve yüklü tüm hizmetleri en son güvenlik ve bakım yamaları ile güncelleştirdiğinizden emin olun.

Windows Server için **Güncelleştirmeleri Denetle** komutunu çalıştırın.

Linux dağıtımları için güncelleştirmeler genellikle bir komut satırı aracı veya grafik yardımcı programı aracılığıyla indirilir ve yüklenir. Örneğin, Ubuntu Linux işletim sistemi güncelleştirmek için [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) komutu ve [Update Manager](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) aracını sağlar.

### <a name="perform-additional-security-checks"></a>Ek güvenlik denetimleri gerçekleştirin

Azure Marketi'ndeki çözüm görselleriniz için yüksek düzeyde güvenlik koruyun. Aşağıdaki makale, size yardımcı olacak güvenlik yapılandırmaları ve yordamlarının bir denetim listesini sağlar: [Azure Marketi Görselleri için Güvenlik Önerileri.](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images) Bu önerilerden bazıları Linux tabanlı görüntülere özgüdir, ancak çoğu herhangi bir VM görüntüsü için geçerlidir.

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Özel yapılandırma ve zamanlanmış görevleri gerçekleştirme

Ek yapılandırma gerekiyorsa, dağıtıldıktan sonra VM'de son değişiklikleri yapmak için başlangıçta çalışan zamanlanmış bir görev kullanın. Ayrıca aşağıdaki önerileri göz önünde bulundurun:

* Bir kez çalıştırılamalı bir görevse, görev başarıyla tamamlandıktan sonra kendisini silmelidir.
* Yapılandırmalar C veya D dışındaki sürücülere güvenmemelidir, çünkü yalnızca bu iki sürücünün her zaman var olduğu garanti edilir (C sürücüsü işletim sistemi dir ve D sürücüsü geçici yerel disktir).

Linux özelleştirmeleri hakkında daha fazla bilgi için, [Linux için Sanal makine uzantıları ve özellikleri](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux)ne bakın.

## <a name="generalize-the-image"></a>Görüntüyü genelleştirin

Azure Marketi'ndeki tüm görüntüler genel bir şekilde yeniden kullanılabilir olmalıdır. Bunu başarmak için, işletim sistemi VHD genelleştirilmiş olması gerekir, bir VM tüm örnek özel tanımlayıcıları ve yazılım sürücüleri kaldırır bir işlem.

### <a name="windows"></a>Windows

Windows işletim sistemi diskleri [sysprep aracı](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)ile genelleştirilmiştir. Daha sonra işletim sistemi güncelleştirmeveya yeniden yapılandırma, sysprep yeniden gerekir.

> [!WARNING]
> Güncelleştirmeler otomatik olarak çalışabilir, sysprep çalıştırdıktan sonra, dağıtılana kadar VM kapatın. Bu kapatma, sonraki güncelleştirmelerin işletim sisteminde veya yüklü hizmetlerde örnek özel değişiklikler yapmasını önler. Sysprep çalıştırma hakkında daha fazla bilgi için, [bir VHD genelleştirmek için Adımlar'a](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep)bakın.

### <a name="linux"></a>Linux

Aşağıdaki işlem bir Linux VM genelleştirir ve ayrı bir VM olarak yeniden dağılar. Ayrıntılar için, [sanal makine veya VHD görüntüsünü nasıl oluşturabilirsiniz'](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image)a bakın. "Yakalanan görüntüden VM oluşturma" bölümüne ulaştığınızda durabilirsiniz.

1. **Azure Linux aracısını kaldırma**

    1. Bir SSH istemcisi kullanarak Linux VM'nize bağlanın.
    2. SSH penceresinde aşağıdaki komutu girin: `sudo waagent -deprovision+user`.
    3. Devam etmek için **Y** yazın (onay adımını önlemek için önceki komuta **kuvvet** parametresi ekleyebilirsiniz).
    d. Komut tamamlandıktan sonra, SSH istemcisini kapatmak için **Çıkış** yazın.

2. **Sanal makineyi durdurma**

    1. Azure portalında kaynak grubunuzu (RG) seçin ve VM'yi ayırın.
    2. VHD'niz artık genelleştirilmiştir ve bu VHD'yi kullanarak yeni bir VM oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yeni Azure tabanlı VHD'nizi oluştururken zorlukla karşılaştıysanız, [VHD oluşturma sırasında sık karşılaşılan sorunlara](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-common-vhd-creation-issues)bakın.

Aksi durumda:.

* [VM resminizi onaylamak,](https://docs.microsoft.com/azure/marketplace/partner-center-portal/get-sas-uri) *Azure Sertifikalı Sertifikalandırma aracıiçin sertifika test aracını* nereden alacağınız ve VM resminizi onaylamak için nasıl kullanacağınız da dahil olmak üzere Azure Marketi sertifikası için vm görüntüsünü nasıl test edip gönderebileceğinizi açıklar.
