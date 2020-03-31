---
title: Chocolatey ile Azure Otomasyon Durumu Yapılandırması Sürekli Dağıtım
description: DevOps, Azure Otomasyon Durumu Yapılandırması, DSC ve Chocolatey paket yöneticisini kullanarak sürekli dağıtım sağlar.  Tam JSON Kaynak Yöneticisi şablonu ve PowerShell kaynağı ile örnek.
services: automation
ms.subservice: dsc
ms.date: 08/08/2018
ms.topic: conceptual
ms.openlocfilehash: 4445f6e9b72380b66f3282d50871b4283f7fc7fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75966735"
---
# <a name="usage-example-continuous-deployment-to-virtual-machines-using-automation-state-configuration-and-chocolatey"></a>Kullanım Örneği: Otomasyon Durumu Yapılandırması ve Chocolatey kullanarak Sanal Makinelere sürekli dağıtım

Bir DevOps dünyada Sürekli Entegrasyon boru hattı çeşitli noktaları ile yardımcı olmak için birçok araç vardır. Azure Otomasyon Durumu Yapılandırması, DevOps ekiplerinin kullanabileceği seçeneklere hoş geldiniz yeni bir ektir. Bu makalede, bir Windows bilgisayarı için Sürekli Dağıtım (CD) kurulumu gösterilmelidir. Tekniği, rol (örneğin bir web sitesi) ve oradan da ek rollere gerektiği kadar Windows bilgisayarını içerecek şekilde kolayca genişletebilirsiniz.

