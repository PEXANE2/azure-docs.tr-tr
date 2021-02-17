---
title: Ölçüm uyarılarını Yapılandırma-Azure portal-MySQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal MySQL için Azure veritabanı esnek sunucusu için ölçüm uyarılarının nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: 632aae766e6fd1328dc6e0135a88a942d7ad0910
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100595795"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql---flexible-server"></a>MySQL için Azure veritabanı-esnek sunucu ölçümleri hakkında uyarı ayarlamak için Azure portal kullanın 

> [!IMPORTANT] 
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı uyarılarını ayarlama konusu gösterilmektedir. Azure hizmetleriniz için izleme ölçümlerini temel alan bir uyarı alabilirsiniz.

Uyarı, belirtilen bir ölçümün değeri atadığınız bir eşiği aştığında tetiklenir. Uyarı hem koşul ilk karşılandığında hem de daha sonra bu koşul karşılanamadığında daha sonra tetiklenir. Ölçüm uyarıları durum bilgisi olur, diğer bir deyişle, yalnızca durum değiştiğinde bildirimleri gönderir.

Bir uyarıyı, tetiklendiğinde aşağıdaki eylemleri yapmak üzere yapılandırabilirsiniz:
* Hizmet yöneticisine ve ortak yöneticilere e-posta bildirimleri gönderme
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Web kancası çağırma

Kullanarak uyarı kuralları hakkında bilgi alabilir ve bunları alabilirsiniz:
* [Azure portalı](../../azure-monitor/alerts/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../../azure-monitor/alerts/alerts-metric.md#with-azure-cli)
* [Azure İzleyici REST API'si](/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Azure portaldan bir ölçüm hakkında uyarı kuralı oluşturma
1. [Azure Portal](https://portal.azure.com/), Izlemek istediğiniz MySQL Için Azure veritabanı esnek sunucusu ' nu seçin.
2. Kenar çubuğunun **izleme** bölümü altında **Uyarılar**' ı seçin.
3. **+ Yeni uyarı kuralı**'nı seçin.
4. **Kural oluşturma** sayfası açılır. Gereken bilgileri doldurun:
5. **Koşul** bölümünde **Koşul Seç**' i seçin.
6. Desteklenen sinyallerin bir listesini göreceksiniz, üzerinde uyarı oluşturmak istediğiniz ölçümü seçin. Örneğin, "depolama alanı yüzdesi" ni seçin.
7. En son altı saat için ölçüm için bir grafik görürsünüz. Ölçüm için daha uzun geçmişi görmeyi seçmek için **grafik dönemi** açılan listesini kullanın.
8. **Eşik** türünü seçin (örn. "Static" veya "Dynamic"), **işleç** (örn. "Büyüktür") ve **toplama türü** (örn. Ortalama). Bu, ölçüm uyarı kuralının değerlendileceğini belirten mantığı belirleyecek.
    - **Statik** eşik kullanıyorsanız, bir **eşik değeri** (örn.) tanımlamaya devam edin. yüzde 85). Ölçüm grafiği ne kadar makul bir eşik olabileceğini belirlemenize yardımcı olabilir.
    - **Dinamik** eşik kullanıyorsanız, **eşik duyarlılığını** tanımlamaya devam edin. Ölçüm grafiği, hesaplanan eşikleri son verilere göre görüntüler. [Dinamik eşikler durum türü ve duyarlılık seçenekleri hakkında daha fazla bilgi edinin](../../azure-monitor/alerts/alerts-dynamic-thresholds.md).
9. Toplama türü işlevi (örn.) kullanılarak veri noktalarının gruplandırılacağı **toplama ayrıntı düzeyi (süre)** aralığını ayarlayarak koşulu daraltın. "30 dakika") ve **Sıklık** (her 15 dakikada bir).
10. **Bitti**’ye tıklayın.
11. Bir eylem grubu ekleyin. Bir eylem grubu, bir Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri koleksiyonudur. **Eylem grupları** bölümünde, uyarı kuralına iliştirilecek zaten varolan bir eylem grubunu seçmek için **Eylem grubunu Seç** ' i seçin.
12. Uyarı üzerinde bildirim almak için yeni bir eylem grubu da oluşturabilirsiniz. Daha fazla bilgi için [eylem grubu oluşturma ve yönetme](../../azure-monitor/alerts/action-groups.md) bölümüne bakın.
13. Yeni bir eylem grubu oluşturmak için **+ eylem grubu oluştur**' u seçin. "Eylem grubu oluştur" formunu bir **abonelik**, **kaynak grubu**, **eylem grubu adı** ve **görünen ad** ile doldurun.
14. Eylem grubu için **bildirimleri** yapılandırın.
    
    **Bildirim türü**' nde, bildirim almak Için abonelik sahipleri, katkıda bulunanlar ve okuyucular ' ı seçmek üzere "e-posta Azure Resource Manager rolü" ni seçin E-postayı göndermek için **Azure Resource Manager rolünü** seçin.
    Ayrıca, belirli alıcılara bildirim göndermek için **e-posta/SMS iletisi/gönderim/ses** ' i de seçebilirsiniz.

    Bildirim türüne **ad** verin ve tamamlandığında, **gözden geçir + oluştur** ' u seçin.

    <!--:::image type="content" source="./media/howto-alert-on-metric/10-action-group-type.png" alt-text="Action group":::-->
    
15. Uyarı kuralı **adı**, **Açıklama**, uyarı **kuralını kaynak grubuna kaydet** ve **önem derecesi** gibi **Uyarı kuralı ayrıntılarını** girin.

    <!--:::image type="content" source="./media/howto-alert-on-metric/11-name-description-severity.png" alt-text="Action group":::-->

16. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' u seçin.
    Birkaç dakika içinde, uyarı etkin ve daha önce açıklandığı gibi tetikler.
## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturduktan sonra, bunu seçebilir ve aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Ölçüm eşiğini ve Bu uyarıyla ilgili olan önceki günün gerçek değerlerini gösteren bir grafik görüntüleyin.
* Uyarı kuralını **düzenleyin** veya **silin** .
* Bildirimleri almayı geçici olarak durdurmak veya yeniden başlatmak istiyorsanız, uyarıyı **devre dışı bırakın** veya **etkinleştirin** .


## <a name="next-steps"></a>Sonraki adımlar
- [Ölçümler üzerinde uyarı ayarlama](../../azure-monitor/alerts/alerts-metric.md)hakkında daha fazla bilgi edinin.
- [MySQL Için Azure veritabanı esnek sunucusu 'nda kullanılabilen ölçümler](./concepts-monitoring.md)hakkında daha fazla bilgi edinin.
- [Azure İzleyici'de ölçüm uyarılarının nasıl çalıştığını anlama](../../azure-monitor/alerts/alerts-metric-overview.md)