---
title: Azure Uygulama Öngörüleri AracıAP başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Enable-ApplicationInsightsMonitoring. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 8bbdc96a49fffc91f80d24a9eb0926766f86ee16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671316"
---
# <a name="application-insights-agent-api-enable-applicationinsightsmonitoring"></a>Uygulama Öngörüleri Aracısı API: Enable-ApplicationInsightsMonitoring

Bu [makalede, Az.ApplicationMonitor PowerShell modülüüyesi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir cmdlet açıklanır.

## <a name="description"></a>Açıklama

Hedef bilgisayarda IIS uygulamalarının kodsuz olarak izlenmesini sağlar.

Bu cmdlet IIS applicationHost.config değiştirmek ve bazı kayıt anahtarları ayarlayın.
Ayrıca, her uygulama tarafından kullanılan enstrümantasyon anahtarını tanımlayan bir applicationinsights.ikey.config dosyası oluşturur.
IIS, redfieldmodule'ı başlangıç olarak yükleyecek ve uygulamalar başladıkça Application Insights SDK'yı uygulamalara enjekte edecektir.
Değişikliklerinizin etkili olması için IIS'yi yeniden başlatın.

İzlemeyi etkinleştirdikten sonra, uygulamanızın bize telemetri gönderip göndermediğini hızlı bir şekilde kontrol etmek için [Canlı Ölçümler'i](live-stream.md) kullanmanızı öneririz.


> [!NOTE] 
> - Başlamak için bir enstrümantasyon anahtarına ihtiyacınız var. Daha fazla bilgi için [bkz.](create-new-resource.md#copy-the-instrumentation-key)
> - Bu cmdlet, lisans ve gizlilik bildirimimizi gözden geçirmenizi ve kabul lenmenizi gerektirir.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri ve yüksek yürütme ilkesi içeren bir PowerShell oturumu gerektirir. Daha fazla bilgi için [PowerShell'i yükseltilmiş yürütme ilkesine sahip yönetici olarak çalıştır'a](status-monitor-v2-detailed-instructions.md#run-powershell-as-admin-with-an-elevated-execution-policy)bakın.

## <a name="examples"></a>Örnekler

### <a name="example-with-a-single-instrumentation-key"></a>Tek bir enstrümantasyon tuşu ile örnek
Bu örnekte, geçerli bilgisayardaki tüm uygulamalara tek bir enstrümantasyon anahtarı atanır.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Enstrümantasyon anahtar haritası ile örnek
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


## <a name="parameters"></a>Parametreler

### <a name="-instrumentationkey"></a>-EnstrümantasyonAnahtar
**Gerekli.** Hedef bilgisayardaki tüm uygulamalar tarafından kullanılmak üzere tek bir enstrümantasyon anahtarı sağlamak için bu parametreyi kullanın.

### <a name="-instrumentationkeymap"></a>-EnstrümantasyonKeyMap
**Gerekli.** Birden çok enstrümantasyon anahtarı ve her uygulama tarafından kullanılan enstrümantasyon anahtarlarının eşlemi sağlamak için bu parametreyi kullanın.
Birkaç bilgisayar için tek bir yükleme `MachineFilter`komut dosyası ayarlayarak oluşturabilirsiniz.

> [!IMPORTANT]
> Uygulamalar, kuralların sağlandığı sırada kurallara göre eşleşir. Bu nedenle, önce en özel kuralları ve en genel kuralları en son belirtmelisiniz.

#### <a name="schema"></a>Şema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'}})`

- **MachineFilter,** bilgisayarın veya VM adının gerekli bir C# regex'idir.
    - '.*' tüm maç olacak
    - 'ComputerName' yalnızca belirtilen adı tam olan bilgisayarlarla eşleşir.
- **AppFilter,** IIS Site Adının gerekli bir C# regex'idir. Komut [get-iissite](https://docs.microsoft.com/powershell/module/iisadministration/get-iissite)çalıştırarak sunucunuzda sitelerin bir listesini alabilirsiniz.
    - '.*' tüm maç olacak
    - 'SiteAdı' yalnızca IIS Sitesi ile tam adı belirtilen şekilde eşleşir.
- **InstrumentationKey,** önceki iki filtreyle eşleşen uygulamaların izlenmesini etkinleştirmek için gereklidir.
    - İzlemeyi hariç tutmak için kurallar tanımlamak istiyorsanız bu değeri boş bırakın.


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Isteğe bağlı.** Enstrümantasyon motorunun yönetilen bir işlemin yürütülmesi sırasında neler olduğuna ilişkin olayları ve iletileri toplamasını sağlamak için bu anahtarı kullanın. Bu olaylar ve iletiler bağımlılık sonuç kodlarını, HTTP fiilleri ve SQL komut metnini içerir.

Enstrümantasyon motoru ek yükü ekler ve varsayılan olarak kapalıdır.

### <a name="-acceptlicense"></a>-Kabul Lisansı
**Isteğe bağlı.** Başsız yüklemelerde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-ignoresharedconfig"></a>-Yok SaygınSharedConfig
Bir web sunucuları kümeniz olduğunda, paylaşılan bir [yapılandırma](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211)kullanıyor olabilirsiniz.
HttpModülü bu paylaşılan yapılandırmaya enjekte edilemez.
Bu komut dosyası, ek yükleme adımları gerekli olduğu iletisi ile başarısız olur.
Bu denetimi yoksaymak ve ön koşulları yüklemeye devam etmek için bu anahtarı kullanın. Daha fazla bilgi için [bkz: bilinen çakışma-iis-paylaşılan yapılandırma](status-monitor-v2-troubleshoot.md#conflict-with-iis-shared-configuration)

### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.

### <a name="-whatif"></a>-WhatIf 
**Ortak parametre.** Aslında izleme etkinleştirmeden giriş parametrelerinizi test etmek ve doğrulamak için bu anahtarı kullanın.

## <a name="output"></a>Çıktı


#### <a name="example-output-from-a-successful-enablement"></a>Başarılı bir etkinleştirmeden örnek çıktı

```
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

## <a name="next-steps"></a>Sonraki adımlar

  Telemetrinizi görüntüleyin:
 - Performansı ve kullanımı izlemek için [ölçümleri keşfedin.](../../azure-monitor/app/metrics-explorer.md)
- Sorunları tanılamak için [olayları ve günlükleri arayın.](../../azure-monitor/app/diagnostic-search.md)
- Daha gelişmiş sorgular için [Analytics'i kullanın.](../../azure-monitor/app/analytics.md)
- [Panolar oluşturun.](../../azure-monitor/app/overview-dashboard.md)
 
 Daha fazla telemetri ekleyin:
 - Sitenizin canlı kalması için [web testleri oluşturun](monitor-web-app-availability.md).
- Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [web istemcisi telemetrisi ekleyin.](../../azure-monitor/app/javascript.md)
- İzleme ve günlük aramaları ekleyebilmeniz [için Uygulama Öngörüleri SDK'yı kodunuza ekleyin.](../../azure-monitor/app/asp-net.md)
 
 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
 - Ayarlarınızın doğru kaydedildiğini doğrulamak için [config'i alın.](status-monitor-v2-api-get-config.md)
 - İzlemeyi incelemek için [durumu alın.](status-monitor-v2-api-get-status.md)
