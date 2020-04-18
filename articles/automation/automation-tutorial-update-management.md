---
title: Azure VM'leriniz için güncelleştirmeleri ve yamaları yönetme
description: Bu makalede, Azure ve Azure olmayan VM'leriniziçin güncelleştirmeleri ve yamaları yönetmek için Azure Otomasyon Güncelleştirme Yönetimi'nin nasıl kullanılacağına genel bir bakış sağlanmaktadır.
services: automation
ms.subservice: update-management
ms.topic: tutorial
ms.date: 04/06/2020
ms.custom: mvc
ms.openlocfilehash: 62c661f75aef77117a61be7e802562e6dde17ba5
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604685"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Azure VM'leriniz için güncelleştirmeleri ve yamaları yönetme

Güncelleştirme Yönetimi çözümünü kullanarak sanal makineleriniz için güncelleştirmeleri ve yamaları yönetebilirsiniz. Bu öğreticide kullanılabilir durumdaki güncelleştirmelerin durumunu değerlendirmeyi, gerekli güncelleştirmelerin yüklenmesini zamanlamayı, dağıtım sonuçlarını gözden geçirmeyi ve güncelleştirmelerin başarılı bir şekilde uygulandığını doğrulamak için bir uyarı oluşturmayı öğreneceksiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme Yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

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

Güncelleştirme Yönetimini etkinleştirdikten sonra, Güncelleştirme yönetimi sayfası açılır. Herhangi bir güncelleştirme eksik olarak tanımlanırsa, Eksik **güncelleştirmelerin** listesi Eksik güncelleştirmeler sekmesinde gösterilir.

**Bilgi bağlantısı**altında, güncelleştirme için destek makalesini açmak için güncelleştirme bağlantısını seçin. Güncelleştirme hakkında önemli bilgiler edinebilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Seçili güncelleştirmenin Günlük arama bölmesini açmak için güncellemede başka bir yere tıklayın. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Ortamınızda dağıtılan veya kaybolan güncelleştirmeler hakkında ayrıntılı bilgileri görüntülemek için bu sorgunuzu değiştirebilir veya kendi sorgunuzu oluşturabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Bu adımda, bir güncelleştirme dağıtımının durumunu bildirmek için bir uyarı ayarlamayı öğrenirsiniz.

### <a name="alert-conditions"></a>Uyarı koşulları

Otomasyon hesabınızda, **İzleme**altındaki **Uyarılar'a** gidin ve ardından Yeni **uyarı kuralını**tıklatın.

Otomasyon hesabınız zaten kaynak olarak seçilmiştir. Değiştirmek **istiyorsanız, Seç'i**tıklatın. Kaynak seç sayfasında, kaynak türü açılır menüsüne **göre Filtre'den** Otomasyon **Hesapları'nı** seçin. Otomasyon hesabınızı seçin ve **ardından Bitti'yi**tıklatın.

Güncelleştirme dağıtımınız için uygun sinyali seçmek için **Ekle koşulunu** tıklatın. Aşağıdaki tablo, kullanılabilir iki sinyalin ayrıntılarını gösterir.

|Sinyal Adı|Boyutlar|Açıklama|
|---|---|---|
|`Total Update Deployment Runs`|- Dağıtım Adını Güncelleştir<br>- Durum|Bu sinyal, bir güncelleştirme dağıtımının genel durumu hakkında uyarır.|
|`Total Update Deployment Machine Runs`|- Dağıtım Adını Güncelleştir</br>- Durum</br>- Hedef Bilgisayar</br>- Dağıtım Çalıştırılanın Kimliğini Güncelleştir|Bu sinyal, belirli makineleri hedefleyen bir güncelleştirme dağıtımının durumu hakkında uyarır.|

Bir boyut için listeden geçerli bir değer seçin. İstediğiniz değer listede yoksa, boyutun **\+** yanındaki işareti tıklatın ve özel adı yazın. Ardından, arayacak değeri seçin. Bir boyut için tüm değerleri seçmek istiyorsanız, **Seç \* ** düğmesini tıklatın. Bir boyut için bir değer seçmezseniz, Güncelleştirme Yönetimi bu boyutu yoksayılsa.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/signal-logic.png)

**Uyarı mantığı** bölümünde **Eşik** alanına **1** değerini girin. İşiniz bittiğinde **Bitti**'yi seçin.

### <a name="alert-details"></a>Uyarı ayrıntıları

**2'nin altında. Uyarı ayrıntılarını tanımlayın,** uyarı için bir ad ve açıklama girin. **Önem Derecesi**'ni başarılı çalıştırmalar için **Bilgilendirici (Önem Derecesi 2)**, başarısız çalıştırmalar içinse **Bilgilendirici (Önem Derecesi 1)** olarak ayarlayın.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/define-alert-details.png)

**Eylem grupları altında,** **Yeni Oluştur'u**seçin. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylemler e-posta bildirimleri, runbook'lar, webhooks ve daha birçok içerebilir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

Eylem **grubu adı** alanına, uyarı için bir ad ve kısa bir ad girin. Güncelleştirme Yönetimi, belirtilen grubu kullanarak bildirim gönderirken tam eylem grubu adı yerine kısa adı kullanır.

