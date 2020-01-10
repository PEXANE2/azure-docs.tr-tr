---
title: Azure Izleyici günlükleri ile izlemeyi ayarlama
description: Azure Service Fabric kümelerinizi izlemek üzere olayları görselleştirme ve çözümleme için Azure Izleyici günlüklerini ayarlamayı öğrenin.
author: srrengar
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: srrengar
ms.openlocfilehash: cf0fab9942dcbb7ee09e554f2c9ba8738f208009
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75609936"
---
# <a name="set-up-azure-monitor-logs-for-a-cluster"></a>Küme için Azure Izleyici günlüklerini ayarlama

Azure Izleyici günlükleri, küme düzeyindeki olayları izlemek için önerimiz. Azure Resource Manager, PowerShell veya Azure Market üzerinden Log Analytics çalışma alanı kurabilir. İleride kullanılmak üzere dağıtımınızın güncelleştirilmiş bir Kaynak Yöneticisi şablonunu korudıysanız, Azure Izleyici günlükleri ortamınızı ayarlamak için aynı şablonu kullanın. Tanılamayı ile dağıtılan bir küme zaten varsa, Market aracılığıyla dağıtımı çok kolaydır. Abonelik düzeyinde erişimi için dağıttığınız hesabı yoksa, PowerShell veya Resource Manager şablonu kullanarak dağıtın.

> [!NOTE]
> Kümenizi izlemek üzere Azure Izleyici günlüklerini ayarlamak için, küme düzeyi veya platform düzeyi olaylarını görüntülemek için tanılamayı etkinleştirmeniz gerekir. Başvurmak [tanılama Windows kümelerinde kurma](service-fabric-diagnostics-event-aggregation-wad.md) ve [Linux kümelerinde tanılama kurma](service-fabric-diagnostics-oms-syslog.md) daha fazla bilgi için

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Azure Marketi kullanarak Log Analytics çalışma alanı dağıtma

Bir küme dağıttıktan sonra bir Log Analytics çalışma alanı eklemek istiyorsanız, portalda Azure Market'e dön ve Ara **Service Fabric analizi**. Service Fabric'e özgü veriler içeren özel bir çözüm için Service Fabric dağıtımları budur. Bu işlem çözüm (öngörülerini görüntülemek için Pano) ve çalışma alanı (temel alınan küme veri toplama) oluşturur.

1. Seçin **yeni** sol gezinti menüsünde. 

2. Arama **Service Fabric analizi**. Görüntülenen kaynağı seçin.

