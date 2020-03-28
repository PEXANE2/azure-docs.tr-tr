---
title: Azure VM'leriniz için güncelleştirmeleri ve yamaları yönetme
description: Bu makalede, Azure ve Azure olmayan VM'leriniziçin güncelleştirmeleri ve yamaları yönetmek için Azure Otomasyon Güncelleştirme Yönetimi'nin nasıl kullanılacağına genel bir bakış sağlanmaktadır.
services: automation
ms.subservice: update-management
ms.topic: tutorial
ms.date: 03/04/2020
ms.custom: mvc
ms.openlocfilehash: f7130f3289ce42ca1481ec14be893c846c9ed55b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80335779"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Azure VM'leriniz için güncelleştirmeleri ve yamaları yönetme

Güncelleştirme Yönetimi çözümünü kullanarak sanal makineleriniz için güncelleştirmeleri ve yamaları yönetebilirsiniz. Bu öğreticide kullanılabilir durumdaki güncelleştirmelerin durumunu değerlendirmeyi, gerekli güncelleştirmelerin yüklenmesini zamanlamayı, dağıtım sonuçlarını gözden geçirmeyi ve güncelleştirmelerin başarılı bir şekilde uygulandığını doğrulamak için bir uyarı oluşturmayı öğreneceksiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme Yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Güncelleştirme değerlendirmesini görüntüleme
> * Uyarıları yapılandırma
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* VM'lerinizden biri veya birkaçı için [Güncelleştirme Yönetimi](automation-update-management.md) çözümü etkinleştirildi.
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** bölmesi açılır. Herhangi bir güncelleştirme eksik olarak tanımlanırsa, Eksik **güncelleştirmelerin** listesi Eksik güncelleştirmeler sekmesinde gösterilir.

**Bilgi bağlantısı**altında, güncelleştirme için destek makalesini açmak için güncelleştirme bağlantısını seçin. Güncelleştirme hakkında önemli bilgiler edinebilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Seçili güncelleştirmenin **Günlük arama** bölmesini açmak için güncellemede başka bir yere tıklayın. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturarak ortamınıza dağıtılmış olan veya ortamınızda eksik olan güncelleştirmeler hakkında ayrıntılı bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Bu adımda, bir güncelleştirme dağıtımının durumunu bildirmek için bir uyarı ayarlamayı öğrenirsiniz.

### <a name="alert-conditions"></a>Uyarı koşulları

Otomasyon Hesabınızda, **İzleme** altında **Uyarılar'a**gidin ve sonra **+ Yeni uyarı kuralını**tıklatın.

Otomasyon Hesabınız zaten kaynak olarak seçilmiştir. Değiştirmek istiyorsanız **Seç'i** tıklatabilir ve **kaynak** sayfası seç'te, kaynak türü açılır düşüşle Filtre'deki **Otomasyon Hesapları'nı** seçin. **Filter by resource type** Otomasyon Hesabınızı ve ardından **Bitti**'yi seçin.

Güncelleştirme dağıtımınız için uygun sinyali seçmek için **Ekle koşulunu** tıklatın. Aşağıdaki tablo, güncelleştirme dağıtımları için kullanılabilir iki sinyalin ayrıntılarını gösterir:

|Sinyal Adı|Boyutlar|Açıklama|
|---|---|---|
|**Toplam Güncelleştirme Dağıtım Çalışır**|- Dağıtım Adını Güncelleştir</br>- Durum|Bu sinyal, bir güncelleştirme dağıtımının genel durumunu uyarmak için kullanılır.|
|**Toplam Güncelleme Dağıtım Makinesi Çalışır**|- Dağıtım Adını Güncelleştir</br>- Durum</br>- Hedef Bilgisayar</br>- Dağıtım Çalıştırılanın Kimliğini Güncelleştir|Bu sinyal, belirli makineleri hedefleyen bir güncelleştirme dağıtımının durumu hakkında uyarmak için kullanılır|

Boyut değerleri için listeden geçerli bir değer seçin. Aradığınız değer listede yoksa, boyutun **\+** yanındaki işareti tıklatın ve özel adı yazın. Daha sonra aramak istediğiniz değeri seçebilirsiniz. Bir boyuttan tüm değerleri seçmek istiyorsanız, ** \* Seç** düğmesini tıklatın. Bir boyut için bir değer seçmezseniz, bu boyut değerlendirme sırasında yoksayılır.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/signal-logic.png)

**Uyarı mantığı** bölümünde **Eşik** alanına **1** değerini girin. İşiniz bittiğinde **Bitti**'yi seçin.

### <a name="alert-details"></a>Uyarı ayrıntıları

**2'nin altında. Uyarı ayrıntılarını tanımlayın,** uyarı için bir ad ve açıklama girin. **Önem Derecesi**'ni başarılı çalıştırmalar için **Bilgilendirici (Önem Derecesi 2)**, başarısız çalıştırmalar içinse **Bilgilendirici (Önem Derecesi 1)** olarak ayarlayın.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/define-alert-details.png)

