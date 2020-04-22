---
title: Azure Uygulama Öngörüleri Aracısı ayrıntılı talimatlar | Microsoft Dokümanlar
description: Application Insights Agent ile başlamak için ayrıntılı talimatlar. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 8f6134e8f8fdb9af3f578afaf0670c32a3896e01
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81766873"
---
# <a name="application-insights-agent-formerly-named-status-monitor-v2-detailed-instructions"></a>Uygulama Öngörüleri Aracısı (eski adıyla Status Monitor v2): Ayrıntılı talimatlar

Bu makalede, PowerShell Galerisi'ne nasıl bindirilen ve ApplicationMonitor modülünü nasıl indirecekleri açıklanmaktadır.
Başlamak için gereken en yaygın parametreler dahildir.
Ayrıca, internet erişiminiz yoksa manuel indirme yönergeleri de sağladık.

## <a name="get-an-instrumentation-key"></a>Enstrümantasyon anahtarı alın

Başlamak için bir enstrümantasyon anahtarına ihtiyacınız var. Daha fazla bilgi için [bkz.](create-new-resource.md#copy-the-instrumentation-key)

## <a name="run-powershell-as-admin-with-an-elevated-execution-policy"></a>PowerShell'i yüksek bir yürütme ilkesiyle Yönetici olarak çalıştırın

### <a name="run-as-admin"></a>Yönetici olarak çalıştırın

PowerShell'in bilgisayarınızda değişiklik yapmak için Yönetici düzeyinde izinlere ihtiyacı vardır.
### <a name="execution-policy"></a>Yürütme ilkesi
- Açıklama: Varsayılan olarak, PowerShell komut dosyaları çalıştıran devre dışı bırakılır. Yalnızca Geçerli kapsam için Uzaktan İmzalı komut dosyalarına izin vermenizi öneririz.
- Başvuru: [Yürütme İlkeleri](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) ve [Set-ExecutionPolicy](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
)hakkında.
- Komut: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`.
- İsteğe bağlı parametre:
    - `-Force`. Onay istemini atlar.

**Örnek hatalar**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>PowerShell için Ön Koşullar

Komutu çalıştırarak PowerShell `$PSVersionTable` örneğini denetle.
Bu komut aşağıdaki çıktıyı üretir:


```
Name                           Value
----                           -----
PSVersion                      5.1.17763.316
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.17763.316
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Bu yönergeler, Windows 10 çalıştıran bir bilgisayarda ve yukarıda listelenen sürümlerde yazılmış ve test edilmiştir.

## <a name="prerequisites-for-powershell-gallery"></a>PowerShell Gallery için Ön Koşullar

Bu adımlar, sunucunuzu PowerShell Gallery'den modülleri indirmeye hazırlar.

> [!NOTE] 
> PowerShell Gallery, Windows 10, Windows Server 2016 ve PowerShell 6'da desteklenir.
> Önceki sürümler hakkında daha fazla bilgi için [PowerShellGet'ı Yükleme'ye](/powershell/scripting/gallery/installing-psget)bakın.


1. PowerShell'i yüksek bir yürütme ilkesiyle Yönetici olarak çalıştırın.
2. NuGet paket sağlayıcısını yükleyin.
    - Açıklama: PowerShell Gallery gibi NuGet tabanlı depolarla etkileşim de dahil olmak üzere bu sağlayıcıya ihtiyacınız vardır.
    - Referans: [Install-PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6).
    - Komut: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`.
    - İsteğe bağlı parametreler:
        - `-Proxy`. İstek için bir proxy sunucusu belirtir.
        - `-Force`. Onay istemini atlar.
    
    NuGet ayarlı değilse bu istemi alırsınız:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. PowerShell Gallery'yi güvenilir bir depo olarak yapılandırın.
    - Açıklama: Varsayılan olarak, PowerShell Galerisi güvenilmeyen bir depodur.
    - Referans: [Set-PSRepozitory](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6).
    - Komut: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`.
    - İsteğe bağlı parametre:
        - `-Proxy`. İstek için bir proxy sunucusu belirtir.

    PowerShell Gallery'ye güvenilemiyorsa bu istemi alırsınız:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    Bu değişikliği onaylayabilir ve komutu `Get-PSRepository` çalıştırarak tüm PSRepositories'i denetleyebilirsiniz.

4. PowerShellGet'in en yeni sürümünü yükleyin.
    - Açıklama: Bu modül PowerShell Galerisi'nden diğer modülleri almak için kullanılan takım içerir. Sürüm 1.0.0.1, Windows 10 ve Windows Server ile birlikte. Sürüm 1.6.0 veya daha yüksek gereklidir. Hangi sürümün yüklü olduğunu belirlemek `Get-Command -Module PowerShellGet` için komutu çalıştırın.
    - Referans: [PowerShellGet yükleme](/powershell/scripting/gallery/installing-psget).
    - Komut: `Install-Module -Name PowerShellGet`.
    - İsteğe bağlı parametreler:
        - `-Proxy`. İstek için bir proxy sunucusu belirtir.
        - `-Force`. "Zaten yüklenmiş" uyarısını atlar ve en son sürümü yükler.

    PowerShellGet'in en yeni sürümünü kullanmıyorsanız bu hatayı alırsınız:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. PowerShell'i yeniden başlatın. Geçerli oturumda yeni sürümü yükleyemezsiniz. Yeni PowerShell oturumları PowerShellGet'in en son sürümünü yükler.

## <a name="download-and-install-the-module-via-powershell-gallery"></a>PowerShell Gallery üzerinden modülü indirin ve kurun

Bu adımlar PowerShell Gallery'den Az.ApplicationMonitor modülünü indirecektir.

1. PowerShell Gallery için tüm ön koşulların karşılandığından emin olun.
2. PowerShell'i yüksek bir yürütme ilkesiyle Yönetici olarak çalıştırın.
3. Az.ApplicationMonitor modülünü yükleyin.
    - Referans: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6).
    - Komut: `Install-Module -Name Az.ApplicationMonitor`.
    - İsteğe bağlı parametreler:
        - `-Proxy`. İstek için bir proxy sunucusu belirtir.
        - `-AllowPrerelease`. Alfa ve beta sürümlerinin yüklenmesini sağlar.
        - `-AcceptLicense`. "Lisansı Kabul Et" istemini atlar
        - `-Force`. "Güvenilmeyen Depo" uyarısını atlar.