**Eylemler**altında, **E-posta Bildirimi**gibi eylem için bir ad girin. **Eylem Türü**için **E-posta/SMS/Push/Voice'ı**seçin. **Ayrıntılar**için, **ayrıntıları edit'i**seçin.

E-posta/SMS/Anında İletme/Ses bölmesine bir ad girin. **E-posta** onay kutusunu seçin ve ardından geçerli bir e-posta adresi girin.

![E-posta eylem grubu yapılandırma](./media/automation-tutorial-update-management/configure-email-action-group.png)

E-posta/SMS/Push/Voice bölmesinde **Tamam'ı**tıklatın. Eylem grubu ekle bölmesinde **Tamam'ı**tıklatın.

Uyarı e-postasının konusunu özelleştirmek için, **Oluşturma kuralı**altında , **eylemleri Özelleştir**altında, **E-posta konusunu**seçin. İşleminiz bittiğinde **Uyarı kuralı oluştur**'u seçin. Uyarı, güncelleştirme dağıtımının ne zaman başarılı olduğunu ve güncelleştirme dağıtım çalışmasının hangi makinelerin parçası olduğunu bildirir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Şimdi güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma dahil etmek için güncelleştirme türlerini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Güncelleştirme dağıtımının zamanlanması, hedef makinelerde güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook'a bağlı bir [zamanlama](shared-resources/schedules.md) kaynağı oluşturur. Zamanlama kaynağını Azure portalından silerseniz veya dağıtımı oluşturduktan sonra PowerShell'i kullanırsanız, silme işlemi zamanlanan güncelleştirme dağıtımını bozar ve zamanlama kaynağını portaldan yeniden yapılandırmaya çalıştığınızda bir hata oluşturur. Zamanlama kaynağını yalnızca ilgili dağıtım zamanlamasını silerek silebilirsiniz.  

Yeni bir VM güncelleştirme dağıtımı zamanlamak için **Güncelleştirme yönetimi**'ne gidip **Güncelleştirme dağıtımı zamanla**'yı seçin.

**Yeni güncelleştirme dağıtımı** bölümüne aşağıdaki bilgileri girin:

* **Ad**: Güncelleştirme dağıtımı için benzersiz bir ad girin.

* **İşletim sistemi**: Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

