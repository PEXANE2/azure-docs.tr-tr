---
title: Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme
description: Bu makalede, Azure ve Azure dışı sanal makinelerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Azure Otomasyonu Güncelleştirme Yönetimi kullanma hakkında genel bakış sunulmaktadır.
services: automation
ms.subservice: update-management
ms.topic: tutorial
ms.date: 03/04/2020
ms.custom: mvc
ms.openlocfilehash: 347f2fbc0f12aa775c42dbb14a4625dc509a20ed
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79239661"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme

Güncelleştirme Yönetimi çözümünü kullanarak sanal makineleriniz için güncelleştirmeleri ve yamaları yönetebilirsiniz. Bu öğreticide kullanılabilir durumdaki güncelleştirmelerin durumunu değerlendirmeyi, gerekli güncelleştirmelerin yüklenmesini zamanlamayı, dağıtım sonuçlarını gözden geçirmeyi ve güncelleştirmelerin başarılı bir şekilde uygulandığını doğrulamak için bir uyarı oluşturmayı öğreneceksiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme Yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Güncelleştirme değerlendirmesini görüntüleme
> * Uyarıları yapılandırma
> * Güncelleştirme dağıtımı zamanlama
> * Dağıtım sonuçlarını görüntüleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Güncelleştirme yönetimi](automation-update-management.md) çözümü bir veya daha fazla VM 'niz için etkinleştirildi.
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** bölmesi açılır. Herhangi bir güncelleştirme eksik olarak tanımlanmışsa eksik güncelleştirmeler listesi **eksik güncelleştirmeler** sekmesinde gösterilir.

**Bilgi bağlantısı**altında, güncelleştirme için destek makalesini açmak üzere Güncelleştir bağlantısını seçin. Güncelleştirme hakkında önemli bilgi edinebilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Seçili güncelleştirme için **günlük araması** bölmesini açmak üzere güncelleştirmede başka herhangi bir yere tıklayın. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturarak ortamınıza dağıtılmış olan veya ortamınızda eksik olan güncelleştirmeler hakkında ayrıntılı bilgilere ulaşabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Bu adımda, bir güncelleştirme dağıtımının durumunu bilmenizi sağlamak için bir uyarı ayarlamayı öğreneceksiniz.

### <a name="alert-conditions"></a>Uyarı koşulları

Otomasyon hesabınızda, **izleme** ' nin altında **Uyarılar**' a gidin ve **+ Yeni uyarı kuralı**' na tıklayın.

Otomasyon hesabınız kaynak olarak zaten seçildi. Bunu değiştirmek istiyorsanız, **Seç** ' e tıklayabilir ve **Kaynak Seç** sayfasında, **kaynak türüne göre filtrele** açılır listesinde **Otomasyon hesapları** ' nı seçin. Otomasyon Hesabınızı ve ardından **Bitti**'yi seçin.

Güncelleştirme dağıtımınız için uygun olan sinyali seçmek için **Koşul Ekle** ' ye tıklayın. Aşağıdaki tabloda, güncelleştirme dağıtımları için kullanılabilen iki sinyalin ayrıntıları gösterilmektedir:

|Sinyal adı|Boyutlar|Açıklama|
|---|---|---|
|**Toplam güncelleştirme dağıtımı çalıştırmaları**|-Güncelleştirme dağıtım adı</br>-Durum|Bu sinyal, bir güncelleştirme dağıtımının genel durumuyla ilgili uyarı almak için kullanılır.|
|**Toplam güncelleştirme dağıtım makinesi çalıştırması**|-Güncelleştirme dağıtım adı</br>-Durum</br>-Hedef bilgisayar</br>-Güncelleştirme dağıtımı çalıştırma KIMLIĞI|Bu sinyal, belirli makinelere hedeflenmiş bir güncelleştirme dağıtımının durumuyla ilgili uyarı almak için kullanılır|

Boyut değerleri için listeden geçerli bir değer seçin. Aradığınız değer listede yoksa, boyutun yanındaki **\+** işaretine tıklayın ve özel adı yazın. Daha sonra aramak istediğiniz değeri seçebilirsiniz. Bir boyutun tüm değerlerini seçmek istiyorsanız, **\*Seç** düğmesine tıklayın. Boyut için bir değer belirtmezseniz, bu boyut değerlendirme sırasında yok sayılır.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/signal-logic.png)