## <a name="download-and-install-the-module-manually-offline-option"></a>Modülü el ile indirin ve kurun (çevrimdışı seçenek)

Herhangi bir nedenle PowerShell modülüne bağlanamıyorsanız, Az.ApplicationMonitor modülünü el ile indirebilir ve yükleyebilirsiniz.

### <a name="manually-download-the-latest-nupkg-file"></a>En son nupkg dosyasını el ile indirin

1. https://www.powershellgallery.com/packages/Az.ApplicationMonitor kısmına gidin.
2. **Sürüm Geçmişi** tablosunda dosyanın en son sürümünü seçin.
3. **Kurulum Seçenekleri**altında, **Manuel İndir'i**seçin.

### <a name="option-1-install-into-a-powershell-modules-directory"></a>Seçenek 1: PowerShell modülleri dizinine yükleyin
PowerShell oturumları tarafından keşfilenebilmek için manuel olarak indirilen PowerShell modüllerini powershell dizinine yükleyin.
Daha fazla bilgi için [powershell modülü yükleme](/powershell/scripting/developer/module/installing-a-powershell-module)ye bakın.


#### <a name="unzip-nupkg-as-a-zip-file-by-using-expand-archive-v1010"></a>Expand-Archive (v1.0.1.0) kullanarak zip dosyası olarak nupkg'ı açma

