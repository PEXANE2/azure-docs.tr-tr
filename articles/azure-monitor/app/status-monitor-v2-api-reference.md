---
title: Azure Uygulama Öngörüleri .Net Agent API başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 02762c4b3af735eb0b4c19aaf450b2b3a416a2be
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733680"
---
# <a name="azure-monitor-application-insights-agent-api-reference"></a>Azure Monitör Uygulama Öngörüleri Aracı API Başvurusu

Bu [makalede, Az.ApplicationMonitor PowerShell modülüüyesi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir cmdlet açıklanır.

> [!NOTE] 
> - Başlamak için bir enstrümantasyon anahtarına ihtiyacınız var. Daha fazla bilgi için [bkz.](create-new-resource.md#copy-the-instrumentation-key)
> - Bu cmdlet, lisans ve gizlilik bildirimimizi gözden geçirmenizi ve kabul lenmenizi gerektirir.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri ve yüksek yürütme ilkesi içeren bir PowerShell oturumu gerektirir. Daha fazla bilgi için [PowerShell'i yükseltilmiş yürütme ilkesine sahip yönetici olarak çalıştır'a](status-monitor-v2-detailed-instructions.md#run-powershell-as-admin-with-an-elevated-execution-policy)bakın.
> - Bu cmdlet, lisans ve gizlilik bildirimimizi gözden geçirmenizi ve kabul lenmenizi gerektirir.
> - Enstrümantasyon motoru ek ek ek yükü ekler ve varsayılan olarak kapalıdır.


## <a name="enable-instrumentationengine"></a>Etkinleştirme-EnstrümantasyonMotoru

Bazı kayıt defteri anahtarlarını ayarlayarak enstrümantasyon motorunu etkinleştirir.
Değişikliklerin etkili olması için IIS'yi yeniden başlatın.

Enstrümantasyon motoru .NET SDK'lar tarafından toplanan verileri tamamlayabilir.
Yönetilen bir işlemin yürütülmesini açıklayan olayları ve iletileri toplar. Bu olaylar ve iletiler bağımlılık sonuç kodları, HTTP fiiller ve [SQL komut metni](asp-net-dependencies.md#advanced-sql-tracking-to-get-full-sql-query)içerir.

Aşağıdakiler varsa enstrümantasyon motorunu etkinleştirin:
- Etkinleştir cmdlet ile izlemeyi zaten etkinleştirdin, ancak enstrümantasyon motorunu etkinleştirmedin.
- Uygulamanızı .NET SDK'larla el ile belgelediniz ve ek telemetri toplamak istiyorsunuz.

### <a name="examples"></a>Örnekler

```powershell
PS C:\> Enable-InstrumentationEngine
```

### <a name="parameters"></a>Parametreler

#### <a name="-acceptlicense"></a>-Kabul Lisansı
**Isteğe bağlı.** Başsız yüklemelerde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

#### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri çıktıetmek için bu anahtarı kullanın.

### <a name="output"></a>Çıktı


##### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Enstrümantasyon motorunun başarılı bir şekilde etkinleştirilmesinden örnek çıktı

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="enable-applicationinsightsmonitoring"></a>Enable-ApplicationInsightsMonitoring

Hedef bilgisayarda IIS uygulamalarının kodsuz olarak izlenmesini sağlar.

Bu cmdlet IIS applicationHost.config değiştirmek ve bazı kayıt anahtarları ayarlayın.
Ayrıca, her uygulama tarafından kullanılan enstrümantasyon anahtarını tanımlayan bir applicationinsights.ikey.config dosyası oluşturur.
IIS, redfieldmodule'ı başlangıç olarak yükleyecek ve uygulamalar başladıkça Application Insights SDK'yı uygulamalara enjekte edecektir.
Değişikliklerinizin etkili olması için IIS'yi yeniden başlatın.

İzlemeyi etkinleştirdikten sonra, uygulamanızın bize telemetri gönderip göndermediğini hızlı bir şekilde kontrol etmek için [Canlı Ölçümler'i](live-stream.md) kullanmanızı öneririz.

### <a name="examples"></a>Örnekler

#### <a name="example-with-a-single-instrumentation-key"></a>Tek bir enstrümantasyon tuşu ile örnek
Bu örnekte, geçerli bilgisayardaki tüm uygulamalara tek bir enstrümantasyon anahtarı atanır.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### <a name="example-with-an-instrumentation-key-map"></a>Enstrümantasyon anahtar haritası ile örnek
Bu örnekte:
- `MachineFilter`joker kartı kullanarak geçerli `'.*'` bilgisayarla eşleşir.
- `AppFilter='WebAppExclude'`bir `null` enstrümantasyon anahtarı sağlar. Belirtilen uygulama enstrümanted olmayacaktır.
- `AppFilter='WebAppOne'`belirtilen uygulamaya benzersiz bir enstrümantasyon anahtarı atar.
- `AppFilter='WebAppTwo'`belirtilen uygulamaya benzersiz bir enstrümantasyon anahtarı atar.
- Son `AppFilter` olarak, `'.*'` joker kartı, önceki kurallarla eşleşmeyan tüm web uygulamalarını eşleştirmek ve varsayılan bir enstrümantasyon anahtarı atamak için de kullanır.
- Okunabilirlik için boşluklar eklenir.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'}},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'}},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'}})

```


### <a name="parameters"></a>Parametreler

#### <a name="-instrumentationkey"></a>-EnstrümantasyonAnahtar
**Gereklidir.** Hedef bilgisayardaki tüm uygulamalar tarafından kullanılmak üzere tek bir enstrümantasyon anahtarı sağlamak için bu parametreyi kullanın.

#### <a name="-instrumentationkeymap"></a>-EnstrümantasyonKeyMap
**Gereklidir.** Birden çok enstrümantasyon anahtarı ve her uygulama tarafından kullanılan enstrümantasyon anahtarlarının eşlemi sağlamak için bu parametreyi kullanın.
Birkaç bilgisayar için tek bir yükleme `MachineFilter`komut dosyası ayarlayarak oluşturabilirsiniz.

> [!IMPORTANT]
> Uygulamalar, kuralların sağlandığı sırada kurallara göre eşleşir. Bu nedenle, önce en özel kuralları ve en genel kuralları en son belirtmelisiniz.

##### <a name="schema"></a>Şema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'}})`

- **MachineFilter,** bilgisayarın veya VM adının gerekli bir C# regex'idir.
    - '.*' tüm maç olacak
    - 'ComputerName' yalnızca belirtilen adı tam olan bilgisayarlarla eşleşir.
- **AppFilter,** IIS Site Adının gerekli bir C# regex'idir. Komut [get-iissite](https://docs.microsoft.com/powershell/module/iisadministration/get-iissite)çalıştırarak sunucunuzda sitelerin bir listesini alabilirsiniz.
    - '.*' tüm maç olacak
    - 'SiteAdı' yalnızca IIS Sitesi ile tam adı belirtilen şekilde eşleşir.
- **InstrumentationKey,** önceki iki filtreyle eşleşen uygulamaların izlenmesini etkinleştirmek için gereklidir.
    - İzlemeyi hariç tutmak için kurallar tanımlamak istiyorsanız bu değeri boş bırakın.


#### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Isteğe bağlı.** Enstrümantasyon motorunun yönetilen bir işlemin yürütülmesi sırasında neler olduğuna ilişkin olayları ve iletileri toplamasını sağlamak için bu anahtarı kullanın. Bu olaylar ve iletiler bağımlılık sonuç kodlarını, HTTP fiilleri ve SQL komut metnini içerir.

Enstrümantasyon motoru ek yükü ekler ve varsayılan olarak kapalıdır.

#### <a name="-acceptlicense"></a>-Kabul Lisansı
**Isteğe bağlı.** Başsız yüklemelerde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

#### <a name="-ignoresharedconfig"></a>-Yok SaygınSharedConfig
Bir web sunucuları kümeniz olduğunda, paylaşılan bir [yapılandırma](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211)kullanıyor olabilirsiniz.
HttpModülü bu paylaşılan yapılandırmaya enjekte edilemez.
Bu komut dosyası, ek yükleme adımları gerekli olduğu iletisi ile başarısız olur.
Bu denetimi yoksaymak ve ön koşulları yüklemeye devam etmek için bu anahtarı kullanın. Daha fazla bilgi için [bkz: bilinen çakışma-iis-paylaşılan yapılandırma](status-monitor-v2-troubleshoot.md#conflict-with-iis-shared-configuration)

#### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.

#### <a name="-whatif"></a>-WhatIf 
**Ortak parametre.** Aslında izleme etkinleştirmeden giriş parametrelerinizi test etmek ve doğrulamak için bu anahtarı kullanın.

### <a name="output"></a>Çıktı

#### <a name="example-output-from-a-successful-enablement"></a>Başarılı bir etkinleştirmeden örnek çıktı

```powershell
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
Installing GAC module 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.0\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll'
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
Found GAC module Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z_1'
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'enable'
Configuring IIS Environment for codeless attach...
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
Updating app pool permissions...
Successfully enabled Application Insights Status Monitor
```

## <a name="disable-instrumentationengine"></a>Devre Dışı Bırakma-InstrumentationEngine

Bazı kayıt defteri anahtarlarını kaldırarak enstrümantasyon motorunu devre dışı kaldırır.
Değişikliklerin etkili olması için IIS'yi yeniden başlatın.

### <a name="examples"></a>Örnekler

```powershell
PS C:\> Disable-InstrumentationEngine
```

### <a name="parameters"></a>Parametreler 

#### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri çıktıetmek için bu anahtarı kullanın.

### <a name="output"></a>Çıktı


##### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Enstrümantasyon motorunun başarılı bir şekilde devre dışı bırakılmasından örnek çıktı

```powershell
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```

## <a name="disable-applicationinsightsmonitoring"></a>Devre Dışı-UygulamaInsightsİzleme

Hedef bilgisayardaki izlemeyi devre dışı kılabilir.
Bu cmdlet IIS applicationHost.config ve kayıt defteri anahtarlarını kaldırmak için yapılan ları kaldırır.

### <a name="examples"></a>Örnekler

```powershell
PS C:\> Disable-ApplicationInsightsMonitoring
```

### <a name="parameters"></a>Parametreler 

#### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.

### <a name="output"></a>Çıktı


##### <a name="example-output-from-successfully-disabling-monitoring"></a>İzlemeyi başarıyla devre dışı bırakmaktan örnek çıktı

```powershell
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-00z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
```


## <a name="get-applicationinsightsmonitoringconfig"></a>Get-ApplicationInsightsMonitoringConfig

Config dosyasını alır ve değerleri konsola yazdırır.

### <a name="examples"></a>Örnekler

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

### <a name="parameters"></a>Parametreler

Parametre gerekmez.

### <a name="output"></a>Çıktı


##### <a name="example-output-from-reading-the-config-file"></a>Config dosyasını okuyarak örnek çıktı

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```

## <a name="get-applicationinsightsmonitoringstatus"></a>Get-ApplicationInsightsİzlemeDurumu

Bu cmdlet Durum İzleyicisi hakkında sorun giderme bilgileri sağlar.
PowerShell Modülü'nün izleme durumunu, sürümünü incelemek ve çalıştırma işlemini incelemek için bu cmdlet'i kullanın.
Bu cmdlet, izleme için gerekli olan önemli dosyalar la ilgili sürüm bilgilerini ve bilgilerini raporedecektir.

### <a name="examples"></a>Örnekler

#### <a name="example-application-status"></a>Örnek: Uygulama durumu

Web sitelerinin izleme durumunu görüntülemek için komutu `Get-ApplicationInsightsMonitoringStatus` çalıştırın.

```powershell

PS C:\Windows\system32> Get-ApplicationInsightsMonitoringStatus

IIS Websites:

SiteName               : Default Web Site
ApplicationPoolName    : DefaultAppPool
SiteId                 : 1
SiteState              : Stopped

SiteName               : DemoWebApp111
ApplicationPoolName    : DemoWebApp111
SiteId                 : 2
SiteState              : Started
ProcessId              : not found

SiteName               : DemoWebApp222
ApplicationPoolName    : DemoWebApp222
SiteId                 : 3
SiteState              : Started
ProcessId              : 2024
Instrumented           : true
InstrumentationKey     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123

SiteName               : DemoWebApp333
ApplicationPoolName    : DemoWebApp333
SiteId                 : 4
SiteState              : Started
ProcessId              : 5184
AppAlreadyInstrumented : true
```

Bu örnekte;
- **Makine Tanımlayıcısı,** sunucunuzu benzersiz olarak tanımlamak için kullanılan anonim bir kimliktir. Bir destek isteği oluşturursanız, sunucunuz için günlükleri bulmak için bu kimliğin gerekir.
- **Varsayılan Web Sitesi** IIS'de Durduruldu
- **DemoWebApp111** IIS'de başlatıldı, ancak herhangi bir istek almadı. Bu rapor, çalışma işlemi olmadığını gösterir (ProcessId: bulunamadı).
- **DemoWebApp222** çalışıyor ve izleniyor (Instrumented: true). Kullanıcı yapılandırmadayanarak, Instrumentation Key xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123 bu site için eşleşti.
- **DemoWebApp333,** Application Insights SDK kullanılarak el ile işletilmiştir. Durum İzleyicisi SDK'yı algıladı ve bu siteyi izlemez.


#### <a name="example-powershell-module-information"></a>Örnek: PowerShell modül bilgileri

Geçerli modül `Get-ApplicationInsightsMonitoringStatus -PowerShellModule` hakkında bilgi görüntülemek için komutu çalıştırın:

```powershell

PS C:\> Get-ApplicationInsightsMonitoringStatus -PowerShellModule

PowerShell Module version:
0.4.0-alpha

Application Insights SDK version:
2.9.0.3872

Executing PowerShell Module Assembly:
Microsoft.ApplicationInsights.Redfield.Configurator.PowerShell, Version=2.8.14.11432, Culture=neutral, PublicKeyToken=31bf3856ad364e35

PowerShell Module Directory:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.2\content\PowerShell

Runtime Paths:
ParentDirectory (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content

ConfigurationPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config

ManagedHttpModuleHelperPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll

RedfieldIISModulePath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

InstrumentationEngine86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\MicrosoftInstrumentationEngine_x86.dll

InstrumentationEngine64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll

InstrumentationEngineExtensionHost86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.ApplicationInsights.ExtensionsHost_x86.dll

InstrumentationEngineExtensionHost64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll

InstrumentationEngineExtensionConfig86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.InstrumentationEngine.Extensions.config

InstrumentationEngineExtensionConfig64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.InstrumentationEngine.Extensions.config

ApplicationInsightsSdkPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll
```

#### <a name="example-runtime-status"></a>Örnek: Çalışma zamanı durumu

Tüm DL'lerin yüklenip yüklenmediğinizi görmek için işlem aracılı bilgisayarda inceleyebilirsiniz. İzleme çalışıyorsa, en az 12 DL yüklenmelidir.

Komutu `Get-ApplicationInsightsMonitoringStatus -InspectProcess`çalıştırın:


```
PS C:\> Get-ApplicationInsightsMonitoringStatus -InspectProcess

iisreset.exe /status
Status for IIS Admin Service ( IISADMIN ) : Running
Status for Windows Process Activation Service ( WAS ) : Running
Status for Net.Msmq Listener Adapter ( NetMsmqActivator ) : Running
Status for Net.Pipe Listener Adapter ( NetPipeActivator ) : Running
Status for Net.Tcp Listener Adapter ( NetTcpActivator ) : Running
Status for World Wide Web Publishing Service ( W3SVC ) : Running

handle64.exe -accepteula -p w3wp
BF0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.ServerTelemetryChannel.dll
C58: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.AzureAppServices.dll
C68: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.DependencyCollector.dll
C78: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.WindowsServer.dll
C98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Web.dll
CBC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.PerfCounterCollector.dll
DB0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Agent.Intercept.dll
B98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll
BB4: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.Contracts.dll
BCC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll
BE0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll

listdlls64.exe -accepteula w3wp
0x0000000019ac0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
0x00000000198b0000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
0x000000000c460000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
0x000000000ad60000  0x108000  C:\Windows\TEMP\2.4.0.0.Microsoft.ApplicationInsights.Extensions.Intercept_x64.dll
```

### <a name="parameters"></a>Parametreler

#### <a name="no-parameters"></a>(Parametre yok)

Varsayılan olarak, bu cmdlet web uygulamalarının izleme durumunu bildirir.
Uygulamanızın başarılı bir şekilde algılanmış olup olmadığını gözden geçirmek için bu seçeneği kullanın.
Ayrıca, hangi Enstrümantasyon Anahtarının sitenizle eşleşebileceğini de inceleyebilirsiniz.


#### <a name="-powershellmodule"></a>-PowerShellModülü
**İsteğe bağlı**. İzleme için gereken DL'lerin sürüm numaralarını ve yollarını bildirmek için bu anahtarı kullanın.
Application Insights SDK de dahil olmak üzere herhangi bir DLL sürümünü tanımlamanız gerekiyorsa bu seçeneği kullanın.

#### <a name="-inspectprocess"></a>-İnceleme Süreci

**İsteğe bağlı**. IIS'nin çalışıp çalışmadığını bildirmek için bu anahtarı kullanın.
Ayrıca, gerekli DL'lerin IIS çalışma süresine yüklenip yüklenmeyeceğini belirlemek için harici araçlar indirir.


Bu işlem herhangi bir nedenle başarısız olursa, bu komutları el ile çalıştırabilirsiniz:
- iisreset.exe /durum
- [handle64.exe](https://docs.microsoft.com/sysinternals/downloads/handle) -p w3wp | findstr /I "InstrumentationEngine AI. UygulamaInsights"
- [listdlls64.exe](https://docs.microsoft.com/sysinternals/downloads/listdlls) w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"


#### <a name="-force"></a>-Force

**İsteğe bağlı**. Yalnızca InspectProcess ile kullanılır. Ek araçlar indirilmeden önce görünen kullanıcı istemini atlamak için bu anahtarı kullanın.


## <a name="set-applicationinsightsmonitoringconfig"></a>Set-ApplicationInsightsMonitoringConfig

Config dosyasını tam yeniden yükleme yapmadan ayarlar.
Değişikliklerinizin etkili olması için IIS'yi yeniden başlatın.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri içeren bir PowerShell oturumu gerektirir.


### <a name="examples"></a>Örnekler

#### <a name="example-with-a-single-instrumentation-key"></a>Tek bir enstrümantasyon tuşu ile örnek
Bu örnekte, geçerli bilgisayardaki tüm uygulamalara tek bir enstrümantasyon anahtarı atanır.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### <a name="example-with-an-instrumentation-key-map"></a>Enstrümantasyon anahtar haritası ile örnek
Bu örnekte:
- `MachineFilter`joker kartı kullanarak geçerli `'.*'` bilgisayarla eşleşir.
- `AppFilter='WebAppExclude'`bir `null` enstrümantasyon anahtarı sağlar. Belirtilen uygulama enstrümanted olmayacaktır.
- `AppFilter='WebAppOne'`belirtilen uygulamaya benzersiz bir enstrümantasyon anahtarı atar.
- `AppFilter='WebAppTwo'`belirtilen uygulamaya benzersiz bir enstrümantasyon anahtarı atar.
- Son `AppFilter` olarak, `'.*'` joker kartı, önceki kurallarla eşleşmeyan tüm web uygulamalarını eşleştirmek ve varsayılan bir enstrümantasyon anahtarı atamak için de kullanır.
- Okunabilirlik için boşluklar eklenir.

```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap `
       @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
          @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'}},
          @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'}},
          @{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'}})
```

### <a name="parameters"></a>Parametreler

#### <a name="-instrumentationkey"></a>-EnstrümantasyonAnahtar
**Gereklidir.** Hedef bilgisayardaki tüm uygulamalar tarafından kullanılmak üzere tek bir enstrümantasyon anahtarı sağlamak için bu parametreyi kullanın.

#### <a name="-instrumentationkeymap"></a>-EnstrümantasyonKeyMap
**Gereklidir.** Birden çok enstrümantasyon anahtarı ve her uygulama tarafından kullanılan enstrümantasyon anahtarlarının eşlemi sağlamak için bu parametreyi kullanın.
Birkaç bilgisayar için tek bir yükleme `MachineFilter`komut dosyası ayarlayarak oluşturabilirsiniz.

> [!IMPORTANT]
> Uygulamalar, kuralların sağlandığı sırada kurallara göre eşleşir. Bu nedenle, önce en özel kuralları ve en genel kuralları en son belirtmelisiniz.

##### <a name="schema"></a>Şema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter,** bilgisayarın veya VM adının gerekli bir C# regex'idir.
    - '.*' tüm maç olacak
    - 'ComputerName' yalnızca belirtilen ada sahip bilgisayarlarla eşleşir.
- **AppFilter,** bilgisayarın veya VM adının gerekli bir C# regex'idir.
    - '.*' tüm maç olacak
    - 'ApplicationName' belirtilen ada sahip yalnızca IIS uygulamalarıyla eşleşir.
- **InstrumentationKey,** önceki iki filtreyle eşleşen uygulamaların izlenmesini etkinleştirmek için gereklidir.
    - İzlemeyi hariç tutmak için kurallar tanımlamak istiyorsanız bu değeri boş bırakın.


#### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.


### <a name="output"></a>Çıktı

Varsayılan olarak, çıktı yok.

##### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>-InstrumentationKey üzerinden config dosyasını ayarlamaktan örnek verbose çıktı

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

##### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>-InstrumentationKeyMap üzerinden config dosyasını ayarlamaktan örnek verbose çıktı

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

## <a name="start-applicationinsightsmonitoringtrace"></a>Başlat-UygulamaInsightsMonitoringTrace

Kodsuz ekleme çalışma saatinden [ETW Olayları](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) toplar. Bu cmdlet [PerfView](https://github.com/microsoft/perfview)çalışan bir alternatiftir.

Toplanan olaylar gerçek zamanlı olarak konsola yazdırılır ve bir ETL dosyasına kaydedilir. Çıktı ETL dosyası daha fazla araştırma için [PerfView](https://github.com/microsoft/perfview) tarafından açılabilir.

Bu cmdlet zaman adedine (varsayılan 5 dakika) ulaşıncaya`Ctrl + C`veya el ile durdurulana kadar çalışır ( ).

### <a name="examples"></a>Örnekler

#### <a name="how-to-collect-events"></a>Etkinlikleri nasıl toplarlar?

Normalde, başvurunuzun neden çalındığını araştırmak için olayları toplamanızı isteriz.

Kodsuz ekleme çalışma süresi, IIS başlatıldığında ve uygulamanız başlatıldığında ETW olayları yayacaktır.

Bu olayları toplamak için:
1. Yönetici ayrıcalıklarına sahip cmd konsolunda, IIS'yi ve tüm web uygulamalarını kapatmak için çalıştırın. `iisreset /stop`
2. Bu cmdlet çalıştırın
3. Yönetici ayrıcalıklarına sahip cmd konsolunda, IIS'yi başlatmak için çalıştırın. `iisreset /start`
4. Uygulamanıza göz atmaya çalışın.
5. Uygulamanız yüklemeyi bitirdikten sonra, uygulamayı`Ctrl + C`el ile durdurabilir veya zaman anına kadar bekleyebilirsiniz.

#### <a name="what-events-to-collect"></a>Hangi etkinliklerin toplanması

Olayları toplarken üç seçeneğiniz var:
1. Application Insights SDK'dan yayılan olayları toplamak için anahtarı `-CollectSdkEvents` kullanın.
2. Status Monitor `-CollectRedfieldEvents` ve Redfield Runtime tarafından yayılan olayları toplamak için anahtarı kullanın. Bu günlükler, IIS ve uygulama başlatma tanılama yardımcı olur.
3. Her iki olay türünü de toplamak için her iki anahtarı da kullanın.
4. Varsayılan olarak, anahtar belirtilmemişse, her iki olay türü de toplanır.


### <a name="parameters"></a>Parametreler

#### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**Isteğe bağlı.** Bu komut dosyasının olayları ne kadar süreyle toplaması gerektiğini ayarlamak için bu parametreyi kullanın. Varsayılan değer 5 dakikadır.

#### <a name="-logdirectory"></a>-LogDirectory
**Isteğe bağlı.** ETL dosyasının çıktı dizini ayarlamak için bu anahtarı kullanın. Varsayılan olarak, bu dosya PowerShell Modülleri dizininde oluşturulur. Tam yol komut dosyası yürütme sırasında görüntülenir.


#### <a name="-collectsdkevents"></a>-CollectSdkEtkinlikler
**Isteğe bağlı.** Application Insights SDK olaylarını toplamak için bu anahtarı kullanın.

#### <a name="-collectredfieldevents"></a>-CollectRedfield Etkinlikler
**Isteğe bağlı.** Durum İzleyicisi ve Redfield çalışma saatinden olayları toplamak için bu anahtarı kullanın.

#### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri çıktıetmek için bu anahtarı kullanın.



### <a name="output"></a>Çıktı


#### <a name="example-of-application-startup-logs"></a>Uygulama başlangıç günlükleri örneği
```powershell
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```

## <a name="next-steps"></a>Sonraki adımlar

  Telemetrinizi görüntüleyin:
 - Performansı ve kullanımı izlemek için [ölçümleri keşfedin.](../../azure-monitor/app/metrics-explorer.md)
- Sorunları tanılamak için [olayları ve günlükleri arayın.](../../azure-monitor/app/diagnostic-search.md)
- Daha gelişmiş sorgular için [analitiği](../../azure-monitor/app/analytics.md) kullanın.
- [Panolar oluşturun.](../../azure-monitor/app/overview-dashboard.md)
 
 Daha fazla telemetri ekleyin:
 - Sitenizin canlı kalması için [web testleri oluşturun](monitor-web-app-availability.md).
- Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [web istemcisi telemetrisi ekleyin.](../../azure-monitor/app/javascript.md)
- İzleme ve günlük aramaları ekleyebilmeniz [için Uygulama Öngörüleri SDK'yı kodunuza ekleyin.](../../azure-monitor/app/asp-net.md)
 
 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.






