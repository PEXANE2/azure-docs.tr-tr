---
title: Azure Bilişsel Arama günlüklerini ve ölçümlerini Power BI ile görselleştirin
description: Azure Bilişsel Arama günlüklerini ve ölçümlerini Power BI ile görselleştirin
manager: eladz
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: e01ac332e61f51909ff1617f1716cd719b67c319
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82127881"
---
# <a name="visualize-azure-cognitive-search-logs-and-metrics-with-power-bi"></a>Azure Bilişsel Arama günlüklerini ve ölçümlerini Power BI ile görselleştirin
[Azure bilişsel arama](https://docs.microsoft.com/azure/search/search-what-is-azure-search) , arama hizmetinize ilişkin işlem günlüklerini ve hizmet ölçümlerini bir Azure depolama hesabında depolamanıza olanak tanır. Bu sayfa, bu bilgileri bir Power BI şablonu uygulaması aracılığıyla görselleştirmenize ilişkin yönergeler sağlar. Uygulama arama, dizin oluşturma, Işlemler ve hizmet ölçümleri hakkında bilgiler de dahil olmak üzere arama hizmetiniz hakkında ayrıntılı öngörüler sağlar.

Azure Bilişsel Arama Power BI şablon uygulamasını bulabilirsiniz: [Power BI Apps marketi](https://appsource.microsoft.com/marketplace/apps)'nde **günlükleri ve ölçümleri analiz edin** .

## <a name="how-to-get-started-with-the-app"></a>Uygulama ile çalışmaya başlama

1. Arama hizmetiniz için ölçüm ve kaynak günlüğünü etkinleştirme:

    1. Günlükleri arşivleyebileceğiniz mevcut bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) oluşturun veya bunu yapın
    1. Azure portal Azure Bilişsel Arama hizmetinize gidin
    1. Sol sütundaki Izleme bölümü altında **Tanılama ayarları** ' nı seçin.

        ![](media/search-monitor-logs-powerbi/diagnostic-settings.png)

    1. **+ Tanılama ayarı Ekle** ' yi seçin
    1. **Arşivi bir depolama hesabına**çekin, depolama hesabı bilgilerinizi sağlayın ve **Operationlogs** ve **allölçümlerini** denetleyin

        ![](media/search-monitor-logs-powerbi/add-diagnostic-setting.png)
    1. **Kaydet** 'i seçin

1. Günlüğe kaydetme etkinleştirildikten sonra, günlükleri ve ölçümleri oluşturmaya başlamak için arama hizmetinizi kullanın. Kapsayıcıların bu günlüklere sahip blob depolamada görünmesi bir saat kadar sürer. Arama trafiği günlükleri için bir **Öngörüler-logs-operationlogs** kapsayıcısı ve ölçümler için bir **Öngörüler-ölçümler-pt1m** kapsayıcısı görürsünüz.

1. Azure Bilişsel Arama Power BI uygulamasını [Power BI Apps Market](https://appsource.microsoft.com/marketplace/apps) 'te bulun ve yeni bir çalışma alanına veya mevcut bir çalışma alanına yüklemeyin. Uygulamaya **Azure bilişsel arama adı verilir: günlükleri ve ölçümleri analiz edin**.

1. Uygulamayı yükledikten sonra Power BI uygulama listenizden uygulamayı seçin.

    ![](media/search-monitor-logs-powerbi/azure-search-app-tile.png)

1. Verilerinizi bağlamak için **Bağlan** ' ı seçin

    ![](media/search-monitor-logs-powerbi/get-started-with-your-new-app.png)

1. Günlüklerinizi ve ölçümlerinizi içeren depolama hesabının adını girin. Varsayılan olarak, uygulama son 10 güne kadar veri arayacaktır, ancak bu değer **günler** parametresiyle değiştirilebilir.

    ![](media/search-monitor-logs-powerbi/connect-to-storage-account.png)

1. Kimlik doğrulama yöntemi olarak **anahtar** ' ı seçin ve depolama hesabı anahtarınızı sağlayın. Gizlilik düzeyi olarak **özel** ' i seçin. Oturum Aç'a tıklayın ve yükleme işlemine başlayın.

    ![](media/search-monitor-logs-powerbi/connect-to-storage-account-step-two.png)

1. Verilerin yenilenmesini bekleyin. Bu işlem, sahip olduğunuz veri miktarına bağlı olarak biraz zaman alabilir. Verilerin yine de aşağıdaki göstergeye göre yenilenip yenilenmediğini görebilirsiniz.

    ![](media/search-monitor-logs-powerbi/workspace-view-refreshing.png)

1. Veri yenileme işlemi tamamlandıktan sonra raporu görüntülemek için **Azure bilişsel arama raporu** ' nu seçin.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-report.png)

1. Verilerle birlikte açılacak şekilde raporu açtıktan sonra sayfayı yenilediğinizden emin olun.

    ![](media/search-monitor-logs-powerbi/powerbi-search.png)

## <a name="how-to-change-the-app-parameters"></a>Uygulama parametrelerini değiştirme
Farklı bir depolama hesabındaki verileri görselleştirmek veya verilerin sorgulanme gün sayısını değiştirmek isterseniz, **gün** ve **storageaccount** parametrelerini değiştirmek için aşağıdaki adımları izleyin.

1. Power BI uygulamalarınıza giderek Azure Bilişsel Arama uygulamanızı bulun ve **uygulamayı Düzenle** düğmesini seçerek çalışma alanını görüntüleyin.

    ![](media/search-monitor-logs-powerbi/azure-search-app-tile-edit.png)

1. Veri kümesi seçeneklerindeki **ayarları** seçin.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-settings.png)

1. Veri kümeleri sekmesinden parametre değerlerini değiştirin ve **Uygula**' yı seçin. Bağlantıyla ilgili bir sorun varsa, veri kaynağı kimlik bilgilerini aynı sayfada güncelleştirin.

1. Çalışma alanına geri gidin ve veri kümesi seçeneklerinde **Şimdi Yenile** ' yi seçin.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-refresh-now.png)

1. Güncelleştirilmiş verileri görüntülemek için raporu açın. Ayrıca, en son verileri görüntülemek için raporu yenilemeniz gerekebilir.

## <a name="troubleshooting"></a>Sorun giderme
Verilerinizi göremiyorsanız, bu sorun giderme adımlarını izleyin:

1. Raporu açın ve en son verileri görüntülediğinizden emin olmak için sayfayı yenileyin. Raporda verileri yenilemek için bir seçenek vardır. En son verileri almak için bunu seçin.

1. Girdiğiniz depolama hesabı adı ve erişim anahtarının doğru olduğundan emin olun. Depolama hesabı adı, arama hizmeti günlüklerinizin yapılandırıldığı hesaba karşılık gelmelidir.

1. Depolama hesabınızın, içgörüler- **Günlükler-operationlogs** ve **Insights-ölçümler-pt1m** ve her kapsayıcının veri içerdiğini doğrulayın. Günlükler ve ölçümler, birkaç klasör katmanında yer alacak.

1. Veri kümesinin yenileme işlemi devam ettiğinden emin olun. Yenileme durumu göstergesi yukarıdaki 8. adımda gösterilmiştir. Yenileme işlemi devam ediyorsa, raporu açmak ve yenilemek için yenileme tamamlanana kadar bekleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Bilişsel Arama hakkında daha fazla bilgi](https://docs.microsoft.com/azure/search/)

[Power BI nedir?](https://docs.microsoft.com/power-bi/fundamentals/power-bi-overview)

[Power BI hizmetinde tasarımcılara yönelik temel kavramlar](https://docs.microsoft.com/power-bi/service-basic-concepts)