---
title: Birden fazla Azure sanal makinesi için güncelleştirmeleri yönetme
description: Bu makalede, Azure ve Azure olmayan sanal makineler için güncelleştirmelerin nasıl yönetilenbir şekilde yönetilen.
services: automation
ms.subservice: update-management
ms.date: 03/26/2020
ms.topic: conceptual
ms.openlocfilehash: c9a3c88ea0c3e656adf0f8c514b418cfc07c9590
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335767"
---
# <a name="manage-updates-for-multiple-machines"></a>Birden çok makine için güncelleştirmeleri yönetme

Windows ve Linux sanal makineleriniz için güncelleştirmeleri ve yamaları yönetmek için Güncelleme Yönetimi çözümlerini kullanabilirsiniz. [Azure Otomasyonu](automation-offering-get-started.md) hesabınızdan şunları yapabilirsiniz:

- Yerleşik sanal makineler
- Kullanılabilir güncelleştirmelerin durumunu değerlendirme
- Gerekli güncelleştirmelerin zamanyüklemesini zamanlama
- Güncelleştirme Yönetimi etkinleştirilmiş tüm sanal makinelere güncelleştirmelerin başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirin

## <a name="prerequisites"></a>Ön koşullar

Güncelleştirme Yönetimi'ni kullanmak için şunları yapmanız gerekir:

- Desteklenen işletim sistemlerinden birinin yüklü olduğu bir sanal makine veya bilgisayar.

- Çözüme bağlı Linux VM'leri için bir güncelleştirme deposuna erişim.

