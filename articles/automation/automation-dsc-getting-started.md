---
title: Azure Otomasyon Durumu Yapılandırması ile başlayın
description: Azure Otomasyon Durumu Yapılandırması'nda (DSC) en yaygın görevlerin açıklaması ve örnekleri
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 04/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: eb285d15814f1105462c414541657d43b5371d98
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383233"
---
# <a name="get-started-with-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması ile başlayın

Bu makalede, yapılandırmaoluşturma, alma ve derleme, yönetilen makinelerde binme ve raporları görüntüleme gibi Azure Otomasyon Durumu Yapılandırması ile en yaygın görevlerin nasıl yapılacağını açıklamaktadır. Azure Otomasyon Durumu Yapılandırmasının ne olduğuna genel bir bakış için [bkz.](automation-dsc-overview.md) İstenen Durum Yapılandırması (DSC) belgeleri için [Windows PowerShell İstenen Durum Yapılandırması Yapılandırmagenel Bakış'a](/powershell/scripting/dsc/overview/overview)bakın.

Bu makalede, Azure Otomasyon Durumu Yapılandırmasını kullanmak için adım adım bir kılavuz verilmektedir. Bu makalede açıklanan adımları izleyerek zaten ayarlanmış bir örnek ortamı istiyorsanız, aşağıdaki Kaynak Yöneticisi şablonu kullanabilirsiniz: [Azure Otomasyon Yönetilen Düğüm şablonu.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-automation-configuration) Bu şablon, Azure Otomasyon Durumu Yapılandırması tarafından yönetilen bir Azure VM'si de dahil olmak üzere tamamlanmış bir Azure Otomasyon Durumu Yapılandırma ortamı nı ayarlar.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki örnekleri tamamlamak için aşağıdakiler gereklidir:

- Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
- Desteklenen bir [işletim sistemi](automation-dsc-overview.md#operating-system-requirements)çalıştıran bir Azure Kaynak Yöneticisi VM (Klasik değil). VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>DSC yapılandırması oluşturma

Düğümleri nasıl atadığınıza bağlı **olarak, Web Sunucusu** Windows Özelliği'nin (IIS) varlığını veya yokluğunu sağlayan basit bir [DSC yapılandırması](/powershell/scripting/dsc/configurations/configurations) oluşturursunuz.

1. [VSCode'u](https://code.visualstudio.com/docs) (veya herhangi bir metin düzenleyicisini) başlatın.
1. Aşağıdaki metni yazın:

    ```powershell
    configuration TestConfig
    {
        Node IsWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
            }
        }

        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
            }
        }
    }
    ```
1. Dosyayı **TestConfig.ps1**olarak kaydedin.

Bu yapılandırma, her düğüm bloğunda, **Web-Server** özelliğinin varlığını veya yokluğunu sağlayan [WindowsFeature kaynağında](/powershell/scripting/dsc/reference/resources/windows/windowsfeatureresource)bir kaynak çağırır.

## <a name="importing-a-configuration-into-azure-automation"></a>Azure Otomasyonuna yapılandırma alma

Ardından, yapılandırmayı Otomasyon hesabına aktarın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** sayfasında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** seçin.
1. Durum **yapılandırması (DSC)** **sayfasında, Yapılandırmalar** sekmesini tıklatın ve sonra **+ Ekle'yi**tıklatın.
1. **Yapılandırmayı İçe Aktar** sayfasında, `TestConfig.ps1` bilgisayarınızdaki dosyaya göz atın.

   ![**Import Configuration** bıçağının ekran görüntüsü](./media/automation-dsc-getting-started/AddConfig.png)

1. **Tamam**'a tıklayın.

## <a name="viewing-a-configuration-in-azure-automation"></a>Azure Otomasyonu'nda yapılandırmayı görüntüleme

Bir yapılandırma aldıktan sonra, azure portalında görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** sayfasında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** seçin.
1. Durum **yapılandırması (DSC)** sayfasında, **Yapılandırmalar** sekmesini tıklatın ve ardından **TestConfig'i** tıklatın (bu, önceki yordamda içe aktardığınız yapılandırmanın adıdır).
1. **TestConfig Yapılandırma** sayfasında, **yapılandırma kaynağını görüntüle'yi**tıklatın.

   ![TestConfig yapılandırma bıçağının ekran görüntüsü](./media/automation-dsc-getting-started/ViewConfigSource.png)

   Yapılandırma için PowerShell kodunu görüntüleyen bir **TestConfig Yapılandırma kaynak** sayfası açılır.

## <a name="compiling-a-configuration-in-azure-automation"></a>Azure Otomasyonu'nda yapılandırma derleme

Bir düğüme istenilen durumu uygulayabilmek için, bu durumu tanımlayan bir DSC yapılandırmasının bir veya daha fazla düğüm yapılandırmasında (MOF belgesi) derlenmeli ve Otomasyon DSC Çekme Sunucusu'na yerleştirilmelidir. Azure Otomasyon Durumu Yapılandırmasında yapılandırmaları derlemenin daha ayrıntılı bir açıklaması için [bkz.](automation-dsc-compile.md)
Yapılandırmaları derleme hakkında daha fazla bilgi için Bkz. [DSC Yapılandırmaları.](/powershell/scripting/dsc/configurations/configurations)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** sayfasında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında, **Yapılandırmalar** sekmesini tıklatın ve ardından **TestConfig'i** (daha önce alınan yapılandırmanın adı) tıklatın.
1. **TestConfig Yapılandırma** **sayfasında, Derle'yi**tıklatın ve sonra **Evet'i**tıklatın. Bu bir derleme işi başlar.

   ![Derleme düğmesini vurgulayan TestConfig yapılandırma sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Azure Otomasyonu'nda bir yapılandırma derlediğinizde, oluşturulan düğüm yapılandırmaM'daki MOF dosyalarını çekme sunucusuna otomatik olarak dağır.

## <a name="viewing-a-compilation-job"></a>Derleme işini görüntüleme

Bir derlemeye başladıktan sonra, **Yapılandırma** sayfasındaki **Derleme İşler** döşemesinde görüntüleyebilirsiniz. **Derleme İşleri** döşemesi şu anda çalışan, tamamlanmış ve başarısız olan işleri gösterir. Bir derleme iş sayfası açtığınızda, karşılaşılan hatalar veya uyarılar, yapılandırmada kullanılan giriş parametreleri ve derleme günlükleri de dahil olmak üzere bu iş hakkında bilgi gösterir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** sayfasında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında, **Yapılandırmalar** sekmesini tıklatın ve ardından **TestConfig'i** (daha önce alınan yapılandırmanın adı) tıklatın.
1. **Derleme işleri**altında, görüntülemek istediğiniz derleme işini seçin. **Derleme İş** sayfası, derleme işinin başlatıldıdığı tarihle birlikte etiketlenmiş olarak açılır.

   ![Derleme İş sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/CompilationJob.png)

1. İş le ilgili daha fazla ayrıntıyı görmek için **Derleme İş** sayfasındaki herhangi bir döşemeye tıklayın.

## <a name="viewing-node-configurations"></a>Düğüm yapılandırmalarını görüntüleme

Derleme işinin başarıyla tamamlanması, bir veya daha fazla yeni düğüm yapılandırması oluşturur. Düğüm yapılandırması, çekme sunucusuna dağıtılan ve bir veya daha fazla düğüm tarafından çekilmeye ve uygulanmaya hazır bir MOF belgesidir. **Durum yapılandırması (DSC)** sayfasında Otomasyon hesabınızdaki düğüm yapılandırmalarını görüntüleyebilirsiniz. Düğüm yapılandırmasının formu `ConfigurationName.NodeName`olan bir adı vardır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** bıçağında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında **Derlenmiş yapılandırmalar** sekmesini tıklatın.

   ![Derlenen Yapılandırmalar sekmesinin ekran görüntüsü](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması ile yönetim için bir Azure VM'ye binme

Azure VM'leri (hem klasik hem de Kaynak Yöneticisi), şirket içi VM'leri, Linux makinelerini, AWS VM'leri ve şirket içi fiziksel makineleri yönetmek için Azure Otomasyon Durumu Yapılandırmasını kullanabilirsiniz. Bu makalede, yalnızca Azure Kaynak Yöneticisi VM'leri yerleşik olarak nasıl atabildiğinizi öğrenirsiniz. Diğer makine türlerine binme hakkında daha fazla bilgi için Azure [Otomasyon Durumu Yapılandırması tarafından yönetilen Onboarding makineleri](automation-dsc-onboarding.md)bölümüne bakın.

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması tarafından yönetim için bir Azure Kaynak Yöneticisi VM'de

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** bıçağında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında, **Düğümler** sekmesinde **ise + Ekle'yi**tıklatın.

   ![Azure VM ekle düğmesini vurgulayan DSC Düğümleri sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/OnboardVM.png)

1. Sanal **Makineler** sayfasında VM'nizi seçin.
1. Sanal **makine** detay sayfasında **+ Bağlan'ı**tıklatın.

   > [!IMPORTANT]
   > Bu, desteklenen bir [işletim sistemi](automation-dsc-overview.md#operating-system-requirements)çalıştıran bir Azure Kaynak Yöneticisi VM olmalıdır.

2. **Kayıt** sayfasında, **Düğüm yapılandırma adı** kutusundaKi VM'ye uygulamak istediğiniz düğüm yapılandırmasının adını seçin. Bu noktada bir ad sağlamak isteğe bağlıdır. Düğüme bindikten sonra atanan düğüm yapılandırmasını değiştirebilirsiniz.
   **Gerekirse Düğümü Yeniden Başlat'ı**denetleyin, ardından **Tamam'ı**tıklatın.

   ![Kayıt bıçağının ekran görüntüsü](./media/automation-dsc-getting-started/RegisterVM.png)

   Belirttiğiniz düğüm **yapılandırması, Konfigürasyon Modu Frekansı**tarafından belirtilen aralıklarla VM'ye uygulanır ve VM **frekansı yenileme**tarafından belirtilen aralıklarla düğüm yapılandırmasına güncelleştirmeleri denetler. Bu değerlerin nasıl kullanıldığı hakkında daha fazla bilgi için [bkz.](/powershell/scripting/dsc/managing-nodes/metaConfig)

Azure, VM'ye binme işlemini başlatır. Tamamlandığında, VM Otomasyon hesabındaKi **Durum yapılandırması (DSC)** sayfasının **Düğümler** sekmesinde gösterir.

## <a name="viewing-the-list-of-managed-nodes"></a>Yönetilen düğümlerin listesini görüntüleme

**Durum yapılandırması (DSC)** sayfasının **Düğümler** sekmesinde Otomasyon hesabınızda yönetim için binen tüm makinelerin listesini görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** bıçağında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında **Düğümler** sekmesini tıklatın.

## <a name="viewing-reports-for-managed-nodes"></a>Yönetilen düğümler için raporları görüntüleme

Azure Otomasyon Durumu Yapılandırması yönetilen bir düğümüzerinde tutarlılık denetimi gerçekleştirdiğinde, düğüm çekme sunucusuna durum raporu gönderir. Bu düğüm için sayfada bu raporları görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** bıçağında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında **Düğümler** sekmesini tıklatın. Burada, Yapılandırma durumuna genel bakışı ve her düğüm için ayrıntıları görebilirsiniz.

   ![Düğüm sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/NodesTab.png)

1. **Düğümler** sekmesinde, raporlamayı açmak için düğüm kaydını tıklatın. Ek raporlama ayrıntılarını görüntülemek istediğiniz raporu tıklatın.

   ![Rapor bıçağının ekran görüntüsü](./media/automation-dsc-getting-started/NodeReport.png)

Tek bir rapor için bıçak üzerinde, ilgili tutarlılık denetimi için aşağıdaki durum bilgilerini görebilirsiniz:

- Rapor durumu — düğüm "Uyumlu", yapılandırma "Başarısız" veya düğüm "Uyumlu değil" (düğüm **ApplyandMonitor** modundayken ve makine istenilen durumda olmadığında) olsun rapor durumu.
- Tutarlılık denetimi için başlangıç zamanı.
- Tutarlılık denetimi için toplam çalışma süresi.
- Tutarlılık denetiminin türü.
- Hata kodu ve hata iletisi de dahil olmak üzere tüm hatalar.
- Yapılandırmada kullanılan tüm DSC kaynakları ve her kaynağın durumu (düğüm bu kaynak için istenilen durumda olsun) — bu kaynak için daha ayrıntılı bilgi almak için her kaynağa tıklayabilirsiniz.
- Düğümün adı, IP adresi ve yapılandırma modu.

Düğümün sunucuya gönderdiği gerçek verileri görmek için **ham raporu görüntüle'yi** de tıklatabilirsiniz.
Bu verileri kullanma hakkında daha fazla bilgi için [bkz.](/powershell/scripting/dsc/pull-server/reportserver)

İlk rapor kullanılabilir hale gelmeden önce bir düğüm gemiye bindikten sonra biraz zaman alabilir. Bir düğüme bindikten sonra ilk rapor için 30 dakika kadar beklemeniz gerekebilir.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Farklı bir düğüm yapılandırmasına düğüm atama

Başlangıçta atadığınızdan farklı bir düğüm yapılandırması kullanmak için bir düğüm atayabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** bıçağında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında **Düğümler** sekmesini tıklatın.
1. **Düğümler** sekmesinde, yeniden atamak istediğiniz düğümün adını tıklatın.
1. Bu düğüm için sayfada, **düğüm yapılandırmasını atay'ı**tıklatın.

    ![Düğüm yapılandırma düğmesini vurgulayarak Düğüm ayrıntıları sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/AssignNode.png)

1. Düğüm **Yapılandırmasını Ata** sayfasında, düğümü atamak istediğiniz düğüm yapılandırmasını seçin ve ardından **Tamam'ı**tıklatın.

    ![Atama Düğümü Yapılandırma sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Düğümü niçin silme

Artık bir düğümün Azure Automation DSC tarafından yönetilmesini istemiyorsanız, düğümün kaydını açabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** bıçağında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** tıklatın.
1. Durum **yapılandırması (DSC)** sayfasında **Düğümler** sekmesini tıklatın.
1. **Düğümler** sekmesinde, kaydını çıkarmak istediğiniz düğümün adını tıklatın.
1. Bu düğümün sayfasında Kaydı **Nı Tıklatın.**

    ![Kayıt Açma düğmesini vurgulayan Düğüm ayrıntıları sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>İlgili Makaleler

- [Azure Otomasyon Durumu Yapılandırması yapılandırmaya genel bakış](automation-dsc-overview.md)
- [Azure Otomasyon Durumu Yapılandırması ile yönetim için onboarding makineleri](automation-dsc-onboarding.md)
- [Windows PowerShell İstenilen Durum Yapılandırmagenel Bakış](/powershell/scripting/dsc/overview/overview)
- [Azure Otomasyon Durum Yapılandırma cmdlets](/powershell/module/azurerm.automation/#automation)
- [Azure Otomasyon Durumu Yapılandırma fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
