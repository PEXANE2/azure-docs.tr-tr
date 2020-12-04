---
title: İşlemleri, olayları ve genel temel Load Balancer sayaçlarını izleme
titleSuffix: Azure Load Balancer
description: Azure Load Balancer için günlük kaydını etkinleştirmeyi öğrenin
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2020
ms.author: allensu
ms.openlocfilehash: 6742723e24df83ac8112e224f1999f116ab82c94
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96572788"
---
# <a name="azure-monitor-logs-for-the-standard-azure-load-balancer"></a>Standart Azure Load Balancer Azure Izleyici günlükleri

Standart yük dengeleyiciler yönetmek ve sorunlarını gidermek için Azure 'da farklı türlerde Günlükler kullanabilirsiniz. Günlükler bir olay hub 'ına veya Log Analytics çalışma alanına akışla eklenebilir. Tüm Günlükler Azure Blob depolamadan ayıklanıp Excel ve Power BI gibi farklı araçlarla görüntülenebilir.  Aşağıdaki listeden farklı türlerdeki Günlükler hakkında daha fazla bilgi edinebilirsiniz.

* **Etkinlik günlükleri:** Azure aboneliğinize gönderilen tüm etkinlikleri ve bunların durumunu görüntülemek için [kaynak eylemlerini izlemek üzere etkinlik günlüklerini görüntüle](../azure-resource-manager/management/view-activity-logs.md) ' yi kullanabilirsiniz. Etkinlik günlükleri varsayılan olarak etkindir ve Azure portal görüntülenebilir. Bu Günlükler hem temel hem de standart yük dengeleyiciler için kullanılabilir.
* **Standart Load Balancer ölçümleri:** Bu günlüğü, standart Azure Load Balancer Günlükler olarak dışarıya aktarılmış ölçümleri sorgulamak için kullanabilirsiniz. Bu günlükler yalnızca standart yük dengeleyiciler için kullanılabilir.

> [!IMPORTANT]
> **Durum araştırması ve Load Balancer uyarı olay günlükleri Şu anda işlevsel değildir ve [Azure Load Balancer bilinen sorunlarda](whats-new.md#known-issues)listelenmiştir.** 

> [!IMPORTANT]
> Günlükler yalnızca Kaynak Yöneticisi dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynaklar için günlük kullanamazsınız. Dağıtım modelleri hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi dağıtımı ve klasik dağıtımı anlama](../azure-resource-manager/management/deployment-models.md).

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Etkinlik günlüğü tüm Kaynak Yöneticisi kaynakları için otomatik olarak etkinleştirilir. Bu Günlükler aracılığıyla kullanılabilir verileri toplamaya başlamak için olay ve durum araştırma günlüğünü etkinleştirin. Günlüğe kaydetmeyi etkinleştirmek için aşağıdaki adımları kullanın.

