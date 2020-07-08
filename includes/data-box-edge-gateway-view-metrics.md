---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 9ac7966538102273b91d6b7f15b90e18ceedd421
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83779928"
---
Ayrıca, cihazın performansını izlemek için ölçümleri ve cihaz sorunlarını gidermeye yönelik bazı örnekleri görüntüleyebilirsiniz.

Seçili cihaz ölçümleri için bir grafik oluşturmak üzere Azure portal aşağıdaki adımları uygulayın.

1. Azure portal kaynağınız için **izleme > ölçümleri** ' ne gidin ve **ölçüm Ekle**' yi seçin.

    ![Ölçüm ekleme](media/data-box-edge-gateway-view-metrics/view-metrics-1.png)

2. Kaynak otomatik olarak doldurulur.  

    ![Geçerli kaynak](media/data-box-edge-gateway-view-metrics/view-metrics-2.png)

    Başka bir kaynak belirtmek için kaynağı seçin. **Kaynak Seç** dikey penceresinde, ölçümü göstermek istediğiniz aboneliği, kaynak grubunu, kaynak türünü ve belirli kaynağı seçin ve **Uygula**' yı seçin.

    ![Başka bir kaynak seçin](media/data-box-edge-gateway-view-metrics/view-metrics-3.png)

3. Açılan listeden, cihazınızı izlemek için bir ölçüm seçin. Bu ölçümlerin tam listesi için bkz. [cihazınızdaki ölçümler](#metrics-on-your-device).

4. Açılan listeden bir ölçüm seçildiğinde, toplama de tanımlanabilir. Toplama, belirli bir zaman aralığında toplanan gerçek değeri ifade eder. Toplanmış değerler ortalama, en düşük veya en büyük değer olabilir. AVG, Max veya min toplamını seçin.

    ![Grafiği görüntüle](media/data-box-edge-gateway-view-metrics/view-metrics-4.png)

5. Seçtiğiniz metriğin birden çok örneği varsa, bölme seçeneği kullanılabilir. **Bölmeyi Uygula** ' yı seçin ve ardından dökümü görmek istediğiniz değeri seçin.

    ![Bölmeyi Uygula](media/data-box-edge-gateway-view-metrics/view-metrics-5.png)

6. Artık dökümü yalnızca birkaç örnek için görmek istiyorsanız, verileri filtreleyebilirsiniz. Örneğin, bu durumda, yalnızca cihazınızdaki iki bağlı ağ arabirimi için ağ aktarım hızını görmek istiyorsanız, bu arabirimlere filtre uygulanabilir. **Filtre Ekle** ' yi seçin ve filtreleme için ağ arabirimi adını belirtin.

    ![Filtre ekle](media/data-box-edge-gateway-view-metrics/view-metrics-6.png)

7. Ayrıca, kolay erişim için grafiği panoya sabitleyebilirsiniz.

    ![Panoya sabitle](media/data-box-edge-gateway-view-metrics/view-metrics-7.png)

8. Grafik verilerini bir Excel elektronik tablosuna aktarmak veya paylaşabileceğiniz grafiğin bir bağlantısını almak için komut çubuğundan Share (paylaşma) seçeneğini belirleyin.

    ![Verileri dışarı aktarma](media/data-box-edge-gateway-view-metrics/view-metrics-8.png)