3. **Oluştur**’u seçin.

    ![Service Fabric analizi pazarında](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Service Fabric analizi oluşturma penceresinde **bir çalışma alanı seçin** için **OMS çalışma alanı** alan ve ardından **yeni bir çalışma alanı oluşturma**. Gerekli girişleri doldurun. Service Fabric kümesi ve çalışma alanı için abonelik aynı olduğunu burada tek gereksinim olmasıdır. Girdilerinizi doğrulandı, çalışma alanınızı dağıtma başlar. Dağıtım, yalnızca birkaç dakika sürer.

5. İşiniz bittiğinde seçin **Oluştur** Service Fabric analizi oluşturma penceresinin altındaki yeniden. Yeni çalışma alanı altında gösterildiğini doğrulayın **OMS çalışma alanı**. Bu eylem, oluşturduğunuz çalışma alanına çözüme ekler.

Windows kullanıyorsanız, Azure Izleyici günlüklerini küme olaylarınızın depolandığı depolama hesabına bağlamak için aşağıdaki adımlarla devam edin. 

>[!NOTE]
>Service Fabric Analytics çözümü yalnızca Windows kümeleri için desteklenir. Linux kümeleri için, [Linux kümeleri Için Azure izleyici günlüklerini ayarlama](service-fabric-diagnostics-oms-syslog.md)makalesindeki makaleye göz atın.  

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Log Analytics çalışma alanı kümenize bağlanın 

1. Çalışma alanı kümenizden gelen Tanılama verileri bağlanması gerekir. Service Fabric analizi çözümü oluşturduğunuz kaynak grubuna gidin. Seçin **ServiceFabric\<nameOfWorkspace\>**  ve kendi genel bakış sayfasına gidin. Burada, çözüm ayarları, çalışma alanı ayarları değiştirin ve Log Analytics çalışma alanına erişim.

2. Sol gezinti menüsünde altında **çalışma alanı veri kaynakları**seçin **depolama hesabı günlükleri**.

3. Üzerinde **depolama hesabı günlükleri** sayfasında **Ekle** kümenizin günlükleri çalışma alanına eklemek için üst.

4. Seçin **depolama hesabı** kümenizde oluşturulan uygun hesabı eklemek için. Varsayılan ad kullandıysanız, depolama hesabıdır **sfdg\<resourceGroupName\>** . Ayrıca bu, kümeniz için kullanılan değerini denetleyerek dağıtmak için kullanılan Azure Resource Manager şablonu ile doğrulayabilirsiniz **applicationDiagnosticsStorageAccountName**. Adı görünmez, aşağıya kaydırın ve **daha fazla Yükle**. Depolama hesabı adını seçin.

5. Veri türü belirtin. Ayarlayın **Service Fabric olayları**.

6. Kaynağı otomatik olarak ayarlandığından emin olun **WADServiceFabric\*EventTable**.

7. Seçin **Tamam** kümenizin günlükleri için çalışma alanınızı bağlamak için.

    ![Azure Izleyici günlüklerine depolama hesabı günlükleri ekleme](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Depolama hesabınızın parçası çalışma alanınızın veri kaynaklarında oturum gibi hesap artık gösterilir.

Service Fabric analizi çözümü, artık doğru kümenizin platform ve Uygulama günlüğünü tabloya bağlandığından bir Log Analytics çalışma alanında ekledik. Ek kaynaklar aynı şekilde çalışma alanına ekleyebilirsiniz.


## <a name="deploy-azure-monitor-logs-with-azure-resource-manager"></a>Azure Izleyici günlüklerini Azure Resource Manager ile dağıtma

Resource Manager şablonu kullanarak bir küme dağıttığınızda, şablonun yeni bir Log Analytics çalışma alanı oluşturur, Service Fabric çözümü çalışma alanına ekler ve uygun depolama tablolardan verileri okumak için yapılandırır.

Kullanabilir ve değiştirme [Bu örnek şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) gereksinimlerinizi karşılayacak şekilde. Bu şablon şunları yapar

* 5 düğümlü Service Fabric kümesi oluşturur
* Bir Log Analytics çalışma alanı ve Service Fabric çözümü oluşturur
* Toplamak ve çalışma alanı için 2 örnek performans sayaçları göndermek için Log Analytics aracısını yapılandırır
* Service Fabric toplanacak WAD yapılandırır ve bunları Azure depolama tabloları gönderir (WADServiceFabric * EventTable)
* Bu tablodan olayları okumak için Log Analytics çalışma alanı yapılandırır


Azure PowerShell modülündeki `New-AzResourceGroupDeployment` API 'sini kullanarak şablonu kümenize Kaynak Yöneticisi yükseltmesi olarak dağıtabilirsiniz. Bir örnek komut şöyle olabilir:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "<resourceGroupName>" -TemplateFile "<templatefile>.json" 
``` 

Azure Resource Manager, bu komut bir güncelleştirme var olan bir kaynak olduğunu algılar. Yalnızca, mevcut dağıtım sürüş şablonu ve sağlanan yeni şablonu arasındaki değişiklikleri işler.

## <a name="deploy-azure-monitor-logs-with-azure-powershell"></a>Azure Izleyici günlüklerini Azure PowerShell ile dağıtma

Log Analytics kaynağınızı PowerShell aracılığıyla `New-AzOperationalInsightsWorkspace` komutunu kullanarak da dağıtabilirsiniz. Bu yöntemi kullanmak için yüklediğinizden emin olun [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps). Yeni bir Log Analytics çalışma alanı oluşturma ve Service Fabric çözümü eklemek için bu betiği kullanın: 

```powershell

$SubID = "<subscription ID>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<Log Analytics workspace name>"
$solution = "ServiceFabric"

# Sign in to Azure and access the correct subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

İşiniz bittiğinde, Azure Izleyici günlüklerini uygun depolama hesabına bağlamak için yukarıdaki bölümdeki adımları izleyin.

Ayrıca, diğer çözümler ekleyin veya PowerShell kullanarak Log Analytics çalışma alanınıza diğer değişiklikleri yapın. Daha fazla bilgi için bkz. [PowerShell kullanarak Azure izleyici günlüklerini yönetme](../azure-monitor/platform/powershell-workspace-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Log Analytics aracısını dağıtmayı](service-fabric-diagnostics-oms-agent.md) üzerine düğümlerinizi performans sayaçları toplamak ve docker istatistikleri ve kapsayıcılarınızı için günlükleri toplamak için
* Azure Izleyici günlüklerinin bir parçası olarak sunulan [günlük araması ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleriyle familiarized alın
* [Azure Izleyici günlüklerinde özel görünümler oluşturmak için görünüm tasarımcısını kullanma](../azure-monitor/platform/view-designer.md)
