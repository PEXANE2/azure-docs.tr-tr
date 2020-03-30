---
title: Karma bir ortam için Azure Monitörünü etkinleştirin | Microsoft Dokümanlar
description: Bu makalede, bir veya daha fazla sanal makine içeren karma bir bulut ortamı için VM'ler için Azure Monitörünü nasıl etkinleştirdiğiniz açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/15/2019
ms.openlocfilehash: 734f61c2e96002516e9e15af88d2c6b0fce00e98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480751"
---
# <a name="enable-azure-monitor-for-vms-for-a-hybrid-environment"></a>Karma bir ortam için VM'ler için Azure Monitörünü etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, veri merkezinizde veya diğer bulut ortamınızda barındırılan sanal makineler veya fiziksel bilgisayarlar için Sanal Bilgisayarlar için Azure Monitor'un nasıl etkinleştirilen olduğu açıklanmaktadır. Bu işlemin sonunda, sanal makinelerinizi ortamınızda başarıyla izlemeye başlamış ve herhangi bir performans veya kullanılabilirlik sorunu yaşayıp yaşamadıklarını öğrenebilirsiniz.

Başlamadan [önce, ön koşulları](vminsights-enable-overview.md) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığını doğruladığını unutmayın. Log Analytics Linux ve Windows aracısı için gereksinimleri ve dağıtım yöntemlerini gözden [geçirin.](../../log-analytics/log-analytics-agent-overview.md)

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

>[!NOTE]
>VM'ler Için Azure Monitörü Harita Bağımlılık aracısı herhangi bir veriyi kendisi iletmez ve güvenlik duvarlarında veya bağlantı noktalarında değişiklik gerektirmez. Harita verileri, BT güvenlik ilkeleriniz ağdaki bilgisayarların Internet'e bağlanmasına izin vermiyorsa, Log Analytics aracısı tarafından her zaman doğrudan veya [Operasyon Yönetimi Paketi ağ geçidi](../../azure-monitor/platform/gateway.md) aracılığıyla Azure Monitor hizmetine iletilir.

Bu görevi tamamlamak için adımlar aşağıdaki gibi özetlenir:

1. Windows veya Linux için Log Analytics aracısı yükleyin. Aracıyı yüklemeden önce, sistem ön koşulları ve dağıtım yöntemlerini anlamak için [Log Analytics aracısına genel bakış](../platform/log-analytics-agent.md) makalesini inceleyin.

