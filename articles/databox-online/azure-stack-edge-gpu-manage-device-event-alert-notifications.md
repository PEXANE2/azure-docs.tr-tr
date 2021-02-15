---
title: Azure Stack Edge Pro kaynaklarınız için cihaz olayı uyarı bildirimlerini yönetme | Microsoft Docs
description: Azure Stack Edge Pro kaynaklarınızın cihaz olayları uyarılarını yönetmek için Azure portal nasıl kullanılacağını açıklar.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/02/2021
ms.author: alkohli
ms.openlocfilehash: 0ab7cdfb3d699a8415739565aae5d4326002bc43
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100389153"
---
# <a name="manage-device-event-alert-notifications-on-azure-stack-edge-pro-resources"></a>Azure Stack Edge Pro kaynaklarında cihaz olayı uyarı bildirimlerini yönetme

Bu makalede, bir kaynak grubu, Azure aboneliği veya tek bir Azure Stack Edge kaynağı içinde gerçekleşen cihaz olaylarına ilişkin uyarı bildirimlerini tetiklemek veya gizlemek için Azure portal eylem kuralları oluşturma işlemi açıklanmaktadır. Bu makale tüm Azure Stack Edge modellerine yöneliktir.  

## <a name="about-action-rules"></a>Eylem kuralları hakkında

Eylem kuralı, uyarı bildirimlerini tetikleyebilir veya göstermez. Eylem kuralı bir *eylem grubuna* eklenir-bir kaynak veya kaynak kümesi için farklı bağlamlarda tetiklenen uyarıları üzerinde davranması gereken kullanıcıları bilgilendirmek için kullanılan bir bildirim tercihleri kümesi.

Eylem kuralları hakkında daha fazla bilgi için bkz. [eylem kuralını yapılandırma](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule). Eylem grupları hakkında daha fazla bilgi için [Azure Portal eylem grupları oluşturma ve yönetme](/azure/azure-monitor/platform/action-groups)konusuna bakın.

> [!NOTE]
> Eylem kuralları özelliği önizlemededir. İşlem iyileştirilme sırasında bazı ekranlar ve adımlar değişebilir.


## <a name="create-an-action-rule"></a>Eylem kuralı oluşturma

Azure Stack Edge cihazınız için bir eylem kuralı oluşturmak üzere Azure portal aşağıdaki adımları uygulayın.

> [!NOTE]
> Bu adımlarda, bir eylem grubuna bildirim gönderen bir eylem kuralı oluşturulur. Bildirimleri bastırmak için bir eylem kuralı oluşturma hakkında ayrıntılı bilgi için bkz. [eylem kuralını yapılandırma](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule).

1. Azure portal Azure Stack Edge cihazına gidin ve ardından **> uyarılarını izleme**' ye gidin. **Işlemleri Yönet**' i seçin.

   ![Uyarıları izleme, eylemleri yönetme görünümü](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/action-rules-open-view-01.png)

2. **Eylem kuralları (Önizleme)** öğesini seçin ve **+ yeni eylem kuralı**' nı seçin.

   ![Eylemleri yönetme, eylem kuralları seçeneği](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/action-rules-open-view-02.png)

3. **Eylem kuralı oluştur** ekranında, **kapsam** kullanarak bir Azure aboneliği, kaynak grubu veya hedef kaynak seçin. Eylem kuralı, bu kapsam içinde oluşturulan tüm uyarılarla çalışır.

   1. **Kapsama** göre **Seç ' i** seçin.

      ![Yeni eylem kuralı için bir kapsam seçin](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-01.png)

   2. Eylem kuralı için **aboneliği** seçin, isteğe bağlı olarak bir **kaynak** türüne göre filtreleyin. Azure Stack Edge kaynaklarına filtre uygulamak için **Data Box Edge cihazlar ' ı (dataBoxEdge)** seçin.

      **Kaynak** alanı, seçimlerinize göre kullanılabilir kaynakları listeler.
  
      ![Kapsam seçme ekranında kullanılabilir kaynaklar](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-02.png)

   3. Kuralı uygulamak istediğiniz her kaynağa göre onay kutusunu seçin. Aboneliği, kaynak gruplarını veya ayrı kaynakları seçebilirsiniz. Ardından **Bitti**'yi seçin.

      ![Eylem kuralı kapsamı için örnek ayarlar](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-03.png)

      **Eylem oluştur kural** ekranı seçili kapsamı gösterir.

      ![Azure Stack Edge eylem kuralı için tamamlanmış kapsam](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-04.png)

