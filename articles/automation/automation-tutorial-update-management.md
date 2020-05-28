---
title: Azure Otomasyonu 'nda Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme
description: Bu makalede, Azure VM 'lerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Güncelleştirme Yönetimi nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 04/06/2020
ms.custom: mvc
ms.openlocfilehash: 5bc71c5462ed1c721d2c94f889146400a07dd19e
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84119207"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme

Bu makalede, Azure sanal makinelerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Azure Otomasyonu [güncelleştirme yönetimi](automation-update-management.md) özelliğini nasıl kullanabileceğiniz açıklanır. Fiyatlandırma bilgisi için bkz. [Güncelleştirme Yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

> [!NOTE]
> Güncelleştirme Yönetimi, birinci taraf güncelleştirmelerin dağıtımını ve düzeltme eklerinin önceden indirilmesini destekler. Bu destek, düzeltme eki eklenen sistemlerde değişiklik yapılmasını gerektirir. Bu ayarları sistemlerinizde nasıl yapılandıracağınızı öğrenmek için bkz. [Azure Automation güncelleştirme yönetimi için Windows Update ayarlarını yapılandırma](automation-configure-windows-update.md) .

Bu makaledeki yordamları kullanmadan önce, şu tekniklerin birini kullanarak sanal makinelerinizdeki Güncelleştirme Yönetimi etkinleştirdiğinizden emin olun:

* [Otomasyon hesabından Güncelleştirme Yönetimi’ni etkinleştirme](automation-onboard-solutions-from-automation-account.md)
* [Azure portal göz atarak Güncelleştirme Yönetimi etkinleştirin](automation-onboard-solutions-from-browse.md)
* [Runbook’tan Güncelleştirme Yönetimi’ni etkinleştirme](automation-onboard-solutions.md)
* [Azure VM’den Güncelleştirme Yönetimi’ni etkinleştirme](automation-onboard-solutions-from-vm.md)

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme değerlendirmesini görüntülemek için:

1. Otomasyon **hesabınızda güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin. 

2. Ortamınız için güncelleştirmeler güncelleştirme yönetimi sayfasında listelenir. Herhangi bir güncelleştirme eksik olarak tanımlanmışsa eksik güncelleştirmeler listesi **eksik güncelleştirmeler** sekmesinde gösterilir.

3. **Bilgi bağlantısı**altında güncelleştirme bağlantısını seçerek güncelleştirme hakkında önemli bilgiler sunan destek makalesini açın.

    ![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

4. Günlük araması bölmesini açmak için güncelleştirmede başka herhangi bir yere tıklayın. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Ayrıntılı bilgileri görüntülemek için bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturabilirsiniz.

    ![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Bir güncelleştirme dağıtımının durumunu öğrenmenizi sağlamak için uyarıları ayarlamak üzere aşağıdaki adımları izleyin:

1. Otomasyon hesabınızda, **izleme**altındaki **Uyarılar** ' a gidin ve ardından **Yeni uyarı kuralı**' nı tıklatın.

2. Uyarı kuralı oluştur sayfasında, Otomasyon hesabınız kaynak olarak zaten seçilidir. Değiştirmek isterseniz **Kaynağı Düzenle**' ye tıklayın. 

3. Kaynak seçin sayfasında, **kaynak türüne göre filtrele** açılan menüsünden **Otomasyon hesapları** ' nı seçin. 

4. Kullanmak istediğiniz Otomasyon hesabını seçin ve **bitti**' ye tıklayın.

5. Güncelleştirme dağıtımınız için uygun olan sinyali seçmek için **Koşul Ekle** ' ye tıklayın. Aşağıdaki tabloda, iki kullanılabilir sinyalin ayrıntıları gösterilmektedir.

    |Sinyal adı|Boyutlar|Açıklama
    |---|---|---|
    |`Total Update Deployment Runs`|-Güncelleştirme dağıtım adı<br>-Durum    |Bir güncelleştirme dağıtımının genel durumu hakkında uyarılar.|
    |`Total Update Deployment Machine Runs`|-Güncelleştirme dağıtım adı</br>-Durum</br>-Hedef bilgisayar</br>-Güncelleştirme dağıtımı çalıştırma KIMLIĞI    |Belirli makinelere hedeflenmiş bir güncelleştirme dağıtımının durumu hakkında uyarılar.|

6. Bir boyut için listeden geçerli bir değer seçin. İstediğiniz değer listede yoksa, boyutun ileri ' ye tıklayın **\+** ve özel adı yazın. Ardından aranacak değeri seçin. Bir boyutun tüm değerlerini seçmek istiyorsanız, **Seç \* ** düğmesine tıklayın. Boyut için bir değer seçmezseniz Güncelleştirme Yönetimi bu boyutu yoksayar.

    ![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/signal-logic.png)

7. **Uyarı mantığı**altında, **zaman toplama** ve **eşik** alanlarına değerler girip **bitti**' ye tıklayın.

8. Sonraki bölmede, uyarı için bir ad ve açıklama girin.

9. Başarısız bir çalıştırma için başarılı bir çalıştırma veya **bilgilendirme (sev 1)** Için **önem derecesi** alanını **bilgilendirici (sev 2)** olarak ayarlayın.

    ![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/define-alert-details.png)

10. Uyarı kuralını nasıl etkinleştirmek istediğinize bağlı olarak **Evet** veya **Hayır**' a tıklayın.

11. Bu kural için uyarılara sahip olmak istemiyorsanız **Uyarıları Gizle**' yi seçin.

## <a name="configure-action-groups-for-your-alerts"></a>Uyarılarınız için eylem gruplarını yapılandırma

Uyarılarınız yapılandırıldıktan sonra, birden çok uyarı arasında kullanılacak bir eylem grubu olan bir eylem grubu ayarlayabilirsiniz. Eylemler e-posta bildirimleri, runbook 'lar, Web kancaları ve çok daha fazlasını içerebilir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

1. Bir uyarı seçin ve ardından **Yeni oluştur** **eylem grupları**' nı seçin. 

2. Eylem grubu için bir tam ad ve kısa bir ad girin. Güncelleştirme Yönetimi, belirtilen grubu kullanarak bildirim gönderirken kısa adı kullanır.

3. **Eylemler**' in altında, eylemi belirten bir ad girin, örneğin **e-posta bildirimi**. 

4. **Eylem türü**için uygun türü seçin (örneğin, **e-posta/SMS/Push/Voice**). 

5. **Ayrıntıları Düzenle**' ye tıklayın.

6. Eylem türü için bölmeyi girin. Örneğin, **e-posta/SMS/Push/seskullanacaksanız**, bir eylem adı girin, **e-posta** kutusunu seçin, geçerli bir e-posta adresi girin ve ardından **Tamam**' a tıklayın.

    ![E-posta eylem grubu yapılandırma](./media/automation-tutorial-update-management/configure-email-action-group.png)

7. Eylem grubu Ekle bölmesinde **Tamam**' ı tıklatın.

8. Bir uyarı e-postası için e-posta konusunu özelleştirebilirsiniz. **Kural oluştur**altında **eylemleri Özelleştir** ' i seçin ve **e-posta konusu**' ı seçin 

9. İşiniz bittiğinde **Uyarı kuralı oluştur**' a tıklayın. 

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirme dağıtımının zamanlanması, hedef makinelerdeki güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook 'una bağlı bir [zamanlama](shared-resources/schedules.md) kaynağı oluşturur. Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlamanız gerekir. Dağıtıma dahil edilecek güncelleştirme türlerini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Azure portal zamanlama kaynağını veya dağıtımı oluşturduktan sonra PowerShell 'i kullanarak silerseniz, silme işlemi zamanlanmış güncelleştirme dağıtımını keser ve zamanlama kaynağını portaldan yeniden yapılandırmayı denediğinizde bir hata oluşturur. Yalnızca ilgili dağıtım zamanlamasını silerek zamanlama kaynağını silebilirsiniz.  

Yeni bir güncelleştirme dağıtımı zamanlamak için:

1. Otomasyon **hesabınızda güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ne gidin ve **güncelleştirme dağıtımını zamanla**' yı seçin.

2. **Yeni güncelleştirme dağıtımı**altında, dağıtımınız için benzersiz bir ad girmek üzere **ad** alanını kullanın.

3. Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

4. **Güncelleştirilecek gruplar (Önizleme)** bölgesinde, dağıtımınıza dahil etmek üzere Azure VM 'lerin dinamik bir grubunu derlemek için aboneliği, kaynak gruplarını, konumları ve etiketleri birleştiren bir sorgu tanımlayın. Daha fazla bilgi için bkz. [güncelleştirme yönetimi ile dinamik grupları kullanma](automation-update-management-groups.md).

5. **Güncelleştirilecek makineler** bölümünde, açılan menüden kaydedilmiş bir arama, içeri aktarılan bir grup seçin ya da **makineleri** seçin ve tek tek makineleri seçin. Bu seçenekle, her makine için Log Analytics aracısının hazır olduğunu görebilirsiniz. Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md).

6. Ürünlerin [güncelleştirme sınıflandırmalarını](automation-view-update-assessments.md#work-with-update-classifications) belirtmek için **güncelleştirme sınıflandırmaları** bölgesini kullanın. Her ürün için desteklenen tüm güncelleştirme sınıflandırmalarının seçimini kaldırın, ancak güncelleştirme dağıtımınıza dahil olanlar.

7. Dağıtım için belirli güncelleştirmeleri seçmek üzere **güncelleştirmeleri dahil et/hariç tut** bölgesini kullanın. Dahil et/hariç tut sayfası, güncelleştirmeleri, dahil etmek veya hariç tutmak için KB makalesinin KIMLIK numaralarına göre görüntüler. 
    
   > [!IMPORTANT]
   > Dışlamalar geçersiz kılmanın dahil olduğunu unutmayın. Örneğin, bir hariç tutma kuralı tanımlarsanız `*` , güncelleştirme yönetimi tüm düzeltme eklerini veya paketleri yüklemeden dışlar. Dışlanan düzeltme ekleri hala makinelerde eksik olarak gösterilir. Linux makineler için, dışlanan bağımlı paketi olan bir paketi eklerseniz Güncelleştirme Yönetimi ana paketi yüklemez.

   > [!NOTE]
   > Güncelleştirme dağıtımına dahil etmek için yenisiyle değiştirilen güncelleştirmeleri belirtemezsiniz.

8. **Zamanlama ayarları**' nı seçin. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

9. Dağıtımın bir kez mi gerçekleşeceğini, yoksa yinelenen bir zamanlamayı mı kullandığını belirtmek için **yinelenme** alanını kullanın ve ardından **Tamam**' a tıklayın.

10. **Ön betikler + betikleri sonrası (Önizleme)** bölgesinde, dağıtımdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [betikleri ve son betikleri yönetme](pre-post-scripts.md).
    
11. Güncelleştirmelerin yüklenmesi için izin verilen süreyi belirtmek için **bakım penceresi (dakika)** alanını kullanın. Bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun:

    * Bakım pencereleri kaç güncelleştirme yüklendiğini denetler.
    * Bakım penceresinin sonuna yaklaşıyorsa Güncelleştirme Yönetimi yeni güncelleştirmeleri yüklemeyi durdurmaz.
    * Bakım penceresi aşılırsa Güncelleştirme Yönetimi devam eden güncelleştirmeleri sonlandıramaz.
    * Bakım penceresi Windows üzerinde aşılırsa, çoğu zaman bir hizmet paketi güncelleştirmesinin yüklenmesi uzun sürmesi nedeniyle oluşur.

    > [!NOTE]
    > Güncelleştirmelerin Ubuntu 'daki bakım penceresinin dışında uygulanmasını önlemek için, `Unattended-Upgrade` otomatik güncelleştirmeleri devre dışı bırakmak üzere paketi yeniden yapılandırın. Paketin nasıl yapılandırılacağı hakkında daha fazla bilgi için [Ubuntu sunucu kılavuzundaki otomatik güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

12. Dağıtım sırasında yeniden başlatmalar işleme yöntemini belirtmek için **yeniden başlatma seçenekleri** alanını kullanın. Aşağıdaki seçenekler mevcuttur: 
    * Gerekirse yeniden Başlat (varsayılan)
    * Her zaman yeniden başlat
    * Hiçbir zaman yeniden başlatma
    * Yalnızca yeniden Başlat; Bu seçenek güncelleştirmeleri yüklemez

    > [!NOTE]
    > [Yeniden başlatmayı yönetmek için kullanılan kayıt](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) defteri anahtarları altında listelenen kayıt defteri anahtarları, **yeniden başlatma seçenekleri** hiçbir şekilde **yeniden başlatma**olarak ayarlandıysa yeniden başlatma olayına neden olabilir.

13. Dağıtım zamanlamasını yapılandırmayı tamamladıktan sonra **Oluştur**' a tıklayın.

    ![Güncelleştirme Zamanlama Ayarları bölmesi](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

14. Durum panosu açılır. Oluşturduğunuz dağıtım zamanlamasını göstermek için **Zamanlanmış güncelleştirme dağıtımları** ' nı seçin.

## <a name="schedule-an-update-deployment-programmatically"></a>Program aracılığıyla bir güncelleştirme dağıtımı zamanlama

REST API güncelleştirme dağıtımı oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create). 

Bir haftalık güncelleştirme dağıtımı oluşturmak için örnek bir runbook kullanabilirsiniz. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="check-deployment-status"></a>Dağıtım durumunu denetle

Zamanlanan dağıtımınız başladıktan sonra, **güncelleştirme yönetimi**altındaki **güncelleştirme dağıtımları** sekmesinde durumunu görebilirsiniz. Dağıtım o anda çalışıyorsa, durum **Sürüyor** şeklinde olur. Dağıtım başarıyla sona erdiğinde durum **başarılı**olarak değişir. Dağıtımda bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **kısmen başarısız**olur.

## <a name="view-results-of-a-completed-update-deployment"></a>Tamamlanmış bir güncelleştirme dağıtımının sonuçlarını görüntüleme

Dağıtım tamamlandığında, panosunu görmek için bu seçeneği belirleyebilirsiniz.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/automation-tutorial-update-management/manageupdates-view-results.png)

**Güncelleştirme sonuçları**altında bir Özet, hedef VM 'lerde toplam güncelleştirme ve dağıtım sonucu sayısını sağlar. Sağ taraftaki tablo, güncelleştirmelerin ayrıntılı bir dökümünü ve her biri için yükleme sonuçlarını gösterir.

Kullanılabilir değerler şunlardır:

* **Denenmedi** -tanımlanan bakım penceresi süresine göre yeterli kullanılabilir zaman olmadığından güncelleştirme yüklenmedi.
* **Seçilmedi** -güncelleştirme dağıtım için seçilmedi.
* **Başarılı** -güncelleştirme başarılı oldu.
* **Başarısız** -güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm Günlükler** ' i seçin.

Hedef VM 'lerde güncelleştirme dağıtımını yönetmekten sorumlu runbook 'un iş akışını görmek için **Çıkış ' ı** seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgiler için **Hatalar**’ı seçin.

## <a name="view-the-deployment-alert"></a>Dağıtım uyarısını görüntüleme

Güncelleştirme dağıtımınız tamamlandığında, dağıtım için kurulum sırasında belirttiğiniz uyarıyı alırsınız. Örneğin, bir düzeltme ekini onaylayan bir e-posta aşağıda verilmiştir.

![E-posta eylem grubunu yapılandırma](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtımlar ile işiniz bittiğinde, [güncelleştirme yönetimi için çalışma alanının Otomasyon hesabından bağlantısını kaldır](automation-unlink-workspace-update-management.md)' a bakın.
* VM 'lerinizi Güncelleştirme Yönetimi silmek için bkz. [güncelleştirme yönetimi VM 'Leri kaldırma](automation-remove-vms-from-update-management.md).