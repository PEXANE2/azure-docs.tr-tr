---
title: Azure genel bakış için İstenen Durum Yapılandırması
description: PowerShell İstenen Durum Yapılandırması (DSC) için Microsoft Azure uzantı işleyicisini nasıl kullanacağınızı öğrenin. Makale önkoşullar, mimari ve cmdlets içerir.
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: Dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: robreed
ms.openlocfilehash: c61ba0840b75bff10af1d802a9b90c922ef1f12f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415881"
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Azure Desired State Configuration uzantısı işleyicisine giriş

Azure VM Aracısı ve ilişkili uzantılar Microsoft Azure altyapı hizmetlerinin bir parçasıdır. VM uzantıları, VM işlevselliğini genişleten ve çeşitli VM yönetim işlemlerini basitleştiren yazılım bileşenleridir.

Azure İstenen Durum Yapılandırması (DSC) uzantısı için birincil kullanım örneği, bir VM'yi [Azure Otomasyon Durumu Yapılandırması (DSC) hizmetine](../../automation/automation-dsc-overview.md)önyükleme yapmaktır.
Hizmet, VM yapılandırmasının sürekli yönetimini ve Azure İzleme gibi diğer işletim araçlarıyla tümleştirmeyi içeren [avantajlar](/powershell/scripting/dsc/managing-nodes/metaConfig#pull-service) sağlar.
VM'leri hizmete kaydetmek için uzantıyı kullanmak, Azure aboneliklerinde bile işe yarayan esnek bir çözüm sağlar.

DSC uzantısını Otomasyon DSC hizmetinden bağımsız olarak kullanabilirsiniz.
Ancak, bu yalnızca VM bir yapılandırma iter.
VM'de yerel olarak dışında devam eden bir raporlama bulunmamaktadır.

Bu makalede, her iki senaryo hakkında da bilgi verilmektedir: Otomasyon onboarding için DSC uzantısını kullanmak ve Azure SDK'yı kullanarak VM'lere yapılandırma atama aracı olarak DSC uzantısını kullanmak.

## <a name="prerequisites"></a>Ön koşullar

- **Yerel makine**: Azure VM uzantısı ile etkileşimde kalmak için Azure portalını veya Azure PowerShell SDK'yı kullanmanız gerekir.
- **Konuk Aracı**: DSC yapılandırması tarafından yapılandırılan Azure VM, Windows Yönetim Çerçevesi (WMF) 4.0 veya sonraki lerini destekleyen bir işletim sistemi olmalıdır. Desteklenen işletim sistemi sürümlerinin tam listesi için [DSC uzantı sürüm geçmişine](/powershell/scripting/dsc/getting-started/azuredscexthistory)bakın.

## <a name="terms-and-concepts"></a>Şartlar ve kavramlar

Bu kılavuz, aşağıdaki kavramlara aşinalık varsayar:

- **Yapılandırma**: DSC yapılandırma belgesi.
- **Düğüm**: DSC yapılandırması için bir hedef. Bu belgede *düğüm* her zaman bir Azure VM'ye başvurur.
- **Yapılandırma verileri**: Yapılandırma için çevresel verilere sahip bir .psd1 dosyası.

## <a name="architecture"></a>Mimari

Azure DSC uzantısı, Azure VM'lerde çalışan DSC yapılandırmalarını sunmak, yürürlüğe koymak ve raporlamak için Azure VM Aracısı çerçevesini kullanır. DSC uzantısı bir yapılandırma belgesini ve bir dizi parametreyi kabul eder. Dosya sağlanmadıysa, [uzantıyla](#default-configuration-script) birlikte varsayılan yapılandırma komut dosyası katıştırılır. Varsayılan yapılandırma komut dosyası yalnızca Yerel [Yapılandırma Yöneticisi'nde](/powershell/scripting/dsc/managing-nodes/metaConfig)meta verileri ayarlamak için kullanılır.

Uzantı ilk kez çağrıldığında, aşağıdaki mantığı kullanarak WMF'nin bir sürümünü yükler:

- Azure VM Işletim Sistemi Windows Server 2016 ise hiçbir işlem yapılmaz. Windows Server 2016 zaten PowerShell en son sürümü yüklü vardır.
- **WMFVersion** özelliği belirtilirse, bu sürüm VM'nin işletim sistemiyle uyumsuz olmadığı sürece WMF'nin bu sürümü yüklenir.
- **WMFVersion** özelliği belirtilmemişse, WMF'nin geçerli en son sürümü yüklenir.

