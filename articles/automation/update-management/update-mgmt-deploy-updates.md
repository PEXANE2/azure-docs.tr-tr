---
title: Azure Otomasyonu Güncelleştirme Yönetimi için güncelleştirme dağıtımları oluşturma
description: Bu makalede, güncelleştirme dağıtımlarının nasıl planlanmakta ve bunların durumlarını incelemesinin nasıl yapılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: f5f7d86ff668a151bdf83908c3199d01a0a53246
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92073759"
---
# <a name="how-to-deploy-updates-and-review-results"></a>Güncelleştirmeler nasıl dağıtılır ve sonuçlar incelenmek

Bu makalede, bir güncelleştirme dağıtımının nasıl zamanlanmakta olduğu ve dağıtım tamamlandıktan sonra işlemin incelenmesi açıklanmaktadır. Seçilen bir Azure sanal makinesinden, seçilen yay etkin sunucusundan veya Otomasyon hesabından tüm yapılandırılmış makineler ve sunucular arasında bir güncelleştirme dağıtımı yapılandırabilirsiniz.

Her senaryo altında, oluşturduğunuz dağıtım makine veya sunucu ' yı seçtiğiniz hedefler ya da Otomasyon hesabınızdan bir dağıtım oluşturmak için bir veya daha fazla makineyi hedefleyebilirsiniz. Bir Azure VM veya Arc etkin sunucusundan bir güncelleştirme dağıtımı zamanladığınızda, adımlar Otomasyon hesabınızdan aşağıdaki özel durumlarla birlikte dağıtımıyla aynıdır:

* İşletim sistemi, makinenin IŞLETIM sistemine göre otomatik olarak önceden seçilir
* Güncelleştirilecek hedef makine, kendisini otomatik olarak hedeflemek üzere ayarlanmış
* Zamanlamayı yapılandırırken, **Şimdi güncelleştirmeyi**belirtebilir, bir kez oluşabilir veya yinelenen bir zamanlama kullanabilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com) oturum açma

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirme dağıtımının zamanlanması, hedef makinede veya makinelerde güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook 'una bağlı bir [zamanlama](../shared-resources/schedules.md) kaynağı oluşturur. Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlamanız gerekir. Dağıtıma dahil edilecek güncelleştirme türlerini seçebilirsiniz. Örneğin, kritik güncelleştirmeler veya güvenlik güncelleştirmeleri ekleyebilir ve güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Azure portal zamanlama kaynağını veya dağıtımı oluşturduktan sonra PowerShell 'i kullanarak silerseniz, silme işlemi zamanlanmış güncelleştirme dağıtımını keser ve zamanlama kaynağını portaldan yeniden yapılandırmayı denediğinizde bir hata oluşturur. Yalnızca ilgili dağıtım zamanlamasını silerek zamanlama kaynağını silebilirsiniz.  

Yeni bir güncelleştirme dağıtımı zamanlamak için aşağıdaki adımları gerçekleştirin. Seçilen kaynağa (Otomasyon hesabı, Arc etkin sunucu, Azure VM) bağlı olarak, aşağıdaki adımlar dağıtım zamanlamasını yapılandırırken küçük farklılıklar ile tümüne uygulanır.

1. Portalda bir dağıtım zamanlamak için:

   * Bir veya daha fazla makine, **Otomasyon hesaplarına** gidin ve listeden güncelleştirme yönetimi etkinleştirilmiş Otomasyon hesabınızı seçin.
   * Bir Azure VM için, **sanal makineler** ' e gidin ve listeden VM 'nizi seçin.
   * Bir yay etkin sunucu için **sunucular-Azure Arc** ' a gidin ve listeden sunucunuzu seçin.

2. Güncelleştirme Yönetimi gitmek için, seçtiğiniz kaynağa bağlı olarak:

   * Otomasyon hesabınızı **seçtiyseniz güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ne gidin ve **güncelleştirme dağıtımını zamanla**' yı seçin.
   * Bir Azure VM 'si seçtiyseniz, **Konuk + ana bilgisayar güncelleştirmeleri**' ne gidin ve ardından **güncelleştirme yönetimi git**' i seçin.
   * Bir yay etkin sunucu seçtiyseniz, **güncelleştirme yönetimi**' a gidin ve **güncelleştirme dağıtımını zamanla**' yı seçin.