- Açıklama: Microsoft.PowerShell.Archive (v1.0.1.0) temel sürümü nupkg dosyaları unzip olamaz. .zip uzantılı dosyayı yeniden adlandırın.
- Başvuru: [Genişlet-Arşiv.](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6)
- Komut:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.3.0-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-by-using-expand-archive-v1100"></a>Expand-Archive (v1.1.0.0) kullanarak unzip nupkg

- Açıklama: Uzantıyı değiştirmeden nupkg dosyalarını açmak için Genişletme-Arşiv'in geçerli bir sürümünü kullanın.
- Başvuru: [Genişlet-Arşiv](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) ve [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0).
- Komut:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-nupkg-manually"></a>Seçenek 2: Zip'i açma ve nupkg'ı el ile içe aktarma
PowerShell oturumları tarafından keşfilenebilmek için manuel olarak indirilen PowerShell modüllerini powershell dizinine yükleyin.
Daha fazla bilgi için [powershell modülü yükleme](/powershell/scripting/developer/module/installing-a-powershell-module)ye bakın.

Modülü başka bir dizine yüklüyorsanız, [İçe Aktar-Modül'ü](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6)kullanarak modülü el ile aktarın.

> [!IMPORTANT] 
> DL'ler göreli yollar üzerinden yüklenir.
> Paketin içeriğini amaçlanan çalışma zamanı dizininde saklayın ve erişim izinlerinin okumaya izin verdiğini ancak yazmadığını onaylayın.

1. Uzantıyı ".zip" olarak değiştirin ve paketin içeriğini amaçlanan yükleme dizininize ayıklayın.
2. Az.ApplicationMonitor.psd1 dosya yolunu bulun.
3. PowerShell'i yüksek bir yürütme ilkesiyle Yönetici olarak çalıştırın.
4. Komutu kullanarak modülü `Import-Module Az.ApplicationMonitor.psd1` yükleyin.
    

## <a name="route-traffic-through-a-proxy"></a>Proxy üzerinden rota trafiği

Özel intranetinizde bir bilgisayarı izlediğinizde, HTTP trafiğini bir proxy üzerinden yönlendirmeniz gerekir.

PowerShell'in Az.ApplicationMonitor'u PowerShell Gallery'den indirmeve yükleme komutları bir `-Proxy` parametreyi destekler.
Yükleme komut dosyalarınızı yazarken önceki yönergeleri gözden geçirin.

Application Insights SDK'nın uygulamanızın telemetrisini Microsoft'a göndermesi gerekir. Web.config dosyanızda uygulamanızın proxy ayarlarını yapılandırmanızı öneririz. Daha fazla bilgi için Bkz. [Uygulama Öngörüleri SSS: Proxy geçişi.](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough)


## <a name="enable-monitoring"></a>İzlemeyi etkinleştirme

İzlemeyi `Enable-ApplicationInsightsMonitoring` etkinleştirmek için komutu kullanın.

Bu cmdlet'in nasıl kullanılacağına ilgili ayrıntılı bir açıklama için [API başvurusuna](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#enable-applicationinsightsmonitoring) bakın.



## <a name="next-steps"></a>Sonraki adımlar

 Telemetrinizi görüntüleyin:

- Performansı ve kullanımı izlemek için [ölçümleri keşfedin.](../../azure-monitor/platform/metrics-charts.md)
- Sorunları tanılamak için [olayları ve günlükleri arayın.](../../azure-monitor/app/diagnostic-search.md)
- Daha gelişmiş sorgular için [Analytics'i kullanın.](../../azure-monitor/app/analytics.md)
- [Panolar oluşturun.](../../azure-monitor/app/overview-dashboard.md)

 Daha fazla telemetri ekleyin:

- Sitenizin canlı kalması için [web testleri oluşturun](monitor-web-app-availability.md).
- Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [web istemcisi telemetrisi ekleyin.](../../azure-monitor/app/javascript.md)
- İzleme ve günlük aramaları ekleyebilmeniz [için Uygulama Öngörüleri SDK'yı kodunuza ekleyin.](../../azure-monitor/app/asp-net.md)

Application Insights Agent ile daha fazlasını yapın:

- Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
