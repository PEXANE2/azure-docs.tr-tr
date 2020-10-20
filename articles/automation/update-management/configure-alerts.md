---
title: Azure Otomasyonu Güncelleştirme Yönetimi için uyarı oluşturma
description: Bu makalede, Azure uyarılarını güncelleştirme değerlendirmelerinin veya dağıtımların durumu hakkında bilgilendirmek üzere nasıl yapılandıracağınız açıklanır.
services: automation
ms.subservice: update-management
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: 150269dd2cc6a25a507f03c7d6b47544a89da12a
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223004"
---
# <a name="how-to-create-alerts-for-update-management"></a>Güncelleştirme Yönetimi için uyarı oluşturma

Azure 'daki uyarılar, runbook işlerinin, hizmet durumu sorunlarının veya Otomasyon hesabınızla ilgili diğer senaryolardan sonuçları size bildirir. Azure Otomasyonu önceden yapılandırılmış uyarı kuralları içermez, ancak oluşturduğu verilere bağlı olarak kendi kendinize de oluşturabilirsiniz. Bu makale, Güncelleştirme Yönetimi dahil edilen ölçümleri kullanarak uyarı kuralları oluşturmaya yönelik rehberlik sağlar.

## <a name="available-metrics"></a>Mevcut ölçümler

Azure Otomasyonu, toplanan ve Azure Izleyici 'ye iletilen Güncelleştirme Yönetimi ilgili iki farklı platform ölçümü oluşturur. Bu ölçüm, [Ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md) kullanılarak analiz için ve bir [ölçüm uyarı kuralı](../../azure-monitor/platform/alerts-metric.md)kullanılarak uyarı vermek için kullanılabilir.

Uygulanan iki ölçüm şunlardır:

* Toplam güncelleştirme dağıtım makinesi çalıştırması
* Toplam güncelleştirme dağıtımı çalıştırmaları

Uyarılar için kullanıldığında her iki ölçüm de uyarıyı belirli bir güncelleştirme dağıtımı ayrıntısına kapsama yardımcı olmak üzere ek bilgiler taşıyan boyutları destekler. Aşağıdaki tabloda, bir uyarı yapılandırılırken kullanılabilen ölçüm ve boyutlar hakkındaki ayrıntılar gösterilmektedir.

|Sinyal adı|Boyutlar|Açıklama
|---|---|---|
|`Total Update Deployment Runs`|-Güncelleştirme dağıtım adı<br>-Durum | Bir güncelleştirme dağıtımının genel durumu hakkında uyarılar.|
|`Total Update Deployment Machine Runs`|-Güncelleştirme dağıtım adı</br>-Durum</br>-Hedef bilgisayar</br>-Güncelleştirme dağıtımı çalıştırma KIMLIĞI    |Belirli makinelere hedeflenmiş bir güncelleştirme dağıtımının durumu hakkında uyarılar.|

## <a name="create-alert"></a>Uyarı oluşturma

