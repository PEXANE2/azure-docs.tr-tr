---
title: Azure DDoS koruması standart raporları ve akış günlükleri
description: Raporların ve akış günlüklerinin nasıl yapılandırılacağını öğrenin.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: 28e977ed68a3f288f9f86a0c2be02af4cbb26ba4
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94886570"
---
# <a name="configure-ddos-attack-mitigation-reports-and-flow-logs"></a>DDoS saldırı azaltma raporları ve akış günlükleri 

Azure DDoS koruması standardı, DDoS saldırı analiziyle ayrıntılı saldırı öngörüleri ve görselleştirmeleri sağlar. Sanal ağlarını DDoS saldırılarına karşı koruyan müşteriler, saldırı saldırılarına karşı saldırı ve risk azaltma & raporları aracılığıyla saldırının etkilerini azaltmak için gerçekleştirilen saldırı ve eylemler hakkında ayrıntılı görünürlük sağlar. Zengin telemetri, DDoS saldırısının süresi boyunca ayrıntılı ölçümler dahil olmak üzere Azure Izleyici aracılığıyla sunulur. DDoS koruması tarafından sunulan Azure Izleyici ölçümlerinden herhangi biri için uyarı yapılandırılabilir. Günlüğe kaydetme, Azure Izleme tanılama arabirimi aracılığıyla [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection), splunk (Azure Event Hubs), OMS Log Analytics ve gelişmiş analiz Için Azure depolama ile daha da tümleştirilebilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * DDoS saldırı risk azaltma raporlarını görüntüleyin ve yapılandırın
> * DDoS saldırıları risk azaltma akış günlüklerini görüntüleyin ve yapılandırın

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Bu öğreticideki adımları tamamlayabilmeniz için önce bir [Azure DDoS standart koruma planı](manage-ddos-protection.md)oluşturmanız gerekir.

## <a name="view-and-configure-ddos-attack-mitigation-reports"></a>DDoS saldırı risk azaltma raporlarını görüntüleyin ve yapılandırın

Saldırı risk azaltma raporları, kaynak üzerinde saldırı hakkında ayrıntılı bilgi sağlamak için toplanan Netflow protokol verilerini kullanır. Genel bir IP kaynağı saldırı altında olduğunda, risk azaltma başladıktan sonra rapor oluşturma başlatılır. Her 5 dakikada bir ve risk azaltma süresi boyunca bir azaltma sonrası raporuyla oluşturulan bir artımlı rapor olacaktır. Bu, DDoS saldırısının daha uzun bir süre boyunca devam ettiğinden emin olmak için, risk azaltma süresi bittikten sonra, her 5 dakikada bir azaltma raporunun en güncel anlık görüntüsünü ve tüm Özeti görüntüleyebileceksiniz. 

