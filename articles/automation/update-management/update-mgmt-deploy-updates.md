---
title: Azure Otomasyonu Güncelleştirme Yönetimi için güncelleştirme dağıtımları oluşturma
description: Bu makalede, güncelleştirme dağıtımlarının nasıl planlanmakta ve bunların durumlarını incelemesinin nasıl yapılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 2a81376b284e0d1df84a69b969335c0e63999a00
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450886"
---
# <a name="how-to-deploy-updates-and-review-results"></a>Güncelleştirmeler nasıl dağıtılır ve sonuçlar incelenmek

Bu makalede, bir güncelleştirme dağıtımının nasıl zamanlanmakta olduğu ve dağıtım tamamlandıktan sonra işlemin incelenmesi açıklanmaktadır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com) oturum açın

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirme dağıtımının zamanlanması, hedef makinelerdeki güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook 'una bağlı bir [zamanlama](../shared-resources/schedules.md) kaynağı oluşturur. Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlamanız gerekir. Dağıtıma dahil edilecek güncelleştirme türlerini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Azure portal zamanlama kaynağını veya dağıtımı oluşturduktan sonra PowerShell 'i kullanarak silerseniz, silme işlemi zamanlanmış güncelleştirme dağıtımını keser ve zamanlama kaynağını portaldan yeniden yapılandırmayı denediğinizde bir hata oluşturur. Yalnızca ilgili dağıtım zamanlamasını silerek zamanlama kaynağını silebilirsiniz.  

Yeni bir güncelleştirme dağıtımı zamanlamak için:

1. Otomasyon **hesabınızda güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ne gidin ve **güncelleştirme dağıtımını zamanla**' yı seçin.

2. **Yeni güncelleştirme dağıtımı**altında, **ad** alanına dağıtımınız için benzersiz bir ad girin.

3. Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

4. **Güncelleştirilecek gruplar (Önizleme)** bölgesinde, dağıtımınıza dahil etmek üzere Azure VM 'lerin dinamik bir grubunu derlemek için aboneliği, kaynak gruplarını, konumları ve etiketleri birleştiren bir sorgu tanımlayın. Daha fazla bilgi için bkz. [güncelleştirme yönetimi ile dinamik grupları kullanma](update-mgmt-groups.md).

5. **Güncelleştirilecek makineler** bölümünde, açılan menüden kaydedilmiş bir arama, içeri aktarılan bir grup seçin ya da **makineleri** seçin ve tek tek makineleri seçin. Bu seçenekle, her makine için Log Analytics aracısının hazır olduğunu görebilirsiniz. Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../../azure-monitor/platform/computer-groups.md).

