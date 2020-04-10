---
title: Chocolatey ile Azure Otomasyon Durumu Yapılandırması sürekli dağıtım
description: DevOps sürekli dağıtımını, Chocolatey paket yöneticisiyle Azure Automation durum yapılandırmasını kullanarak açıklar. Tam JSON Kaynak Yöneticisi şablonu ve PowerShell kaynağı içeren bir örnek içerir.
services: automation
ms.subservice: dsc
ms.date: 08/08/2018
ms.topic: conceptual
ms.openlocfilehash: 79eaac74fd4475a613c0309476a12292e400dbaa
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010979"
---
# <a name="provide-continuous-deployment-to-virtual-machines-using-automation-state-configuration-and-chocolatey"></a>Otomasyon Durumu Yapılandırması ve Chocolatey kullanarak sanal makinelere sürekli dağıtım sağlayın

Bir DevOps dünyada, sürekli entegrasyon boru hattı çeşitli noktaları ile yardımcı olmak için birçok araç vardır. Azure Otomasyon [Durumu Yapılandırması,](automation-dsc-overview.md) DevOps ekiplerinin kullanabileceği seçeneklere hoş geldiniz yeni bir ektir. 

Azure Otomasyonu, Microsoft Azure'da çalışma kitapları, düğümler ve kimlik bilgileri, zamanlamalar ve genel değişkenler gibi paylaşılan kaynakları kullanarak çeşitli görevleri otomatikleştirmenize olanak tanıyan yönetilen bir hizmettir. Azure Otomasyon Durumu Yapılandırması, bu otomasyon yeteneğini PowerShell İstenen Durum Yapılandırması (DSC) araçlarını içerecek şekilde genişletir. İşte harika bir [genel bakış.](automation-dsc-overview.md)

Bu makalede, bir Windows bilgisayarı için Sürekli Dağıtım (CD) nasıl ayarlanan gösteriş. Tekniği, örneğin bir web sitesi gibi role gerektiği kadar Windows bilgisayarı içerecek şekilde kolayca genişletebilir ve buradan ek rollere geçebilirsiniz.

