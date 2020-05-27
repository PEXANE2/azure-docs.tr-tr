---
title: Azure Otomasyonu 'nda Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme
description: Bu makalede, Azure VM 'lerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Güncelleştirme Yönetimi nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 04/06/2020
ms.custom: mvc
ms.openlocfilehash: 5b5172df6ed6993742a08d5ac08cf700681dfc6a
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83829163"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme

Bu makalede, Azure sanal makinelerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Azure Otomasyonu [güncelleştirme yönetimi](automation-update-management.md) özelliğini nasıl kullanabileceğiniz açıklanır. 

Fiyatlandırma bilgisi için bkz. [Güncelleştirme Yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

## <a name="prerequisites"></a>Ön koşullar

* [Güncelleştirme yönetimi](automation-update-management.md) özelliği, VM 'lerinizin bir veya daha fazlası için etkinleştirildi. 
* Güncelleştirme Yönetimi için etkin bir [sanal makine](../virtual-machines/windows/quick-create-portal.md) .

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirdikten sonra, güncelleştirme yönetimi sayfası açılır. Herhangi bir güncelleştirme eksik olarak tanımlanmışsa eksik güncelleştirmeler listesi **eksik güncelleştirmeler** sekmesinde gösterilir.

**Bilgi bağlantısı**altında, güncelleştirme için destek makalesini açmak üzere Güncelleştir bağlantısını seçin. Güncelleştirme hakkında önemli bilgi edinebilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Seçili güncelleştirme için günlük araması bölmesini açmak üzere güncelleştirmede başka herhangi bir yere tıklayın. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Ortamınızda dağıtılan veya eksik güncelleştirmeler hakkında ayrıntılı bilgileri görüntülemek için bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturabilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Bu adımda, bir güncelleştirme dağıtımının durumunu bilmenizi sağlamak için bir uyarı ayarlamayı öğreneceksiniz.

### <a name="alert-conditions"></a>Uyarı koşulları

Otomasyon hesabınızda, **izleme**altındaki **Uyarılar** ' a gidin ve ardından **Yeni uyarı kuralı**' nı tıklatın.

Otomasyon hesabınız kaynak olarak zaten seçildi. Değiştirmek istiyorsanız, **Seç**' e tıklayın. Kaynak seçin sayfasında, **kaynak türüne göre filtrele** açılan menüsünden **Otomasyon hesapları** ' nı seçin. Otomasyon hesabınızı seçin ve **bitti**' ye tıklayın.

Güncelleştirme dağıtımınız için uygun olan sinyali seçmek için **Koşul Ekle** ' ye tıklayın. Aşağıdaki tabloda, iki kullanılabilir sinyalin ayrıntıları gösterilmektedir.

|Sinyal adı|Boyutlar|Açıklama|
|---|---|---|
|`Total Update Deployment Runs`|-Güncelleştirme dağıtım adı<br>-Durum|Bu sinyal, bir güncelleştirme dağıtımının genel durumu hakkında uyarır.|
|`Total Update Deployment Machine Runs`|-Güncelleştirme dağıtım adı</br>-Durum</br>-Hedef bilgisayar</br>-Güncelleştirme dağıtımı çalıştırma KIMLIĞI|Bu sinyal, belirli makinelere hedeflenmiş bir güncelleştirme dağıtımının durumu hakkında uyarır.|

Bir boyut için listeden geçerli bir değer seçin. İstediğiniz değer listede yoksa, **\+** boyutun yanındaki işaretine tıklayın ve özel adı yazın. Ardından aranacak değeri seçin. Bir boyutun tüm değerlerini seçmek istiyorsanız, **Seç \* ** düğmesine tıklayın. Boyut için bir değer seçmeyin, Güncelleştirme Yönetimi bu boyutu yoksayar.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/signal-logic.png)

**Uyarı mantığı** bölümünde **Eşik** alanına **1** değerini girin. İşiniz bittiğinde **Bitti**'yi seçin.

### <a name="alert-details"></a>Uyarı ayrıntıları

**2 altında. Uyarı ayrıntılarını tanımlayın**, uyarı için bir ad ve açıklama girin. **Önem Derecesi**'ni başarılı çalıştırmalar için **Bilgilendirici (Önem Derecesi 2)**, başarısız çalıştırmalar içinse **Bilgilendirici (Önem Derecesi 1)** olarak ayarlayın.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-update-management/define-alert-details.png)

**Eylem grupları**' nın altında **Yeni oluştur**' u seçin. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylemler e-posta bildirimleri, runbook 'lar, Web kancaları ve çok daha fazlasını içerebilir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

**Eylem grubu adı** alanında, uyarı için bir ad ve kısa bir ad girin. Güncelleştirme Yönetimi, belirtilen grup kullanılarak bildirim gönderilirken tam eylem grubu adının yerine kısa adı kullanır.

