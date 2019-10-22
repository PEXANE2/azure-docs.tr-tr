---
title: Web uygulaması sorunlarını bulmak için Azure Izleyici 'de uygulama değişikliği analizini kullanma | Microsoft Docs
description: Azure App Service üzerindeki canlı sitelerde uygulama sorunlarını gidermek için Azure Izleyici 'de uygulama değişikliği analizini kullanın.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/07/2019
ms.openlocfilehash: 3805d7b39c25bcb213a1d4f110161dcd00eb3630
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72678245"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Azure Izleyici 'de uygulama değişikliği analizini (Önizleme) kullanma

Canlı bir site sorunu veya kesintisi oluştuğunda, kök nedenin hızla belirlenmesi kritik öneme sahiptir. Standart izleme çözümleri sizi bir sorunla ilgili olarak uyarabilir. Bunlar, hangi bileşenin başarısız olduğunu bile gösterebilir. Ancak bu uyarı hatanın nedenini her zaman açıklamayacaktır. Sitenizde beş dakika önce çalıştık ve artık bozulmuş. Son beş dakika içinde ne değişti? Bu, uygulama değişikliği analizinin Azure Izleyici 'de yanıtlamak üzere tasarlandığına yönelik sorudır.

[Azure Kaynak Grafiği](https://docs.microsoft.com/azure/governance/resource-graph/overview)'nin gücüyle çalışırken, değişiklik Analizi Observability artırmak ve MTTR 'i azaltmak için Azure uygulamanızın değişiklikleriyle ilgili öngörüler sağlar (ortalama onarım süresi).

> [!IMPORTANT]
> Değişiklik Analizi Şu anda önizleme aşamasındadır. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sunulmaktadır. Bu sürüm, üretim iş yükleri için önerilmez. Bazı özellikler desteklenmeyebilir veya kısıtlı özelliklere sahip olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Genel Bakış

Değişiklik analizi, altyapı katmanından uygulama dağıtımına kadar olan çeşitli değişiklik türlerini algılar. Bu, abonelikteki kaynak değişikliklerini denetleyen abonelik düzeyinde bir Azure Kaynak sağlayıcısıdır. Değişiklik analizi, kullanıcıların sorunlara neden olabilecek değişiklikler olduğunu anlamalarına yardımcı olmak için çeşitli tanılama araçları için veri sağlar.

Aşağıdaki diyagramda değişiklik analizinin mimarisi gösterilmektedir:

![Değişiklik analizinin değişiklik verilerini nasıl aldığı ve istemci araçlarına sağladığı mimari diyagramı](./media/change-analysis/overview.png)

Şu anda değişiklik Analizi App Service Web uygulamasındaki **sorunları Tanıla ve çöz** deneyimiyle tümleşiktir. Değişiklik algılamayı etkinleştirmek ve Web uygulamasındaki değişiklikleri görüntülemek için, bu makalenin ilerleyen kısımlarında *Web Apps özelliği Için değişiklik Analizi* bölümüne bakın.

### <a name="azure-resource-manager-deployment-changes"></a>Dağıtım değişikliklerini Azure Resource Manager

[Azure Kaynak Grafiği](https://docs.microsoft.com/azure/governance/resource-graph/overview)'ni kullanarak, değişiklik analizi, uygulamanızı barındıran Azure kaynaklarının zaman içinde nasıl değiştiğini gösteren bir geçmiş kaydı sağlar. Değişiklik analizi, IP yapılandırma kuralları, Yönetilen kimlikler ve SSL ayarlarındaki değişiklikleri algılayabilir. Bu nedenle, bir Web uygulamasına bir etiket eklenirse değişiklik Analizi değişikliği yansıtır. Bu bilgiler, Azure aboneliğinde `Microsoft.ChangeAnalysis` kaynak sağlayıcısı etkinleştirildiği sürece kullanılabilir.

### <a name="changes-in-web-app-deployment-and-configuration"></a>Web uygulaması dağıtımı ve yapılandırmasındaki değişiklikler

Değişiklik analizi, bir uygulamanın dağıtım ve yapılandırma durumunu her 4 saatte bir yakalar. Uygulama ortamı değişkenlerindeki değişiklikleri algılayabilir. Araç farkları hesaplar ve nelerin değiştiğini gösterir. Kaynak Yöneticisi değişikliklerden farklı olarak, kod dağıtımı değişiklik bilgileri araç içinde hemen kullanılamayabilir. Değişiklik analizinde yapılan en son değişiklikleri görüntülemek için **değişiklikleri şimdi Tara**' yı seçin.

!["Değişiklikleri şimdi Tara" düğmesinin ekran görüntüsü](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Bağımlılık değişiklikleri

Kaynak bağımlılıklarındaki değişiklikler de bir Web uygulamasında sorunlara neden olabilir. Örneğin, bir Web uygulaması Redsıs önbelleğine çağırırsa, Redsıs Cache SKU 'SU Web uygulaması performansını etkileyebilir. Bağımlılıklarda yapılan değişiklikleri algılamak için, değişiklik Analizi Web uygulamasının DNS kaydını denetler. Bu şekilde, tüm uygulama bileşenlerinde sorunlara neden olabilecek değişiklikler tanımlanmaktadır.
Şu anda aşağıdaki bağımlılıklar desteklenir:
- Web Apps
- Azure Depolama
- Azure SQL


## <a name="change-analysis-for-the-web-apps-feature"></a>Web Apps özelliği için değişiklik Analizi

Azure Izleyici 'de, değişiklik Analizi Şu anda self servis **Tanılama ve çözme sorunları** deneyiminde yerleşik olarak bulunur. App Service uygulamanızın **genel bakış** sayfasından bu deneyimle erişin.

!["Genel bakış" düğmesinin ve "sorunları tanılama ve çözme" düğmesinin ekran görüntüsü](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Sorunları tanılama ve çözme aracında değişiklik analizini etkinleştirme

1. **Kullanılabilirlik ve performans ' ı**seçin.

    !["Kullanılabilirlik ve performans" sorun giderme seçeneklerinin ekran görüntüsü](./media/change-analysis/availability-and-performance.png)

1. **Uygulama değişikliklerini**seçin. Özelliği **uygulama Kilitlenmelerinde**da kullanılabilir değildir.

   !["Uygulama kilitlenmeler" düğmesinin ekran görüntüsü](./media/change-analysis/application-changes.png)

1. Değişiklik analizini etkinleştirmek için **Şimdi etkinleştir**' i seçin.

   !["Uygulama kilitlenmeler" seçeneklerinin ekran görüntüsü](./media/change-analysis/enable-changeanalysis.png)

1. **Değişiklik analizini** açın ve **Kaydet**' i seçin.

    !["Değişiklik analizini etkinleştir" Kullanıcı arabiriminin ekran görüntüsü](./media/change-analysis/change-analysis-on.png)


1. Değişiklik analizine erişmek için,**kullanılabilirlik ve performans**  > **uygulama kilitlenmelerine** >  **Tanılama ve çözme sorunlarını çözün** . Zaman içinde değişiklik türlerini özetleyen bir grafik görürsünüz ve bu değişiklikler hakkındaki ayrıntılarla birlikte:

     ![Değişiklik fark görünümünün ekran görüntüsü](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Ölçek üzerinde değişiklik analizini etkinleştir

Aboneliğiniz çok sayıda Web uygulaması içeriyorsa, hizmeti Web uygulaması düzeyinde etkinleştirmek verimsiz olur. Aboneliğinizdeki tüm Web uygulamalarını etkinleştirmek için aşağıdaki betiği çalıştırın.

Önkoşulların önkoşulları:
* PowerShell az Module. [Azure PowerShell modülünü yüklerken](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.6.0) yönergeleri izleyin

Şu betiği çalıştırın:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"


# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```



## <a name="next-steps"></a>Sonraki adımlar

- [Azure Uygulama Hizmetleri uygulamaları](azure-web-apps.md)için Application Insights etkinleştirin.
- [Azure VM ve Azure sanal makine ölçek kümesi için Application Insights ETKINLEŞTIRME IIS tarafından barındırılan uygulamalar](azure-vm-vmss-apps.md).
- Power Change analizine yardımcı olan [Azure Kaynak Grafiği](https://docs.microsoft.com/azure/governance/resource-graph/overview)hakkında daha fazla bilgi edinin.
