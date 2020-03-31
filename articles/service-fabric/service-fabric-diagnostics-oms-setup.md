---
title: Azure Monitor günlükleriyle izleme ayarlama
description: Azure Hizmet Kumaşı kümelerinizi izlemek için olayları görselleştirmek ve analiz etmek için Azure Monitor günlüklerini nasıl ayarlayizleyeceğinizi öğrenin.
author: srrengar
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: srrengar
ms.openlocfilehash: cf0fab9942dcbb7ee09e554f2c9ba8738f208009
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609936"
---
# <a name="set-up-azure-monitor-logs-for-a-cluster"></a>Bir küme için Azure Monitor günlüklerini ayarlama

Azure Monitor günlükleri küme düzeyindeki olayları izlemek için tavsiyemizdir. Azure Kaynak Yöneticisi, PowerShell veya Azure Marketi aracılığıyla Günlük Analizi çalışma alanı ayarlayabilirsiniz. Dağıtımınızın güncelleştirilmiş bir Kaynak Yöneticisi şablonunu ileride kullanmak üzere korursanız, Azure Monitor günlükortamınızı ayarlamak için aynı şablonu kullanın. Tanılama etkinleştirilmiş bir küme zaten varsa, Market üzerinden dağıtım daha kolaydır. Dağıtım yaptığınız hesapta abonelik düzeyinde erişiminiz yoksa PowerShell veya Kaynak Yöneticisi şablonunu kullanarak dağıtın.

> [!NOTE]
> Kümenizi izlemek için Azure Monitor günlükleri ayarlamak için, küme düzeyinde veya platform düzeyindeki olayları görüntülemek için tanılama nın etkinleştirilmiş olması gerekir. Windows [kümelerinde tanılamanın nasıl ayarlanınve](service-fabric-diagnostics-event-aggregation-wad.md) linux kümelerinde daha fazla sınaimi için [nasıl tanılama nın ayarlanın](service-fabric-diagnostics-oms-syslog.md)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Azure Marketi'ni kullanarak Günlük Analizi çalışma alanını dağıtma

Bir küme dağıttıktan sonra bir Log Analytics çalışma alanı eklemek istiyorsanız, portaldaki Azure Marketi'ne gidin ve **Service Fabric Analytics'i**arayın. Bu, Service Fabric'e özgü verilere sahip Service Fabric dağıtımları için özel bir çözümdür. Bu işlemde hem çözüm (öngörüleri görüntülemek için pano) hem de çalışma alanı (temel küme verilerinin toplanması) oluşturursunuz.

1. Sol daki gezinme menüsünde **Yeni'yi** seçin. 

2. Servis **Kumaş Analizi**arayın. Görünen kaynağı seçin.