* **Güncelleştirecek gruplar (önizleme)**: Dağıtımınıza dahil etmek üzere dinamik bir Azure VM grubu oluşturmak için abonelik, kaynak grupları, konumlar ve etiketleri birleştiren bir sorgu tanımlayın. Daha fazla bilgi için [Dinamik Gruplar'a](automation-update-management-groups.md)bakın.

* **Güncelleştirilecek Makineler**: Kaydedilen arama, Alınan grup veya açılan menüden **Makineler'i** seçin ve tek tek makineleri seçin. **Makineler'i**seçerseniz, her makinenin hazır durumu **Güncelleştirme aracısı hazır olma** sütununda gösterilir. Azure Monitor günlüklerinde bilgisayar grupları oluşturmanın farklı yöntemleri hakkında bilgi edinmek için [Azure Monitor günlüklerinde bilgisayar gruplarına](../azure-monitor/platform/computer-groups.md)bakın.

* **Güncelleştirme sınıflandırması**: Her ürün için, desteklenen tüm güncelleştirme sınıflandırmalarını ancak güncelleştirme dağıtımınıza dahil olacak sınıflandırmaları seçin. Bu öğretici için, tüm ürünler için seçilen tüm türleri bırakın.

  Sınıflandırma türleri şunlardır:

   |İşletim Sistemi  |Tür  |
   |---------|---------|
   |Windows     | Kritik güncelleştirmeler</br>Güvenlik güncelleştirmeleri</br>Güncelleştirme paketleri</br>Özellik paketleri</br>Hizmet paketleri</br>Tanım güncelleştirmeleri</br>Araçlar</br>Güncelleştirmeler<br>Sürücü        |
   |Linux     | Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri</br>Diğer güncelleştirmeler       |

   Sınıflandırma türlerinin açıklamaları için [bkz.](automation-view-update-assessments.md#update-classifications)

* **Ekle/hariç tutmak için güncelleştirmeler - Ekle/Hariç** tut sayfasını açar. Eklenecek veya dışlanacak güncelleştirmeler, KB Madde Kimlik numaraları belirtilmek suretiyle ayrı sekmelerde bulunur. Bir veya daha fazla kimlik numarası belirtirken, güncelleştirme dağıtımıyla birlikte tüm sınıflandırmaları kaldırmanız veya denetlemeniz gerekir. Bu, güncelleştirme iI'lerini belirtirken güncelleştirme paketinizde başka güncelleştirmenin dahil olmamasını sağlar.

> [!NOTE]
> Dışlamaların kapsayıcıları geçersiz kdığını bilmek önemlidir. Örneğin, bir dışlama kuralı `*`tanımlarsanız, Update Management, tümü hariç olduğu için hiçbir düzeltme eki veya paket yüklemez. Dışlanmış yamalar hala makineden eksik olarak göstermektedir. Linux makineleri için, dışlanmış bağımlı bir pakete sahip bir paket eklerseniz, Güncelleştirme Yönetimi ana paketi yüklemez.

> [!NOTE]
> Güncelleştirme dağıtımıyla birlikte eklenmesi değiştirilen güncelleştirmeleri belirtemezsiniz.
>

* **Zamanlama ayarları**: Zamanlama Ayarları bölmesi açılır. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. **Yinelenme** bölümünde **Bir Kez**'i seçin. Varsayılanı 1 gün olarak bırakın ve **Tamam'ı**tıklatın. Bu girişler yinelenen bir zamanlama ayarlar.

* **Ön betikler + Son betikler**: Dağıtımınızdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [Ön ve Son betikleri yönetme](pre-post-scripts.md).

* **Bakım penceresi (dakika)**: Varsayılan değeri bırakın. Bakım pencereleri güncelleştirmelerin yüklenmesi için izin verilen süreyi denetler. Bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun:

  * Bakım pencereleri kaç güncelleştirmenin yüklü olduğunu denetler.
  * Güncelleştirme Yönetimi, bakım penceresinin sonu yaklaşıyorsa yeni güncelleştirmeler yüklemeyi durdurmaz.
  * Güncelleştirme Yönetimi, bakım penceresi aşıldığında devam eden güncelleştirmeleri sonlandırmaz.
  * Windows'da bakım penceresi aşıldıysa, bunun nedeni genellikle bir hizmet paketi güncelleştirmesinin yüklenmesinin uzun zaman almasıdır.

  > [!NOTE]
  > Güncelleştirmelerin Ubuntu'daki bir bakım penceresinin dışında uygulanmasını önlemek için, Katılımsız Yükseltme paketini otomatik güncelleştirmeleri devre dışı kilmek için yeniden yapılandırın. Paketin nasıl yapılandırılabildiğini öğrenmek için [Ubuntu Server Guide'daki Otomatik Güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

* **Yeniden başlatma seçenekleri**: Yeniden başlatma işlemlerini işlemek için seçenekleri belirtmek için kullanın. Aşağıdaki seçenekler mevcuttur:
  * Gerekirse yeniden başlatma (varsayılan)
  * Her zaman yeniden başlat
  * Hiçbir zaman yeniden başlatma
  * Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez

> [!NOTE]
> [Yeniden başlatmayı yönetmek için kullanılan Kayıt Defteri anahtarları](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) altında listelenen kayıt defteri anahtarları, yeniden başlatma **seçenekleri** asla yeniden başlatılacaya ayarlanırsa yeniden başlatma olayına neden olabilir. **Never reboot**

Zamanlamayı yapılandırmayı bitirdiğinizde **Oluştur'u**tıklatın.

![Güncelleştirme Zamanlama Ayarları bölmesi](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Durum panosu açılır. Yalnızca oluşturulan dağıtım zamanlamasını göstermek için **Zamanlanmış Güncelleştir dağıtımlarını** seçin.

> [!NOTE]
> Update Management, birinci taraf güncelleştirmeleri dağıtmayı ve ön indirme düzeltme emlelerini destekler. Bu destek, yamalı sistemlerde değişiklikler gerektirir. Sistemlerinizde bu ayarları nasıl yapılandırılabildiğini öğrenmek için [Birinci taraf ve ön indirme desteğine](automation-configure-windows-update.md) bakın.

Güncelleştirme dağıtımlarını programlı olarak da oluşturabilirsiniz. REST API ile nasıl bir güncelleştirme dağıtımı oluşturabilirsiniz öğrenmek için Bkz. [Yazılım Güncelleştirme Yapılandırmaları - Oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık güncelleştirme dağıtımı oluşturmak için kullanabileceğiniz örnek bir runbook da vardır. Bu runbook hakkında daha fazla bilgi edinmek [için](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1)bkz.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz. Dağıtım o anda çalışıyorsa, durum **Sürüyor** şeklinde olur. Dağıtım başarıyla bittiğinde, durum **Başarılı**olarak değişir. Dağıtımda bir veya daha fazla güncelleştirmeyle ilgili hatalar varsa, durum **kısmen başarısız olur.**

Panosunu görmek için tamamlanmış güncelleştirme dağıtımını seçin.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/automation-tutorial-update-management/manageupdates-view-results.png)

**Güncelleştirme sonuçları** bölümünde toplam güncelleştirme sayısının bir özeti ve VM'deki dağıtım sonuçları gösterilir. Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir.

Kullanılabilir değerler şunlardır:

* **Denenmedi**: Tanımlanan bakım penceresi süresine bağlı olarak yeterli zaman olmadığı için güncelleştirme yüklenmedi.
* **Başarılı**: Güncelleştirme başarılı oldu.
* **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükleri** seçin.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı**'yı seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgiler için **Hatalar**’ı seçin.

Güncelleştirme dağıtımınız başarılı olduğunda, aşağıdakilere benzer bir onay e-postası alırsınız:

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