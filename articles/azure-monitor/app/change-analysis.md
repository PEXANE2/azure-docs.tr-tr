---
title: Web uygulaması sorunlarını bulmak için Azure Izleyici 'de uygulama değişikliği analizini kullanma | Microsoft Docs
description: Azure App Service üzerindeki canlı sitelerde uygulama sorunlarını gidermek için Azure Izleyici 'de uygulama değişikliği analizini kullanın.
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: a08fc7d7822b4aeddafb588fdb73e86559ce2b12
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68849162"
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
- Azure Storage
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


1. Değişiklik analizine erişmek için, **sorunları Tanıla ve çöz sorun** > **kullanılabilirliği ve performans** > **uygulaması kilitlenmeleri**' ni seçin. Zaman içinde değişiklik türlerini özetleyen bir grafik görürsünüz ve bu değişiklikler hakkındaki ayrıntılarla birlikte:

     ![Değişiklik fark görünümünün ekran görüntüsü](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Ölçek üzerinde değişiklik analizini etkinleştir

Aboneliğiniz çok sayıda Web uygulaması içeriyorsa, hizmeti Web uygulaması düzeyinde etkinleştirmek verimsiz olur. Bu durumda, bu alternatif yönergeleri izleyin.

### <a name="register-the-change-analysis-resource-provider-for-your-subscription"></a>Aboneliğiniz için değişiklik Analizi kaynak sağlayıcısını kaydedin

1. Değişiklik Analizi Özellik bayrağını (Önizleme) kaydedin. Özellik bayrağı önizlemede olduğundan, aboneliğiniz için görünür olması için kaydetmeniz gerekir:

   1. [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)'i açın.

      ![Değişiklik Cloud Shell ekran görüntüsü](./media/change-analysis/cloud-shell.png)

   1. Kabuk türünü **PowerShell**olarak değiştirin.

      ![Değişiklik Cloud Shell ekran görüntüsü](./media/change-analysis/choose-powershell.png)

   1. Aşağıdaki PowerShell komutunu çalıştırın:

        ``` PowerShell
        Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to enable
        Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
        Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag
        ```

1. Abonelik için değişiklik Analizi kaynak sağlayıcısını kaydedin.

   - **Abonelikler**' e gidin ve değişiklik hizmetinde etkinleştirmek istediğiniz aboneliği seçin. Sonra kaynak sağlayıcıları ' nı seçin:

        ![Değişiklik Analizi kaynak sağlayıcısının nasıl kaydedileceği gösteren ekran görüntüsü](./media/change-analysis/register-rp.png)

       - **Microsoft. ChangeAnalysis**öğesini seçin. Ardından sayfanın en üstünde **Kaydet**' i seçin.

       - Kaynak sağlayıcı etkinleştirildikten sonra, dağıtım düzeyindeki değişiklikleri algılamak için Web uygulamasında gizli bir etiket ayarlayabilirsiniz. Gizli bir etiket ayarlamak için, **değişiklik Analizi bilgilerini getirmek için**aşağıdaki yönergeleri izleyin.

   - Alternatif olarak, kaynak sağlayıcısını kaydetmek için bir PowerShell betiği de kullanabilirsiniz:

        ```PowerShell
        Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
        ```

        Bir Web uygulamasında gizli bir etiket ayarlamak üzere PowerShell 'i kullanmak için şu komutu çalıştırın:

        ```powershell
        $webapp=Get-AzWebApp -Name <name_of_your_webapp>
        $tags = $webapp.Tags
        $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
        Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
        ```

     > [!NOTE]
     > Gizli etiketi ekledikten sonra, değişiklikleri görmeye başlamadan önce 4 saate kadar beklemeniz gerekebilir. Değişiklik Analizi Web uygulamanızı yalnızca 4 saatte bir taradığından sonuçlar gecikiyor. 4 saatlik zamanlama, taramanın performans etkisini sınırlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Uygulama Hizmetleri uygulamaları](azure-web-apps.md)için Application Insights etkinleştirin.
- [Azure VM ve Azure sanal makine ölçek kümesi için Application Insights ETKINLEŞTIRME IIS tarafından barındırılan uygulamalar](azure-vm-vmss-apps.md).
- Power Change analizine yardımcı olan [Azure Kaynak Grafiği](https://docs.microsoft.com/azure/governance/resource-graph/overview)hakkında daha fazla bilgi edinin.
