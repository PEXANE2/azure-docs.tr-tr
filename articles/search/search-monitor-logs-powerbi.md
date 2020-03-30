---
title: Power BI ile Azure Bilişsel Arama Günlüklerini ve Ölçümlerini Görselleştirin
description: Power BI ile Azure Bilişsel Arama Günlüklerini ve Ölçümlerini Görselleştirin
manager: eladz
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 618033e24236d26601fafaf7b0678ee83ec973e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650186"
---
# <a name="visualize-azure-cognitive-search-logs-and-metrics-with-power-bi"></a>Power BI ile Azure Bilişsel Arama Günlüklerini ve Ölçümlerini Görselleştirin
[Azure Bilişsel Arama,](https://docs.microsoft.com/azure/search/search-what-is-azure-search) arama hizmetinizle ilgili işlem günlüklerini ve hizmet ölçümlerini bir Azure Depolama hesabında depolamanıza olanak tanır. Bu sayfa, bir Power BI Şablon Uygulaması aracılığıyla bu bilgileri nasıl görselleştirebileceğinize dair yönergeler sağlar. Uygulama, Arama, Dizin oluşturma, Operasyonlar ve Hizmet ölçümleri hakkındaki bilgiler de dahil olmak üzere arama hizmetiniz hakkında ayrıntılı bilgiler sağlar.

Power BI Şablon Uygulaması **Azure Bilişsel Arama:Power** BI Apps [pazar yerinde](https://appsource.microsoft.com/marketplace/apps)Günlükleri ve Ölçümleri analiz edin.

## <a name="how-to-get-started-with-the-app"></a>Uygulama ile nasıl başlarsınız?
1. Arama hizmetiniz için tanısal günlük'ünüzü etkinleştirin:
    1. Günlükleri arşivleyebileceğiniz varolan bir [Azure Depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) oluşturma veya tanımlama
    1. Azure portalında Azure Bilişsel Arama hizmetinize gidin
    1. Sol sütundaki İzleme bölümünde **Tanılama ayarlarını** seçin

        ![](media/search-monitor-logs-powerbi/diagnostic-settings.png)

    1. Select **+ Tanılama ayarı ekle**
    1. **Arşiv'i bir depolama hesabına**kontrol edin, Depolama hesabı bilgilerinizi sağlayın ve **OperationLogs** ve **AllMetrics'i** kontrol edin

        ![](media/search-monitor-logs-powerbi/add-diagnostic-setting.png)
    1. **Kaydet**’i seçin

1. Günlüğe kaydetme etkinleştirildikten sonra, günlükler ve ölçümler oluşturmaya başlamak için arama hizmetinizi kullanın. Kapsayıcıların bu günlüklerle Blob depolama alanında görünmesi bir saat kadar sürer. Arama trafiği günlükleri için bir **öngörü-günlük-işlem günlüğü** konteyneri ve ölçümler için bir **öngörü-metrik-pt1m** kapsayıcı göreceksiniz.

1. [Power BI Apps pazar](https://appsource.microsoft.com/marketplace/apps) yerinde Azure Bilişsel Arama Gücü BI Uygulamasını bulun ve yeni bir çalışma alanına veya varolan bir çalışma alanına yükleyin. Uygulamanın adı **Azure Bilişsel Arama: Günlükleri ve Ölçümleri Analiz**Et.

1. Uygulamayı yükledikten sonra Power BI'deki uygulama listenizden uygulamayı seçin.

    ![](media/search-monitor-logs-powerbi/azure-search-app-tile.png)

1. Verilerinizi bağlamak için **Bağlan'ı** seçin

    ![](media/search-monitor-logs-powerbi/get-started-with-your-new-app.png)

1. Günlüklerinizi ve ölçümlerinizi içeren depolama hesabının adını girdin. Varsayılan olarak uygulama son 10 günlük verilere bakar, ancak bu değer **Gün** parametresi ile değiştirilebilir.

    ![](media/search-monitor-logs-powerbi/connect-to-storage-account.png)

1. Kimlik doğrulama yöntemi olarak **Anahtar'ı** seçin ve depolama hesabı anahtarınızı sağlayın. Gizlilik düzeyi olarak **Özel'i** seçin. Oturum Aç'a tıklayın ve yükleme işlemine başlayın.

    ![](media/search-monitor-logs-powerbi/connect-to-storage-account-step-two.png)

1. Verilerin yenilenmesini bekleyin. Bu, ne kadar veriye sahip olduğunuza bağlı olarak biraz zaman alabilir. Aşağıdaki göstergeye göre verilerin hala yenilenip yenilenmediğinizi görebilirsiniz.

    ![](media/search-monitor-logs-powerbi/workspace-view-refreshing.png)

1. Veri yenileme tamamlandıktan sonra, raporu görüntülemek için **Azure Bilişsel Arama Raporu'nu** seçin.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-report.png)

1. Raporu açtıktan sonra sayfayı verilerinizle birlikte açılacak şekilde yenilediğinden emin olun.

    ![](media/search-monitor-logs-powerbi/powerbi-search.png)

## <a name="how-to-change-the-app-parameters"></a>Uygulama parametreleri nasıl değiştirilir?
Farklı bir depolama hesabındaki verileri görselleştirmek veya gün veri sayısını sorgulamak istiyorsanız, **Günler** ve **Depolama Hesabı** parametrelerini değiştirmek için aşağıdaki adımları izleyin.

1. Power BI uygulamalarınızda gezinin, Azure Bilişsel Arama uygulamanızı bulun ve çalışma alanını görüntülemek için **Edit uygulamasını** seçin.

    ![](media/search-monitor-logs-powerbi/azure-search-app-tile-edit.png)

1. Veri Kümesi seçeneklerinden **Ayarlar'ı** seçin.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-settings.png)

1. Veri kümeleri sekmesindeyken parametre değerlerini değiştirin ve **Uygula'yı**seçin. Bağlantıyla ilgili bir sorun varsa, aynı sayfadaki veri kaynağı kimlik bilgilerini güncelleştirin.

1. Çalışma alanına geri gidin ve Dataset seçeneklerinden **şimdi Yenile'yi** seçin.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-refresh-now.png)

1. Güncelleştirilmiş verileri görüntülemek için raporu açın. En son verileri görüntülemek için raporu yenilemeniz de gerekebilir.

## <a name="troubleshooting"></a>Sorun giderme
Verilerinizi göremediğinizi fark ederseniz aşağıdaki sorun giderme adımlarını izleyin:

1. En son verileri görüntülediğinizden emin olmak için raporu açın ve sayfayı yenileyin. Raporda verileri yenileme seçeneği vardır. En son verileri almak için bunu seçin.

1. Sağladığınız depolama hesabı adı ve erişim anahtarının doğru olduğundan emin olun. Depolama hesabı adı, arama hizmeti günlüklerinizle yapılandırılan hesaba karşılık gelir.

1. Depolama hesabınızın **kapsayıcılar-günlükler-işlem günlükleri** ve **öngörüler-ölçümler-pt1m** içerdiğini ve her kapsayıcının veriye sahip olduğunu doğrulayın. Günlükler ve ölçümler birkaç klasör katmanı içinde olacaktır.

1. Veri kümesinin hala yenileniyor olup olmadığını kontrol edin. Yenileme durumu göstergesi yukarıdaki adım 8'de gösterilir. Hala yenileniyorsa, raporu açmak ve yenilemek için yenileme tamamlanana kadar bekleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Bilişsel Arama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/search/)

[Power BI nedir?](https://docs.microsoft.com/power-bi/fundamentals/power-bi-overview)

[Power BI hizmetinde tasarımcılara yönelik temel kavramlar](https://docs.microsoft.com/power-bi/service-basic-concepts)