**Eylemler**' in altında, eylem Için **e-posta bildirimi**gibi bir ad girin. **Eylem türü**Için **e-posta/SMS/Push/Voice**' ı seçin. **Ayrıntılar**Için **Ayrıntıları Düzenle**' yi seçin.

E-posta/SMS/Anında İletme/Ses bölmesine bir ad girin. **E-posta** onay kutusunu seçin ve geçerli bir e-posta adresi girin.

![E-posta eylem grubu yapılandırma](./media/automation-tutorial-update-management/configure-email-action-group.png)

E-posta/SMS/gönderim/ses bölmesinde **Tamam**' ı tıklatın. Eylem grubu Ekle bölmesinde **Tamam**' ı tıklatın.

Uyarı e-postası konusunu özelleştirmek için, **kural oluştur**altında, **Eylemler Özelleştir**altında **e-posta konusu**' ı seçin. İşleminiz bittiğinde **Uyarı kuralı oluştur**'u seçin. Uyarı, bir güncelleştirme dağıtımının başarılı olduğunu ve hangi makinelerin güncelleştirme dağıtım çalıştırmasının bir parçası olduğunu söyler.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma dahil edilecek güncelleştirme türlerini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Güncelleştirme dağıtımının zamanlanması, hedef makinelerdeki güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook 'una bağlı bir [zamanlama](shared-resources/schedules.md) kaynağı oluşturur. Azure portal zamanlama kaynağını veya dağıtımı oluşturduktan sonra PowerShell 'i kullanarak silerseniz, silme işlemi zamanlanmış güncelleştirme dağıtımını keser ve zamanlama kaynağını portaldan yeniden yapılandırmayı denediğinizde bir hata oluşturur. Yalnızca ilgili dağıtım zamanlamasını silerek zamanlama kaynağını silebilirsiniz.  

Yeni bir VM güncelleştirme dağıtımı zamanlamak için **Güncelleştirme yönetimi**'ne gidip **Güncelleştirme dağıtımı zamanla**'yı seçin.

**Yeni güncelleştirme dağıtımı** bölümüne aşağıdaki bilgileri girin:

* **Ad**: Güncelleştirme dağıtımı için benzersiz bir ad girin.

* **İşletim sistemi**: Güncelleştirme dağıtımı için hedeflenecek işletim sistemini seçin.

* **Güncelleştirilecek gruplar (Önizleme)**: dağıtımınıza dahil etmek üzere Azure VM 'lerin dinamik bir grubunu derlemek için aboneliği, kaynak gruplarını, konumları ve etiketleri birleştiren bir sorgu tanımlayın. Daha fazla bilgi için bkz. [Dinamik Gruplar](automation-update-management-groups.md).

* **Güncelleştirilecek makineler**: açılan menüden kaydedilmiş bir arama, içeri aktarılan grup veya Select **makineleri** seçin ve tek tek makineleri seçin. **Makineler**' i seçerseniz, her makinenin hazırlığı **Güncelleştirme Aracısı hazırlığı** sütununda gösterilir. Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md).