**Uyarı mantığı** bölümünde **Eşik** alanına **1** değerini girin. İşiniz bittiğinde **Bitti**'yi seçin.

### <a name="alert-details"></a>Uyarı ayrıntıları

**2 altında. Uyarı ayrıntılarını tanımlayın**, uyarı için bir ad ve açıklama girin. **Önem Derecesi**'ni başarılı çalıştırmalar için **Bilgilendirici (Önem Derecesi 2)** , başarısız çalıştırmalar içinse **Bilgilendirici (Önem Derecesi 1)** olarak ayarlayın.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/define-alert-details.png)

**Eylem grupları**' nın altında **Yeni oluştur**' u seçin. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylemlere e-posta bildirimleri, runbook'lar, web kancaları ve diğer birçok şey dahildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

**Eylem grubu adı** kutusuna uyarı için ad ve kısa ad. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

**Eylemler**' in altında, eylem Için **e-posta bildirimleri**gibi bir ad girin. **Eylem türü**altında **e-posta/SMS/Push/Voice**' ı seçin. **Ayrıntılar**altında **Ayrıntıları Düzenle**' yi seçin.

**E-posta/SMS/Anında İletme/Ses** bölmesine bir ad girin. **E-posta** onay kutusunu seçip geçerli bir e-posta adresi girin.

![E-posta eylem grubu yapılandırma](./media/automation-tutorial-update-management/configure-email-action-group.png)

**E-posta/SMS/gönderim/ses** bölmesinde **Tamam**' ı seçin. **Eylem grubu Ekle** bölmesinde **Tamam**' ı seçin.

Uyarı e-postası konusunu özelleştirmek için, **kural oluştur**altında, **Eylemler Özelleştir**altında **e-posta konusu**' ı seçin. İşleminiz bittiğinde **Uyarı kuralı oluştur**'u seçin. Bu uyarı, bir güncelleştirme dağıtımı başarılı olduğunda sizi uyarır ve güncelleştirme dağıtımı çalıştırmasının hangi makineleri kapsadığını bildirir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Şimdi güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Bir güncelleştirme dağıtımı zamanladığınızda, hedef makinelerdeki güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook 'una bağlı bir [zamanlama](shared-resources/schedules.md) kaynağı oluşturur. Azure portal zamanlama kaynağını veya dağıtımı oluşturduktan sonra PowerShell 'i kullanarak silerseniz, zamanlanan güncelleştirme dağıtımını keser ve portaldan yeniden yapılandırmayı denediğinizde bir hata oluşturur. Yalnızca ilgili dağıtım zamanlamasını silerek zamanlama kaynağını silebilirsiniz.  
>

Yeni bir VM güncelleştirme dağıtımı zamanlamak için **Güncelleştirme yönetimi**'ne gidip **Güncelleştirme dağıtımı zamanla**'yı seçin.

**Yeni güncelleştirme dağıtımı** bölümüne aşağıdaki bilgileri girin:

* **Ad**: Güncelleştirme dağıtımı için benzersiz bir ad girin.

* **İşletim sistemi**: Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

* **Güncelleştirilecek gruplar (önizleme)** : Dağıtımınıza dahil edilecek Azure sanal makinelerinin dinamik grubunu derlemek için bir abonelik, kaynak grupları, konumlar ve etiketler birleşimine göre bir sorgu tanımlayın. Daha fazla bilgi için bkz. [Dinamik Gruplar](automation-update-management-groups.md)

* **Güncelleştirilecek makineler**: Kayıtlı bir aramayı veya İçeri aktarılan grubu seçin veya açılan menüden Makine'yi seçerek belirli makineleri seçin. **Makineler**' i seçerseniz makinenin hazırlığı, **Güncelleştirme Aracısı hazırlığı** sütununda gösterilir. Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md)

