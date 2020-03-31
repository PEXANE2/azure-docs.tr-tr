---
title: Kamu Temel Yük Dengeleyicisi için işlemleri, olayları ve sayaçları izleme
titleSuffix: Azure Load Balancer
description: Uyarı olaylarını nasıl etkinleştireceklerini öğrenin ve genel Temel Yük Dengeleyicisi için sistem durumu günlüğe kaydetmeyi
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: allensu
ms.openlocfilehash: 0a21af683d9fa7849d3e96c545983c9f40a8d4c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76935326"
---
# <a name="azure-monitor-logs-for-public-basic-load-balancer"></a>Genel Temel Yük Dengeleyici için Azure İzleyici günlükleri

>[!IMPORTANT]
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Temel Yük Dengeleyici anlatılmaktadır. Standart Yük Dengeleyicisi hakkında daha fazla bilgi için Azure Monitor'da çok boyutlu ölçümler aracılığıyla telemetriyi ortaya çıkaran [Standart Yük Dengeleyicisi genel görünümüne](load-balancer-standard-overview.md) bakın.

Temel Yük Dengeleyicilerini yönetmek ve sorun gidermek için Azure'da farklı türde günlükler kullanabilirsiniz. Bu günlüklerin bazılarına portaldan erişilebilir. Günlükler bir etkinlik hub'ına veya Log Analytics çalışma alanına aktarılabilir. Tüm günlükler Azure blob depolamadan ayıklanabilir ve Excel ve Power BI gibi farklı araçlarda görüntülenebilir.  Aşağıdaki listeden farklı günlük türleri hakkında daha fazla bilgi edinebilirsiniz.

* **Etkinlik günlükleri:** Azure aboneliğinize gönderilen tüm etkinlikleri ve bunların durumlarını görüntülemek için kaynaklardaki eylemleri izlemek için [Etkinlik günlüklerini görüntüleyebilir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) Etkinlik günlükleri varsayılan olarak etkinleştirilir ve Azure portalında görüntülenebilir.
* **Uyarı olay günlükleri:** Bu günlüğü, yük dengeleyicisi tarafından yükseltilen uyarıları görüntülemek için kullanabilirsiniz. Yük dengeleyicisinin durumu her beş dakikada bir toplanır. Bu günlük yalnızca bir yük dengeleyici uyarısı olayı yükseltilirse yazılır.
* **Sistem durumu sondası günlükleri:** Bu günlüğü, sistem durumu sondası hataları nedeniyle yük bakiyesi nden istek almayan arka uç havuzunuzdaki örneklerin sayısı gibi sağlık sondanız tarafından algılanan sorunları görüntülemek için kullanabilirsiniz. Bu günlük, sağlık sondası durumunda bir değişiklik olduğunda yazılır.