![IaaS VM'ler için Sürekli Dağıtım](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="at-a-high-level"></a>Yüksek düzeyde

Burada oldukça fazla şey oluyor, ama neyse ki iki ana süreç içine ayrılabilir:

- Kod yazma ve test etme, ardından sistemin ana ve küçük sürümleri için yükleme paketleri oluşturma ve yayımlama.
- Paketlerdeki kodu yükleyen ve çalıştıran VM'ler oluşturma ve yönetme.  

Bu temel işlemlerin her ikisi de yerine geçtikten sonra, yeni sürümler oluşturuldukça ve dağıtılınca, paketi VM'lerinizde otomatik olarak güncelleştirmek kolaydır.

## <a name="component-overview"></a>Bileşene genel bakış

[Apt-get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) gibi paket yöneticileri Linux dünyasında iyi bilinir, ancak Windows dünyasında çok fazla değildir.
[Chocolatey](https://chocolatey.org/) böyle bir şey, ve Scott Hanselman'ın [blog](https://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) konu yla ilgili büyük bir giriş. Özetle, Chocolatey merkezi bir depodan paketleri windows işletim sistemine yüklemek için komut satırını kullanmanıza olanak tanır. Kendi deponuzu oluşturabilir ve yönetebilirsiniz ve Chocolatey belirlediğiniz herhangi bir sayıdaki depodan paketler yükleyebilir.

[PowerShell DSC](/powershell/scripting/dsc/overview/overview)) bir makine için istediğiniz yapılandırmayı bildirmenizi sağlayan bir PowerShell aracıdır. Örneğin, Chocolatey'nin yüklenmesini, IIS'nin yüklü olmasını, bağlantı noktası nın 80'inin açılmasını ve web sitenizin 1.0.0 sürümünün yüklü olmasını istiyorsanız, DSC Local Configuration Manager (LCM) bu yapılandırmayı uygular. Bir DSC çekme sunucusu, makineleriniz için bir yapılandırma deposu tutar. Her makinedeki LCM, yapılandırmasının depolanan yapılandırmayla eşleşip eşleşmeyip eşleşmeyini görmek için düzenli aralıklarla giriş yaptı. Durumu bildirebilir veya makineyi depolanan yapılandırmayla hizaya getirmeye çalışabilir. Bir makinenin veya makine kümesinin değiştirilen yapılandırmayla hizalanmasına neden olacak şekilde çekme sunucusunda depolanan yapılandırmayı değiştirebilirsiniz.

DSC kaynağı, ağ yönetimi, Active Directory veya SQL Server gibi belirli özelliklere sahip bir kod modülüdür. Chocolatey DSC Resource, NuGet Server'a (diğerlerinin yanı sıra), paketleri indirmeyi, paketleri yüklemeyi vb. nasıl erişeceklerini bilir. [PowerShell Galerisi'nde](https://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title)birçok diğer DSC Kaynakları vardır.
Bu modüller Azure Otomasyon Durumu Yapılandırma Çekme Sunucunuza (sizin) yüklenir, böylece yapılandırmalarınız tarafından kullanılabilir.

Kaynak Yöneticisi şablonları, altyapınızı oluşturmanın bildirimsel bir yolunu sağlar (örneğin, ağlar, alt ağlar, ağ güvenliği ve yönlendirme, yük dengeleyicileri, NIC'ler, VM'ler ve benzeri. Burada, Kaynak Yöneticisi dağıtım modelini (bildirim) ile Azure Hizmet Yönetimi (ASM veya klasik) dağıtım modeli (zorunlu) karşılaştıran bir [makale](../azure-resource-manager/management/deployment-models.md) verebilirsiniz. Bu makalede, temel kaynak sağlayıcıların bir tartışma içerir: bilgi işlem, depolama ve ağ.

Kaynak Yöneticisi şablonunun önemli özelliklerinden biri, VM'ye sağlanan bir VM uzantısı yükleyebilme özelliğidir. VM uzantısı, özel bir komut dosyası çalıştırma, virüsten koruma yazılımı yükleme ve DSC yapılandırma komut dosyası çalıştırma gibi belirli özelliklere sahiptir. VM uzantıları diğer birçok türü vardır.

## <a name="quick-trip-around-the-diagram"></a>Diyagram etrafında hızlı yolculuk

En üstten başlayarak, kodunuzu yazar, oluşturur, test edin, sonra bir yükleme paketi oluşturursunuz. Chocolatey, MSI, MSU, ZIP gibi çeşitli yükleme paketlerini işleyebilir. Chocolatey'nin yerel yetenekleri buna uygun değilse, gerçek kurulumu yapmak için PowerShell'in tüm gücüne sahipsiniz. Paketi ulaşılabilir bir yere koyun – bir paket deposu. Bu kullanım örneği, Azure blob depolama hesabında ortak bir klasör kullanır, ancak her yerde olabilir. Chocolatey, paket meta verilerin yönetimi için NuGet sunucuları ve birkaç diğer sunucuyla yerel olarak çalışır. [Bu makalede](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) seçenekleri açıklanır. Bu kullanım örneği NuGet kullanır. Nuspec, paketleriniz hakkında meta verilerdir. Nuspec'ler NuPkg's'a "derlenir" ve bir NuGet sunucusunda saklanır. Yapılandırmanız bir paketi ada göre istediğinde ve bir NuGet sunucusuna başvurursa, VM'deki Chocolatey DSC kaynağı paketi alır ve sizin için yükler. Paketin belirli bir sürümünü de isteyebilirsiniz.

Resmin sol alt kısmında bir Azure Kaynak Yöneticisi şablonu vardır. Bu kullanım örneğinde, VM uzantısı VM'yi Azure Otomasyon Durumu Yapılandırması çekme sunucusuna düğüm olarak kaydeder. Yapılandırma çekme sunucusunda depolanır.
Aslında, iki kez saklanır: bir kez düz metin olarak ve bir kez bir MOF dosyası olarak derlenir. Azure portalında MOF bir "düğüm yapılandırmasıdır" (basitçe "yapılandırma"nın aksine). Düğümün yapılandırmasını bilmesi için düğümle ilişkili yapıdır. Aşağıdaki ayrıntılar düğüm yapılandırmasının düğüme nasıl atandığını gösterir.

Nuspec oluşturma, derleme ve nuget sunucuda depolamak küçük bir şeydir. Ve zaten VM'leri yönetiyorsun. 

Sürekli dağıtım için bir sonraki adımı atmak, çekme sunucusunu bir kez ayarlamayı, düğümlerinizi onunla kaydetmeyi (bir kez) ve ilk yapılandırmayı oluşturmayı ve orada depolamayı gerektirir. Daha sonra, paketler yükseltildikçe ve depoya dağıtılınca, yalnızca gerektiğinde çekme sunucusundaki yapılandırmayı ve düğüm yapılandırmasını yenilemeniz gerekir. 

Kaynak Yöneticisi şablonuyla başlamıyorsanız, sorun değil. VM'lerinizi çekme sunucusuna ve diğer tüm sunuculara kaydetmenize yardımcı olacak PowerShell vardır. Daha fazla bilgi için şu makaleye bakın: [Azure Otomasyon Durumu Yapılandırması tarafından yönetilen makinelerde.](automation-dsc-onboarding.md)

## <a name="about-the-usage-example"></a>Kullanım örneği hakkında

Bu makaledeki kullanım örneği, Azure galerisinden genel bir Windows Server 2012 R2 görüntüsündeki bir VM ile başlar. Herhangi bir saklanan görüntüden başlayabilir ve daha sonra DSC yapılandırması ile oradan tweak.
Ancak, görüntüye dönüştürülen yapılandırmayı değiştirmek, DSC kullanarak yapılandırmayı dinamik olarak güncelleştirmekten çok daha zordur.

Bu tekniği VM'lerinizle kullanmak için Kaynak Yöneticisi şablonu ve VM uzantısı kullanmanız gerekmez. Ayrıca, CD yönetimi altında olmak için VM'lerinizin Azure'da olması gerekmez. Gerekli olan tek şey Chocolatey yüklü olması ve LCM'nin VM'de yapılandırılması, böylece çekme sunucusunun nerede olduğunu bilmesidir.

Üretimde olan bir VM'de bir paketi güncellediğinizde, güncelleştirme yüklenirken o VM'yi döndürme dışına çıkarmanız gerekir. Bunu nasıl yapacağınız büyük ölçüde değişir. Örneğin, Bir Azure Yük Dengeleyicisi'nin arkasında bir VM ile Özel Sonda ekleyebilirsiniz. VM güncellerken, sonda uç noktası 400'ü döndürün. Bu değişikliğe neden olmak için gerekli olan tweak yapılandırmanızın içinde olabilir, güncelleştirme tamamlandıktan sonra 200'e geri döndürmek için tweak de olabilir.

Bu kullanım örneğinin tam kaynağı GitHub'daki [visual studio projesindedir.](https://github.com/sebastus/ARM/tree/master/CDIaaSVM)

## <a name="step-1-set-up-the-pull-server-and-automation-account"></a>Adım 1: Çekme sunucusu ve Otomasyon hesabı nı ayarlama

Kimlik doğrulaması (`Connect-AzAccount`) PowerShell komut satırında: (çekme sunucusu ayarlanırken birkaç dakika sürebilir)

```azurepowershell-interactive
New-AzResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
New-AzAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT
```

Otomasyon hesabınızı aşağıdaki bölgelerden herhangi biri (konumlar olarak da bilinir): Doğu ABD 2, Güney Orta ABD, ABD Gov Virginia, Batı Avrupa, Güneydoğu Asya, Japonya Doğu, Orta Hindistan ve Avustralya Güneydoğu, Kanada Orta, Kuzey Avrupa.

## <a name="step-2-make-vm-extension-tweaks-to-the-resource-manager-template"></a>Adım 2: Kaynak Yöneticisi şablonunda VM uzantısı tweaks olun

Bu [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver)Şablonu'nda sağlanan VM kaydıiçin ayrıntılar (PowerShell DSC VM uzantısı kullanılarak)
Bu adım, yeni VM'nizi Durum Yapılandırma Düğümleri listesinde çekme sunucusuyla kaydeder. Bu kaydın bir bölümü düğüme uygulanacak düğüm yapılandırmasını belirtmektir. Bu düğüm yapılandırması çekme sunucusunda henüz var olmak zorunda değildir, bu nedenle adım 4'ün ilk kez yapıldığı yer olması iyidir. Ancak burada Adım 2'de düğümün adını ve yapılandırmanın adını karar vermiş olmanız gerekir. Bu kullanım örneğinde düğüm 'isvbox' ve yapılandırma 'ISVBoxConfig' dir. Yani düğüm yapılandırma adı (DeploymentTemplate.json'da belirtilecek) 'ISVBoxConfig.isvbox'tır.

## <a name="step-3-add-required-dsc-resources-to-the-pull-server"></a>Adım 3: Çekme sunucusuna gerekli DSC kaynaklarını ekleme

PowerShell Galerisi, Azure Otomasyon hesabınıza DSC kaynaklarını yüklemek için uygundur.
İstediğiniz kaynağa gidin ve "Azure Otomasyonuna Dağıt" düğmesini tıklayın.

![PowerShell Gallery örneği](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Azure portalına yakın zamanda eklenen başka bir teknik, yeni modülleri çekmenize veya varolan modülleri güncelleştirmenize olanak tanır. Otomasyon hesap kaynağı, Varlıklar döşemesi ve son olarak Modüller döşemesi aracılığıyla tıklayın. Galeriye Gözat simgesi, galerideki modüllerin listesini görmenizi, ayrıntılara inmenizi ve sonuçta Otomasyon hesabınıza aktarmanızı sağlar. Bu, modüllerinizi zaman zaman güncel tutmak için harika bir yoldur. Ayrıca, alma özelliği hiçbir şeyin senkronize olmadığından emin olmak için diğer modüllerle bağımlılıkları denetler.

Ya da, manuel yaklaşım var. Bir Windows bilgisayarı için PowerShell Tümleştirme Modülü'nün klasör yapısı, Azure Otomasyonu tarafından beklenen klasör yapısından biraz farklıdır.
Bu sizin için biraz tweaking gerektirir. Ama zor değil ve kaynak başına sadece bir kez yapılır (gelecekte yükseltmek istemiyorsanız.) PowerShell Tümleştirme Modüllerinin yazılması hakkında daha fazla bilgi için şu makaleye bakın: [Azure Otomasyonu için Tümleştirme Modülleri Yazma](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

- İş istasyonunuzun ihtiyacınız olan modülünüzü aşağıdaki gibi yükleyin:
  - [Windows Yönetim Çerçevesi,v5'i](https://aka.ms/wmf5latest) (Windows 10 için gerekli değildir) yükleyin
  - `Install-Module –Name MODULE-NAME`<—PowerShell Galerisi'nden modülü kaptı
- Modül klasörünü `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` geçici bir klasöre kopyalama
- Ana klasörden örnekleri ve belgeleri silme
- Zip dosyasını klasörle tam olarak aynı adlandırma, ana klasörü zip 
- ZIP dosyasını, Azure Depolama Hesabı'ndaki blob depolama gibi ulaşılabilir bir HTTP konumuna koyun.
- Bu PowerShell çalıştırın:

  ```powershell
  New-AzAutomationModule `
    -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Name MODULE-NAME –ContentLink 'https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip'
  ```

Dahil edilen örnek, cChoco ve xNetworking için bu adımları uygular. 

## <a name="step-4-add-the-node-configuration-to-the-pull-server"></a>Adım 4: Düğüm yapılandırmasını çekme sunucusuna ekleme

Yapılandırmanızı çekme sunucusuna ilk kez içe aktarıp derlemeniz konusunda özel bir şey yoktur. Daha sonra tüm almalar veya aynı yapılandırma derlemeleri tam olarak aynı görünüyor. Paketinizi her güncellediğiniz ve paketinizi üretime itmeniz gerektiğinde, yapılandırma dosyasının doğru olduğundan emin olduktan sonra paketinizin yeni sürümü de dahil olmak üzere bu adımı yaparsınız. İşte yapılandırma dosyası ve PowerShell:

ISVBoxConfig.ps1:

```powershell
Configuration ISVBoxConfig
{
    Import-DscResource -ModuleName cChoco
    Import-DscResource -ModuleName xNetworking

    Node 'isvbox' {

        cChocoInstaller installChoco
        {
            InstallDir = 'C:\choco'
        }

        WindowsFeature installIIS
        {
            Ensure = 'Present'
            Name   = 'Web-Server'
        }

        xFirewall WebFirewallRule
        {
            Direction    = 'Inbound'
            Name         = 'Web-Server-TCP-In'
            DisplayName  = 'Web Server (TCP-In)'
            Description  = 'IIS allow incoming web site traffic.'
            Enabled       = 'True'
            Action       = 'Allow'
            Protocol     = 'TCP'
            LocalPort    = '80'
            Ensure       = 'Present'
        }

        cChocoPackageInstaller trivialWeb
        {
            Name      = 'trivialweb'
            Version   = '1.0.0'
            Source    = 'MY-NUGET-V2-SERVER-ADDRESS'
            DependsOn = '[cChocoInstaller]installChoco','[WindowsFeature]installIIS'
        }
    }
}
```

New-ConfigurationScript.ps1 (Az modülkullanmak için modifiye):

```powershell
Import-AzAutomationDscConfiguration `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 `
    -Published –Force

$jobData = Start-AzAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -ConfigurationName ISVBoxConfig

$compilationJobId = $jobData.Id

Get-AzAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Id $compilationJobId
```

Bu adımlar, çekme sunucusuna "ISVBoxConfig.isvbox" adlı yeni bir düğüm yapılandırması ile sonuçlanır. Düğüm yapılandırma adı "configurationName.nodeName" olarak oluşturulur.

## <a name="step-5-create-and-maintain-package-metadata"></a>Adım 5: Paket meta verileri oluşturma ve koruma

Paket deposuna koyduğunuz her paket için, bunu açıklayan bir nuspec gerekir.
Bu nuspec derlenmeli ve NuGet sunucunuzda depolanmalıdır. Bu işlem [burada](https://docs.nuget.org/create/creating-and-publishing-a-package)açıklanmıştır. MyGet.org NuGet sunucusu olarak kullanabilirsiniz. Bu hizmeti satıyorlar, ama ücretsiz bir başlangıç SKU'su var. NuGet.org, özel paketleriniz için kendi NuGet sunucunuzu yükleme yönergelerini bulacaksınız.

## <a name="step-6-tie-it-all-together"></a>Adım 6: Hepsini birbirine bağlayın

Bir sürüm QA'dan geçtiğinde ve dağıtım için onaylandığı her seferde paket oluşturulur ve nuspec ve nupkg güncellenir ve NuGet sunucusuna dağıtılır. Yapılandırma (Yukarıdaki Adım 4) yeni sürüm numarasıyla aynı fikirde olacak şekilde de güncelleştirilmelidir. Daha sonra çekme sunucusuna gönderilmeli ve derlenmelidir.

Bu noktadan itibaren, güncelleştirmeyi çekmek ve yüklemek için bu yapılandırmaya bağlı Olan VM'lere bağlıdır. Bu güncellemelerin her biri basittir - PowerShell'in sadece bir ya da iki satırı. Azure DevOps'ler için, bazıları bir yapıda birbirine zincirlenebilen yapı görevleriyle kapsüllenir. Bu [makalede](https://www.visualstudio.com/docs/alm-devops-feature-index#continuous-delivery) daha fazla ayrıntı sağlar. Bu [GitHub repo](https://github.com/Microsoft/vso-agent-tasks) kullanılabilir yapı görevlerini ayrıntılarıyla anlatır.

## <a name="related-articles"></a>İlgili makaleler:
* [Azure Otomasyon DSC Genel Bakış](automation-dsc-overview.md)
* [Azure Otomasyon DSC cmdlets](https://docs.microsoft.com/powershell/module/azurerm.automation#automation)
* [Azure Automation DSC ile yönetim için onboarding makineleri](automation-dsc-onboarding.md)

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz: [Azure Otomasyon Durumu Yapılandırması](automation-dsc-overview.md)
- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)
- DSC yapılandırmalarını hedef düğümlerine atamak için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlet'ine](/powershell/module/azurerm.automation/#automation) bakın
- Fiyatlandırma bilgileri için Bkz. [Azure Otomasyon Durumu Yapılandırma fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)