[Azure portalda](https://portal.azure.com) oturum açın. Zaten bir yük dengeleyiciniz yoksa devam etmeden önce [yük dengeleyici oluşturun](./quickstart-load-balancer-standard-public-portal.md) .

1. Portalda **kaynak grupları**' na tıklayın.
2. **\<resource-group-name>** Yük dengeleyicinizin nerede olduğunu seçin.
3. Yük dengeleyicinizi seçin.
4. **Etkinlik günlüğü**  >  **Tanılama ayarları**' nı seçin.
5. **Tanılama ayarları** bölmesinde, **Tanılama ayarları** altında **+ Tanılama ayarı Ekle**' yi seçin.
6. **Tanılama ayarları** oluşturma bölmesinde **ad** alanına **mylbdiagnostics** ' i girin.
7. **Tanılama ayarları** için üç seçeneğiniz vardır.  Bunlardan birini, ikisini veya üçünü seçebilirsiniz ve her birini gereksinimleriniz için yapılandırabilirsiniz:
   * **Bir depolama hesabına Arşivle**
   * **Olay hub’ına akış yapma**
   * **Log Analytics’e gönderme**

    ### <a name="archive-to-a-storage-account"></a>Bir depolama hesabına arşivle
    Bu işlem için zaten oluşturulmuş bir depolama hesabınız olması gerekir.  Depolama hesabı oluşturmak için bkz. [depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)

    1. **Depolama hesabında Arşivle** seçeneğinin yanındaki onay kutusunu işaretleyin.
    2. **Depolama hesabı seç** bölmesini açmak için **Yapılandır** ' ı seçin.
    3. Aşağı açılan kutuda depolama hesabınızın oluşturulduğu **aboneliği** seçin.
    4. Açılır kutuda depolama **hesabı** altında depolama hesabınızın adını seçin.
    5. Tamam'ı seçin.

    ### <a name="stream-to-an-event-hub"></a>Bir olay hub'ına akış yap
    Bu işlem için önceden oluşturulmuş bir olay hub 'ı gerekir.  Bir olay hub 'ı oluşturmak için bkz [. hızlı başlangıç: Azure Portal kullanarak bir olay hub 'ı oluşturma](../event-hubs/event-hubs-create.md)

    1. **Olay Hub 'ına akış** ' nın yanındaki onay kutusunu seçin
    2. **Olay Hub 'ını Seç** bölmesini açmak için **Yapılandır** ' ı seçin.
    3. Aşağı açılan kutuda Olay Hub 'ınızın oluşturulduğu **aboneliği** seçin.
    4. Açılır kutuda **Olay Hub 'ı ad alanını seçin** .
    5. Açılır kutuda **Olay Hub 'ı ilke adı** ' nı seçin.
    6. Tamam'ı seçin.

    ### <a name="send-to-log-analytics"></a>Log Analytics’e gönderme
    Bu işlem için oluşturulmuş ve yapılandırılmış bir Log Analytics çalışma alanınız olması gerekir.  Log Analytics çalışma alanı oluşturmak için, bkz [. Azure portal Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md)

    1. **Log Analytics gönder**' in yanındaki onay kutusunu seçin.
    2. Log Analytics çalışma alanınızın açılır kutuda olduğu **aboneliği** seçin.
    3. Açılır kutudan **Log Analytics çalışma alanını** seçin.


8.  **Tanılama ayarları** bölmesindeki **ölçüm** bölümünde, şunun yanındaki onay kutusunu işaretleyin: **allölçümler**

9. Her şeyin doğru göründüğünü doğrulayın ve **Tanılama ayarları** oluştur bölmesinin en üstünde **Kaydet** ' e tıklayın.

## <a name="activity-log"></a>Etkinlik günlüğü

Etkinlik günlüğü varsayılan olarak oluşturulur. [Bu makaledeki yönergelerden sonraki](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log)bir abonelik düzeyinde aktarılacak şekilde yapılandırılabilir. [Kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüle](../azure-resource-manager/management/view-activity-logs.md) makalesindeki bu Günlükler hakkında daha fazla bilgi edinin makalesini okuyun.

### <a name="view-and-analyze-the-activity-log"></a>Etkinlik günlüğünü görüntüleme ve analiz etme

Aşağıdaki yöntemlerden herhangi birini kullanarak, etkinlik günlüğü verilerini görüntüleyebilir ve çözümleyebilirsiniz:

* **Azure Araçları:** Azure PowerShell, Azure komut satırı arabirimi (CLı), Azure REST API veya Azure portal aracılığıyla etkinlik günlüğünden bilgi alın. Her yöntemin adım adım yönergeleri [Kaynak Yöneticisi makalesinde denetim işlemlerinde](../azure-resource-manager/management/view-activity-logs.md) ayrıntılı olarak açıklanmıştır.
* **Power BI:** Zaten bir [Power BI](https://powerbi.microsoft.com/pricing) hesabınız yoksa ücretsiz olarak deneyebilirsiniz. [Power BI Için Azure denetim günlükleri tümleştirmesini](https://powerbi.microsoft.com/integrations/azure-audit-logs/)kullanarak verilerinizi önceden yapılandırılmış panolar ile çözümleyebilir veya görünümleri gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

## <a name="metrics-as-logs"></a>Günlük olarak ölçümler
Azure Izleyici tarafından sunulan dışarı aktarma işlevselliğini günlüğe kaydederek ölçümleri kullanarak Load Balancer ölçümlerini dışarı aktarabilirsiniz. Bu ölçümler, her bir dakika örnekleme aralığı için bir günlük girişi oluşturur.

Günlük dışa aktarma ölçümü kaynak düzeyinde etkinleştirilir. Bu günlükleri Tanılama ayarları dikey penceresine giderek, kaynak grubuna göre filtreleyip ve ölçüm dışarı aktarılmasını etkinleştirmek istediğiniz Load Balancer seçerek etkinleştirebilirsiniz. Load Balancer Tanılama ayarları sayfası çalışırken uygun ölçümleri Günlükler olarak dışarı aktarmak için Allölçümleri ' ni seçin.

Ölçüm dışa aktarma sınırlamaları için bu makalenin [sınırlamalar](#limitations) bölümüne bakın.

### <a name="view-and-analyze-metrics-as-logs"></a>Ölçümleri günlük olarak görüntüleyin ve çözümleyin
Standart Load Balancer tanılama ayarlarında Allölçümlerini etkinleştirdikten sonra, bir olay hub 'ı veya Log Analytics çalışma alanı kullanılıyorsa, bu Günlükler AzureMonitor tablosunda doldurulur. Depolama alanına veriyorsanız depolama hesabınıza bağlanın ve olay ve durum araştırma günlükleri için JSON günlüğü girdilerini alın. JSON dosyalarını indirdikten sonra Excel, Power BI veya başka bir veri görselleştirme aracında CSV 'ye dönüştürebilir ve bu verileri görüntüleyebilirsiniz. 

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.

## <a name="stream-to-an-event-hub"></a>Bir olay hub'ına akış yap
Tanılama bilgileri bir olay hub 'ına akış yapıldığında, Azure Izleyici tümleştirmesiyle üçüncü taraf SıEM aracında Merkezi günlük analizi için kullanılabilir. Daha fazla bilgi için bkz. [Azure izleme verilerini bir olay hub 'ına akış](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#partner-tools-with-azure-monitor-integration)

## <a name="send-to-log-analytics"></a>Log Analytics’e gönderme
Azure 'daki kaynakların, tanılama bilgileri, sorun giderme ve analizine yönelik bilgilere karşı, karmaşık sorguların çalıştırılabileceği bir Log Analytics çalışma alanına doğrudan gönderilmesini sağlayabilir.  Daha fazla bilgi için bkz. Azure [izleyici 'de Log Analytics çalışma alanında Azure Kaynak günlüklerini toplama](../azure-monitor/platform/resource-logs.md#send-to-log-analytics-workspace)

## <a name="limitations"></a>Sınırlamalar
Yük dengeleyiciler için günlüğe dışa aktarma özelliği için ölçümler kullanılırken şu sınırlamalar vardır:
* Ölçümler Şu anda, günlük olarak verildiğinde iç adlar kullanılarak görüntülenir ve eşleme aşağıdaki tabloda bulunabilir
* Ölçümlerin boyutlılık korunmaz. Örneğin, bir DipAvailability (durum araştırma durumu) gibi ölçümlerde, arka uç IP adresine göre bölünemeyecek veya görüntüleyemezsiniz
* Kullanılan SNAT bağlantı noktaları ve ayrılmış SNAT bağlantı noktaları ölçümleri Şu anda Günlükler olarak dışa aktarma için kullanılabilir değil

## <a name="next-steps"></a>Sonraki Adımlar
* [Load Balancer için kullanılabilir ölçümleri gözden geçirin](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)
* [Azure Izleyici yönergelerinden sonra sorgu oluşturma ve test etme](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)
* Aşağıdaki bağlantıları kullanarak bu makale veya Load Balancer işlevselliği hakkında geri bildirim sağlayın
