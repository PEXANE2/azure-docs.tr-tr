---
title: Ölçüm uyarılarını Yapılandırma-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal 'ten MySQL için Azure veritabanı 'nın ölçüm uyarılarını yapılandırma ve erişme açıklanır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: 81003be986524b94e9c4487cd0d8606540445d16
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94542091"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı ölçümleri hakkında uyarı ayarlamak için Azure portal kullanın 

Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı uyarılarını ayarlama konusu gösterilmektedir. Azure hizmetleriniz için izleme ölçümlerini temel alan bir uyarı alabilirsiniz.

Uyarı, belirtilen bir ölçümün değeri atadığınız bir eşiği aştığında tetiklenir. Uyarı hem koşul ilk karşılandığında hem de daha sonra bu koşul karşılanamadığında daha sonra tetiklenir. 

Bir uyarıyı, tetiklendiğinde aşağıdaki eylemleri yapmak üzere yapılandırabilirsiniz:
* Hizmet yöneticisine ve ortak yöneticilere e-posta bildirimleri gönderme
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Web kancası çağırma

Kullanarak uyarı kuralları hakkında bilgi alabilir ve bunları alabilirsiniz:
* [Azure Portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure İzleyici REST API'si](/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Azure portal bir ölçümde uyarı kuralı oluşturma
1. [Azure Portal](https://portal.azure.com/), Izlemek istediğiniz MySQL Için Azure veritabanı sunucusunu seçin.

2. Kenar çubuğunun **izleme** bölümü altında gösterildiği gibi **Uyarılar** ' ı seçin:

   :::image type="content" source="./media/howto-alert-on-metric/2-alert-rules.png" alt-text="Uyarı kurallarını seçin":::

3. **Ölçüm uyarısı Ekle** (+ simgesi) seçeneğini belirleyin.

4. **Kural oluştur** sayfası aşağıda gösterildiği gibi açılır. Gereken bilgileri doldurun:

   :::image type="content" source="./media/howto-alert-on-metric/4-add-rule-form.png" alt-text="Ölçüm uyarısı formu Ekle":::

5. **Koşul** bölümünde **Koşul Ekle** ' yi seçin.

6. Uyarı almak için sinyaller listesinden bir ölçüm seçin. Bu örnekte, "depolama alanı yüzdesi" ni seçin.
   
   :::image type="content" source="./media/howto-alert-on-metric/6-configure-signal-logic.png" alt-text="Ölçüm seçin":::

7. **Koşul** (örn.) gibi uyarı mantığını yapılandırın. "Büyüktür"), **eşik** (örn. yüzde 85), **zaman toplama** , ölçüm kuralının uyarı tetikleyiciden önce karşılanması **gereken süre (** örn. "Son 30 dakika içinde") ve **Sıklık**.
   
   Tamamlandığında **bitti** ' yi seçin.

   :::image type="content" source="./media/howto-alert-on-metric/7-set-threshold-time.png" alt-text="Ölçüm 2 ' yi seçin":::

8. Uyarı üzerinde bildirim almak üzere yeni bir grup oluşturmak için **eylem grupları** bölümünde **Yeni oluştur** ' u seçin.

9. "Eylem grubu Ekle" formunu bir ad, kısa ad, abonelik ve kaynak grubuyla doldurun.

10. **E-posta/SMS/Push/Voice** eylem türünü yapılandırın.
    
    Bildirimleri almak için abonelik sahipleri, katkıda bulunanlar ve okuyucular seçmek üzere "e-posta Azure Resource Manager rolü" nü seçin.
   
    İsteğe bağlı olarak, uyarı tetiklendiğinde çağırılabilmesi istiyorsanız **Web kancası** alanında GEÇERLI bir URI sağlayın.

    Tamamlandığında **Tamam ' ı** seçin.

    :::image type="content" source="./media/howto-alert-on-metric/10-action-group-type.png" alt-text="Eylem grubu":::

11. Bir uyarı kuralı adı, açıklaması ve önem derecesi belirtin.

    :::image type="content" source="./media/howto-alert-on-metric/11-name-description-severity.png" alt-text="Eylem grubu 2"::: 

12. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' u seçin.

    Birkaç dakika içinde, uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturduktan sonra, bunu seçebilir ve aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Ölçüm eşiğini ve Bu uyarıyla ilgili olan önceki günün gerçek değerlerini gösteren bir grafik görüntüleyin.
* Uyarı kuralını **düzenleyin** veya **silin** .
* Bildirimleri almayı geçici olarak durdurmak veya yeniden başlatmak istiyorsanız, uyarıyı **devre dışı bırakın** veya **etkinleştirin** .


## <a name="next-steps"></a>Sonraki adımlar
* [Uyarılarda Web kancalarını yapılandırma](../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
* Hizmetinizin kullanılabilir olduğundan ve yanıt verebilmesini sağlamak için [ölçüm koleksiyonuna genel bakış](../azure-monitor/platform/data-platform.md) alın.