3. **Oluştur'u**seçin.

    ![Pazarda Hizmet Kumaş Analizi](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Service Fabric Analytics oluşturma penceresinde, **OMS Çalışma Alanı** alanı için **bir çalışma alanı seçin'i** seçin ve ardından yeni bir çalışma **alanı oluşturun.** Gerekli girişleri doldurun. Buradaki tek gereksinim, Hizmet Kumaşı kümesi ve çalışma alanı aboneliğinin aynı olmasıdır. Girişleriniz doğrulandığında, çalışma alanınız dağıtmaya başlar. Dağıtım yalnızca birkaç dakika sürer.

5. Tamamlandığında, Service Fabric Analytics oluşturma penceresinin altındaki yeniden **Oluştur'u** seçin. Yeni çalışma alanının **OMS Çalışma Alanı**altında olduğundan emin olun. Bu eylem, oluşturduğunuz çalışma alanına çözüm ekler.

Windows kullanıyorsanız, Azure Monitor günlüklerini küme olaylarınızın depolandığı depolama hesabına bağlamak için aşağıdaki adımları kullanmaya devam edin. 

>[!NOTE]
>Service Fabric Analytics çözümü yalnızca Windows kümeleri için desteklenir. Linux kümeleri [için, Linux kümeleri için Azure Monitor günlüklerinin nasıl ayarlanış edilene](service-fabric-diagnostics-oms-syslog.md)ilişkin makalemize göz atın.  

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Log Analytics çalışma alanını kümenize bağlayın 

1. Çalışma alanının kümenizden gelen tanılama verilerine bağlanması gerekir. Service Fabric Analytics çözümlerini oluşturduğunuz kaynak grubuna gidin. **ServiceFabric\<\> nameOfWorkspace'i** seçin ve genel bakış sayfasına gidin. Buradan çözüm ayarlarını, çalışma alanı ayarlarını değiştirebilir ve Log Analytics çalışma alanına erişebilirsiniz.

2. Soldaki gezinti menüsünde, **Çalışma Alanı Veri Kaynakları**altında, Depolama hesapları **günlüklerini**seçin.

3. Depolama **hesabı günlükleri** sayfasında, kümenizin günlüklerini çalışma alanına eklemek için en üstte **Ekle'yi** seçin.

4. Kümenizde oluşturulan uygun hesabı eklemek için **Depolama hesabını** seçin. Varsayılan adı kullandıysanız, depolama hesabı **sfdg\<resourceGroupName'dir.\>** Ayrıca, uygulama için kullanılan değeri denetleyerek, kümenizi dağıtmak için kullanılan Azure Kaynak Yöneticisi şablonu ile de **onaylayabilirsinizDiagnosticsStorageAccountName**. Ad görünmüyorsa, aşağı kaydırın ve **Daha Fazla Yükle'yi**seçin. Depolama hesabı adını seçin.

5. Veri Türünü belirtin. **Servis Kumaş Etkinlikleri**ayarlayın.

6. Kaynağın otomatik olarak **WADServiceFabric\*EventTable**olarak ayarlandığından emin olun.

7. Çalışma alanınızı kümenizin günlüklerine bağlamak için **Tamam'ı** seçin.

    ![Azure Monitor günlüklerine depolama hesabı günlükleri ekleme](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Hesap artık çalışma alanınızın veri kaynaklarında depolama hesabı günlüklerinizin bir parçası olarak gösterilmektedir.

Artık kümenizin platformuna ve uygulama günlüğü tablosuna doğru şekilde bağlanan Bir Log Analytics çalışma alanına Service Fabric Analytics çözümlerini eklediniz. Çalışma alanına aynı şekilde ek kaynaklar ekleyebilirsiniz.


## <a name="deploy-azure-monitor-logs-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile Azure Monitör günlüklerini dağıtma

Bir kaynak yöneticisi şablonu kullanarak bir küme dağıttığınızda, şablon yeni bir Log Analytics çalışma alanı oluşturur, Çalışma alanına Hizmet Dokusu çözümekler ve uygun depolama tablolarından verileri okuyacak şekilde yapılandırır.

Gereksinimlerinizi karşılamak için [bu örnek şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) kullanabilir ve değiştirebilirsiniz. Bu şablon aşağıdakileri yapar

* 5 düğüm Hizmet Kumaş kümesi oluşturur
* Log Analytics çalışma alanı ve Hizmet Kumaşçözümü oluşturur
* Log Analytics aracısını, çalışma alanına 2 örnek performans sayacı toplamak ve göndermek için yapılandırır
* Hizmet Kumaşını toplamak için WAD'ı yapılandırır ve Azure depolama tablolarına gönderir (WADServiceFabric*EventTable)
* Bu tablolardaki olayları okumak için Log Analytics çalışma alanını yapılandırır


Azure PowerShell modülündeki `New-AzResourceGroupDeployment` API'yi kullanarak şablonu kümenize Kaynak Yöneticisi yükseltmesi olarak dağıtabilirsiniz. Örnek bir komut:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "<resourceGroupName>" -TemplateFile "<templatefile>.json" 
``` 

Azure Kaynak Yöneticisi, bu komutun varolan bir kaynak için bir güncelleştirme olduğunu algılar. Yalnızca varolan dağıtımı yönlendiren şablon ile sağlanan yeni şablon arasındaki değişiklikleri işler.

## <a name="deploy-azure-monitor-logs-with-azure-powershell"></a>Azure PowerShell ile Azure Monitör günlüklerini dağıtma

`New-AzOperationalInsightsWorkspace` Ayrıca, komutu kullanarak günlük analiz kaynağınızı PowerShell üzerinden dağıtabilirsiniz. Bu yöntemi kullanmak için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps)yüklediğinizden emin olun. Yeni bir Log Analytics çalışma alanı oluşturmak ve Service Fabric çözümlerini eklemek için bu komut dosyasını kullanın: 

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

İşleminiz bittiğinde, Azure Monitor günlüklerini uygun depolama hesabına bağlamak için önceki bölümdeki adımları izleyin.

PowerShell'i kullanarak Başka çözümler ekleyebilir veya Log Analytics çalışma alanınızda başka değişiklikler yapabilirsiniz. Daha fazla bilgi için [PowerShell'i kullanarak Azure Monitörünü Yönet günlüklerine](../azure-monitor/platform/powershell-workspace-configuration.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Performans sayaçları toplamak ve konteynerleriniz için docker istatistikleri ve günlükleri toplamak için [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md) düğümlerinize dağıtın
* Azure Monitor [günlüklerinin](../log-analytics/log-analytics-log-searches.md) bir parçası olarak sunulan günlük arama ve sorgulama özelliklerine aşina olun
* [Azure Monitor günlüklerinde özel görünümler oluşturmak için Görünüm Tasarımcısı'nı kullanma](../azure-monitor/platform/view-designer.md)