3. **Yeni güncelleştirme dağıtımı**altında, **ad** alanına dağıtımınız için benzersiz bir ad girin.

4. Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

    > [!NOTE]
    > Bir Azure VM veya Arc etkin sunucu seçtiyseniz bu seçenek kullanılamaz. İşletim sistemi otomatik olarak tanımlanır.

5. **Güncelleştirilecek gruplar (Önizleme)** bölgesinde, dağıtımınıza dahil etmek üzere Azure VM 'lerin dinamik bir grubunu derlemek için aboneliği, kaynak gruplarını, konumları ve etiketleri birleştiren bir sorgu tanımlayın. Daha fazla bilgi için bkz. [güncelleştirme yönetimi ile dinamik grupları kullanma](update-mgmt-groups.md).

    > [!NOTE]
    > Bir Azure VM veya Arc etkin sunucu seçtiyseniz bu seçenek kullanılamaz. Makine, zamanlanan dağıtım için otomatik olarak hedeflenir.

6. **Güncelleştirilecek makineler** bölümünde, açılan menüden kaydedilmiş bir arama, içeri aktarılan bir grup seçin ya da **makineleri** seçin ve tek tek makineleri seçin. Bu seçenekle, her makine için Log Analytics aracısının hazır olduğunu görebilirsiniz. Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../../azure-monitor/platform/computer-groups.md). Zamanlanmış bir güncelleştirme dağıtımında en fazla 500 makine ekleyebilirsiniz.

    > [!NOTE]
    > Bir Azure VM veya Arc etkin sunucu seçtiyseniz bu seçenek kullanılamaz. Makine, zamanlanan dağıtım için otomatik olarak hedeflenir.