6. Ürünlerin [güncelleştirme sınıflandırmalarını](update-mgmt-view-update-assessments.md#work-with-update-classifications) belirtmek için **güncelleştirme sınıflandırmaları** bölgesini kullanın. Her ürün için desteklenen tüm güncelleştirme sınıflandırmalarının seçimini kaldırın, ancak güncelleştirme dağıtımınıza dahil olanlar.

7. Dağıtım için belirli güncelleştirmeleri seçmek üzere **güncelleştirmeleri dahil et/hariç tut** bölgesini kullanın. Dahil et/hariç tut sayfası, güncelleştirmeleri, dahil etmek veya hariç tutmak için KB makalesinin KIMLIK numaralarına göre görüntüler.
    
   > [!IMPORTANT]
   > Dışlamalar geçersiz kılmanın dahil olduğunu unutmayın. Örneğin, bir hariç tutma kuralı tanımlarsanız `*` , güncelleştirme yönetimi tüm düzeltme eklerini veya paketleri yüklemeden dışlar. Dışlanan düzeltme ekleri hala makinelerde eksik olarak gösterilir. Linux makineler için, dışlanan bağımlı paketi olan bir paketi eklerseniz Güncelleştirme Yönetimi ana paketi yüklemez.

   > [!NOTE]
   > Güncelleştirme dağıtımına dahil etmek için yenisiyle değiştirilen güncelleştirmeleri belirtemezsiniz.

8. **Zamanlama ayarları**' nı seçin. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

9. Dağıtımın bir kez mi gerçekleşeceğini, yoksa yinelenen bir zamanlamayı mı kullandığını belirtmek için **yinelenme** alanını kullanın ve ardından **Tamam**' ı seçin.

10. **Ön betikler + betikleri sonrası (Önizleme)** bölgesinde, dağıtımdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [betikleri ve son betikleri yönetme](update-mgmt-pre-post-scripts.md).
    
11. Güncelleştirmelerin yüklenmesi için izin verilen süreyi belirtmek için **bakım penceresi (dakika)** alanını kullanın. Bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun:

    * Bakım pencereleri kaç güncelleştirme yüklendiğini denetler.
    * Bakım penceresinin sonuna yaklaşıyorsa Güncelleştirme Yönetimi yeni güncelleştirmeleri yüklemeyi durdurmaz.
    * Bakım penceresi aşılırsa Güncelleştirme Yönetimi devam eden güncelleştirmeleri sonlandıramaz.
    * Bakım penceresi Windows üzerinde aşılırsa, çoğu zaman bir hizmet paketi güncelleştirmesinin yüklenmesi uzun sürmesi nedeniyle oluşur.

    > [!NOTE]
    > Güncelleştirmelerin Ubuntu 'daki bakım penceresinin dışında uygulanmasını önlemek için, `Unattended-Upgrade` otomatik güncelleştirmeleri devre dışı bırakmak üzere paketi yeniden yapılandırın. Paketin nasıl yapılandırılacağı hakkında daha fazla bilgi için [Ubuntu sunucu kılavuzundaki otomatik güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

12. Dağıtım sırasında yeniden başlatmalar işleme yöntemini belirtmek için **yeniden başlatma seçenekleri** alanını kullanın. Aşağıdaki seçenekler kullanılabilir: 
    * Gerekirse yeniden Başlat (varsayılan)
    * Her zaman yeniden başlat
    * Hiçbir zaman yeniden başlatma
    * Yalnızca yeniden Başlat; Bu seçenek güncelleştirmeleri yüklemez

    > [!NOTE]
    > [Yeniden başlatmayı yönetmek için kullanılan kayıt](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) defteri anahtarları altında listelenen kayıt defteri anahtarları, **yeniden başlatma seçenekleri** hiçbir şekilde **yeniden başlatma**olarak ayarlandıysa yeniden başlatma olayına neden olabilir.

13. Dağıtım zamanlamasını yapılandırmayı bitirdiğinizde **Oluştur**' u seçin.

    ![Güncelleştirme Zamanlama Ayarları bölmesi](./media/update-mgmt-deploy-updates/manageupdates-schedule-win.png)

14. Durum panosu açılır. Oluşturduğunuz dağıtım zamanlamasını göstermek için **Zamanlanmış güncelleştirme dağıtımları** ' nı seçin.

## <a name="schedule-an-update-deployment-programmatically"></a>Program aracılığıyla bir güncelleştirme dağıtımı zamanlama

REST API güncelleştirme dağıtımı oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create).

Bir haftalık güncelleştirme dağıtımı oluşturmak için örnek bir runbook kullanabilirsiniz. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="check-deployment-status"></a>Dağıtım durumunu denetle

Zamanlanan dağıtımınız başladıktan sonra, **güncelleştirme yönetimi**altındaki **güncelleştirme dağıtımları** sekmesinde durumunu görebilirsiniz. Dağıtım o anda çalışıyorsa, durum **Sürüyor** şeklinde olur. Dağıtım başarıyla sona erdiğinde durum **başarılı**olarak değişir. Dağıtımda bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **kısmen başarısız**olur.

## <a name="view-results-of-a-completed-update-deployment"></a>Tamamlanmış bir güncelleştirme dağıtımının sonuçlarını görüntüleme

Dağıtım tamamlandığında, sonuçlarını görmek için bunu seçebilirsiniz.

[![Belirli bir dağıtım için dağıtım durumu panosunu Güncelleştir](./media/update-mgmt-deploy-updates/manageupdates-view-results.png)](./media/update-mgmt-deploy-updates/manageupdates-view-results-expanded.png#lightbox)

**Güncelleştirme sonuçları**altında bir Özet, hedef VM 'lerde toplam güncelleştirme ve dağıtım sonucu sayısını sağlar. Sağ taraftaki tablo, güncelleştirmelerin ayrıntılı bir dökümünü ve her biri için yükleme sonuçlarını gösterir.

Kullanılabilir değerler şunlardır:

* **Denenmedi** -tanımlanan bakım penceresi süresine göre yeterli kullanılabilir zaman olmadığından güncelleştirme yüklenmedi.
* **Seçilmedi** -güncelleştirme dağıtım için seçilmedi.
* **Başarılı** -güncelleştirme başarılı oldu.
* **Başarısız** -güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm Günlükler** ' i seçin.

Hedef VM 'lerde güncelleştirme dağıtımını yönetmekten sorumlu runbook 'un iş akışını görmek için **Çıkış ' ı** seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgiler için **Hatalar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım sonuçlarını güncelleştirme hakkında sizi bilgilendirmek üzere uyarılar oluşturmayı öğrenmek için bkz. [güncelleştirme yönetimi uyarı oluşturma](update-mgmt-configure-alerts.md).