**Eylem grupları altında,** **Yeni Oluştur'u**seçin. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylemlere e-posta bildirimleri, runbook'lar, web kancaları ve diğer birçok şey dahildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

**Eylem grubu adı** kutusuna uyarı için ad ve kısa ad. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

**Eylemler**altında, **E-posta bildirimleri**gibi eylem için bir ad girin. **Eylem türü altında,** **E-posta/SMS/Push/Voice'ı**seçin. **Ayrıntılar**altında, **ayrıntıları edit'i**seçin.

**E-posta/SMS/Anında İletme/Ses** bölmesine bir ad girin. **E-posta** onay kutusunu seçip geçerli bir e-posta adresi girin.

![E-posta eylem grubu yapılandırma](./media/automation-tutorial-update-management/configure-email-action-group.png)

**E-posta/SMS/Push/Voice** bölmesinde **Tamam'ı**seçin. Eylem **grubu ekle** bölmesinde **Tamam'ı**seçin.

Uyarı e-postasının konusunu özelleştirmek için, **Oluşturma kuralı**altında , **eylemleri Özelleştir**altında, **E-posta konusunu**seçin. İşleminiz bittiğinde **Uyarı kuralı oluştur**'u seçin. Bu uyarı, bir güncelleştirme dağıtımı başarılı olduğunda sizi uyarır ve güncelleştirme dağıtımı çalıştırmasının hangi makineleri kapsadığını bildirir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Şimdi güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Bir güncelleştirme dağıtımı zamanladiğinizde, hedef makinelerde güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook'a bağlı bir [zamanlama](shared-resources/schedules.md) kaynağı oluşturur. Zamanlama kaynağını Azure portalından silerseniz veya dağıtımı oluşturduktan sonra PowerShell'i kullanırsanız, zamanlanan güncelleştirme dağıtımını bozar ve portaldan yeniden yapılandırmaya çalıştığınızda bir hata oluşturur. Zamanlama kaynağını yalnızca ilgili dağıtım zamanlamasını silerek silebilirsiniz.  
>

Yeni bir VM güncelleştirme dağıtımı zamanlamak için **Güncelleştirme yönetimi**'ne gidip **Güncelleştirme dağıtımı zamanla**'yı seçin.

**Yeni güncelleştirme dağıtımı** bölümüne aşağıdaki bilgileri girin:

* **Ad**: Güncelleştirme dağıtımı için benzersiz bir ad girin.

* **İşletim sistemi**: Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