WMF'nin yüklenmesi yeniden başlatmayı gerektirir. Yeniden başladıktan sonra uzantı, **modüllerde** belirtilen .zip dosyasını indirirUrl özelliği, eğer sağlanırsa. Bu konum Azure Blob depolama alanındaysa, dosyaya erişmek için **sasToken** özelliğinde bir SAS belirteci belirtebilirsiniz. .zip indirilip boşaltıldıktan sonra **configurationFunction'da** tanımlanan yapılandırma işlevi bir .mof[(Yönetilen Nesne Biçimi)](https://docs.microsoft.com/windows/win32/wmisdk/managed-object-format--mof-)dosyası oluşturmak için çalışır. Uzantı daha `Start-DscConfiguration -Force` sonra oluşturulan .mof dosyasını kullanarak çalışır. Uzantı çıktıyı yakalar ve Azure durum kanalına yazar.

### <a name="default-configuration-script"></a>Varsayılan yapılandırma komut dosyası

Azure DSC uzantısı, Azure Otomasyon USC hizmetinde bir VM'de yken kullanılması amaçlanan varsayılan bir yapılandırma komut dosyası içerir. Komut dosyası parametreleri [Yerel Configuration Manager'ın](/powershell/scripting/dsc/managing-nodes/metaConfig)yapılandırılabilir özellikleriyle hizalanır. Komut dosyası parametreleri için, [Azure Kaynak Yöneticisi şablonları ile İstenilen Durum Yapılandırması uzantısında](dsc-template.md)Varsayılan yapılandırma komut [dosyasına](dsc-template.md#default-configuration-script) bakın. Tam komut dosyası için [GitHub'daki Azure hızlı başlatma şablonuna](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true)bakın.

## <a name="information-for-registering-with-azure-automation-state-configuration-dsc-service"></a>Azure Otomasyon Durum Yapılandırması (DSC) hizmetine kaydolmak için bilgiler

Devlet Yapılandırma hizmetine bir düğüm kaydetmek için DSC Uzantısı kullanırken, üç değer sağlanmalıdır.

- RegistrationUrl - Azure Otomasyon hesabının https adresi
- RegistrationKey - hizmete düğümleri kaydetmek için kullanılan paylaşılan bir sır
- NodeConfigurationName - Sunucu rolünü yapılandırmak için hizmetten çekmek için Düğüm Yapılandırması (MOF) adı

Bu bilgiler [Azure portalında](../../automation/automation-dsc-onboarding.md#onboard-vms-by-using-the-azure-portal) görülebilir veya PowerShell'i kullanabilirsiniz.

```powershell
(Get-AzAutomationRegistrationInfo -ResourceGroupName <resourcegroupname> -AutomationAccountName <accountname>).Endpoint
(Get-AzAutomationRegistrationInfo -ResourceGroupName <resourcegroupname> -AutomationAccountName <accountname>).PrimaryKey
```

Düğüm Yapılandırması adı için düğüm yapılandırmasının Azure Durum Yapılandırmasında bulunduğundan emin olun.  Aksi takdirde, uzantı dağıtımı bir hata döndürecek.  Ayrıca, Yapılandırma'yı değil, *Düğüm* Yapılandırması'nın adını kullandığınızdan emin olun.
Yapılandırma, [Düğüm Yapılandırması'nı (MOF dosyası) derlemek için](https://docs.microsoft.com/azure/automation/automation-dsc-compile)kullanılan bir komut dosyasında tanımlanır.
Ad her zaman yapılandırma bir dönem `.` ve `localhost` ya da belirli bir bilgisayar adı takip olacaktır.

## <a name="dsc-extension-in-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarında DSC uzantısı

Çoğu senaryoda, Kaynak Yöneticisi dağıtım şablonları DSC uzantısı ile çalışmak için beklenen yoldur. Daha fazla bilgi ve Kaynak Yöneticisi dağıtım şablonlarına DSC uzantısının nasıl eklenebildiğini öğrenmek için Azure [Kaynak Yöneticisi şablonlarıyla İstenilen Durum Yapılandırması uzantısına](dsc-template.md)bakın.

## <a name="dsc-extension-powershell-cmdlets"></a>DSC uzatma PowerShell cmdlets

DSC uzantısını yönetmek için kullanılan PowerShell cmdlet'ler en iyi etkileşimli sorun giderme ve bilgi toplama senaryolarında kullanılır. Cmdlets'i DSC uzantı dağıtımlarını paketlemek, yayımlamak ve izlemek için kullanabilirsiniz. DSC uzantısı için cmdlets henüz varsayılan yapılandırma [komut dosyası](#default-configuration-script)ile çalışmak için güncelleştirdeğil.

**Publish-AzVMDscConfiguration** cmdlet bir yapılandırma dosyası alır, bağımlı DSC kaynakları için tarar ve sonra bir .zip dosyası oluşturur. .zip dosyası yapılandırmayı yürürlüğe koymak için gereken yapılandırmayı ve DSC kaynaklarını içerir. Cmdlet ayrıca *-OutputArchivePath* parametresini kullanarak paketi yerel olarak oluşturabilir. Aksi takdirde, cmdlet blob depolama için .zip dosyasını yayımlar ve sonra bir SAS belirteci ile güvenli.

Cmdlet'in oluşturduğu .ps1 yapılandırma komut dosyası arşiv klasörünün kökündeki .zip dosyasında dır. Modül klasörü kaynaklardaki arşiv klasörüne yerleştirilir.

**Set-AzVMDscExtension** cmdlet, PowerShell DSC uzantısı nın gerektirdiği ayarları bir VM yapılandırma nesnesine enjekte eder.

**Get-AzVMDscExtension** cmdlet, belirli bir VM'nin DSC uzantı durumunu alır.

**Get-AzVMDscExtensionStatus** cmdlet, DSC uzantı işleyicisi tarafından yürürlüğe konan DSC yapılandırmasının durumunu alır. Bu eylem tek bir VM'de veya bir grup VM'de gerçekleştirilebilir.

**Remove-AzVMDscExtension** cmdlet, uzantı işleyicisini belirli bir VM'den kaldırır. Bu cmdlet yapılandırmayı *kaldırmaz,* WMF'yi kaldırmaz veya VM'deki uygulanan ayarları değiştirmez. Yalnızca uzantı işleyicisini kaldırır. 

Kaynak Yöneticisi DSC uzantısı cmdlets hakkında önemli bilgiler:

- Azure Kaynak Yöneticisi cmdlets eşzamanlıdır.
- *ResourceGroupName,* *VMName,* *ArchiveStorageAccountName,* *Sürüm*ve *Konum* parametreleri gereklidir.
- *ArchiveResourceGroupName* isteğe bağlı bir parametredir. Depolama hesabınız VM'nin oluşturulduğu kaynak grubundan farklı bir kaynak grubuna ait olduğunda bu parametreyi belirtebilirsiniz.
- Kullanılabilir olduğunda uzantı işleyicisini otomatik olarak en son sürüme güncellemek için **Otomatik Güncelleştirme** anahtarını kullanın. Bu parametre, WMF'nin yeni bir sürümü yayımlandığında VM'de yeniden başlatılmasına neden olabilir.

### <a name="get-started-with-cmdlets"></a>Cmdlets ile başlayın

Azure DSC uzantısı, dağıtım sırasında Azure VM'leri doğrudan yapılandırmak için DSC yapılandırma belgelerini kullanabilir. Bu adım düğümü Otomasyon'a kaydetmez. Düğüm merkezi olarak *yönetilmez.*

Aşağıdaki örnek, yapılandırmanın basit bir örneğini gösterir. yapılandırmayı yerel olarak iisInstall.ps1 olarak kaydedin.

```powershell
configuration IISInstall
{
    node "localhost"
    {
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }
    }
}
```

Aşağıdaki komutlar belirtilen VM üzerinde iisInstall.ps1 komut dosyası yer. Komutlar yapılandırmayı da yürütür ve sonra durum hakkında rapor lanır.

```powershell
$resourceGroup = 'dscVmDemo'
$vmName = 'myVM'
$storageName = 'demostorage'
#Publish the configuration script to user storage
Publish-AzVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzVMDscExtension -Version '2.76' -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName 'iisInstall.ps1.zip' -AutoUpdate -ConfigurationName 'IISInstall'
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Azure CLI, DSC uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir.

Windows çalıştıran sanal bir makine için:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name Microsoft.Powershell.DSC \
  --publisher Microsoft.Powershell \
  --version 2.77 --protected-settings '{}' \
  --settings '{}'
```

Linux çalıştıran sanal bir makine için:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name DSCForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 2.7 --protected-settings '{}' \
  --settings '{}'
```

## <a name="azure-portal-functionality"></a>Azure portalı işlevselliği

Portalda DSC'yi kurmak için:

1. VM'ye git.
2. **Ayarlar** bölümünde **Uzantılar**’ı seçin.
3. Oluşturulan yeni sayfada + Ekle **'yi**seçin ve ardından **PowerShell İstenen Durum Yapılandırmasını**seçin.
4. Uzantı bilgileri sayfasının altındaki **Oluştur'u** tıklatın.

Portal aşağıdaki girişi toplar:

- **Yapılandırma Modülleri veya Komut Dosyası**: Bu alan zorunludur (form [varsayılan yapılandırma komut dosyası](#default-configuration-script)için güncelleştirilmemiştir). Yapılandırma modülleri ve komut dosyaları, kökünde .ps1 yapılandırma komut dosyası olan bir yapılandırma komut dosyası veya .zip dosyası olan bir .ps1 dosyası gerektirir. Bir .zip dosyası kullanıyorsanız, tüm bağımlı kaynaklar .zip'teki modül klasörlerine eklenmelidir. .zip dosyasını, Azure PowerShell SDK'da bulunan **Publish-AzureVMDscConfiguration -OutputArchivePath** cmdlet'ini kullanarak oluşturabilirsiniz. .zip dosyası kullanıcı blob depolamanıza yüklenir ve bir SAS belirteci tarafından güvenli hale gelir.

- **Modül nitelikli Yapılandırma Adı**: Bir .ps1 dosyasına birden çok yapılandırma işlevi ekleyebilirsiniz. Ardından gelen yapılandırma .ps1 komut \\ dosyasının adını ve yapılandırma işlevinin adını girin. Örneğin, .ps1 komut dosyanızda configuration.ps1 adı varsa ve yapılandırma **IisInstall**ise **configuration.ps1\IisInstall**adresini girin.

- **Yapılandırma Bağımsız Değişkenleri**: Yapılandırma işlevi bağımsız değişkenler alıyorsa, bunları buraya formatta girin **argümanName1=value1,argumentName2=value2**. Bu biçim, powershell cmdlets veya Kaynak Yöneticisi şablonlarında yapılandırma bağımsız değişkenlerinin kabul edildiği farklı bir biçimdir.

- **Yapılandırma Veri PSD1 Dosya**: Bu alan isteğe bağlıdır. Yapılandırmanız .psd1'de bir yapılandırma veri dosyası gerektiriyorsa, veri alanını seçmek ve kullanıcı blob depolama alanınıza yüklemek için bu alanı kullanın. Yapılandırma veri dosyası blob depolama bir SAS belirteci tarafından güvenlidir.

- **WMF Sürümü**: Windows Yönetim Çerçevesi'nin (WMF) VM'nize yüklenmesi gereken sürümünü belirtir. Bu özelliği en son olarak ayarlamak WMF'nin en son sürümünü yükler. Şu anda, bu özellik için tek olası değerler 4.0, 5.0, 5.1 ve en son. Bu olası değerler güncelleştirmelere tabidir. Varsayılan değer **en sondur.**

- **Veri Toplama**: Uzantın telemetri toplayıp toplameyeceğini belirler. Daha fazla bilgi için Azure [DSC uzantısı veri koleksiyonuna](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/)bakın.

- **Sürüm**: DSC uzantısı sürümünü yüklemek için belirtir. Sürümler hakkında daha fazla bilgi için [Bkz. DSC uzantı lı sürüm geçmişi.](/powershell/scripting/dsc/getting-started/azuredscexthistory)

- **Otomatik Yükseltme Küçük Sürüm**: Bu alan cmdlets **AutoUpdate** anahtarı için eşler ve yükleme sırasında otomatik olarak en son sürüme güncelleme uzantısı sağlar. **Evet,** uzantı işleyicisine en son kullanılabilir sürümü kullanmasını emreder ve **Hayır,** belirtilen **Sürümün** yüklenmesini zorlar. **Evet** veya **Hayır'ı** **seçmek, Hayır'ı**seçmekle aynıdır.

## <a name="logs"></a>Günlükler

Uzantın günlükleri aşağıdaki konumda depolanır:`C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>`

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell DSC hakkında daha fazla bilgi için [PowerShell dokümantasyon merkezine](/powershell/scripting/dsc/overview/overview)gidin.
- [DSC uzantısı için Kaynak Yöneticisi şablonu inceleyin.](dsc-template.md)
- PowerShell DSC'yi kullanarak yönetebileceğiniz daha fazla işlevsellik ve daha fazla DSC kaynağı için [PowerShell galerisine](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0)göz atın.
- Hassas parametreleri yapılandırmalara geçirme yle ilgili ayrıntılar [için](dsc-credentials.md)bkz.
