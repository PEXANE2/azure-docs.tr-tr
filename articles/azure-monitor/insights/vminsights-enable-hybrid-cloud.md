---
title: Karma ortam için Azure Izleyicisini (Önizleme) etkinleştirme | Microsoft Docs
description: Bu makalede, bir veya daha fazla sanal makine içeren karma bulut ortamı için VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: b06fe477f551977b1357f3b1b185cb340a948052
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69905491"
---
# <a name="enable-azure-monitor-for-vms-preview-for-a-hybrid-environment"></a>Karma ortam için VM'ler için Azure İzleyici (Önizleme) etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, veri merkezinizde veya diğer bulut ortamınızda barındırılan sanal makineler veya fiziksel bilgisayarlar için VM'ler için Azure İzleyici (Önizleme) nasıl etkinleştirileceği açıklanır. Bu işlemin sonunda, ortamınızdaki sanal makinelerinizi izlemeye başarıyla başlacaksınız ve performans veya kullanılabilirlik sorunlarıyla karşılaşmadığını öğrenirsiniz. 

Başlamadan önce, [önkoşulları](vminsights-enable-overview.md) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun. Dağıtım yöntemleri ve gereksinimleri gözden [Log Analytics Linux ve Windows Aracısı](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

>[!NOTE]
>Azure İzleyici Vm'leri harita bağımlılık aracısı için hiçbir veri aktarır değil ve güvenlik duvarları veya bağlantı noktaları için herhangi bir değişiklik yapılması gerekmez. Harita verileri her zaman doğrudan veya [Operations Management Suite ağ geçidi](../../azure-monitor/platform/gateway.md) aracılığıyla Log Analytics ARACıSıDıR ve BT güvenlik ilkeleriniz ağdaki bilgisayarların internet 'e bağlanmasına izin vermez.

Bu görevi tamamlamaya yönelik adımlar şu şekilde özetlenmiştir:

1. Windows veya Linux için Log Analytics Aracısı 'nı yükler. Aracıyı yüklemeden önce, sistem önkoşullarını ve dağıtım yöntemlerini anlamak için [Log Analytics aracısına genel bakış](../platform/log-analytics-agent.md) makalesini gözden geçirin.

2. Vm'leri harita bağımlılık aracısı için Azure İzleyicisi'ni yükleyip [Windows](https://aka.ms/dependencyagentwindows) veya [Linux](https://aka.ms/dependencyagentlinux).

3. Performans sayaçlarını toplamayı etkinleştirin.

4. VM'ler için Azure İzleyici dağıtın.

>[!NOTE]
>Bağımlılık aracısını dağıtmaya yönelik bu makalede açıklanan bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir.  

## <a name="install-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık aracısını yükleme

Bağımlılık aracısını el ile Windows bilgisayarlarda çalıştırarak yükleyebilirsiniz `InstallDependencyAgent-Windows.exe`. Bu yürütülebilir dosya hiçbir seçenek olmadan çalıştırırsanız, aracıyı etkileşimli olarak yüklemek için izlemeniz gereken bir Kurulum Sihirbazı başlar.

>[!NOTE]
>*Yönetici* ayrıcalıkları yükleyin veya aracıyı kaldırmak için gereklidir.

Aşağıdaki tabloda kurulum tarafından desteklenen komut satırı aracı için parametreleri vurgular.

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini döndürür. |
| /S | Kullanıcı etkileşimi olmadan Sessiz bir yükleme gerçekleştirir. |

Örneğin, yükleme programını `/?` parametresiyle çalıştırmak için, **InstallDependencyAgent-Windows. exe/?** girin.

Windows bağımlılık aracısını için dosyalar yüklenir *C:\Program Files\Microsoft bağımlılık aracısını* varsayılan olarak. Kurulum tamamlandıktan sonra bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Günlük dizini *%Programfiles%\Microsoft bağımlılık Agent\logs*.

## <a name="install-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık aracısını yükleme

Bağımlılık Aracısı'nı Linux sunuculardan yüklü *InstallDependencyAgent Linux64.bin*, kendi kendine ayıklanan bir ikili içeren bir kabuk betiği. Kullanarak dosyayı çalıştırabilirsiniz `sh` veya yürütme izinleri dosya için.

>[!NOTE]
> Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.
>

| Parametre | Açıklama |
|:--|:--|
| -Yardım | Komut satırı seçeneklerinin listesini alır. |
| -s | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| --denetleyin | İzinler ve işletim sistemini denetleyin, ancak aracıyı yüklemeyin. |

Örneğin, yükleme programını `-help` parametresiyle çalıştırmak için, **InstallDependencyAgent-linux64. bin-Help**girin.

Komutunu `sh InstallDependencyAgent-Linux64.bin`çalıştırarak Linux bağımlılık aracısını kök olarak yükler.

Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları, günlük dizindir */var/opt/microsoft/dependency-agent/log*.

Bağımlılık Aracısı'nı dosyaları aşağıdaki dizinlerde yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Yükleme betiği örnekleri

Bağımlılık aracısını birçok sunucuda tek seferde kolayca dağıtmak için aşağıdaki betiği örneği indirip bağımlılık aracısını Windows veya Linux'ta sağlanır.

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

Desired State Configuration ' nı (DSC) kullanarak bağımlılık aracısını dağıtmak için aşağıdaki örnek kod ile xPSDesiredStateConfiguration modülü kullanabilirsiniz:

```powershell
configuration ServiceMap {

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

## <a name="enable-performance-counters"></a>Performans sayaçları sağlar

Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki şekilde yapabilirsiniz:
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* [Azure PowerShell galerisinde](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) bulunan bir PowerShell betiğini indirip çalıştırarak

## <a name="deploy-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici dağıtma

Bu yöntem, Log Analytics çalışma alanınızda çözüm bileşenlerini etkinleştirmek için yapılandırmasını belirten bir JSON şablonu içerir.

Bir şablon kullanarak kaynakların nasıl dağıtılacağını bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLı 'yi kullanmak için, önce CLı 'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Azure CLı 'yı yüklemek veya yükseltmek için bkz. [Azure CLI 'Yi yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-and-execute-a-template"></a>Oluşturma ve bir şablonu yürütme

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
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    },
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },
                        "plan": {
                            "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Bu dosyayı farklı Kaydet *installsolutionsforvminsights.json* yerel bir klasöre.

1. *Çalışmaadı*, *Resourcegroupname*ve *WorkspaceLocation*değerlerini yakalayın. Çalışmaadı değeri , Log Analytics çalışma alanınızın adıdır. Değeri *WorkspaceLocation* çalışma alanı içinde tanımlanan bölgedir.

1. Aşağıdaki PowerShell komutunu kullanarak bu şablonu dağıtmaya hazırsınız:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. İşiniz bittiğinde, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

    ```powershell
    provisioningState       : Succeeded
    ```
   İzleme etkinleştirdikten sonra sistem durumunu ve karma bilgisayar için ölçümleri görmeden önce yaklaşık 10 dakika sürebilir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="vm-doesnt-appear-on-the-map"></a>VM haritada görünmüyor

Bağımlılık aracısı yüklemeniz başarılı oldu, ancak bilgisayarınızı haritada görmüyorsanız, bu adımları izleyerek sorunu tanılayın.

1. Bağımlılık Aracısı'nı başarıyla yüklü mü? Bu hizmet yüklü olup olmadığını denetliyor ve çalıştırarak doğrulayabilirsiniz.

    **Windows**: "Microsoft bağımlılık Aracısı" adlı hizmeti arayın. 

    **Linux**: "Microsoft-Dependency-Agent" çalışan işlemini arayın.

2. [Log Analytics ücretsiz fiyatlandırma katmanında](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)misiniz? Ücretsiz plan, beş adede kadar benzersiz bilgisayara izin verir. Önceki beş, artık veri göndermese bile, sonraki tüm bilgisayarlar haritada gösterilmez.

3. Bilgisayar Azure Izleyici günlüklerine günlük ve performans verileri gönderiyor mu? Bilgisayarınız için aşağıdaki sorguyu gerçekleştirin: 

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    Bir veya daha fazla sonuç döndürüyor mu? Verilerin güncel mi? Bu durumda, Log Analytics aracınız doğru şekilde çalışıyor ve hizmetle iletişim kuruyor. Aksi takdirde, sunucunuzdaki aracıyı kontrol edin: [Linux sorun giderme Için](../platform/agent-linux-troubleshoot.md) [Windows sorun giderme](../platform/agent-windows-troubleshoot.md) veya Log Analytics Aracısı Log Analytics Aracısı.

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>Bilgisayar haritada görünüyor ancak işleme sahip değil

Sunucunuzu haritada görürseniz, ancak bir işlem veya bağlantı verisi yoksa, bağımlılık aracısının yüklü olduğunu ve çalıştığını, ancak çekirdek sürücüsünün yüklenmediğini belirtir. 

C:\Program Files\Microsoft bağımlılık Agent\logs\wrapper.log dosyası (Windows) veya /var/opt/microsoft/dependency-agent/log/service.log dosyası (Linux) kontrol edin. Dosyanın son satırları çekirdek neden yüklenmedi belirtmeniz gerekir. Örneğin, çekirdek güncelleştirilmiş çekirdek Linux üzerinde desteklenmeyebilir.


## <a name="next-steps"></a>Sonraki adımlar

Artık sanal makineleriniz için izleme etkin olduğuna göre, bu bilgiler VM'ler için Azure İzleyici analiz için kullanılabilir.
 
- Sistem durumu özelliğini nasıl kullanacağınızı öğrenmek için bkz. [VM'ler için Azure izleyici durumunu görüntüleme](vminsights-health.md).
- Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).
- VM performanlarınızın performans sorunlarını ve genel kullanımını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md).
- Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).