* **Güncelleştirme sınıflandırması**: her bir ürün için, desteklenen tüm güncelleştirme sınıflandırmalarının seçimini kaldırın, ancak güncelleştirme dağıtımınıza dahil olanlar. Sınıflandırma türlerinin açıklamaları için bkz. [güncelleştirme sınıflandırmaları](automation-view-update-assessments.md#work-with-update-classifications).

* **Dahil edilecek/hariç tutulacak güncelleştirmeler** -dahil etme/hariç tutma sayfasını açar. Dahil edilecek veya hariç tutulacak güncelleştirmeler, KB makalesi KIMLIK numaralarını belirterek ayrı sekmelerde bulunur. Bir veya daha fazla KIMLIK numarası belirtirken güncelleştirme dağıtımıyla tüm sınıflandırmaların kaldırılması veya işaretini kaldırmanız gerekir. Bu, güncelleştirme kimlikleri belirtildiğinde güncelleştirme paketinize başka hiçbir güncelleştirme eklenmemesini sağlar.

> [!NOTE]
> Dışlamaları geçersiz kılmanın dahil olduğunu bilmemiz önemlidir. Örneğin, bir dışlama kuralı tanımlarsanız `*` , güncelleştirme yönetimi hepsi hariç tutuldukları için düzeltme eki veya paket içermez. Dışlanan düzeltme ekleri hala makinede eksik olarak gösterilir. Linux makineler için, dışlanan bağımlı paketi olan bir paketi eklerseniz Güncelleştirme Yönetimi ana paketi yüklemez.

> [!NOTE]
> Güncelleştirme dağıtımına dahil edilmek üzere yenisiyle değiştirilen güncelleştirmeleri belirtemezsiniz.
>
* **Zamanlama ayarları**: Zamanlama Ayarları bölmesi açılır. Varsayılan başlangıç zamanı, geçerli zamandan 30 dakika sonradır. Başlangıç zamanını en düşük 10 dakika olmak üzere istediğiniz değere ayarlayabilirsiniz.

   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. **Yinelenme** bölümünde **Bir Kez**'i seçin. Varsayılanı 1 gün olarak bırakın ve **Tamam**' a tıklayın. Bu girişler yinelenen bir zamanlama ayarlar.

* **Ön betikler + Son betikler**: Dağıtımınızdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [Ön ve Son betikleri yönetme](pre-post-scripts.md).

* **Bakım penceresi (dakika)**: Varsayılan değeri bırakın. Bakım pencereleri, güncelleştirmelerin yüklenmesi için izin verilen süre miktarını denetler. Bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun:

  * Bakım pencereleri kaç güncelleştirme yüklendiğini denetler.
  * Bakım penceresinin sonuna yaklaşıyorsa Güncelleştirme Yönetimi yeni güncelleştirmeleri yüklemeyi durdurmaz.
  * Bakım penceresi aşılırsa Güncelleştirme Yönetimi devam eden güncelleştirmeleri sonlandıramaz.
  * Bakım penceresi Windows üzerinde aşılırsa, çoğu zaman bir hizmet paketi güncelleştirmesinin yüklenmesi uzun sürmesi nedeniyle oluşur.

  > [!NOTE]
  > Güncelleştirmelerin Ubuntu 'da bakım penceresi dışında uygulanmasını önlemek için, katılımsız yükseltme paketini otomatik güncelleştirmeleri devre dışı bırakacak şekilde yeniden yapılandırın. Paketin nasıl yapılandırılacağı hakkında daha fazla bilgi için [Ubuntu sunucu kılavuzunda otomatik güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

* **Yeniden başlatma seçenekleri: yeniden**başlatmalar işleme seçeneklerini belirtmek için kullanın. Aşağıdaki seçenekler mevcuttur:
  * Gerekirse yeniden Başlat (varsayılan)
  * Her zaman yeniden başlat
  * Hiçbir zaman yeniden başlatma
  * Yalnızca yeniden Başlat-güncelleştirmeleri yüklemez

> [!NOTE]
> [Yeniden başlatmayı yönetmek için kullanılan kayıt](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) defteri anahtarları altında listelenen kayıt defteri anahtarları, **yeniden başlatma seçenekleri** hiçbir şekilde **yeniden başlatma**olarak ayarlandıysa yeniden başlatma olayına neden olabilir.

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur**' a tıklayın.

![Güncelleştirme Zamanlama Ayarları bölmesi](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Durum panosu açılır. Yeni oluşturulan dağıtım zamanlamasını göstermek için **Zamanlanmış güncelleştirme dağıtımları** ' nı seçin.

> [!NOTE]
> Güncelleştirme Yönetimi, birinci taraf güncelleştirmelerin dağıtılmasını ve düzeltme eklerinin önceden indirilmesini destekler. Bu destek, düzeltme eki eklenen sistemlerde değişiklik yapılmasını gerektirir. Bu ayarları sistemlerinizde nasıl yapılandıracağınızı öğrenmek için [ilk taraf ve indirme öncesi destek](automation-configure-windows-update.md) bölümüne bakın.

Ayrıca, programlama yoluyla güncelleştirme dağıtımları da oluşturabilirsiniz. REST API güncelleştirme dağıtımı oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca haftalık güncelleştirme dağıtımı oluşturmak için kullanabileceğiniz örnek bir runbook vardır. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz. Dağıtım o anda çalışıyorsa, durum **Sürüyor** şeklinde olur. Dağıtım başarıyla tamamlandığında durum **başarılı**olarak değişir. Dağıtımda bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **kısmen başarısız**olur.

Panosunu görmek için tamamlanan güncelleştirme dağıtımını seçin.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/automation-tutorial-update-management/manageupdates-view-results.png)

**Güncelleştirme sonuçları** bölümünde toplam güncelleştirme sayısının bir özeti ve VM'deki dağıtım sonuçları gösterilir. Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir.

Kullanılabilir değerler şunlardır:

* **Denenmedi**: sağlanan bakım penceresi süresine göre yeterli kullanılabilir zaman olmadığından güncelleştirme yüklenmedi.
* **Başarılı**: Güncelleştirme başarılı oldu.
* **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm Günlükler** ' i seçin.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı**'yı seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgiler için **Hatalar**’ı seçin.

Güncelleştirme dağıtımınız başarılı olduğunda, aşağıdakine benzer bir onaylama e-postası alırsınız:

![E-posta eylem grubunu yapılandırma](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

* Güncelleştirme Yönetimi Ayrıntılar için bkz. [güncelleştirme yönetimi genel bakış](automation-update-management.md).