* **Güncelleştirilecek gruplar (önizleme)**: Dağıtımınıza dahil edilecek Azure sanal makinelerinin dinamik grubunu derlemek için bir abonelik, kaynak grupları, konumlar ve etiketler birleşimine göre bir sorgu tanımlayın. Daha fazla bilgi için [Dinamik Gruplar'a](automation-update-management-groups.md) bakın

* **Güncelleştirilecek makineler**: Kayıtlı bir aramayı veya İçeri aktarılan grubu seçin veya açılan menüden Makine'yi seçerek belirli makineleri seçin. **Makineler'i**seçerseniz, makinenin hazır durumu **Güncelleştirme aracısı hazır olma** sütununda gösterilir. Azure Monitor günlüklerinde bilgisayar grupları oluşturmanın farklı yöntemleri hakkında bilgi edinmek için [Azure Monitor günlüklerinde Bilgisayar gruplarına](../azure-monitor/platform/computer-groups.md) bakın

* **Güncelleştirme sınıflandırması**: Güncelleştirme dağıtımına dahil edilebilecek her ürün için kullanılabilen desteklenen güncelleştirme sınıflandırmalarını seçin. Bu öğreticide tüm türleri seçili halde bırakın.

  Sınıflandırma türleri şunlardır:

   |İşletim Sistemi  |Tür  |
   |---------|---------|
   |Windows     | Kritik güncelleştirmeler</br>Güvenlik güncelleştirmeleri</br>Güncelleştirme paketleri</br>Özellik paketleri</br>Hizmet paketleri</br>Tanım güncelleştirmeleri</br>Araçlar</br>Güncelleştirmeler        |
   |Linux     | Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri</br>Diğer güncelleştirmeler       |

   Sınıflandırma türlerinin açıklaması için bkz. [sınıflandırmaları güncelleştirme](automation-view-update-assessments.md#update-classifications).

* **Dahil edilecek/dışlanacak güncelleştirmeler** - Böylece **Dahil Et / Dışla** sayfası açılır. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir.

> [!NOTE]
> Dışlamaların kapsayıcıları geçersiz kdığını bilmek önemlidir. Örneğin, bir dışlama kuralı `*`tanımlarsanız, bunların tümü hariç olduğu için hiçbir düzeltme ekleri veya paketler yüklenmez. Dışlanmış yamalar hala makineden eksik olarak göstermektedir. Linux makineleri için bir paket dahil edildiyse ancak dışlanan bağımlı bir paket varsa, paket yüklenmez.

> [!NOTE]
> Güncelleştirme dağıtımıyla birlikte eklenmesi değiştirilen güncelleştirmeleri belirtemezsiniz.
>

* **Zamanlama ayarları**: **Zamanlama Ayarları** bölmesi açılır. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. **Yinelenme** bölümünde **Bir Kez**'i seçin. Varsayılanı 1 gün olarak bırakın ve **Tamam'ı**seçin. Yinelenen bir zamanlama oluşturulur.

* **Ön betikler + Son betikler**: Dağıtımınızdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [Ön ve Son betikleri yönetme](pre-post-scripts.md).

* **Bakım penceresi (dakika)**: Varsayılan değeri bırakın. Bakım pencereleri güncelleştirmelerin yüklenmesi için izin verilen süreyi denetler. Bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun.

  * Bakım pencereleri kaç güncelleştirmenin yüklenmeden çalışılsın denetlenir.
  * Güncelleştirme Yönetimi, bakım penceresinin sonuna yaklaşıyorsa yeni güncelleştirmeler yüklemeyi durdurmaz.
  * Bakım penceresi aşıldığında, Güncelleştirme Yönetimi devam eden güncelleştirmeleri sonlandırmaz.
  * Windows'da bakım penceresi aşıldıysa, bunun nedeni genellikle bir hizmet paketi güncelleştirmesinin yüklenmesi uzun zaman alır.

  > [!NOTE]
  > Güncelleştirmelerin Ubuntu'daki bir bakım penceresinin dışında uygulanmasını önlemek için, Katılımsız Yükseltme paketini otomatik güncelleştirmeleri devre dışı kilmek için yeniden yapılandırın. Paketin nasıl yapılandırılabildiğini öğrenmek için [Ubuntu Server Guide'daki Otomatik Güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

* **Yeniden başlatma seçenekleri**: Bu ayar, yeniden başlatma işlemlerinin nasıl gerçekleştirileceğini belirler. Kullanılabilen seçenekler:
  * Gerekirse yeniden başlat (Varsayılan)
  * Her zaman yeniden başlat
  * Hiçbir zaman yeniden başlatma
  * Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez

> [!NOTE]
> [Yeniden başlatmayı yönetmek için kullanılan Kayıt Defteri anahtarları](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) altında listelenen Kayıt Defteri anahtarları, Yeniden Başlatma **Denetimi'ni Asla Yeniden Başlatma**olarak ayarlanmışsa yeniden başlatma olayına neden olabilir. **Reboot Control**

Zamanlamayı yapılandırdıktan sonra **Oluştur**'u seçin.

![Güncelleştirme Zamanlama Ayarları bölmesi](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Durum panosu açılır. Oluşturduğunuz dağıtım zamanlamasını göstermek için **Zamanlanan güncelleştirme dağıtımları**'nı seçin.

> [!NOTE]
> Güncelleştirme Yönetimi, birinci taraf güncelleştirmelerini ve indirme öncesi yamalarını dağıtmayı destekler. Bu işlem sistemlerdeki değişikliklere yama yapılmasını gerektirir. Sistemlerinde bu ayarları yapılandırmayı öğrenmek için bkz. [birinci taraf ve indirme öncesi destek](automation-configure-windows-update.md).

**Güncelleştirme Dağıtımları** da programlı olarak oluşturulabilir. REST API ile **Güncelleştirme Dağıtımı'nı** nasıl oluşturabilirsiniz öğrenmek için Bkz. [Yazılım Güncelleştirme Yapılandırmaları - Oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık **Güncelleştirme Dağıtımı**oluşturmak için kullanılabilecek örnek bir runbook da vardır. Bu runbook hakkında daha fazla bilgi edinmek [için](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1)bkz.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz. Dağıtım o anda çalışıyorsa, durum **Sürüyor** şeklinde olur. Dağıtım tamamlandıktan sonra başarılıysa durumu **Başarılı** olarak değişir. Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.

Tamamlanan güncelleştirme dağıtımını seçerek bu güncelleştirme dağıtımının panosunu görebilirsiniz.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/automation-tutorial-update-management/manageupdates-view-results.png)

**Güncelleştirme sonuçları** bölümünde toplam güncelleştirme sayısının bir özeti ve VM'deki dağıtım sonuçları gösterilir. Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir.

Aşağıdaki listede görüntülenebilecek değerler gösterilmiştir:

* **Denenmedi**: Tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* **Başarılı**: Güncelleştirme başarılı oldu.
* **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı**'yı seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgiler için **Hatalar**’ı seçin.

Güncelleştirme dağıtımınız başarılı olursa, dağıtımın başarılı olduğunu göstermek için aşağıdaki örneğe benzer bir e-posta gönderilir:

![E-posta eylem grubunu yapılandırma](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Güncelleştirme Yönetimi için VM ekleme
> * Güncelleştirme değerlendirmesini görüntüleme
> * Uyarıları yapılandırma
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

Güncelleştirme Yönetimi çözümüne genel bakış bölümüne geçin.

> [!div class="nextstepaction"]
> [Güncelleştirme Yönetimi çözümü](automation-update-management.md)