Güncelleştirme dağıtımının durumunu bilmenizi sağlamak üzere uyarıları ayarlamak için aşağıdaki adımları izleyin. Azure uyarılarınız hakkında daha fazla bilgi için bkz. [Azure Alerts 'e genel bakış](../../azure-monitor/platform/alerts-overview.md).

1. Otomasyon hesabınızda, **izleme**altında **Uyarılar** ' ı seçin ve ardından **Yeni uyarı kuralı**' nı seçin.

2. **Uyarı kuralı oluştur** sayfasında, Otomasyon hesabınız kaynak olarak zaten seçilidir. Değiştirmek isterseniz, **Kaynağı Düzenle**' yi seçin.

3. Kaynak seçin sayfasında, **kaynak türüne göre filtrele** açılan listesinden **Otomasyon hesapları** ' nı seçin.

4. Kullanmak istediğiniz Otomasyon hesabını seçin ve **bitti**' yi seçin.

5. Gereksinime uygun olan sinyali seçmek için **Koşul Ekle** ' yi seçin.

6. Bir boyut için listeden geçerli bir değer seçin. İstediğiniz değer listede yoksa, boyutun yanındaki ' ı seçin **\+** ve özel adı yazın. Ardından aranacak değeri seçin. Bir boyutun tüm değerlerini seçmek istiyorsanız, **Seç \* ** düğmesini seçin. Boyut için bir değer seçmezseniz Güncelleştirme Yönetimi bu boyutu yoksayar.

    ![Sinyal mantığını yapılandırma](./media/manage-updates-for-vm/signal-logic.png)

7. **Uyarı mantığı**altında, **zaman toplama** ve **eşik** alanlarına değerler girip **bitti**' yi seçin.

8. Sonraki sayfada, uyarı için bir ad ve açıklama girin.

9. Başarısız bir çalıştırma için başarılı bir çalıştırma veya **bilgilendirme (sev 1)** Için **önem derecesi** alanını **bilgilendirici (sev 2)** olarak ayarlayın.

    ![Ekran görüntüsü, uyarı kuralı adı, açıklaması ve önem derecesi alanları vurgulanmış şekilde uyarı ayrıntılarını tanımla bölümünü gösterir.](./media/manage-updates-for-vm/define-alert-details.png)

10. Uyarı kuralını etkinleştirmek için **Evet** ' i seçin.

## <a name="configure-action-groups-for-your-alerts"></a>Uyarılarınız için eylem gruplarını yapılandırma

Uyarılarınız yapılandırıldıktan sonra, birden çok uyarı arasında kullanılacak bir eylem grubu olan bir eylem grubu ayarlayabilirsiniz. Eylemler e-posta bildirimleri, runbook 'lar, Web kancaları ve çok daha fazlasını içerebilir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../../azure-monitor/platform/action-groups.md).

1. Bir uyarı seçin ve ardından **Yeni oluştur** **eylem grupları**' nı seçin.

2. Eylem grubu için bir tam ad ve kısa bir ad girin. Güncelleştirme Yönetimi, belirtilen grubu kullanarak bildirim gönderirken kısa adı kullanır.

3. **Eylemler**' in altında, eylemi belirten bir ad girin, örneğin **e-posta bildirimi**.

4. **Eylem türü**için uygun türü seçin (örneğin, **e-posta/SMS/Push/Voice**).

5. **Ayrıntıları düzenle** seçeneğini belirleyin.

6. Eylem türü için bölmeyi girin. Örneğin, **e-posta/SMS/Push/seskullanacaksanız**, bir eylem adı girin, **e-posta** kutusunu seçin, geçerli bir e-posta adresi girin ve **Tamam**' ı seçin.

    ![E-posta eylem grubu yapılandırma](./media/manage-updates-for-vm/configure-email-action-group.png)

7. Eylem grubu ekle bölmesinde **Tamam**'ı seçin.

8. Bir uyarı e-postası için e-posta konusunu özelleştirebilirsiniz. **Kural oluştur**altında **eylemleri Özelleştir** ' i seçin ve **e-posta konusu**' ı seçin

9. İşleminiz bittiğinde **Uyarı kuralı oluştur**'u seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici 'de uyarılar](../../azure-monitor/platform/alerts-overview.md)hakkında daha fazla bilgi edinin.

* Log Analytics çalışma alanından verileri almak ve analiz etmek için [günlük sorguları](../../azure-monitor/log-query/log-query-overview.md) hakkında bilgi edinin.

* [Azure Izleyici günlükleri ile kullanımı ve maliyetleri](../../azure-monitor/platform/manage-cost-storage.md) yönetme, veri saklama dönemini değiştirerek maliyetlerinizi nasıl denetleyeceğinizi ve veri kullanımınızın nasıl analiz edileceğini ve uyarılanacağını açıklar.