4. Kuralın, seçili kapsam içindeki uyarıların alt kümesine yönelik uygulamasını daraltmak için **filtre** seçeneklerini kullanın.

   1. **Ekle** ' yi seçerek **Filtre Ekle** bölmesini açın.

      ![Eylem kuralına filtre ekleme seçeneği](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-filter-01.png)

   2. **Filtreler** altında, uygulamak istediğiniz her bir filtreyi ekleyin. Her filtre için filtre türü, **işleç** ve **değer** seçin.
   
      Filtre seçeneklerinin bir listesi için bkz. [filtre ölçütü](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#filter-criteria).

      Aşağıdaki örnek filtreler, Izleme hizmetinin Azure Stack Edge kaynakları için oluşturabileceği önem düzeyleri 2, 3 ve 4 ' te tüm uyarılar için geçerlidir.

      Filtre eklemeyi bitirdiğinizde **bitti**' yi seçin.
   
      ![Eylem kuralı için örnek filtre](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-filter-02.png)

5. **Eylem kuralı oluştur** ekranında, bildirim gönderen bir kural oluşturmak için **eylem grubu** ' nu seçin. Sonra **eylemlere** göre **Seç**' i seçin.

   ![Bildirim gönderen eylem kuralı oluşturmak için eylem grubu seçeneği](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-action-group-01.png)

   > [!NOTE]
   > Bildirimleri gösteren bir kural oluşturmak için, **gizleme**' yi seçin. Daha fazla bilgi için bkz. [eylem kuralını yapılandırma](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule).

6. Bu eylem kuralıyla birlikte kullanmak istediğiniz eylem grubunu seçin. Ardından **Seç** seçeneğini belirleyin. Yeni eylem kuralınız seçili eylem grubunun bildirim tercihlerine eklenecektir.

   Yeni bir eylem grubu oluşturmanız gerekiyorsa **+ eylem grubu** oluştur ' u seçin ve [Azure Portal kullanarak eylem grubu oluşturma](/azure/azure-monitor/platform/action-groups#create-an-action-group-by-using-the-azure-portal)bölümündeki adımları izleyin.

   ![Kuralla kullanılacak bir eylem grubu seçin ve ardından Seç ' i seçin.](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-action-group-02.png)

7. Yeni eylem kuralına bir **ad** ve **Açıklama** verin ve kuralı bir kaynak grubuna atayın.

9. Yeni kural varsayılan olarak etkindir. Kuralı hemen kullanmaya başlamak istemiyorsanız, **kural güncelleştirme oluşturmayı etkinleştir** için **Hayır** ' ı seçin.

10. Ayarlarınızı tamamladığınızda **Oluştur**' u seçin.

    ![Uyarı bildirimleri gönderecek bir eylem kuralı için tamamlanmış ayarlar](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-completed-settings.png)

    **Eylem kuralları (Önizleme)** ekranı açılır, ancak yeni eylem kuralınızı hemen görmeyebilirsiniz. Odak **Tüm** kaynak gruplarıdır.

11. Yeni eylem kuralınızı görmek için, kural için kaynak grubunu seçin.

    ![Yeni kural görüntülenirken eylem kuralları ekranı](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-displayed.png)


## <a name="view-notifications"></a>Bildirimleri görüntüleme

Yeni bir olay, bir eylem kuralı kapsamındaki bir kaynak için bir uyarı harekete geçirdiğinde bildirimler geçer.

E-posta, kısa mesaj hizmeti (SMS) iletisi veya her ikisini de içeren bir bildirim ve bildirim alan bir kural kümesi için eylem grubu.

Uyarı tetiklendikten sonra bildirimlerin gelmesi birkaç dakika sürebilir.

E-posta bildirimi şuna benzer olacaktır.

![Eylem kuralı için örnek e-posta bildirimi](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/sample-action-rule-email-notification.png)


## <a name="next-steps"></a>Sonraki adımlar

<!-- - See [Create and manage action groups in the Azure portal](/azure/azure-monitor/platform/action-groups) for guidance on creating a new action group.
- See [Configure an action rule](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule) for more info about creating action rules that send or suppress alert notifications. -2 bullets referenced above. Making room for local tasks in "Next Steps." --> 
- Cihaz olaylarını, donanım durumunu ve ölçüm grafiklerini gözden geçirme hakkında bilgi için bkz. [Azure Stack Edge Pro 'Yu izleme](azure-stack-edge-monitor.md) . 
- Azure Stack Edge Pro GPU cihazları için Azure Izleyicisini en iyi duruma getirme hakkında bilgi için bkz. [Azure Izleyicisini kullanma](azure-stack-edge-gpu-enable-azure-monitor.md) .
- Tek tek uyarıları yönetme hakkında bilgi için bkz. [Azure Izleyici bağlantı hedefini kullanarak ölçüm uyarılarını oluşturma, görüntüleme ve yönetme](/azure/azure-monitor/platform/alerts-metric) .
