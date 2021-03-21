---
title: Karma ortam için Azure Izleyicisini etkinleştirme
description: Bu makalede, bir veya daha fazla sanal makine içeren karma bulut ortamı için VM öngörülerinin nasıl etkinleştirileceği açıklanır.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: c3c8495b7355ee5d9ee8c28b4e0097a0080964d4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102046658"
---
# <a name="enable-vm-insights-for-a-hybrid-virtual-machine"></a>Karma sanal makine için VM öngörülerini etkinleştirme
Bu makalede, şirket içi ve diğer bulut ortamları dahil olmak üzere Azure dışındaki bir sanal makine için VM öngörülerinin nasıl etkinleştirileceği açıklanır.

> [!IMPORTANT]
> Karma VM 'Leri etkinleştirmenin önerilen yöntemi, sanal makinelerin Azure sanal makinelerine benzer süreçler kullanılarak VM öngörüleri için etkinleştirilebilmesi için öncelikle [sunucular Için Azure yayı](../../azure-arc/servers/overview.md) 'yi etkinleştirir. Bu makalede, Azure Arc 'ı kullanmayı tercih ediyorsanız karma VM 'Lerin nasıl ekleneceği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- [Log Analytics çalışma alanı oluşturun ve yapılandırın](./vminsights-configure-workspace.md).
- Etkinleştirmiş olduğunuz sanal makine veya sanal makine ölçek kümesinin işletim sisteminin desteklendiğinden emin olmak için [desteklenen işletim sistemleri](./vminsights-enable-overview.md#supported-operating-systems) bölümüne bakın. 


## <a name="overview"></a>Genel Bakış
Azure dışındaki sanal makineler, Azure VM 'Leri için kullanılan aynı Log Analytics aracısını ve bağımlılık aracısını gerektirir. Ancak aracıları yüklemek için VM uzantılarını kullanamıyoruz, bu uygulamaları Konuk işletim sistemine el ile yüklemeli veya başka bir yöntemle yüklemiş olmanız gerekir. 

Log Analytics aracısını dağıtma hakkında ayrıntılı bilgi için bkz. [Windows bilgisayarlarını Azure izleyici 'ye bağlama](../agents/agent-windows.md) veya [Linux bilgisayarlarını Azure izleyici 'ye bağlama](../agents/agent-linux.md) . Bu makalede bağımlılık aracısının ayrıntıları verilmiştir. 

## <a name="firewall-requirements"></a>Güvenlik duvarı gereksinimleri
Log Analytics aracısına yönelik güvenlik duvarı gereksinimleri [Log Analytics aracıya genel bakış](../agents/log-analytics-agent.md#network-requirements)bölümünde verilmiştir. VM öngörüleri eşleme bağımlılık Aracısı herhangi bir veri iletmez ve güvenlik duvarları veya bağlantı noktalarında değişiklik gerektirmez. Harita verileri her zaman doğrudan veya [Operations Management Suite ağ geçidi](../../azure-monitor/agents/gateway.md) aracılığıyla Log Analytics ARACıSıDıR ve BT güvenlik ilkeleriniz ağdaki bilgisayarların internet 'e bağlanmasına izin vermez.


## <a name="dependency-agent"></a>Bağımlılık aracısı

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler [hizmet eşlemesi çözümü](./service-map.md)için de geçerlidir.  

Bağımlılık aracısını şu konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.10.7.12710 | CA29CC328F991D7301FD0360F4F56DF78275545BB8CDA853679899CA885E96F0  |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.10.7.12710 | 98380DBEB2E2A5848F2202BC22422C68B20B62090C1BFC1DECAB37ED5451ED8C |


## <a name="install-the-dependency-agent-on-windows"></a>Windows 'a bağımlılık Aracısı 'nı yükler

' I çalıştırarak, Windows bilgisayarlarına bağımlılık aracısını el ile yükleyebilirsiniz `InstallDependencyAgent-Windows.exe` . Bu yürütülebilir dosyayı herhangi bir seçenek olmadan çalıştırırsanız, aracı etkileşimli olarak yüklemek için izleyebileceğiniz bir Kurulum Sihirbazı başlatılır. Aracıyı yüklemek veya kaldırmak için konuk işletim sisteminde *yönetici* ayrıcalıklarına sahip olmanız gerekir.

Aşağıdaki tabloda, komut satırından aracının kurulumu tarafından desteklenen parametreler vurgulanmıştır.

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin bir listesini döndürür. |
| Sn | Kullanıcı etkileşimi olmadan sessiz yükleme gerçekleştirir. |

Örneğin, yükleme programını parametresiyle çalıştırmak için `/?` **InstallDependencyAgent-Windows.exe/?** girin.

Windows bağımlılık aracısının dosyaları varsayılan olarak *C:\Program Files\Microsoft Dependency Agent* 'e yüklenir. Kurulum tamamlandıktan sonra bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Günlük dizini *%ProgramFiles%\Microsoft Dependency Fıles\logs* dizinidir.

### <a name="powershell-script"></a>PowerShell betiği
Aracıyı indirmek ve yüklemek için aşağıdaki örnek PowerShell betiğini kullanın:

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```


## <a name="install-the-dependency-agent-on-linux"></a>Linux 'a bağımlılık Aracısı 'nı yükler

Bağımlılık Aracısı, kendi kendine ayıklanan ikiliye sahip bir kabuk betiği olan *InstallDependencyAgent-linux64. bin*' dan Linux sunucularına yüklenir. Dosyasını kullanarak `sh` veya dosya üzerinde yürütme izinleri ekleyerek dosyayı çalıştırabilirsiniz.

>[!NOTE]
> Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.
>

| Parametre | Açıklama |
|:--|:--|
| -yardım | Komut satırı seçeneklerinin listesini alır. |
| -s | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| --denetle | İzinleri ve işletim sistemini denetleyin, ancak aracıyı yüklemeyin. |

Örneğin, yükleme programını parametresiyle çalıştırmak için `-help` , **InstallDependencyAgent-linux64. bin-Help** girin. Komutunu çalıştırarak Linux bağımlılık aracısını kök olarak yükler `sh InstallDependencyAgent-Linux64.bin` .

Bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Linux aracılarında günlük dizini */var/seçenek/Microsoft/Dependency-Agent/log* olur.

Bağımlılık aracısına ait dosyalar aşağıdaki dizinlere yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

### <a name="shell-script"></a>Kabuk betiği 
Aracıyı indirmek ve yüklemek için aşağıdaki örnek kabuk betiğini kullanın:

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>İstenen Durum Yapılandırması

Istenen durum yapılandırması (DSC) kullanarak bağımlılık aracısını dağıtmak için, xPSDesiredStateConfiguration modülünü aşağıdaki örnek kodla kullanabilirsiniz:

```powershell
configuration VMInsights {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```



## <a name="troubleshooting"></a>Sorun giderme

### <a name="vm-doesnt-appear-on-the-map"></a>VM haritada görünmüyor

Bağımlılık aracısı yüklemeniz başarılı oldu, ancak bilgisayarınızı haritada görmüyorsanız, bu adımları izleyerek sorunu tanılayın.

1. Bağımlılık aracısı başarıyla yüklendi mi? Bunu doğrulamak için hizmetin yüklü ve çalışır durumda olup olmadığını kontrol edebilirsiniz.

    **Windows**: "Microsoft bağımlılık Aracısı" adlı hizmeti arayın.

    **Linux**: "Microsoft-Dependency-Agent" çalıştırma işlemini arayın.

2. [Log Analytics ücretsiz fiyatlandırma katmanında](../insights/solutions.md)misiniz? Ücretsiz plan, beş adede kadar benzersiz bilgisayara izin verir. Önceki beş, artık veri göndermese bile, sonraki tüm bilgisayarlar haritada gösterilmez.

3. Bilgisayar Azure Izleyici günlüklerine günlük ve performans verileri gönderiyor mu? Bilgisayarınız için aşağıdaki sorguyu gerçekleştirin:

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    Bir veya daha fazla sonuç döndürüyor mu? Veriler güncel mi? Bu durumda, Log Analytics aracınız doğru şekilde çalışıyor ve hizmetle iletişim kuruyor. Aksi takdirde, sunucunuzdaki aracıyı denetleyin: [Windows sorun giderme için Log Analytics Aracısı](../agents/agent-windows-troubleshoot.md) veya [Linux sorun giderme için Log Analytics Aracısı](../agents/agent-linux-troubleshoot.md).

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>Bilgisayar haritada görünüyor ancak işleme sahip değil

Sunucunuzu haritada görürseniz, ancak bir işlem veya bağlantı verisi yoksa, bağımlılık aracısının yüklü olduğunu ve çalıştığını, ancak çekirdek sürücüsünün yüklenmediğini belirtir.

C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log dosyasını (Windows) veya /var/opt/microsoft/dependency-agent/log/service.log doyasını (Linux) kontrol edin. Dosyanın son satırları, çekirdeğin neden yüklenmediğini göstermelidir. Örneğin çekirdeğinizi güncelleştirdiyseniz yeni çekirdek için Linux desteği sunulmuyor olabilir.


## <a name="next-steps"></a>Sonraki adımlar

Artık sanal makineleriniz için izleme etkin olduğuna göre, bu bilgiler VM öngörüleri ile analiz edilmek üzere kullanılabilir.

- Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM Insights haritasını görüntüleme](vminsights-maps.md).

- VM performanlarınızın performans sorunlarını ve genel kullanımını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md).