> [!IMPORTANT]
> Azure Monitor günlükleri şu anda yalnızca genel Temel yük dengeleyicileri için çalışır. Günlükler yalnızca Kaynak Yöneticisi dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynaklar için günlük kullanamazsınız. Dağıtım modelleri hakkında daha fazla bilgi için kaynak [yöneticisi dağıtımını ve klasik dağıtımı anlama'ya](../azure-resource-manager/management/deployment-models.md)bakın.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Etkinlik günlüğü tüm Kaynak Yöneticisi kaynakları için otomatik olarak etkinleştirilir. Bu günlükler aracılığıyla kullanılabilir verileri toplamaya başlamak için olay ve sistem durumu sondası günlüğe kaydetmeyi etkinleştirin. Günlüğe kaydetmeyi etkinleştirmek için aşağıdaki adımları kullanın.

[Azure portalında](https://portal.azure.com)oturum açın. Zaten bir yük dengeleyiciniz yoksa, devam etmeden önce [bir yük dengeleyicioluşturun.](https://docs.microsoft.com/azure/load-balancer/quickstart-create-basic-load-balancer-portal)

1. Portalda Kaynak **gruplarını**tıklatın.
2. Yük bakiyecinizin bulunduğu yer ** \<>kaynak grubu adını** seçin.
3. Yük bakiyecinizi seçin.
4. **İzleme** > **Tanılama ayarlarını**seçin.
5. **Tanılama ayarları** bölmesinde, **Tanılama ayarları**nın altında , **+ Tanılama ayarı ekle'yi**seçin.
6. **Tanılama ayarları** oluşturma bölmesine, **Ad** alanına **myLBDiagnostics'i** girin.
7. **Tanılama ayarları**için üç seçeneğiniz vardır.  Bir, iki veya üç ünü de seçebilir ve gereksinimleriniz için her birini yapılandırabilirsiniz:
   * **Depolama hesabına arşivleme**
   * **Olay merkezine akış**
   * **Log Analytics’e gönderme**

    ### <a name="archive-to-a-storage-account"></a>Bir depolama hesabına arşivle
    Bu işlem için zaten oluşturulmuş bir depolama hesabı gerekir.  Depolama hesabı oluşturmak için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)

    1. Arşiv'in yanındaki onay kutusunu **bir depolama hesabına**seçin.
    2. **Depolama hesabı** seç bölmesini açmak için **Yapıl'ı** seçin.
    3. Açılan kutuda depolama hesabınızın oluşturulduğu **Abonelik'i** seçin.
    4. Çekmece kutusunda Ki Depolama **hesabının** altındaki depolama hesabınızın adını seçin.
    5. Tamam'ı seçin.

    ### <a name="stream-to-an-event-hub"></a>Bir olay hub'ına akış yap
    Bu işlem için zaten oluşturulmuş bir olay hub'ına ihtiyacınız olacak.  Etkinlik merkezi oluşturmak için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak bir etkinlik merkezi oluşturma](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)

    1. Olay hub'ına **Akış'ın** yanındaki onay kutusunu seçin
    2. **Olay hub'ını seç** bölmesini açmak için **Yapıya'yı** seçin.
    3. Açılan kutuda etkinlik merkezinizin oluşturulduğu **Abonelik'i** seçin.
    4. Çekme kutusunda **olay hub'ı ad alanını seçin.**
    5. Açılan kutuda **olay merkezi ilkesi adını seçin.**
    6. Tamam'ı seçin.

    ### <a name="send-to-log-analytics"></a>Log Analytics’e gönderme
    Bu işlem için zaten bir günlük analizi çalışma alanı oluşturulması ve yapılandırılması gerekir.  Günlük Analitiği çalışma alanı oluşturmak için [bkz.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)

    1. **Günlük Analitiğine Gönder'in**yanındaki onay kutusunu seçin.
    2. Log Analytics çalışma alanınızın çekmece kutusunda olduğu **Abonelik'i** seçin.
    3. Açılan kutudaki **Günlük Analizi Çalışma Alanını** seçin.


8. **Tanılama ayarları** bölmesindeki **LOG** bölümünün altında, her ikisinin yanındaki onay kutusunu seçin:
   * **LoadBalancerAlertOlay**
   * **LoadBalancerProbeSağlıkDurumu**

9.  **Tanılama ayarları** bölmesindeki **METRIC** bölümünün altında, aşağıdakilerin yanındaki onay kutusunu seçin:
   * **Tüm Ölçümler**

11. Her şeyin doğru göründüğünü doğrulayın ve **Tanılama ayarları** bölmesinin üst kısmında **kaydet'i** tıklatın.

## <a name="activity-log"></a>Etkinlik günlüğü

Etkinlik günlüğü varsayılan olarak oluşturulur. Günlükler Azure'un Etkinlik Günlükleri mağazasında 90 gün boyunca saklanır. Kaynaklar makalesindeki eylemleri izlemek [için Etkinlik günlüklerini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) görüntüle'yi okuyarak bu günlükler hakkında daha fazla bilgi edinin.

## <a name="archive-to-storage-account-logs"></a>Depolama hesap günlüklerine arşivleme

### <a name="alert-event-log"></a>Uyarı olay günlüğü

Bu günlük yalnızca yük bakiyesi başına etkinleştirdiyseniz oluşturulur. Olaylar JSON biçiminde günlüğe kaydedilir ve günlüğe kaydetmeyi etkinleştirdiğinizde belirttiğiniz depolama hesabında depolanır. Aşağıdaki örnek bir olaydır.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

JSON çıkışı, yük dengeleyicisinin bir uyarı oluşturma nedenini açıklayan *olay adı* özelliğini gösterir. Bu durumda, oluşturulan uyarı kaynak IP NAT sınırları (SNAT) nedeniyle TCP bağlantı noktası tükenmesi nedeniyle oldu.

### <a name="health-probe-log"></a>Sistem durumu sondası günlüğü

Bu günlük, yalnızca yukarıda açıklandığı gibi yük bakiyesi başına etkinleştirdiyseniz oluşturulur. Veriler, günlüğe kaydetmeyi etkinleştirdiğinizde belirttiğiniz depolama hesabında depolanır. 'Insights-logs-loadbalancerprobehealthstatus' adlı bir kapsayıcı oluşturulur ve aşağıdaki veriler günlüğe kaydedilir:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

JSON çıkışı özellikleri alanında sonda sağlık durumu için temel bilgileri gösterir. *DipDownCount* özelliği, başarısız sonda yanıtları nedeniyle ağ trafiği almayan arka uçtaki toplam örnek sayısını gösterir.

### <a name="view-and-analyze-the-activity-log"></a>Etkinlik günlüğünü görüntüleme ve analiz etme

Etkinlik günlüğü verilerini aşağıdaki yöntemlerden herhangi birini kullanarak görüntüleyebilir ve çözümleyebilirsiniz:

* **Azure araçları:** Azure PowerShell, Azure Komut Satırı Arabirimi (CLI), Azure REST API veya Azure portalı aracılığıyla etkinlik günlüğünden bilgi alın. Her yöntem için adım adım yönergeler Kaynak Yöneticisi makalesi [ile Denetim işlemlerinde](../azure-resource-manager/management/view-activity-logs.md) ayrıntılı olarak açıklanır.
* **Güç BI:** Zaten bir [Power BI](https:// .microsoft.com/pricing) hesabınız yoksa, ücretsiz olarak deneyebilirsiniz. Power [BI için Azure Denetim Günlükleri içerik paketini](https:// .microsoft.com/documentation/ -content-pack-azure-audit-logs)kullanarak verilerinizi önceden yapılandırılmış panolarla analiz edebilir veya görünümleri gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

### <a name="view-and-analyze-the-health-probe-and-event-log"></a>Sistem durumu sondasını ve olay günlüğünü görüntüleme ve analiz

Depolama hesabınıza bağlanın ve olay ve sistem durumu sondası günlükleri için JSON günlük girişlerini alın. JSON dosyalarını indirdikten sonra, bunları CSV'ye dönüştürebilir ve Excel, Power BI veya başka bir veri görselleştirme aracında görüntüleyebilirsiniz.

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.

## <a name="stream-to-an-event-hub"></a>Bir olay hub'ına akış yap
Tanılama bilgileri bir olay hub'ına aktarıldığında, Azure Monitör Tümleştirmesi ile üçüncü taraf bir SIEM aracında merkezi günlük çözümlemesi için kullanılabilir. Daha fazla bilgi için [bkz.](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#partner-tools-with-azure-monitor-integration)

## <a name="send-to-log-analytics"></a>Log Analytics’e gönderme
Azure'daki kaynaklar, tanılama bilgilerini doğrudan karmaşık sorguların sorun giderme ve analiz bilgileriyle karşılaştırılabildiği bir Log Analytics çalışma alanına gönderilebilir.  Daha fazla bilgi için bkz. Azure [Monitor'da Günlük Analizi çalışma alanında Azure kaynak günlüklerini topla](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace)

## <a name="next-steps"></a>Sonraki adımlar

[Yük dengeleyici araştırmalarını anlama](load-balancer-custom-probe-overview.md)