![IaaS VM'ler için Sürekli Dağıtım](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>Yüksek düzeyde

Burada biraz oluyor, ama neyse ki iki ana süreçlere bölünebilir:

- Kod yazma ve test etme, ardından sistemin ana ve küçük sürümleri için yükleme paketleri oluşturma ve yayımlama.
- Paketlerdeki kodu yükleyecek ve yürütecek VM'ler oluşturma ve yönetme.  

Bu temel işlemlerin her ikisi de yerleştirildikten sonra, yeni sürümler oluşturuldukça ve dağıtılırken belirli bir VM'de çalışan paketi otomatik olarak güncellemek için kısa bir adımdır.

## <a name="component-overview"></a>Bileşene genel bakış

[Apt-get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) gibi paket yöneticileri Linux dünyasında oldukça iyi bilinir, ancak Windows dünyasında çok fazla değildir.
[Chocolatey](https://chocolatey.org/) böyle bir şey, ve Scott Hanselman'ın [blog](https://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) konu yla ilgili büyük bir giriş. Özetle, Chocolatey komut satırını kullanarak bir Windows sistemine paketleri merkezi bir depodan paketleri yüklemenize olanak sağlar. Kendi deponuzu oluşturabilir ve yönetebilirsiniz ve Chocolatey belirlediğiniz herhangi bir sayıdaki depodan paketler yükleyebilir.

İstenilen Durum Yapılandırması (DSC)[(genel bakış),](/powershell/scripting/dsc/overview/overview)bir makine için istediğiniz yapılandırmayı bildirmenize olanak tanıyan bir PowerShell aracıdır. Örneğin, "Chocolatey'nin yüklenmesini istiyorum, IIS'nin yüklenmesini istiyorum, bağlantı noktası 80'in açılmasını istiyorum, web sitemin 1.0.0 sürümünün yüklenmesini istiyorum." diyebilirsiniz. DSC Yerel Yapılandırma Yöneticisi (LCM) bu yapılandırmayı uygular. DSC Pull Server, makineleriniz için bir yapılandırma deposu na sahiptir. Her makinedeki LCM, yapılandırmasının depolanan yapılandırmayla eşleşip eşleşmeyip eşleşmeyini görmek için düzenli aralıklarla giriş yaptı. Durumu bildirebilir veya makineyi depolanan yapılandırmayla hizaya getirmeye çalışabilir. Bir makinenin veya makine kümesinin değiştirilen yapılandırmayla hizalanmasına neden olacak şekilde çekme sunucusunda depolanan yapılandırmayı değiştirebilirsiniz.

Azure Otomasyonu, Microsoft Azure'da çalışma kitapları, düğümler, kimlik bilgileri, kaynaklar ve zamanlamalar ve genel değişkenler gibi varlıkları kullanarak çeşitli görevleri otomatikleştirmenize olanak tanıyan yönetilen bir hizmettir.
Azure Otomasyon Durumu Yapılandırması, bu otomasyon yeteneğini PowerShell DSC araçlarını içerecek şekilde genişletir. İşte harika bir [genel bakış.](automation-dsc-overview.md)

DSC Kaynağı, ağ yönetimi, Active Directory veya SQL Server gibi belirli özelliklere sahip bir kod modülüdür. Chocolatey DSC Resource, NuGet Server'a (diğerlerinin yanı sıra), paketleri indirmeyi, paketleri yüklemeyi vb. nasıl erişeceklerini bilir. [PowerShell Galerisi'nde](https://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title)birçok diğer DSC Kaynakları vardır.
Bu modüller Azure Otomasyon Durumu Yapılandırma Çekme Sunucunuza (sizin) yüklenir, böylece yapılandırmalarınız tarafından kullanılabilir.

Kaynak Yöneticisi şablonları, altyapınızı oluşturmanın açıklayıcı bir yolunu sağlar ( ağlar, alt ağlar, ağ güvenliği ve yönlendirme, yük dengeleyicileri, NIC'ler, VM'ler ve benzeri şeyler. Burada, Kaynak Yöneticisi dağıtım modelini (bildirimsel) Ile Azure Hizmet Yönetimi (ASM veya klasik) dağıtım modeli (zorunlu) karşılaştıran ve temel kaynak sağlayıcıları, bilgi işlem, depolama ve ağı tartışan bir [makale](../azure-resource-manager/management/deployment-models.md) vereyim.

Kaynak Yöneticisi şablonunun önemli özelliklerinden biri, VM'ye sağlanan bir VM uzantısı yükleyebilme özelliğidir. VM uzantısı, özel bir komut dosyası çalıştırma, virüsten koruma yazılımı yükleme veya DSC yapılandırma komut dosyası çalıştırma gibi belirli özelliklere sahiptir. VM uzantıları diğer birçok türü vardır.

## <a name="quick-trip-around-the-diagram"></a>Diyagram etrafında hızlı yolculuk

En üstten başlayarak, kodunuzu yazar, oluşturur ve sınar, ardından bir yükleme paketi oluşturursunuz.
Chocolatey, MSI, MSU, ZIP gibi çeşitli yükleme paketlerini işleyebilir. Chocolateys'in yerel yetenekleri tam olarak buna uygun değilse, gerçek kurulumu yapmak için PowerShell'in tüm gücüne sahipsiniz. Paketi ulaşılabilir bir yere koyun – bir paket deposu. Bu kullanım örneği, Azure blob depolama hesabında ortak bir klasör kullanır, ancak her yerde olabilir. Chocolatey, paket meta verilerin yönetimi için NuGet sunucuları ve birkaç diğer sunucuyla yerel olarak çalışır. [Bu makalede](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) seçenekleri açıklanır. Bu kullanım örneği NuGet kullanır. Nuspec, paketleriniz hakkında meta verilerdir. Nuspec'ler NuPkg's'a "derlenir" ve bir NuGet sunucusunda saklanır. Yapılandırmanız bir paketi ada göre istediğinde ve bir NuGet sunucusuna başvuruyorsa, Chocolatey DSC Resource (şimdi VM'de) paketi yakalar ve sizin için yükler. Paketin belirli bir sürümünü de isteyebilirsiniz.

Resmin sol alt kısmında bir Azure Kaynak Yöneticisi şablonu vardır. Bu kullanım örneğinde, VM uzantısı VM'yi Azure Otomasyon Durumu Yapılandırma Çekme Sunucusu (yani çekme sunucusu) düğüm olarak kaydeder. Yapılandırma çekme sunucusunda depolanır.
Aslında, iki kez saklanır: bir kez düz metin olarak ve bir kez bir MOF dosyası olarak derlenmiş (bu tür şeyler hakkında bilenler için.) Portalda, MOF bir "düğüm yapılandırması" (sadece "yapılandırma" aksine). Düğümün yapılandırmasını bilmesi için düğümle ilişkili yapıdır. Aşağıdaki ayrıntılar düğüm yapılandırmasının düğüme nasıl atandığını gösterir.

Muhtemelen zaten en üstte, ya da çoğu kısmında biraz yapıyoruz. Nuspec oluşturma, derleme ve nuGet sunucuda depolamak küçük bir şeydir. Ve zaten VM'leri yönetiyorsun. Sürekli dağıtım için bir sonraki adımı atmak, çekme sunucusunu (bir kez) ayarlamayı, düğümlerinizi onunla kaydetmeyi (bir kez) ve yapılandırmayı oluşturmayı ve depolamayı (başlangıçta) gerektirir. Ardından paketler yükseltilip depoya dağıtılınca, çekme sunucusundaki Yapılandırma ve Düğüm Yapılandırmasını yenileyin (gerektiğinde yineleyin).

Kaynak Yöneticisi şablonuyla başlamıyorsanız, bu da sorun değil. VM'lerinizi çekme sunucusuna ve diğer tüm bunlara kaydettirmenize yardımcı olmak için tasarlanmış PowerShell cmdlets vardır. Daha fazla bilgi için şu makaleye bakın: [Azure Otomasyon Durumu Yapılandırması tarafından yönetilen makinelerde.](automation-dsc-onboarding.md)

## <a name="step-1-setting-up-the-pull-server-and-automation-account"></a>Adım 1: Çekme sunucusu ve otomasyon hesabının ayarlanması

Kimlik doğrulaması (`Connect-AzureRmAccount`) PowerShell komut satırında: (çekme sunucusu ayarlanırken birkaç dakika sürebilir)

```azurepowershell-interactive
New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT
```

Otomasyon hesabınızı aşağıdaki bölgelerden herhangi biri (aka konumu): Doğu ABD 2, Güney Orta ABD, ABD Gov Virginia, Batı Avrupa, Güneydoğu Asya, Japonya Doğu, Orta Hindistan ve Avustralya Güneydoğu, Kanada Orta, Kuzey Avrupa.

## <a name="step-2-vm-extension-tweaks-to-the-resource-manager-template"></a>Adım 2: Kaynak Yöneticisi şablonunda VM uzantısı tweaks

Bu [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver)Şablonu'nda sağlanan VM kaydıiçin ayrıntılar (PowerShell DSC VM uzantısı kullanılarak)
Bu adım, yeni VM'nizi Durum Yapılandırma Düğümleri listesinde çekme sunucusuyla kaydeder. Bu kaydın bir bölümü düğüme uygulanacak düğüm yapılandırmasını belirtmektir. Bu düğüm yapılandırması çekme sunucusunda henüz var olmak zorunda değildir, bu nedenle Adım 4'ün ilk kez yapıldığı yer olması normaldir. Ancak burada Adım 2'de düğümün adını ve yapılandırmanın adını karar vermiş olmanız gerekir. Bu kullanım örneğinde düğüm 'isvbox' ve yapılandırma 'ISVBoxConfig' dir. Yani düğüm yapılandırma adı (DeploymentTemplate.json'da belirtilecek) 'ISVBoxConfig.isvbox'tır.

## <a name="step-3-adding-required-dsc-resources-to-the-pull-server"></a>Adım 3: Çekme sunucusuna gerekli DSC kaynakları ekleme

PowerShell Galerisi, Azure Otomasyon hesabınıza DSC kaynaklarını yüklemek için uygundur.
İstediğiniz kaynağa gidin ve "Azure Otomasyonuna Dağıt" düğmesini tıklayın.

![PowerShell Gallery örneği](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Azure Portalı'na yakın zamanda eklenen bir diğer teknik de yeni modülleri çekmenize veya varolan modülleri güncelleştirmenize olanak tanır. Otomasyon Hesabı kaynağına, Varlıklar döşemesini ve son olarak Modüller döşemesini tıklatın. Galeriye Gözat simgesi, galerideki modüllerin listesini görmenizi, ayrıntılara inmenizi ve nihayetinde Otomasyon Hesabınıza aktarmanızı sağlar. Bu, modüllerinizi zaman zaman güncel tutmak için harika bir yoldur. Ayrıca, alma özelliği hiçbir şeyin senkronize olmadığından emin olmak için diğer modüllerle bağımlılıkları denetler.

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
  New-AzureRmAutomationModule `
    -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Name MODULE-NAME –ContentLink 'https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip'
  ```

Dahil edilen örnek, cChoco ve xNetworking için bu adımları gerçekleştirir. CChoco için özel kullanım için [Notlar'a](#notes) bakın.

## <a name="step-4-adding-the-node-configuration-to-the-pull-server"></a>Adım 4: Çekme sunucusuna düğüm yapılandırması ekleme

Yapılandırmanızı çekme sunucusuna ilk kez içe aktarıp derlemeniz konusunda özel bir şey yoktur. Aynı yapılandırmanın sonraki tüm alma/derlemeleri tamamen aynı görünür. Paketinizi her güncellediğiniz ve paketinizi üretime itmeniz gerektiğinde, yapılandırma dosyasının doğru olduğundan emin olduktan sonra paketinizin yeni sürümü de dahil olmak üzere bu adımı yaparsınız. İşte yapılandırma dosyası ve PowerShell:

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

Yeni YapılandırmaScript.ps1:

```powershell
Import-AzureRmAutomationDscConfiguration `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 `
    -Published –Force

$jobData = Start-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -ConfigurationName ISVBoxConfig

$compilationJobId = $jobData.Id

Get-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Id $compilationJobId
```

Bu adımlar, çekme sunucusuna "ISVBoxConfig.isvbox" adlı yeni bir düğüm yapılandırması ile sonuçlanır. Düğüm yapılandırma adı "configurationName.nodeName" olarak oluşturulur.

## <a name="step-5-creating-and-maintaining-package-metadata"></a>Adım 5: Paket meta verilerini oluşturma ve koruma

Paket deposuna koyduğunuz her paket için, bunu açıklayan bir nuspec gerekir.
Bu nuspec derlenmeli ve NuGet sunucunuzda depolanmalıdır. Bu işlem [burada](https://docs.nuget.org/create/creating-and-publishing-a-package)açıklanmıştır. MyGet.org NuGet sunucusu olarak kullanabilirsiniz. Bu hizmeti satıyorlar, ama ücretsiz bir başlangıç SKU'su var. NuGet.org özel paketleriniz için kendi NuGet sunucunuzu yükleme yönergeleri bulacaksınız.

## <a name="step-6-tying-it-all-together"></a>Adım 6: Hepsini birbirine bağlama

Bir sürüm QA'dan geçtiğinde ve dağıtım için onaylandığı her seferde paket oluşturulur, nuspec ve nupkg güncellenir ve NuGet sunucusuna dağıtılır. Buna ek olarak, yapılandırma (Yukarıdaki Adım 4) yeni sürüm numarası ile aynı fikirde güncelleştirilmelidir. Çekme sunucusuna gönderilmeli ve derlenmelidir.
Bu noktadan itibaren, güncelleştirmeyi çekmek ve yüklemek için bu yapılandırmaya bağlı Olan VM'lere bağlıdır. Bu güncellemelerin her biri basittir - PowerShell'in sadece bir ya da iki satırı. Azure DevOps durumunda, bazıları bir yapıda birbirine zincirlenebilen yapı görevleriyle kapsüllenir. Bu [makalede](https://www.visualstudio.com/docs/alm-devops-feature-index#continuous-delivery) daha fazla ayrıntı sağlar. Bu [GitHub repo,](https://github.com/Microsoft/vso-agent-tasks) kullanılabilir çeşitli yapı görevlerini ayrıntılarıyla anlatır.

## <a name="notes"></a>Notlar

Bu kullanım örneği, Azure galerisinden genel bir Windows Server 2012 R2 görüntüsündeki bir VM ile başlar. Herhangi bir saklanan görüntüden başlayabilir ve daha sonra DSC yapılandırması ile oradan tweak.
Ancak, görüntüye dönüştürülen yapılandırmayı değiştirmek, DSC kullanarak yapılandırmayı dinamik olarak güncelleştirmekten çok daha zordur.

Bu tekniği VM'lerinizle kullanmak için Kaynak Yöneticisi şablonu ve VM uzantısı kullanmanız gerekmez. Ayrıca, CD yönetimi altında olmak için VM'lerinizin Azure'da olması gerekmez. Gerekli olan tek şey Chocolatey yüklü olması ve LCM'nin VM'de yapılandırılması, böylece çekme sunucusunun nerede olduğunu bilmesidir.

Elbette, üretimde olan bir VM'de bir paketi güncellediğinizde, güncelleştirme yüklenirken bu VM'yi döndürme dışına çıkarmanız gerekir. Bunu nasıl yapacağınız büyük ölçüde değişir. Örneğin, Bir Azure Yük Dengeleyicisi'nin arkasında bir VM ile Özel Sonda ekleyebilirsiniz. VM güncellerken, sonda uç noktası 400'ü döndürün. Bu değişikliğe neden olmak için gerekli olan tweak yapılandırmanızın içinde olabilir, güncelleştirme tamamlandıktan sonra 200'e geri döndürmek için tweak de olabilir.

Bu kullanım örneğinin tam kaynağı GitHub'daki [visual studio projesindedir.](https://github.com/sebastus/ARM/tree/master/CDIaaSVM)

## <a name="related-articles"></a>İlgili Makaleler
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