2. [Windows](https://aka.ms/dependencyagentwindows) veya [Linux](https://aka.ms/dependencyagentlinux)için VM Harita Bağımlılığı aracısını indirin ve yükleyin.

3. Performans sayaçlarının toplanmasını etkinleştirin.

4. VM'ler için Azure Monitörünü dağıtın.

>[!NOTE]
>Bağımlılık aracısını dağıtmak için bu makalede açıklanan [bilgiler, Hizmet Haritası çözümü](service-map.md)için de geçerlidir.  

## <a name="install-the-dependency-agent-on-windows"></a>Bağımlılık aracısını Windows'a yükleme

Bağımlılık aracısını Windows bilgisayarlarına çalıştırarak el `InstallDependencyAgent-Windows.exe`ile yükleyebilirsiniz. Bu yürütülebilir dosyayı herhangi bir seçenek olmadan çalıştırın, aracıyı etkileşimli olarak yüklemek için izleyebileceğiniz bir kurulum sihirbazı başlatır.

>[!NOTE]
>*Aracıyı* yüklemek veya kaldırmak için yönetici ayrıcalıkları gereklidir.

Aşağıdaki tablo, komut satırından aracı için kurulum tarafından desteklenen parametreleri vurgular.

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini verir. |
| /s | Kullanıcı etkileşimi olmayan sessiz bir yükleme gerçekleştirir. |

Örneğin, yükleme programını `/?` parametre ile çalıştırmak için **InstallDependencyAgent-Windows.exe /?** girin.

Windows Bağımlılık aracısı için dosyalar varsayılan olarak *C:\Program Files\Microsoft Dependency Agent* yüklü. Bağımlılık aracısı kurulum tamamlandıktan sonra başlatılamazsa, ayrıntılı hata bilgileri için günlükleri denetleyin. Günlük dizini *%Programfiles%\Microsoft Bağımlılık Aracısı\günlükleridir.*

## <a name="install-the-dependency-agent-on-linux"></a>Bağımlılık aracısını Linux'a yükleyin

Bağımlılık aracısı Linux sunucularında *InstallDependencyAgent-Linux64.bin,* kendi kendine ayıklayan bir ikili ile bir kabuk komut dosyası yüklenir. Dosyayı kullanarak `sh` çalıştırabilir veya dosyanın kendisine yürütme izinleri ekleyebilirsiniz.

>[!NOTE]
> Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.
>

| Parametre | Açıklama |
|:--|:--|
| -yardım | Komut satırı seçeneklerinin listesini alır. |
| -s | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| --kontrol et | İzinleri ve işletim sistemini denetleyin, ancak aracıyı yüklemeyin. |

Örneğin, `-help` yükleme programını parametre ile çalıştırmak için **InstallDependencyAgent-Linux64.bin -help**girin.

Komutu `sh InstallDependencyAgent-Linux64.bin`çalıştırarak Linux Bağımlılık aracısını kök olarak yükleyin.

Bağımlılık aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları üzerinde, günlük dizini */var/opt/microsoft/dependency-agent/log'* dur.

Bağımlılık aracısı için dosyalar aşağıdaki dizinlere yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Yükleme betiği örnekleri

Bağımlılık aracısını aynı anda birçok sunucuya kolayca dağıtmak için, Bağımlılık aracısını Windows veya Linux'a indirmek ve yüklemek için aşağıdaki komut dosyası örneği sağlanır.

### <a name="powershell-script-for-windows"></a>Windows için PowerShell betiği

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Linux için kabuk betiği

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>İstenen Durum Yapılandırması

İstenilen Durum Yapılandırması 'nı (DSC) kullanarak Bağımlılık aracısını dağıtmak için aşağıdaki örnek kodu içeren xPSDesiredStateConfiguration modüllerini kullanabilirsiniz:

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

## <a name="enable-performance-counters"></a>Performans sayaçlarını etkinleştirme

Çözümün başvurulan Log Analytics çalışma alanı, çözümün gerektirdiği performans sayaçlarını toplamak için zaten yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki şekilde yapabilirsiniz:
* [Log Analytics'teki Windows ve Linux performans veri kaynaklarında](../../azure-monitor/platform/data-sources-performance-counters.md) açıklandığı gibi el ile
* [Azure PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) kullanılabilen bir PowerShell komut dosyasını indirip çalıştırarak

## <a name="deploy-azure-monitor-for-vms"></a>VM'ler için Azure Monitörünü dağıtma

Bu yöntem, Log Analytics çalışma alanınızdaki çözüm bileşenlerini etkinleştirmek için yapılandırmayı belirten bir JSON şablonu içerir.

Bir şablon kullanarak kaynakları nasıl dağıtacağını bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmak için öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.27 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemek veya yükseltmek için [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

### <a name="create-and-execute-a-template"></a>Şablon oluşturma ve yürütme

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'VMInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Bu *dosyayı installsolutionsforvminsights.json* olarak yerel bir klasöre kaydedin.

1. *WorkspaceName,* *ResourceGroupName*ve *WorkspaceLocation*değerlerini yakalayın. *WorkspaceName'nin* değeri, Log Analytics çalışma alanınızın adıdır. *WorkspaceLocation* için değer, çalışma alanının tanımlandığı bölgedir.

1. Aşağıdaki PowerShell komutunu kullanarak bu şablonu dağıtmaya hazırsınız:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Bittiğinde, aşağıdakilere benzer bir ileti görüntülenir ve sonucu içerir:

    ```powershell
    provisioningState       : Succeeded
    ```
   İzlemeyi etkinleştirdikten sonra, karma bilgisayarın sistem durumu ve ölçümlerini görüntüleyebiliyor sanız, yaklaşık 10 dakika sürebilir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="vm-doesnt-appear-on-the-map"></a>VM haritada görünmüyor

Bağımlılık aracısı yüklemeniz başarılı olduysa, ancak bilgisayarınızı haritada göremiyorsanız, aşağıdaki adımları izleyerek sorunu tanılayın.

1. Bağımlılık aracısı başarıyla yüklendi mi? Hizmetin yüklü olup olmadığını ve çalıştırılıp çalışmadığınızı denetleyerek bunu doğrulayabilirsiniz.

    **Windows**: "Microsoft Bağımlılık aracısı" adlı hizmeti arayın.

    **Linux**: "microsoft-dependency-agent" adlı çalışma işlemini arayın.

2. [Log Analytics'in Ücretsiz fiyatlandırma katmanında](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)mısın? Ücretsiz plan en fazla beş benzersiz bilgisayara izin verir. Önceki beş bilgisayar artık veri göndermese bile, sonraki bilgisayarlar haritada gösterilmez.

3. Bilgisayar, Azure Monitor Günlükleri'ne günlük ve perf veri gönderiyor mu? Bilgisayarınız için aşağıdaki sorguyu gerçekleştirin:

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    Bir veya daha fazla sonuç döndürüldü mi? Veriler yeni mi? Bu nedenle, Log Analytics temsilciniz doğru çalışıyor ve hizmetle iletişim kuruyor. Değilse, sunucunuzdaki aracıyı kontrol edin: [Windows sorun giderme için Log Analytics aracısı](../platform/agent-windows-troubleshoot.md) veya [Linux sorun giderme için Log Analytics aracısı.](../platform/agent-linux-troubleshoot.md)

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>Bilgisayar haritada görünür, ancak işlem yoktur

Sunucunuzu haritada görürseniz, ancak üzerinde işlem veya bağlantı verisi yoksa, bu Bağımlılık aracısının yüklü ve çalıştığını gösterir, ancak çekirdek sürücüsü yüklenmedi.

C:\Program Files\Microsoft Bağımlılık Aracısı\logs\wrapper.log dosyası (Windows) veya /var/opt/microsoft/dependency-agent/log/service.log.log dosyası (Linux) kontrol edin. Dosyanın son satırları çekirdeğin neden yüklenmediğini belirtmelidir. Örneğin, çekirdeğinizi güncellediyseniz çekirdek Linux'ta desteklenmeyebilir.


## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için izleme etkinleştirildiğinden, bu bilgiler Sanal Makineler için Azure Monitor ile analiz edilebilir.

- Keşfedilen uygulama bağımlılıklarını görüntülemek [için, VM'ler Haritası için Azure Monitörünü Görüntüle'ye](vminsights-maps.md)bakın.

- VM'nizin performansıyla birlikte darboğazları ve genel kullanımı belirlemek için Azure [VM performansını görüntüleyin.](vminsights-performance.md)
