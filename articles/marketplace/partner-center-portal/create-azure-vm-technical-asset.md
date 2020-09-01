---
title: Azure sanal makinesi teknik varlıkları oluşturma
description: Azure Marketi için bir sanal makine (VM) teklifiyle ilgili teknik varlıkları oluşturma ve yapılandırma hakkında bilgi edinin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 08/14/2020
ms.openlocfilehash: 496cd4aeb96ca1849e950331658014d91dc6d6ba
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89143935"
---
# <a name="create-azure-virtual-machine-technical-assets"></a>Azure sanal makinesi teknik varlıkları oluşturma

Sanal makine (VM) görüntülerinizi Azure Marketi 'ne yayımlarken, Azure ekibi, önyükleme, güvenlik ve Azure uyumluluğunu sağlamak için VM görüntüsünü doğrular. Yüksek kaliteli testlerin herhangi biri başarısız olursa, yayımlama hata ve olası [düzeltme adımlarını](https://docs.microsoft.com/azure/marketplace/partner-center-portal/vm-certification-issues-solutions)içeren bir iletiyle başarısız olur.

Bu makalede, Azure Marketi için bir sanal makine (VM) teklifi için teknik varlıkların nasıl oluşturulacağı ve yapılandırılacağı açıklanmaktadır. VM iki bileşen içerir: işletim sistemi sanal sabit diski (VHD) ve isteğe bağlı ilişkili veri diskleri VHD 'leri:

1. **İşletim sistemi VHD 'si** : teklifinizle birlikte dağıtılan işletim sistemini ve çözümü içerir. VHD 'yi hazırlama işlemi, Linux tabanlı, Windows tabanlı veya özel tabanlı bir VM olmasına bağlı olarak farklılık gösterir.

2. **Veri diski VHD** 'leri – bir VM için ayrılmış, kalıcı depolama. Kalıcı bilgileri depolamak için işletim sistemi VHD 'sini (örneğin, C: sürücüsü) kullanmayın.

Bir VM görüntüsü, bir işletim sistemi diski ve en fazla 16 veri diski içerir. Disk boş olsa bile, veri diski başına bir VHD kullanın.

> [!NOTE]
> Kullandığınız işletim sisteminden bağımsız olarak yalnızca çözüm için gereken en az sayıda veri diski ekleyin. Müşteriler, dağıtım sırasında bir görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler.

> [!IMPORTANT]
> Bir plandaki her sanal makine görüntüsünün aynı sayıda veri diski olmalıdır.

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkların tasarlanmasına, oluşturulmasına ve test edilmesine zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir. Çözüm etki alanınız ' ne ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

