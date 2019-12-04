---
title: Ölçüm uyarılarını Yapılandırma-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal 'ten MySQL için Azure veritabanı 'nın ölçüm uyarılarını yapılandırma ve erişme açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 487ca76115b93020a762cfa2afb5fd5a3be40a66
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770671"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı ölçümleri hakkında uyarı ayarlamak için Azure portal kullanın 

Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı uyarılarını ayarlama konusu gösterilmektedir. Azure hizmetleriniz için izleme ölçümlerini temel alan bir uyarı alabilirsiniz.

Uyarı, belirtilen bir ölçümün değeri atadığınız bir eşiği aştığında tetiklenir. Uyarı hem koşul ilk karşılandığında hem de daha sonra bu koşul karşılanamadığında daha sonra tetiklenir. 

Bir uyarıyı, tetiklendiğinde aşağıdaki eylemleri yapmak üzere yapılandırabilirsiniz:
* hizmet yöneticisine ve ortak yöneticilere e-posta bildirimleri gönderme
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Web kancası çağırma

Kullanarak uyarı kuralları hakkında bilgi alabilir ve bunları alabilirsiniz:
* [Azure portalda](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure Izleyici REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Azure portal bir ölçümde uyarı kuralı oluşturma
1. [Azure Portal](https://portal.azure.com/), Izlemek istediğiniz MySQL Için Azure veritabanı sunucusunu seçin.

2. Kenar çubuğunun **izleme** bölümü altında gösterildiği gibi **Uyarılar** ' ı seçin:

   ![Uyarı kurallarını seçin](./media/howto-alert-on-metric/2-alert-rules.png)

3. **Ölçüm uyarısı Ekle** (+ simgesi) seçeneğini belirleyin.

4. **Kural oluştur** sayfası aşağıda gösterildiği gibi açılır. Gerekli bilgileri girin:

   ![Ölçüm uyarısı formu Ekle](./media/howto-alert-on-metric/4-add-rule-form.png)

5. **Koşul** bölümünde **Koşul Ekle**' yi seçin.

6. Uyarı almak için sinyaller listesinden bir ölçüm seçin. Bu örnekte, "depolama alanı yüzdesi" ni seçin.
   
   ![Ölçüm seçin](./media/howto-alert-on-metric/6-configure-signal-logic.png)

7. **Koşul** (örn.) gibi uyarı mantığını yapılandırın. "Büyüktür"), **eşik** (örn. yüzde 85), **zaman toplama**, ölçüm kuralının uyarı tetikleyiciden önce karşılanması **gereken süre (** örn. "Son 30 dakika içinde") ve **Sıklık**.
   
   Tamamlandığında **bitti** ' yi seçin.

   ![Ölçüm seçin](./media/howto-alert-on-metric/7-set-threshold-time.png)

8. Uyarı üzerinde bildirim almak üzere yeni bir grup oluşturmak için **eylem grupları** bölümünde **Yeni oluştur** ' u seçin.

9. "Eylem grubu Ekle" formunu bir ad, kısa ad, abonelik ve kaynak grubuyla doldurun.

10. **E-posta/SMS/Push/Voice** eylem türünü yapılandırın.
    
    Bildirimleri almak için abonelik sahipleri, katkıda bulunanlar ve okuyucular seçmek üzere "e-posta Azure Resource Manager rolü" nü seçin.
   
    İsteğe bağlı olarak, uyarı tetiklendiğinde çağırılabilmesi istiyorsanız **Web kancası** alanında GEÇERLI bir URI sağlayın.

    Tamamlandığında **Tamam ' ı** seçin.

    ![Eylem grubu](./media/howto-alert-on-metric/10-action-group-type.png)

11. Bir uyarı kuralı adı, açıklaması ve önem derecesi belirtin.

    ![Eylem grubu](./media/howto-alert-on-metric/11-name-description-severity.png) 

12. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' u seçin.

    Birkaç dakika içinde, uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetin
Bir uyarı oluşturduktan sonra, bunu seçebilir ve aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Ölçüm eşiğini ve Bu uyarıyla ilgili olan önceki günün gerçek değerlerini gösteren bir grafik görüntüleyin.
* Uyarı kuralını **düzenleyin** veya **silin** .
* Bildirimleri almayı geçici olarak durdurmak veya yeniden başlatmak istiyorsanız, uyarıyı **devre dışı bırakın** veya **etkinleştirin** .


## <a name="next-steps"></a>Sonraki adımlar
* [Uyarılarda Web kancalarını yapılandırma](../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
* Hizmetinizin kullanılabilir olduğundan ve yanıt verebilmesini sağlamak için [ölçüm koleksiyonuna genel bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) alın.
