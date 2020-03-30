---
title: Azure Monitör günlükleri ile Performans İzleme
description: Azure Service Fabric kümeleriniz için kapsayıcıları ve performans sayaçlarını izlemek için Log Analytics Aracısını nasıl ayarlayizleyeceğinizi öğrenin.
author: srrengar
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: c3c1bf511f3313e7408d6ce90b73de60bd1309f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366754"
---
# <a name="performance-monitoring-with-azure-monitor-logs"></a>Azure Monitör günlükleri ile Performans İzleme

Bu makalede, Log Analytics aracısını sanal makine ölçeği kümesine uzantı olarak eklemek ve mevcut Azure Log Analytics çalışma alanınıza bağlamak için gereken adımları kapsar. Bu, kapsayıcılar, uygulamalar ve performans izleme hakkında tanılama verilerinin toplanmasını sağlar. Azure Kaynak Yöneticisi, sanal makine ölçeği kümesi kaynağına uzantı olarak ekleyerek, kümeyi ölçeklendirirken bile her düğüme yüklenmesini sağlar.

> [!NOTE]
> Bu makalede, zaten ayarlanmış bir Azure Log Analytics çalışma alanınız olduğu varsayar. Bunu yapmazsanız, Azure [Monitör günlüklerini ayarlama](service-fabric-diagnostics-oms-setup.md)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="add-the-agent-extension-via-azure-cli"></a>Azure CLI ile aracı uzantısı ekleme

Log Analytics aracısını kümenize eklemenin en iyi yolu, Azure CLI ile kullanılabilen sanal makine ölçeği API'leri üzerinden elde edilir. Azure CLI henüz ayarlamadıysanız, Azure portalına gidin ve bir [Bulut Kabuğu](../cloud-shell/overview.md) örneği açın veya [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli)

1. Bulut Shell'iniz istendikten sonra kaynağınızla aynı abonelikte çalıştığınızdan emin olun. Bunu kontrol `az account show` edin ve "ad" değerinin kümenizin aboneliğiyle eşleştiğinden emin olun.

2. Portal'da, Log Analytics çalışma alanınızın bulunduğu kaynak grubuna gidin. Günlük analizi kaynağına tıklayın (kaynağın türü Log Analytics çalışma alanı olacaktır). Kaynağa genel bakış sayfasına gittikten sonra, sol menüdeki Ayarlar bölümünün altındaki **Gelişmiş Ayarlar'a** tıklayın.

    ![Günlük analiz özellikleri sayfası](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)

3. Bir Windows kümesini ayakta duruyorsanız **Windows Sunucuları'nı** ve bir Linux kümesi oluşturuyorsanız Linux **Sunucularını** tıklatın. Bu sayfa size `workspace ID` ve `workspace key` (portalda Birincil Anahtar olarak listelenen) gösterecektir. Bir sonraki adım için ikisine de ihtiyacınız olacak.

4. API'yi kullanarak Log Analytics aracısını kümenize `vmss extension set` yüklemek için komutu çalıştırın:

    Bir Windows kümesi için:

    ```azurecli
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Bir Linux kümesi için:

    ```azurecli
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Aşağıda, Windows kümesine eklenen Log Analytics aracısının bir örneği verilmiştir.

    ![Log Analytics aracıc komutu](media/service-fabric-diagnostics-oms-agent/cli-command.png)

5. Bu başarıyla düğümleri için aracı eklemek için 15 dakikadan az sürer. `az vmss extension list` API'yi kullanarak aracıların eklendiğini doğrulayabilirsiniz:

    ```azurecli
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Kaynak Yöneticisi şablonu aracılığıyla aracı ekleme

Bir Azure Log Analytics çalışma alanı dağıtan ve düğümlerinizin her birine bir aracı ekleyen Örnek Kaynak Yöneticisi şablonları [Windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) veya [Linux](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeType-Secure-OMS)için kullanılabilir.

İhtiyaçlarınıza en uygun kümeyi dağıtmak için bu şablonu indirebilir ve değiştirebilirsiniz.

## <a name="view-performance-counters"></a>Performans Sayaçlarını Görüntüle

Artık Log Analytics aracısını eklediğinize göre, hangi performans sayaçlarını toplamak istediğinizi seçmek için Log Analytics portalına gidin.

1. Azure portalında, Service Fabric Analytics çözümlerini oluşturduğunuz kaynak grubuna gidin. **ServiceFabric\<adını seçinOfLog AnalyticsWorkspace\>**.

2. **Log Analytics**’i tıklayın.

3. **Gelişmiş Ayarlar'ı**tıklatın.

4. **Veri'yi**tıklatın, ardından **Windows veya Linux Performans Sayaçları'nı**tıklatın. Etkinleştirmeyi seçebileceğiniz varsayılan sayaçların bir listesi vardır ve toplama aralığını da ayarlayabilirsiniz. Toplamak için [ek performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) da ekleyebilirsiniz. Uygun biçim bu [makalede](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx)başvurulan .

5. **Kaydet'i**tıklatın, ardından **Tamam'ı**tıklatın.

6. Gelişmiş Ayarlar bıçağını kapatın.

7. Genel başlığın altında **Çalışma Alanı özetini**tıklatın.

8. Hizmet Kumaşı için bir tane de dahil olmak üzere, etkinleştirilen çözümlerin her biri için grafik şeklinde kutucuklar görürsünüz. Service **Fabric** Analytics çözümüne devam etmek için Service Fabric grafiğini tıklatın.

9. Operasyonel kanal ve güvenilir hizmet etkinlikleri nde grafikleri olan birkaç kutucuk göreceksiniz. Seçtiğiniz sayaçlar için akan verilerin grafik gösterimi Düğüm Ölçümleri altında görünür.

10. Ek ayrıntıları görmek için Kapsayıcı Metrik grafiğini tıklatın. Ayrıca, Kusto sorgu dilini kullanarak performans sayacı verilerini kümeleme olaylarına benzer şekilde sorgulayabilir ve düğümlerde, perf sayacı adı ve değerleri filtreleyebilirsiniz.

![Log Analytics perf sayıcı sorgusu](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Sonraki adımlar

* İlgili [performans sayaçlarını](service-fabric-diagnostics-event-generation-perf.md)toplayın. Log Analytics aracısını belirli performans sayaçları toplamak üzere yapılandırmak için, yapılandırılan veri kaynaklarını gözden [geçirin.](../azure-monitor/platform/agent-data-sources.md#configuring-data-sources)
* Algılama ve tanılama da yardımcı olmak için [otomatik uyarı](../log-analytics/log-analytics-alerts.md) oluşturmak için Azure Monitor günlüklerini yapılandırın
* Alternatif olarak Azure Tanılama uzantısı aracılığıyla performans sayaçları toplayabilir [ve Bunları Uygulama Öngörüleri'ne gönderebilirsiniz](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template)