Güncelleştirme Yönetimi için sistem gereksinimleri hakkında bilgi edinmek için, [Güncelleştirme Yönetimi istemci gereksinimlerine](automation-update-management.md#clients)bakın.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Azure sanal makineleri için Güncelleştirme Yönetimini etkinleştirme

Azure portalında Otomasyon hesabınızı açın ve ardından **Yönetim güncelleştir'i**seçin.

**Azure VM ekle'yi**seçin.

![Azure VM ekle sekmesi](./media/manage-update-multi/update-onboard-vm.png)

Eklemek için bir sanal makine seçin.

**Güncelleştirme Yönetimini Etkinleştir**altında, sanal makinede **Etkinleştir'i** seçin.

![Güncelleştirme Yönetimini Etkinleştir iletişim kutusu](./media/manage-update-multi/update-enable.png)

Onboarding bittiğinde, Sanal makineniz için Güncelleştirme Yönetimi etkinleştirilir.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Azure'a uygun olmayan sanal makineler ve bilgisayarlar için Güncelleştirme Yönetimini etkinleştirme

Windows ve Linux için Log Analytics aracısının, Güncelleştirme Yönetimi ile bunları etkinleştirmek için kurumsal ağınızda veya diğer bulut ortamında çalışan VM'lere yüklenmesi gerekir. Aracıyı Azure dışında barındırılan makinelere dağıtmak için sistem gereksinimlerini ve desteklenen yöntemleri öğrenmek için, [Log Analytics aracısına Genel Bakış](../azure-monitor/platform/log-analytics-agent.md)bölümüne bakın.

## <a name="view-computers-attached-to-your-automation-account"></a>Otomasyon hesabınıza bağlı bilgisayarları görüntüleme

Makineleriniz için Güncelleştirme Yönetimi'ni etkinleştirdikten sonra, **Bilgisayarlar'ı**seçerek makine bilgilerini görüntüleyebilirsiniz. *Makine adı,* *uyumluluk durumu,* *ortam,* *işletim sistemi türü,* *kritik ve güvenlik güncelleştirmeleri yüklü,* *diğer güncelleştirmeler yüklü*ve bilgisayarlarınız için aracı *hazırlığını güncelleştirme* hakkında bilgi görebilirsiniz.

  ![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-computers-tab.png)

Son zamanlarda Güncelleştirme Yönetimi için etkinleştirilen bilgisayarlar henüz değerlendirilmemiş olabilir. Bu bilgisayarlar için uyumluluk durumu **durumu değerlendirilmez.** Uyumluluk durumu için olası değerlerin bir listesi aşağıda vevereme durumu vereme durumu aşağıda veda edebilirsiniz:

- **Uyumlu**: Kritik veya güvenlik güncelleştirmelerini eksik olmayan bilgisayarlar.

- **Uyumlu olmayan**: En az bir kritik veya güvenlik güncelleştirmesi eksik olan bilgisayarlar.

- **Değerlendirilmemiştir**: Güncelleştirme değerlendirme verileri beklenen zaman dilimi içinde bilgisayardan alınmamıştır. Linux bilgisayarlar için, beklenen zaman dilimi son bir saat içinde. Windows bilgisayarlarda beklenen zaman dilimi son 12 saattir.

Aracının durumunu görüntülemek için, **temsilciye hazırlık** sütunundaki bağlantıyı seçin. Bu seçeneği **nseçilmesi, Karma İşçi** bölmesini açar ve Karma İşçi'nin durumunu gösterir. Aşağıdaki resim, uzun bir süre güncelleştirme yönetimine bağlı olmayan bir aracının bir örneğini gösterir:

![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** bölmesi açılır. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

## <a name="collect-data"></a>Veri toplama

Sanal makinelere ve bilgisayarlara yüklenen aracılar güncelleştirmelerle ilgili veri toplar. Aracılar verileri Azure Update Management'a gönderir.

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Update Management, Windows aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve gerekli güncelleştirmelerin yüklenmesini başlatır. |
| Linux aracıları |Evet |Update Management, Linux ajanlarından sistem güncelleştirmeleri hakkında bilgi toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklenmesini başlatır. |
| Operations Manager yönetim grubu |Evet |Update Management, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar. |
| Azure Depolama hesabınızın |Hayır |Azure Depolama, sistem güncelleştirmeleri hakkında bilgi içermez. |

### <a name="collection-frequency"></a>Toplama sıklığı

Bilgisayar güncelleştirme uyumluluğu için bir taramaya çıktıktan sonra, aracı bilgileri toplu olarak Azure Monitor günlüklerine iletir. Windows bilgisayarında uyumluluk tonu varsayılan olarak her 12 saatte bir çalıştırılır.

Taramaya ek olarak, güncelleştirme uyumluluğu için taramaya, MMA'nın yeniden başlatılmasından sonraki 15 dakika içinde, güncelleştirme yüklemesinden önce ve güncelleştirme yüklemesinden sonra başlatılır.

Bir Linux bilgisayarı için uyumluluk tonu varsayılan olarak her saat başı gerçekleştirilir. MMA aracısı yeniden başlatılırsa, 15 dakika içinde uyumluluk tayini başlatılır.

Yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin panoda görüntülenmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için, sürüm zamanlamanızla ve hizmet pencerenizle uyumlu bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Bir güncelleştirme dağıtımı zamanladiğinizde, hedef makinelerde güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook'a bağlı bir [zamanlama](shared-resources/schedules.md) kaynağı oluşturur. Zamanlama kaynağını Azure portalından silerseniz veya dağıtımı oluşturduktan sonra PowerShell'i kullanırsanız, zamanlanan güncelleştirme dağıtımını bozar ve portaldan yeniden yapılandırmaya çalıştığınızda bir hata oluşturur. Zamanlama kaynağını yalnızca ilgili dağıtım zamanlamasını silerek silebilirsiniz.
>

Bir veya daha fazla sanal makine için yeni bir güncelleştirme dağıtımı **zamanlamak**için, Güncelleştirme yönetimi altında, **Güncelle'yi**Zamanla'yı seçin.

Yeni **güncelleştirme dağıtım** bölmesinde aşağıdaki bilgileri belirtin:

- **Adı**: Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad girin.
- **İşletim sistemi**: **Windows** veya **Linux'u**seçin.
- **Güncelleştirme grupları**: Dağıtımınıza dahil etmek üzere dinamik bir Azure VM grubu oluşturmak için abonelik, kaynak grupları, konumlar ve etiketlerin birleşimini temel alan bir sorgu tanımlayın. Azure olmayan VM'ler için, kaydedilmiş aramalar dağıtımınıza dahil etmek üzere dinamik bir grup oluşturmak için kullanılır. Daha fazla bilgi için [bkz.](automation-update-management-groups.md)
- **Güncelleştirilecek Makineler**: Güncelleştirmek istediğiniz makineleri seçmek için Kaydedilmiş Arama, İçe Aktarılan grup veya Makineler'i seçin.

   >[!NOTE]
   >Kayıtlı Arama seçeneğini seçmek makine kimliklerini döndürmez, yalnızca adlarını döndürer. Birden çok kaynak grubunda aynı ada sahip birden fazla VM'niz varsa, bunlar sonuçlarda döndürülür. Grupları **güncelleştirme** seçeneğini kullanmak, ölçütlerinize uygun benzersiz VM'ler eklemeniz önerilir.

   **Makineler'i**seçerseniz, makinenin hazır durumu **Güncelleştirme aracısı hazır olma** sütununda gösterilir. Güncelleştirme dağıtımını zamanlamadan önce makinenin sistem durumu durumunu görebilirsiniz. Azure Monitor günlüklerinde bilgisayar grupları oluşturmanın farklı yöntemleri hakkında bilgi edinmek için [Azure Monitor günlüklerinde Bilgisayar gruplarına](../azure-monitor/platform/computer-groups.md) bakın

  ![Yeni güncelleştirme dağıtım bölmesi](./media/manage-update-multi/update-select-computers.png)

- **Güncelleştirme sınıflandırması**: Güncelleştirme dağıtımına dahil etmek için yazılım türlerini seçin. Sınıflandırma türlerinin açıklaması için [bkz.](automation-view-update-assessments.md#update-classifications) Sınıflandırma türleri şunlardır:
  - Kritik güncelleştirmeler
  - Güvenlik güncelleştirmeleri
  - Güncelleştirme paketleri
  - Özellik paketleri
  - Hizmet paketleri
  - Tanım güncelleştirmeleri
  - Araçlar
  - Güncelleştirmeler

- **Dahil edilecek/dışlanacak güncelleştirmeler** - Böylece **Dahil Et / Dışla** sayfası açılır. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir. İçermenin nasıl işleyiş işleyiş yaptığı hakkında daha fazla bilgi için [bkz.](automation-tutorial-update-management.md#schedule-an-update-deployment)

> [!NOTE]
> Dışlamaların kapsayıcıları geçersiz kdığını bilmek önemlidir. Örneğin, bir dışlama kuralı `*`tanımlarsanız, bunların tümü hariç olduğu için hiçbir düzeltme ekleri veya paketler yüklenmez. Dışlanmış yamalar hala makineden eksik olarak göstermektedir. Linux makineleri için bir paket dahil edildiyse ancak dışlanan bağımlı bir paket varsa, paket yüklenmez.

> [!NOTE]
> Güncelleştirme dağıtımıyla birlikte eklenmesi değiştirilen güncelleştirmeleri belirtemezsiniz.
>

- **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilirsiniz. Farklı bir zaman da belirtebilirsiniz.

   Ayrıca, dağıtımın bir kez veya yinelenen bir zamanlamaya göre gerçekleşeceğini belirtebilirsiniz. Yinelenen bir zamanlama ayarlamak **için, Yineleme**altında **Yinelenen'i**seçin.

   ![Zamanlama Ayarları iletişim kutusu](./media/manage-update-multi/update-set-schedule.png)

- **Ön betikler + Son betikler**: Dağıtımınızdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [Ön ve Son betikleri yönetme](pre-post-scripts.md).
- **Bakım penceresi (dakika)**: Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin. Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

- **Yeniden başlatma denetimi** - Bu ayar, güncelleştirme dağıtımı için yeniden başlatmaların nasıl işleneceğini belirler.

   |Seçenek|Açıklama|
   |---|---|
   |Gerekirse yeniden başlatın| **(Varsayılan)** Gerekirse, bakım penceresi izin veriyorsa yeniden başlatılır.|
   |Her zaman yeniden başlat|Gerekli olup olmadığına bakılmaksızın yeniden başlatma başlatılır. |
   |Hiçbir zaman yeniden başlatma|Yeniden başlatma gerekip gerekmeden de, yeniden başlatmalar bastırılır.|
   |Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|Bu seçenek güncelleştirmeleri yüklemeyi yoksa ve yalnızca yeniden başlatmayı başlatır.|

Zamanlamayı yapılandırmayı bitirdiğinizde, durum panosuna dönmek için **Oluştur** düğmesini seçin. **Zamanlanan** tablo, oluşturduğunuz dağıtım zamanlamasını gösterir.

> [!NOTE]
> Güncelleştirme Yönetimi, birinci taraf güncelleştirmelerini ve indirme öncesi yamalarını dağıtmayı destekler. Bu, sistemlerinizde bu ayarları nasıl yapılandırılabildiğini öğrenmek için yamalı sistemlerde değişiklikler gerektirir, [birinci taraf ve ön indirme desteğine](automation-configure-windows-update.md#pre-download-updates) bakın.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.

Dağıtım o anda çalışıyorsa, **Sürüyor** durumundadır. Dağıtım başarıyla bittikten sonra, durum **Başarılı**olarak değişir.

Dağıtımdaki bir veya daha fazla güncelleştirme başarısız olursa durum **Kısmen başarısız** olur.

![Güncelleştirme dağıtımının durumu](./media/manage-update-multi/update-view-results.png)

Bir güncelleştirme dağıtımının panosunu görmek için tamamlanan dağıtımı seçin.

**Güncelleştirme sonuçları** bölmesi, sanal makinenin toplam güncelleştirme sayısını ve dağıtım sonuçlarını gösterir. Sağdaki tablo, her güncelleştirmenin ve yükleme sonuçlarının ayrıntılı bir dökümünü verir. Yükleme sonuçları aşağıdaki değerlerden biri olabilir:

- **Denenmedi**: Tanımlı bakım penceresine bağlı olarak yeterli zaman olmadığı için güncelleştirme yüklenmedi.
- **Başarılı**: Güncelleştirme başarılı oldu.
- **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef sanal makinede güncelleştirme dağıtımını yöneten runbook'un iş akışını görmek için çıktı döşemesini seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Update Management günlükleri, çıktıları ve hataları hakkında daha fazla bilgi edinmek [için, Güncelleştirme Yönetimi için Sorgu güncelleştirme kayıtlarına](automation-update-management-query-logs.md)bakın.