7. Ürünlerin [güncelleştirme sınıflandırmalarını](update-mgmt-view-update-assessments.md#work-with-update-classifications) belirtmek için **güncelleştirme sınıflandırmaları** bölgesini kullanın. Her ürün için desteklenen tüm güncelleştirme sınıflandırmalarının seçimini kaldırın, ancak güncelleştirme dağıtımınıza dahil olanlar.

    Dağıtımınız yalnızca bir seçim kümesi uygulamasa, bir sonraki adımda açıklandığı gibi, **güncelleştirmeleri dahil et/hariç tut** seçeneğini yapılandırırken, önceden seçilmiş tüm güncelleştirme sınıflandırmalarının seçimini kaldırmak gerekir. Bu, hedef makinelere yalnızca bu dağıtıma *dahil* etmek için belirttiğiniz güncelleştirmelerin yüklü olmasını sağlar.

8. Seçili güncelleştirmeleri dağıtıma eklemek veya dağıtımdan dışlamak için **güncelleştirmeleri dahil et/hariç tut** bölgesini kullanın. **Dahil et/hariç tut** sayfasında, dahil etmek veya hariç tutmak istediğiniz KB makalesinin kimlik numaralarını girersiniz.

   > [!IMPORTANT]
   > Dışlamalar geçersiz kılmanın dahil olduğunu unutmayın. Örneğin, bir hariç tutma kuralı tanımlarsanız `*` , güncelleştirme yönetimi tüm düzeltme eklerini veya paketleri yüklemeden dışlar. Dışlanan düzeltme ekleri hala makinelerde eksik olarak gösterilir. Linux makineler için, dışlanan bağımlı paketi olan bir paketi eklerseniz Güncelleştirme Yönetimi ana paketi yüklemez.

   > [!NOTE]
   > Güncelleştirme dağıtımına dahil etmek için yenisiyle değiştirilen güncelleştirmeleri belirtemezsiniz.

9. **Zamanlama ayarları**' nı seçin. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

    > [!NOTE]
    > Bu seçenek, bir yay etkin sunucu seçtiyseniz farklılık fark edilir. **Şimdi Güncelleştir** ' i veya gelecek başlangıç saatini 20 dakika olarak seçebilirsiniz.

10. Dağıtımın bir kez mi gerçekleşeceğini, yoksa yinelenen bir zamanlamayı mı kullandığını belirtmek için **yinelemeyi** kullanın, sonra **Tamam**' ı seçin.

11. **Ön betikler + betikleri sonrası (Önizleme)** bölgesinde, dağıtımdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [betikleri ve son betikleri yönetme](update-mgmt-pre-post-scripts.md).

12. Güncelleştirmelerin yüklenmesi için izin verilen süreyi belirtmek için **bakım penceresi (dakika)** alanını kullanın. Bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun:

    * Bakım pencereleri kaç güncelleştirme yüklendiğini denetler.
    * Bakım penceresinin sonuna yaklaşıyorsa Güncelleştirme Yönetimi yeni güncelleştirmeleri yüklemeyi durdurmaz.
    * Bakım penceresi aşılırsa Güncelleştirme Yönetimi devam eden güncelleştirmeleri sonlandıramaz. Yüklenecek kalan güncelleştirmeler denenmez. Bu sürekli olarak kullanılıyorsa, bakım pencerenizin süresini yeniden değerlendirmelisiniz.
    * Bakım penceresi Windows üzerinde aşılırsa, çoğu zaman bir hizmet paketi güncelleştirmesinin yüklenmesi uzun sürmesi nedeniyle oluşur.

    > [!NOTE]
    > Güncelleştirmelerin Ubuntu 'daki bakım penceresinin dışında uygulanmasını önlemek için, `Unattended-Upgrade` otomatik güncelleştirmeleri devre dışı bırakmak üzere paketi yeniden yapılandırın. Paketin nasıl yapılandırılacağı hakkında daha fazla bilgi için [Ubuntu sunucu kılavuzundaki otomatik güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

13. Dağıtım sırasında yeniden başlatmalar işleme yöntemini belirtmek için **yeniden başlatma seçenekleri** alanını kullanın. Aşağıdaki seçenekler kullanılabilir: 
    * Gerekirse yeniden Başlat (varsayılan)
    * Her zaman yeniden başlat
    * Hiçbir zaman yeniden başlatma
    * Yalnızca yeniden Başlat; Bu seçenek güncelleştirmeleri yüklemez

    > [!NOTE]
    > [Yeniden başlatmayı yönetmek için kullanılan kayıt](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) defteri anahtarları altında listelenen kayıt defteri anahtarları, **yeniden başlatma seçenekleri** hiçbir şekilde **yeniden başlatma**olarak ayarlandıysa yeniden başlatma olayına neden olabilir.

14. Dağıtım zamanlamasını yapılandırmayı bitirdiğinizde **Oluştur**' u seçin.

    ![Güncelleştirme Zamanlama Ayarları bölmesi](./media/update-mgmt-deploy-updates/manageupdates-schedule-win.png)

    > [!NOTE]
    > Seçili bir yay etkin sunucu için dağıtım zamanlamasını yapılandırmayı bitirdiğinizde, **gözden geçir + oluştur**' u seçin.

15. Durum panosu açılır. Oluşturduğunuz dağıtım zamanlamasını göstermek için **dağıtım zamanlamaları** ' nı seçin.

## <a name="schedule-an-update-deployment-programmatically"></a>Program aracılığıyla bir güncelleştirme dağıtımı zamanlama

REST API güncelleştirme dağıtımı oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create).

Bir haftalık güncelleştirme dağıtımı oluşturmak için örnek bir runbook kullanabilirsiniz. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="check-deployment-status"></a>Dağıtım durumunu denetle

Zamanlanan dağıtımınız başladıktan sonra, **güncelleştirme yönetimi**altındaki **Geçmiş** sekmesinde durumunu görebilirsiniz. Dağıtım o anda çalışıyorsa, durum **Sürüyor** şeklinde olur. Dağıtım başarıyla sona erdiğinde durum **başarılı**olarak değişir. Dağıtımda bir veya daha fazla güncelleştirme ile ilgili hata varsa, durum **başarısız**olur.

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