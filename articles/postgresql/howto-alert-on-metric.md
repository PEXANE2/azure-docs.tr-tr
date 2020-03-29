---
title: Uyarıları yapılandırma - Azure portalı - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, Azure portalından PostgreSQL - Single Server için Azure Veritabanı için metrik uyarıların nasıl yapılandırılabildiğini ve bunlara erişilenler açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: fe099dcb49d176d27466c08749a5873904d1ae2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74766846"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı ölçümlerinde uyarılar ayarlamak için Azure portalını kullanın - Tek Sunucu

Bu makalede, Azure portalını kullanarak PostgreSQL uyarıları için Azure Veritabanı'nı nasıl ayarlayabileceğiniz gösterilmektedir. Azure hizmetleriniz için izleme ölçümlerine dayalı bir uyarı alabilirsiniz.

Belirli bir metnin değeri atadığınız bir eşiği geçtiğinde uyarı tetikler. Uyarı, hem koşul ilk karşılaştığında hem de daha sonra bu koşul artık karşılanmadığında tetikler. 

Bir uyarıyı tetiklediğinde aşağıdaki eylemleri yapmak üzere yapılandırabilirsiniz:
* Hizmet yöneticisine ve yardımcı yöneticilere e-posta bildirimleri gönderin.
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Bir webhook çağırın.

Şu bilgileri kullanarak uyarı kurallarını yapılandırabilir ve bilgi alabilirsiniz:
* [Azure portalında](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure İzleyici REST API'si](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Azure portalından bir metrik te uyarı kuralı oluşturma
1. Azure [portalında,](https://portal.azure.com/)izlemek istediğiniz PostgreSQL sunucusu için Azure Veritabanı'nı seçin.

2. Kenar çubuğunun **İzleme** bölümü altında, gösterildiği gibi **Uyarılar'ı** seçin:

   ![Uyarı Kurallarını Seçin](./media/howto-alert-on-metric/2-alert-rules.png)

3. **Metrik uyarı ekle** (+ simgesi) seçeneğini belirleyin.

4. **Oluştur kuralı** sayfası aşağıda gösterildiği gibi açılır. Gerekli bilgileri doldurun:

   ![Metrik uyarı formu ekleme](./media/howto-alert-on-metric/4-add-rule-form.png)

5. **Koşul** bölümünde, **Koşul Ekle'yi**seçin.

6. Uyarılacak sinyaller listesinden bir metrik seçin. Bu örnekte, "Depolama yüzdesi"ni seçin.
   
   ![Ölçüm seçin](./media/howto-alert-on-metric/6-configure-signal-logic.png)

7. **Koşul** (ör. "Büyük"), **Eşik** (ör. Yüzde 85), **Zaman Toplama**, **Zaman zaman** metrik kural uyarı tetikler önce memnun edilmelidir (ör. "Son 30 dakika içinde") ve **Frekans**.
   
   Tamamlandığında **Bitti'yi** seçin.

   ![Ölçüm seçin](./media/howto-alert-on-metric/7-set-threshold-time.png)

8. Eylem **Grupları** bölümünde, uyarıda bildirim almak için yeni bir grup oluşturmak için **Yeni** Oluştur'u'nu seçin.

9. "Eylem grubu ekle" formunu bir ad, kısa ad, abonelik ve kaynak grubuyla doldurun.

10. **E-posta/SMS/Push/Voice** eylem türünü yapılandırın.
    
    Bildirim almak için abonelik Sahiplerini, Katkıda Bulunanları ve Okuyucuları seçmek için "Azure Kaynak Yöneticisi Rolü'nü e-posta yla gönder"i seçin.
   
    İsteğe bağlı olarak, uyarı çıktığında çağrılsın istiyorsanız **Webhook** alanında geçerli bir URI sağlayın.

    Tamamlandığında **Tamam'ı** seçin.

    ![Eylem grubu](./media/howto-alert-on-metric/10-action-group-type.png)

11. Uyarı kural adı, Açıklama ve Önem belirtin.

    ![Eylem grubu](./media/howto-alert-on-metric/11-name-description-severity.png) 

12. Uyarıyı oluşturmak için **uyarı oluştur kuralını** seçin.

    Birkaç dakika içinde, uyarı etkin dir ve daha önce açıklandığı gibi tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturduktan sonra, uyarıyı seçebilir ve aşağıdaki eylemleri yapabilirsiniz:

* Bu uyarıyla ilgili önceki güne ait metrik eşiği ve gerçek değerleri gösteren bir grafiği görüntüleyin.
* Uyarı kuralını **düzenle** veya **sil.**
* Bildirimleri geçici olarak durdurmak veya bildirim almaya devam etmek istiyorsanız, uyarıyı **devre dışı bırakın** veya **etkinleştirin.**

## <a name="next-steps"></a>Sonraki adımlar
* [Uyarılardaki web hook'ları yapılandırma](../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
* Hizmetinizin kullanılabilir ve yanıt veren olduğundan emin olmak için [metrik koleksiyonuna genel bir bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) alın.
