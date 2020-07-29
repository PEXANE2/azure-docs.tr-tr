---
title: Karma ortam için Azure Izleyicisini etkinleştirme
description: Bu makalede, bir veya daha fazla sanal makine içeren karma bulut ortamı için VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 7a6105e8742a4cb3d2f113c6ef723f6171baf4d9
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87328442"
---
# <a name="enable-azure-monitor-for-vms-for-a-hybrid-virtual-machine"></a>Karma sanal makine için VM'ler için Azure İzleyici etkinleştirme
Bu makalede, şirket içi ve diğer bulut ortamları dahil olmak üzere Azure dışındaki bir sanal makine için VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır.

> [!IMPORTANT]
> Karma VM 'Leri etkinleştirmenin önerilen yöntemi, VM 'Lerin Azure sanal makinelerine benzer süreçler kullanılarak VM'ler için Azure İzleyici için etkinleştirilebilmesi için öncelikle [sunucular Için Azure yayı 'yi](/azure-arc/servers/overview.md) etkinleştirir. Bu makalede, Azure Arc 'ı kullanmayı tercih ediyorsanız karma VM 'Lerin nasıl ekleneceği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- [Log Analytics çalışma alanı oluşturun ve yapılandırın](vminsights-configure-workspace.md).
- Etkinleştirmiş olduğunuz sanal makine veya sanal makine ölçek kümesinin işletim sisteminin desteklendiğinden emin olmak için [desteklenen işletim sistemleri](vminsights-enable-overview.md#supported-operating-systems) bölümüne bakın. 


## <a name="overview"></a>Genel Bakış
Azure dışındaki sanal makineler, Azure VM 'Leri için kullanılan aynı Log Analytics aracısını ve bağımlılık aracısını gerektirir. Ancak aracıları yüklemek için VM uzantılarını kullanamıyoruz, bu uygulamaları Konuk işletim sistemine el ile yüklemeli veya başka bir yöntemle yüklemiş olmanız gerekir. 

Log Analytics aracısını dağıtma hakkında ayrıntılı bilgi için bkz. [Windows bilgisayarlarını Azure izleyici 'ye bağlama](../platform/agent-windows.md) veya [Linux bilgisayarlarını Azure izleyici 'ye bağlama](../platform/agent-linux.md) . Bu makalede bağımlılık aracısının ayrıntıları verilmiştir. 

## <a name="firewall-requirements"></a>Güvenlik duvarı gereksinimleri
Log Analytics aracısına yönelik güvenlik duvarı gereksinimleri [Log Analytics aracıya genel bakış](..//platform/log-analytics-agent.md#network-requirements)bölümünde verilmiştir. VM'ler için Azure İzleyici Map bağımlılık Aracısı herhangi bir veri iletmez ve güvenlik duvarları veya bağlantı noktalarında değişiklik gerektirmez. Harita verileri her zaman doğrudan veya [Operations Management Suite ağ geçidi](../../azure-monitor/platform/gateway.md) aracılığıyla Log Analytics ARACıSıDıR ve BT güvenlik ilkeleriniz ağdaki bilgisayarların internet 'e bağlanmasına izin vermez.


## <a name="dependency-agent"></a>Bağımlılık aracısı

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir.  

Bağımlılık aracısını şu konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.10.4.10090 | B4E1FF9C1E5CD254AA709AEF9723A81F04EC0763C327567C582CE99C0C5A0BAE  |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.10.4.10090 | A56E310D297CE3B343AE8F4A6F72980F1C3173862D6169F1C713C2CA09660A9F |


## <a name="install-the-dependency-agent-on-windows"></a>Windows 'a bağımlılık Aracısı 'nı yükler

' I çalıştırarak, Windows bilgisayarlarına bağımlılık aracısını el ile yükleyebilirsiniz `InstallDependencyAgent-Windows.exe` . Bu yürütülebilir dosyayı herhangi bir seçenek olmadan çalıştırırsanız, aracı etkileşimli olarak yüklemek için izleyebileceğiniz bir Kurulum Sihirbazı başlatılır. Aracıyı yüklemek veya kaldırmak için konuk işletim sisteminde *yönetici* ayrıcalıklarına sahip olmanız gerekir.

Aşağıdaki tabloda, komut satırından aracının kurulumu tarafından desteklenen parametreler vurgulanmıştır.

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin bir listesini döndürür. |
| Sn | Kullanıcı etkileşimi olmadan sessiz yükleme gerçekleştirir. |

Örneğin, yükleme programını parametresiyle çalıştırmak için `/?` **InstallDependencyAgent-Windows.exe/?** girin.

Windows bağımlılık aracısının dosyaları varsayılan olarak *C:\Program Files\Microsoft Dependency Agent* 'e yüklenir. Kurulum tamamlandıktan sonra bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Günlük dizini *%ProgramFiles%\Microsoft Dependency Fıles\logs*dizinidir.

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

Örneğin, yükleme programını parametresiyle çalıştırmak için `-help` , **InstallDependencyAgent-linux64. bin-Help**girin. Komutunu çalıştırarak Linux bağımlılık aracısını kök olarak yükler `sh InstallDependencyAgent-Linux64.bin` .

Bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Linux aracılarında günlük dizini */var/seçenek/Microsoft/Dependency-Agent/log*olur.

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

2. [Log Analytics ücretsiz fiyatlandırma katmanında](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)misiniz? Ücretsiz plan, beş adede kadar benzersiz bilgisayara izin verir. Önceki beş, artık veri göndermese bile, sonraki tüm bilgisayarlar haritada gösterilmez.

3. Bilgisayar Azure Izleyici günlüklerine günlük ve performans verileri gönderiyor mu? Bilgisayarınız için aşağıdaki sorguyu gerçekleştirin:

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    Bir veya daha fazla sonuç döndürüyor mu? Veriler güncel mi? Bu durumda, Log Analytics aracınız doğru şekilde çalışıyor ve hizmetle iletişim kuruyor. Aksi takdirde, sunucunuzdaki aracıyı denetleyin: [Windows sorun giderme için Log Analytics Aracısı](../platform/agent-windows-troubleshoot.md) veya [Linux sorun giderme için Log Analytics Aracısı](../platform/agent-linux-troubleshoot.md).

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>Bilgisayar haritada görünüyor ancak işleme sahip değil

Sunucunuzu haritada görürseniz, ancak bir işlem veya bağlantı verisi yoksa, bağımlılık aracısının yüklü olduğunu ve çalıştığını, ancak çekirdek sürücüsünün yüklenmediğini belirtir.

C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log dosyasını (Windows) veya /var/opt/microsoft/dependency-agent/log/service.log doyasını (Linux) kontrol edin. Dosyanın son satırları, çekirdeğin neden yüklenmediğini göstermelidir. Örneğin çekirdeğinizi güncelleştirdiyseniz yeni çekirdek için Linux desteği sunulmuyor olabilir.


## <a name="next-steps"></a>Sonraki adımlar

Artık sanal makineleriniz için izleme etkin olduğuna göre, bu bilgiler VM'ler için Azure İzleyici analiz için kullanılabilir.

- Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını görüntüleme](vminsights-maps.md).

- VM performanlarınızın performans sorunlarını ve genel kullanımını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md).