1. Portalın sol tarafındaki **tüm hizmetler** ' i seçin.
2. **Filtre** kutusuna *izleyiciyi* girin. Sonuçlarda **izleyici** göründüğünde seçin.
3. **Ayarlar** altında **Tanılama ayarları**' nı seçin.
4. Günlüğe kaydetmek istediğiniz genel IP adresini içeren **aboneliği** ve **kaynak grubunu** seçin.
5. **Kaynak türü** IÇIN **genel IP adresi** ' ni seçin ve ardından ölçümlerini günlüğe kaydetmek istediğiniz belirli genel IP adresini seçin.
6. **DDoSMitigationReports günlüğünü toplamak için tanılamayı aç** ' ı seçin ve ardından ihtiyacınız olan aşağıdaki seçeneklerden birçoğunu seçin:

    - **Bir depolama hesabına arşivleme**: veriler bir Azure depolama hesabına yazılır. Bu seçenek hakkında daha fazla bilgi edinmek için bkz. [Arşiv kaynak günlükleri](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Bir olay hub 'ına akış**: günlük alıcının bir Azure Olay Hub 'ı kullanarak günlükleri seçmesine izin verir. Olay Hub 'ları, splunk veya diğer SıEM sistemleriyle tümleştirmeyi etkinleştirir. Bu seçenek hakkında daha fazla bilgi edinmek için bkz. [kaynak günlüklerini bir olay hub 'ına akış](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Log Analytics gönder**: günlükleri Azure izleyici hizmetine yazar. Bu seçenek hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlüklerinde kullanılacak günlükleri toplama](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Hem artımlı & saldırı sonrası risk azaltma raporları aşağıdaki alanları içerir
- Saldırı vektörleri
- Trafik istatistikleri
- Bırakılan paketlerin nedeni
- Dahil edilen protokoller
- İlk 10 kaynak ülke veya bölge
- İlk 10 kaynak ASNs

## <a name="view-and-configure-ddos-attack-mitigation-flow-logs"></a>DDoS saldırıları risk azaltma akış günlüklerini görüntüleyin ve yapılandırın
Saldırı risk azaltma akış günlükleri, etkin bir DDoS saldırısı sırasında bırakılan trafiği, iletilen trafiği ve diğer ilgi çekici veri noktalarını neredeyse gerçek zamanlı olarak incelemenizi sağlar. Bu verilerin sabit akışını Azure Sentinel 'e veya üçüncü taraf SıEM sistemlerine, neredeyse gerçek zamanlı izleme için Olay Hub 'ı aracılığıyla alabilir, olası eylemleri gerçekleştirebilir ve savunma operasyonlarınızın gereksinimini ortadan kaldırabilirsiniz.

1. Portalın sol tarafındaki **tüm hizmetler** ' i seçin.
2. **Filtre** kutusuna *izleyiciyi* girin. Sonuçlarda **izleyici** göründüğünde seçin.
3. **Ayarlar** altında **Tanılama ayarları**' nı seçin.
4. Günlüğe kaydetmek istediğiniz genel IP adresini içeren **aboneliği** ve **kaynak grubunu** seçin.
5. **Kaynak türü** IÇIN **genel IP adresi** ' ni seçin ve ardından ölçümlerini günlüğe kaydetmek istediğiniz belirli genel IP adresini seçin.
6. **DDoSMitigationFlowLogs günlüğünü toplamak için tanılamayı aç** ' ı seçin ve ardından ihtiyacınız olan aşağıdaki seçeneklerden birçoğunu seçin:

    - **Bir depolama hesabına arşivleme**: veriler bir Azure depolama hesabına yazılır. Bu seçenek hakkında daha fazla bilgi edinmek için bkz. [Arşiv kaynak günlükleri](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Bir olay hub 'ına akış**: günlük alıcının bir Azure Olay Hub 'ı kullanarak günlükleri seçmesine izin verir. Olay Hub 'ları, splunk veya diğer SıEM sistemleriyle tümleştirmeyi etkinleştirir. Bu seçenek hakkında daha fazla bilgi edinmek için bkz. [kaynak günlüklerini bir olay hub 'ına akış](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Log Analytics gönder**: günlükleri Azure izleyici hizmetine yazar. Bu seçenek hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlüklerinde kullanılacak günlükleri toplama](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Bu [şablon](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Enable%20Diagnostic%20Logging/Azure%20Policy) , tanılama günlüğünü etkinleştirmek Için bir Azure ilke tanımı oluşturur.

### <a name="azure-sentinel-data-connector"></a>Azure Sentinel veri Bağlayıcısı

Azure Sentinel 'e bağlanarak verilerinizi çalışma kitaplarında görüntüleyebilir ve analiz edebilir, özel uyarılar oluşturabilir ve bunları araştırma işlemlerine ekleyebilirsiniz. Azure Sentinel 'e bağlanmak için bkz. [Azure Sentinel 'e bağlanma](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection). 

![Azure Sentinel DDoS Bağlayıcısı](./media/ddos-attack-telemetry/azure-sentinel-ddos.png)

### <a name="azure-ddos-protection-workbook"></a>Azure DDoS koruması çalışma kitabı

Azure Analytics panosu 'nda akış günlüğü verilerini görüntülemek için, örnek panoyu şuradan içeri aktarabilirsiniz https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Azure%20DDoS%20Protection%20Workbook

Akış günlükleri aşağıdaki alanlara sahip olur: 
- Kaynak IP
- Hedef IP
- Kaynak Bağlantı Noktası 
- Hedef bağlantı noktası 
- Protokol türü 
- Risk azaltma sırasında gerçekleştirilecek eylem

![DDoS koruması çalışma kitabı](./media/ddos-attack-telemetry/ddos-attack-analytics-workbook.png)

Saldırı Analizi, yalnızca genel IP adresinin sanal ağında DDoS koruma standardı etkinse çalışır. 

## <a name="sample-log-outputs"></a>Örnek günlük çıkışları

Aşağıdaki ekran görüntüleri örnek günlük çıktılardır:

### <a name="ddosmitigationflowlogs"></a>DDoSMitigationFlowLogs

![DDoS koruması DDoSMitigationFlowLogs](./media/ddos-attack-telemetry/ddos-mitigation-flow-logs.png)

### <a name="ddosprotectionnotifications"></a>DDoSProtectionNotifications

![DDoS koruması DDoSProtectionNotifications](./media/ddos-attack-telemetry/ddos-protection-notifications.png)

### <a name="ddosmitigationreports"></a>DDoSMitigationReports

![DDoS koruması DDoSMitigationReports](./media/ddos-attack-telemetry/ddos-mitigation-reports.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

- DDoS saldırı risk azaltma raporlarını görüntüleyin ve yapılandırın
- DDoS saldırıları risk azaltma akış günlüklerini görüntüleyin ve yapılandırın

DDoS saldırısını test etme ve benzetimini yapma hakkında bilgi edinmek için bkz. simülasyon test Kılavuzu:

> [!div class="nextstepaction"]
> [Simülasyonlar aracılığıyla test etme](test-through-simulations.md)