* **Güncelleştirme sınıflandırması**: güncelleştirme dağıtımına dahil edilebilir her bir ürün için desteklenen güncelleştirme sınıflandırmalarını seçin. Bu öğreticide tüm türleri seçili halde bırakın.

  Sınıflandırma türleri şunlardır:

   |İşletim Sistemi  |Tür  |
   |---------|---------|
   |Windows     | Kritik güncelleştirmeler</br>Güvenlik güncelleştirmeleri</br>Güncelleştirme paketleri</br>Özellik paketleri</br>Hizmet paketleri</br>Tanım güncelleştirmeleri</br>Araçlar</br>Güncelleştirmeler        |
   |Linux     | Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri</br>Diğer güncelleştirmeler       |

   Sınıflandırma türlerinin açıklaması için bkz. [sınıflandırmaları güncelleştirme](automation-view-update-assessments.md#update-classifications).

* **Dahil edilecek/dışlanacak güncelleştirmeler** - Böylece **Dahil Et / Dışla** sayfası açılır. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir.

> [!NOTE]
> Dışlamaları geçersiz kılmanın dahil olduğunu bildirmek önemlidir. Örneğin, `*`hariç tutma kuralı tanımlarsanız, hepsi hariç tutulduklarında hiçbir düzeltme eki veya paket yüklenmez. Dışlanan düzeltme ekleri hala makinede eksik olarak gösterilir. Linux makineler için bir paket dahil, ancak dışlanan bağımlı bir paket varsa, paket yüklenmez.

> [!NOTE]
> Güncelleştirme dağıtımına dahil edilmek üzere yenisiyle değiştirilen güncelleştirmeleri belirtemezsiniz.
>

* **Zamanlama ayarları**: **Zamanlama Ayarları** bölmesi açılır. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. **Yinelenme** bölümünde **Bir Kez**'i seçin. Varsayılanı 1 gün olarak bırakın ve **Tamam**' ı seçin. Yinelenen bir zamanlama oluşturulur.

* **Ön betikler + Son betikler**: Dağıtımınızdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [Ön ve Son betikleri yönetme](pre-post-scripts.md).

* **Bakım penceresi (dakika)** : Varsayılan değeri bırakın. Bakım pencereleri, güncelleştirmelerin yüklenmesi için izin verilen süre miktarını denetler. Bir bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun.

  * Bakım pencereleri, kaç güncelleştirme yüklenedenebir şekilde kontrol edilir.
  * Bakım penceresinin sonuna yaklaşıyorsa Güncelleştirme Yönetimi yeni güncelleştirmeleri yüklemeyi durdurmaz.
  * Bakım penceresi aşıldığında Güncelleştirme Yönetimi devam eden güncelleştirmeleri sonlandıramaz.
  * Bakım penceresi Windows üzerinde aşılırsa, çoğu zaman bir hizmet paketi güncelleştirmesinin yüklenmesi uzun sürmesi nedeniyle oluşur.

  > [!NOTE]
  > Güncelleştirmelerin Ubuntu 'da bakım penceresi dışında uygulanmasını önlemek için, katılımsız yükseltme paketini otomatik güncelleştirmeleri devre dışı bırakacak şekilde yeniden yapılandırın. Paketin nasıl yapılandırılacağı hakkında daha fazla bilgi için [Ubuntu sunucu kılavuzunda otomatik güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

* **Yeniden başlatma seçenekleri**: Bu ayar, yeniden başlatma işlemlerinin nasıl gerçekleştirileceğini belirler. Kullanılabilen seçenekler:
  * Gerekirse yeniden başlat (Varsayılan)
  * Her zaman yeniden başlat
  * Hiçbir zaman yeniden başlatma
  * Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez

> [!NOTE]
> [Yeniden başlatmayı yönetmek için kullanılan kayıt](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) defteri anahtarları altında listelenen kayıt defteri anahtarları, **yeniden başlatma denetimi** hiçbir şekilde **yeniden başlatma**olarak ayarlandıysa yeniden başlatma olayına neden olabilir.

Zamanlamayı yapılandırdıktan sonra **Oluştur**'u seçin.

![Güncelleştirme Zamanlama Ayarları bölmesi](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Durum panosu açılır. Oluşturduğunuz dağıtım zamanlamasını göstermek için **Zamanlanan güncelleştirme dağıtımları**'nı seçin.

> [!NOTE]
> Güncelleştirme Yönetimi, birinci taraf güncelleştirmelerini ve indirme öncesi yamalarını dağıtmayı destekler. Bu işlem sistemlerdeki değişikliklere yama yapılmasını gerektirir. Sistemlerinde bu ayarları yapılandırmayı öğrenmek için bkz. [birinci taraf ve indirme öncesi destek](automation-configure-windows-update.md).

**Güncelleştirme dağıtımları** da programlı bir şekilde oluşturulabilir. REST API **güncelleştirme dağıtımı** oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık **güncelleştirme dağıtımı**oluşturmak için kullanılabilecek bir örnek runbook vardır. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

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
> [Güncelleştirme Yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)