- [Azure hizmetlerini](https://azure.microsoft.com/services/) temel olarak anlama
- [Azure uygulamalarını tasarlama ve mimarın](https://azure.microsoft.com/solutions/architecture/)
- [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage)ve [Azure ağı](https://azure.microsoft.com/services/?filter=networking) ile çalışma hakkında bilgi
- [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) çalışma bilgileri
- [JSON](https://www.json.org/) hakkında çalışma bilgisi

### <a name="optional-suggested-tools"></a>İsteğe bağlı önerilen araçlar

VM 'Lerin ve VHD 'lerin yönetilmesine yardımcı olmak için aşağıdaki komut dosyası ortamlarının birini kullanmayı göz önünde bulundurun:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [Azure CLI](https://code.visualstudio.com/)

Ayrıca, aşağıdaki araçları geliştirme ortamınıza eklemeyi göz önünde bulundurun:

- [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-a-vm-image-using-an-approved-base"></a>Onaylanan bir taban kullanarak VM görüntüsü oluşturma

Kendi şirketinizde oluşturduğunuz bir görüntüyü kullanarak sanal makine teknik varlıklarınızı oluşturmak için, aşağıda [onaylanan bir temel kullanarak VM görüntüsü oluşturma](#create-a-vm-image-using-an-approved-base) bölümüne bakın.

Bu bölümde, Uzak Masaüstü Protokolü (RDP), sanal makine için boyut seçme, en son Windows güncelleştirmelerini yükleme ve VHD görüntüsünü Genelleştirme gibi, onaylı bir temel kullanmanın çeşitli yönleri açıklanmaktadır.

Önceden yapılandırılmış, onaylı bir işletim sistemi içeren bir VM oluşturmak için Azure 'u kullanmak üzere bu makaledeki yönergeleri izleyin. Çözümünüz ile uyumlu değilse, onaylanmış bir işletim sistemi kullanarak şirket içi bir VM oluşturmak ve yapılandırmak mümkündür. Daha sonra, [Azure 'a yüklemek üzere bir WINDOWS VHD veya vhdx hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)bölümünde açıklandığı gibi karşıya yükleme için bu uygulamayı yapılandırabilir ve hazırlarsınız.

### <a name="select-an-approved-base-image"></a>Onaylanan bir temel görüntü seçin

Tabanınız olarak Windows işletim sistemi veya Linux ' u seçin.

VM Görüntünüzün işletim sistemi VHD 'SI, Windows Server veya SQL Server içeren bir Azure onaylı temel görüntüye dayalı olmalıdır.

Görüntünüzü güncelleştirmelerle yeniden yayımlamak için bir istek gönderdiğinizde, bölüm numarası doğrulama test çalışması başarısız olabilir. Bu örnekte, görüntünüz onaylanmayacaktır.

Bu hata, başka bir yayımcıya ait olan ve görüntüyü güncelleştirdiğiniz bir temel görüntü kullandığınızda oluşur. Bu durumda, görüntünüzü yayımlamanıza izin verilmez.

Bu sorunu onarmak için Azure Marketi 'nden en son görüntünüzü alın ve bu görüntüde değişiklik yapın. Görüntünüzü arayabilecek onaylanan temel görüntüleri görüntülemek için aşağıdakilere bakın:

#### <a name="windows"></a>Windows

- Windows Server ([2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 R2 datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
- SQL Server 2019 ([Enterprise](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview), [Standard](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview), [Web](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview))
- SQL Server 2014 ([Enterprise](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Standard](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Web](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance))
- SQL Server 2012 SP2 ([Enterprise](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Standard](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Web](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance))

#### <a name="linux"></a>Linux

Azure, onaylı bir Linux dağıtımları yelpazesi sunar. Geçerli bir liste için bkz. [Azure tarafından onaylanan dağıtımlara Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).

### <a name="create-vm-on-the-azure-portal"></a>Azure portal VM oluşturma

[Azure Portal](https://ms.portal.azure.com/)temel VM görüntüsünü oluşturmak için aşağıdaki adımları izleyin:

1. VM teklifinizi yayımlamak için kullanmak istediğiniz Azure aboneliğiyle ilişkili Microsoft hesabı [Azure Portal](https://ms.portal.azure.com/) oturum açın.
2. Yeni bir kaynak grubu oluşturun ve **kaynak grubu adınızı**, **aboneliğinizi**ve **kaynak grubu konumunu**belirtin. Ayrıntılar için bkz. [kaynakları yönetme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

    :::image type="content" source="media/vm/create-resource-group.png" alt-text="Kaynak grubu oluşturma başlangıcını gösterir.":::

3. Sanal makineler Ayrıntılar sayfasını göstermek için sol gezinti çubuğunda **sanal makineler** ' i seçin.
4. **+ Ekle** ' yi seçerek **sanal makine oluşturma deneyimini**açın.
5. Açılan listeden görüntüyü seçin veya tüm kullanılabilir sanal makine görüntülerini aramak veya taramak için **tüm ortak ve özel görüntülere gözatıp '** yi seçin. Örnek:

    :::image type="content" source="media/vm/create-resource-group-example.png" alt-text="Örnek bir VM görüntüsünü gösterir.":::

6. Aşağıdaki önerileri kullanarak dağıtılacak VM 'nin boyutunu seçin:
    1. VHD 'yi şirket içinde geliştirmeyi planlıyorsanız, boyut önemi yoktur. Küçük VM 'lerden birini kullanmayı göz önünde bulundurun.
    2. Görüntüyü Azure 'da geliştirmeyi planlıyorsanız, seçilen görüntü için önerilen VM boyutlarından birini kullanmayı göz önünde bulundurun.

    :::image type="content" source="media/vm/create-virtual-machine.png" alt-text="VM boyutu seçimini gösterir.":::

7. **Diskler** bölümünde, **Gelişmiş** bölümünü genişletin ve **yönetilen diskleri kullan** seçeneğini **Hayır**olarak ayarlayın.

    :::image type="content" source="media/vm/use-managed-disks.png" alt-text="Yönetilen diskleri kullanma seçeneğini gösterir.":::

8. VM oluşturmak için gereken diğer ayrıntıları sağlayın.
9. Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

Azure, belirttiğiniz sanal makineyi sağlamaya başlar. Sol taraftaki **sanal makineler** sekmesini seçerek ilerlemesini izleyebilirsiniz. Oluşturulduktan sonra durum **çalışıyor**olarak değişir.

### <a name="create-a-generation-2-vm-on-the-azure-portal"></a>Azure portal 2. nesil VM oluşturma

Azure portal 'de 2. nesil (Gen2) bir VM oluşturun.

1. [https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.
2. **Kaynak oluştur**’u seçin.
3. Sol taraftaki Azure Marketi 'nden **Tümünü gör** ' ü seçin.
4. Gen2 destekleyen bir görüntü seçin.
5. **Oluştur**’u seçin.
6. **Gelişmiş** sekmesinde, **VM oluşturma** bölümünde **Gen 2** seçeneğini belirleyin.
7. **Temel bilgiler** sekmesinde, **örnek ayrıntıları**altında **Boyut** ' a gidin ve **VM boyutu Seç** dikey penceresini açın.
8. [Desteklenen Gen 2 VM](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2#generation-2-vm-sizes) ve boyutunun önerilen boyutunu seçin.
9. VM oluşturma işleminin tamamlanabilmesi için [Azure Portal oluşturma akışından](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) ilerleyin.

    :::image type="content" source="media/vm/vm-generation.png" alt-text="VM oluşturmayı seçme seçeneğini gösterir.":::

## <a name="connect-to-your-azure-vm"></a>Azure VM 'nize bağlanma

Bu bölümde, Azure 'da oluşturduğunuz sanal makineye bağlanma ve oturum açma işlemlerinin nasıl yapılacağı açıklanmaktadır. Başarıyla bağlandıktan sonra, ana bilgisayar sunucusunda yerel olarak oturum açmış olduğunuz gibi VM ile çalışabilirsiniz.

### <a name="connect-to-a-windows-based-vm"></a>Windows tabanlı bir VM 'ye bağlanma

Azure 'da barındırılan Windows tabanlı VM 'ye bağlanmak için uzak masaüstü istemcisini kullanın. Windows 'un çoğu sürümü yerel olarak Uzak Masaüstü Protokolü (RDP) desteği içerir. Diğer işletim sistemleri için, [Uzak Masaüstü istemcilerindeki](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)istemciler hakkında daha fazla bilgi edinebilirsiniz.

Bu makalede, yerleşik Windows RDP desteğinin sanal makinenize bağlanmak için nasıl kullanılacağı açıklanır: [Windows çalıştıran bir Azure sanal makinesine bağlanma ve oturum açma](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

> [!TIP]
> İşlem sırasında güvenlik uyarıları alabilirsiniz. Örneğin, ". rdp dosyası bilinmeyen bir yayımcıdan" veya "Kullanıcı kimlik bilgileriniz doğrulanamıyor" gibi uyarılar. Bu uyarıları yoksaymak güvenlidir.

### <a name="connect-to-a-linux-based-vm"></a>Linux tabanlı bir VM 'ye bağlanma

Linux tabanlı bir VM 'ye bağlanmak için güvenli bir kabuk Protokolü (SSH) istemcisine ihtiyacınız vardır. Aşağıdaki adımlarda ücretsiz [Putty](https://www.ssh.com/ssh/putty/) Shh terminali kullanılır.

1. [Azure portalına](https://ms.portal.azure.com/) gidin.
2. Sanal makineleri arayın ve seçin.
3. Bağlanmak istediğiniz VM 'yi seçin.
4. Zaten çalışmıyorsa VM 'yi başlatın.
5. Genel Bakış sayfasını açmak için VM 'nin adını seçin.
6. VM 'nizin genel IP adresi ve DNS adı ' nı not edin (Bu değerler ayarlanmamışsa, [bir ağ arabirimi oluşturmanız](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface)gerekir.
7. PuTTY uygulamasını açın.
8. PuTTY yapılandırması iletişim kutusunda, sanal makinenizin IP adresini veya DNS adını girin.

    :::image type="content" source="media/vm/putty-configuration.png" alt-text="Ana bilgisayar adı ve bağlantı noktası alanlarını vurgulayarak PuTTY terminal ayarlarını gösterir.":::

9. PuTTY terminali açmak için **Aç** ' ı seçin.
10. İstendiğinde, Linux VM hesabınızın hesap adını ve parolasını girin.

## <a name="configure-the-virtual-machine"></a>Sanal makineyi yapılandırma

Bu bölümde bir Azure VM 'nin nasıl boyutlandırılacağını, güncelleştirileceğini ve genelleştirirsiniz. Bu adımlar, VM 'nizi Azure Marketi 'nde dağıtılacak şekilde hazırlamak için gereklidir.

### <a name="sizing-the-vhds"></a>VHD 'leri boyutlandırma

Aşağıdaki kurallar, işletim sistemi disk boyutuyla ilgili sınırlamalar için geçerlidir. Herhangi bir istek gönderdiğinizde, işletim sistemi disk boyutunun Linux veya Windows için sınırlamanın içinde olduğundan emin olun.

| İşletim Sistemi | Önerilen VHD boyutu |
| --- | --- |
| Linux | 30 ila 1023 GB |
| Windows | 30 ila 250 GB |

VM 'Ler temeldeki işletim sistemine erişime izin verse de VHD boyutunun VHD için yeterince büyük olduğundan emin olun. Diskler kesinti olmadan Genişletilebilir olmadığından, 30 ile 50 GB arasında bir disk boyutu kullanın &nbsp; .

| VHD boyutu | Dolu gerçek boyut | Çözüm |
| --- | --- | --- |
| >500 TB | yok | Özel durum onayı için destek ekibine başvurun. |
| 250-500 TB | >200 GB, blob boyutundan farklı | Özel durum onayı için destek ekibine başvurun. |

### <a name="install-the-most-current-updates"></a>En güncel güncelleştirmeleri yükler

İşletim sistemi VM 'lerinin temel görüntülerinin yayımlanan tarihleri en son güncelleştirmeleri içermesi gerekir. Oluşturduğunuz işletim sistemi VHD 'sini yayımlamadan önce, IŞLETIM sistemini ve yüklü tüm hizmetleri en son güvenlik ve bakım düzeltme ekleriyle güncelleştirdiğinizden emin olun.

- Windows Server için, güncelleştirmeleri denetle komutunu çalıştırın.
- Linux dağıtımları için güncelleştirmeler genellikle bir komut satırı aracı veya bir grafik yardımcı programıyla indirilir ve yüklenir. Örneğin Ubuntu Linux, işletim sistemini güncelleştirmek için [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) komutunu ve [Güncelleştirme Yöneticisi](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) aracını sağlar.

#### <a name="perform-additional-security-checks"></a>Ek güvenlik denetimleri gerçekleştirme

Azure Marketi 'nde çözüm görüntüleriniz için yüksek düzeyde güvenlik koruyun. Güvenlik yapılandırmalarının ve yordamların denetim listesi için bkz. [Azure Market görüntüleri Için güvenlik önerileri](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images). Bu önerilerin bazıları Linux tabanlı görüntülere özgüdür, ancak çoğu sanal makine görüntüsü için geçerlidir.

#### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Özel yapılandırma ve zamanlanmış görevler gerçekleştirme

Ek yapılandırmaya ihtiyacınız varsa, sanal makinede dağıtıldıktan sonra son değişiklikler yapmak için başlangıçta çalışan zamanlanmış bir görev kullanın. Ayrıca aşağıdakileri dikkate alın:

- Bir kez çalıştır görevi varsa, görev başarıyla tamamlandıktan sonra görevin kendisini silmesi gerekir.
- Yapılandırma C veya D dışındaki sürücülere güvenmemelidir, çünkü yalnızca bu iki sürücünün var olduğu garanti edilir (sürücü C, işletim sistemi diski ve D sürücüsü geçici yerel disktir).

Linux özelleştirmeleri hakkında daha fazla bilgi için bkz. [Linux Için sanal makine uzantıları ve özellikleri](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux).

## <a name="generalize-the-image"></a>Görüntüyü genelleştirin

Azure Marketi 'ndeki tüm görüntülerin genel bir biçimde yeniden kullanılabilir olması gerekir. Bunu başarmak için, işletim sistemi VHD 'SI, bir VM 'den örneğe özgü tüm tanımlayıcıları ve yazılım sürücülerini kaldıran bir işlem olan genelleştirilmelidir.

### <a name="for-windows"></a>Windows için

Windows işletim sistemi diskleri, [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) aracıyla genelleştirilmiştir. Daha sonra işletim sistemini güncelleştirirseniz veya yeniden yapılandırırsanız, Sysprep 'i yeniden çalıştırmanız gerekir.

> [!WARNING]
> Sysprep 'i çalıştırdıktan sonra, güncelleştirmeler otomatik olarak çalıştırılabileceğinden, sanal makineyi dağıtıldıktan sonra devre dışı bırakın. Bu kapanıyor, sonraki güncelleştirmelerin işletim sisteminde veya yüklü hizmetlerde örneğe özgü değişiklikler yapmasını önler. Sysprep 'i çalıştırma hakkında daha fazla bilgi için bkz. [BIR VHD Genelleştirme adımları](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep).

### <a name="for-linux"></a>Linux için

Aşağıdaki işlem bir Linux sanal makinesini genelleştirir ve ayrı bir VM olarak yeniden dağıtır. Ayrıntılar için bkz. [sanal makine veya VHD 'nin görüntüsünü oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image). "Yakalanan görüntüden sanal makine oluşturma" adlı bölüme ulaştığınızda durulabiliyor olabilirsiniz.

1. Azure Linux aracısını kaldırma

    1. SSH istemcisi kullanarak Linux VM 'nize bağlanma
    2. SSH penceresinde şu komutu girin: `sudo waagent –deprovision+user` .
    3. Devam etmek için Y yazın (onay adımını önlemek için önceki komuta-zorlama parametresini ekleyebilirsiniz).
    4. Komut tamamlandıktan sonra SSH istemcisini kapatmak için exit yazın.

2. Sanal makineyi durdurma

    1. Azure portal, kaynak grubunuzu (RG) seçin ve VM 'yi serbest olarak ayırın.
    2. VHD 'niz artık genelleştirilmiştir ve bu VHD 'YI kullanarak yeni bir VM oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Yeni Azure tabanlı VHD 'nizi oluştururken zorluk yaşıyorsanız, bkz. [VHD oluşturma sırasında yaygın sorunlar](common-issues-during-vhd-creation.md).
- Aksi takdirde, [VHD 'den dağıtılan test sanal makinesi (VM)](azure-vm-image-certification.md) , Azure Sertifikalı aracı Için sertifika test aracı 'nı nereden ALACAĞıNıZ ve sanal makine görüntünüzü onaylamak için nasıl kullanılacağı dahil olmak üzere Azure Marketi sertifikası IÇIN bir VM görüntüsünü test etme ve gönderme hakkında bilgiler sağlar.
