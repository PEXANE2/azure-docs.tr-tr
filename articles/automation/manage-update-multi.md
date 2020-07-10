---
title: Azure Otomasyonu 'nda birden çok VM için güncelleştirmeleri yönetme
description: Bu makalede, birden çok VM için güncelleştirmelerin nasıl yönetileceği açıklanır.
services: automation
ms.subservice: update-management
ms.date: 03/26/2020
ms.topic: conceptual
ms.openlocfilehash: a2d16bdca18b7fc0afab2a3deb325d1a75be3bb8
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185186"
---
# <a name="manage-updates-for-multiple-vms"></a>Birden çok VM için güncelleştirmesini yönetme

Windows ve Linux sanal makinelerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Azure Otomasyonu Güncelleştirme Yönetimi kullanabilirsiniz. [Azure Otomasyonu](./index.yml) hesabınızdan şunları yapabilirsiniz:

- Güncelleştirme yönetimi için VM 'Leri etkinleştirin.
- Kullanılabilir güncelleştirmelerin durumunu değerlendirme.
- Gerekli güncelleştirmeleri yüklemeyi zamanlama.
- Güncelleştirmelerin Güncelleştirme Yönetimi etkinleştirildiği tüm sanal makinelere başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirin.

Güncelleştirme Yönetimi sistem gereksinimleri hakkında bilgi edinmek için bkz. [güncelleştirme yönetimi istemci gereksinimleri](automation-update-management.md#client-requirements).

## <a name="prerequisites"></a>Önkoşullar

* Desteklenen işletim sistemlerinden birine sahip bir VM veya bilgisayar.
* Güncelleştirme Yönetimi için etkinleştirilen Linux VM 'Leri için bir güncelleştirme deposuna erişim.

## <a name="enable-update-management-for-azure-vms"></a>Azure VM 'Leri için Güncelleştirme Yönetimi etkinleştirme

1. Azure portal Otomasyon hesabınızı açın ve **güncelleştirme yönetimi**' ni seçin.

2. **Azure VM 'Leri Ekle**' yi seçin.

    ![Azure VM ekle sekmesi](./media/manage-update-multi/update-onboard-vm.png)

3. Etkinleştirmek için bir VM seçin ve Etkinleştir ' in **güncelleştirme yönetimi**altında **Etkinleştir** ' i seçin.

    ![Güncelleştirme Yönetimini Etkinleştir iletişim kutusu](./media/manage-update-multi/update-enable.png)

    İşlem tamamlandığında, VM 'niz üzerinde Güncelleştirme Yönetimi etkinleştirilir.

## <a name="enable-update-management-for-non-azure-vms-and-computers"></a>Azure dışı VM 'Ler ve bilgisayarlar için Güncelleştirme Yönetimi etkinleştirme

Windows ve Linux için Log Analytics aracısının, kurumsal ağınızda veya diğer bulut ortamınızda çalışan VM 'lerde Güncelleştirme Yönetimi sağlamak için yüklü olması gerekir. Aracıyı Azure dışında barındırılan makinelere dağıtmaya yönelik sistem gereksinimleri ve desteklenen yöntemler hakkında bilgi edinmek için bkz. [Log Analytics aracısına genel bakış](../azure-monitor/platform/log-analytics-agent.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Otomasyon hesabınıza bağlı bilgisayarları görüntüleme

Makineleriniz için Güncelleştirme Yönetimi etkinleştirdikten sonra **bilgisayarlar**' ı seçerek makine bilgilerini görebilirsiniz. Makine adı, uyumluluk durumu, ortam, işletim sistemi türü, kritik güncelleştirmeler ve güvenlik güncelleştirmeleri, yüklü diğer güncelleştirmeler ve bilgisayarlarınıza Güncelleştirme Aracısı hazırlığı hakkında bilgi görebilirsiniz.

  ![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-computers-tab.png)

Güncelleştirme Yönetimi için yakın zamanda etkinleştirilen bilgisayarlar henüz değerlendirilemeyebilir. Bu bilgisayarların uyumluluk durumu `Not assessed` . Uyumluluk durumu için olası değerlerin bir listesi aşağıda verilmiştir:

- `Compliant`: Kritik veya güvenlik güncelleştirmeleri eksik olan bilgisayarlar.
- `Non-compliant`: En az bir kritik veya güvenlik güncelleştirmesi eksik olan bilgisayarlar.
- `Not assessed`: Güncelleştirme değerlendirmesi verileri bilgisayardan beklenen zaman çerçevesi içinde alınmadı. Linux bilgisayarları için beklenen zaman dilimi son saattir. Windows bilgisayarları için beklenen zaman dilimi son 12 saattir.

Aracının durumunu görüntülemek için, **Güncelleştirme Aracısı hazırlığı** sütunundaki bağlantıyı seçin. Bu seçeneğin belirlenmesi karma çalışanı bölmesini açar ve karma çalışanın durumunu gösterir. Aşağıdaki görüntüde, uzun bir süre Güncelleştirme Yönetimi bağlı olmayan bir aracının örneği gösterilmektedir:

![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra, Güncelleştirme Yönetimi bölmesi açılır. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

## <a name="collect-data"></a>Veri toplama

VM 'Ler ve bilgisayarlara yüklenen aracılar güncelleştirmeler hakkında veri toplar. Aracılar, verileri Azure Güncelleştirme Yönetimi 'a gönderir.

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda Güncelleştirme Yönetimi tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Desteklenir | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Güncelleştirme Yönetimi, Windows aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Güncelleştirme Yönetimi, Linux aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Güncelleştirme Yönetimi bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar. |
| Azure Storage hesabı |Hayır |Azure depolama, sistem güncelleştirmeleriyle ilgili bilgiler içermez. |

### <a name="collection-frequency"></a>Toplama sıklığı

Bir bilgisayar güncelleştirme uyumluluğu taramasını tamamladıktan sonra, aracı bilgileri toplu olarak Azure Izleyici günlüklerine iletir. Bir Windows bilgisayarda, uyumluluk taraması varsayılan olarak her 12 saatte bir çalıştırılır.

Tarama zamanlamasının yanı sıra, güncelleştirme uyumluluğuna yönelik tarama, güncelleştirme yüklemesinden önce ve güncelleştirme yüklemesinden sonra, 15 dakika içinde başlatılır.

Bir Linux bilgisayar için, uyumluluk taraması varsayılan olarak saatte bir gerçekleştirilir. MMA Aracısı yeniden başlatılırsa, 15 dakika içinde bir uyumluluk taraması başlatılır.

Yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin panoda görüntülenmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için, sürüm zamanlamanız ve hizmet pencerenize göre hizalanan bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

>[!NOTE]
>Bir güncelleştirme dağıtımı zamanladığınızda, hedef makinelerdeki güncelleştirme dağıtımını işleyen **Patch-MicrosoftOMSComputers** runbook 'una bağlı bir [zamanlama](shared-resources/schedules.md) kaynağı oluşturur. Azure portal zamanlama kaynağını veya dağıtımı oluşturduktan sonra PowerShell 'i kullanarak silerseniz, zamanlanan güncelleştirme dağıtımını keser ve portaldan yeniden yapılandırmayı denediğinizde bir hata oluşturur. Yalnızca ilgili dağıtım zamanlamasını silerek zamanlama kaynağını silebilirsiniz.
>

Bir veya daha fazla VM için yeni bir güncelleştirme dağıtımı zamanlamak için, **güncelleştirme yönetimi**altında **güncelleştirme dağıtımı zamanla**' yı seçin.

**Yeni güncelleştirme dağıtımı** bölmesinde aşağıdaki bilgileri belirtin:

- **Ad**: güncelleştirme dağıtımını tanımlamak için benzersiz bir ad girin.
- **İşletim sistemi**: **Windows** veya **Linux**seçeneğini belirleyin.
- **Güncelleştirilecek gruplar**: dağıtımınıza dahil etmek üzere Azure VM 'lerin dinamik bir grubunu derlemek için abonelik, kaynak grubu, konum ve etiketlerin birleşimine göre bir sorgu tanımlayın. Azure dışı VM 'Ler için, kayıtlı aramalar dağıtımınıza dahil etmek üzere dinamik bir grup oluşturmak için kullanılır. Daha fazla bilgi için bkz. [Dinamik Gruplar](automation-update-management-groups.md).
- **Güncelleştirilecek makineler**: güncelleştirmek istediğiniz makineleri seçmek Için, kaydedilmiş bir arama, içeri aktarılan grup veya makine seçin ' i seçin.

   >[!NOTE]
   >Kaydedilen arama seçeneğinin belirlenmesi makine kimliklerini, yalnızca adlarını döndürmez. Birden çok kaynak grubunda aynı ada sahip birden fazla VM varsa, bunlar sonuçlarda döndürülür. Ölçütlerinizle eşleşen benzersiz VM 'Leri dahil etmeniz için **güncelleştirilecek gruplar** seçeneğinin kullanılması önerilir.

   **Makineler**' i seçerseniz makinenin hazırlığı, **Güncelleştirme Aracısı hazırlığı** sütununda gösterilir. Güncelleştirme dağıtımını zamanlamadan önce makinenin sistem durumunu görebilirsiniz. Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md)

  ![Yeni güncelleştirme dağıtım bölmesi](./media/manage-update-multi/update-select-computers.png)

- **Güncelleştirme sınıflandırması**: güncelleştirme dağıtımına dahil edilecek yazılım türlerini seçin. Sınıflandırma türlerinin açıklaması için bkz. [güncelleştirme sınıflandırmaları](automation-view-update-assessments.md#work-with-update-classifications). Sınıflandırma türleri şunlardır:
  - Kritik güncelleştirmeler
  - Güvenlik güncelleştirmeleri
  - Güncelleştirme paketleri
  - Özellik paketleri
  - Hizmet paketleri
  - Tanım güncelleştirmeleri
  - Araçlar
  - Güncelleştirmeler

- **Dahil edilecek/dışlanacak güncelleştirmeler** - Böylece Dahil Et / Dışla sayfası açılır. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir. Ekleme hakkında daha fazla bilgi için bkz. [güncelleştirme dağıtımı zamanlama](automation-tutorial-update-management.md#schedule-an-update-deployment).

> [!NOTE]
> Dışlamaları geçersiz kılmanın dahil olduğunu bilmemiz önemlidir. Örneğin, hariç tutma kuralı tanımlarsanız `*` , hepsi hariç tutulduklarında hiçbir düzeltme eki veya paket yüklenmez. Dışlanan düzeltme ekleri hala makinede eksik olarak gösterilir. Linux makineler için bir paket dahil, ancak dışlanan bağımlı bir paket varsa, paket yüklenmez.

> [!NOTE]
> Güncelleştirme dağıtımına dahil edilmek üzere yenisiyle değiştirilen güncelleştirmeleri belirtemezsiniz.

- **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilirsiniz. Ayrıca, farklı bir saat belirtebilirsiniz.

   Ayrıca, dağıtımın bir kez veya yinelenen bir zamanlamaya göre gerçekleşeceğini belirtebilirsiniz. Yinelenen bir zamanlama ayarlamak için **yinelenme**altında **yineleme**' yi seçin.

   ![Zamanlama Ayarları iletişim kutusu](./media/manage-update-multi/update-set-schedule.png)

- **Ön betikler + Son betikler**: Dağıtımınızdan önce ve sonra çalıştırılacak betikleri seçin. Daha fazla bilgi için bkz. [Ön ve Son betikleri yönetme](pre-post-scripts.md).
- **Bakım penceresi (dakika)**: güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin. Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

- **Yeniden başlatma denetimi** -Bu ayar, güncelleştirme dağıtımı için yeniden başlatmalar nasıl işlendiğini belirler.

   |Seçenek|Açıklama|
   |---|---|
   |Gerekirse yeniden Başlat| **(Varsayılan)** Gerekirse, bakım penceresi izin veriyorsa yeniden başlatma başlatılır.|
   |Her zaman yeniden başlat|Bir yeniden başlatma, gerekli olup olmadığına bakılmaksızın başlatılır. |
   |Hiçbir zaman yeniden başlatma|Yeniden başlatma gerekli olursa olsun, yeniden başlatmalar bastırılır.|
   |Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|Bu seçenek güncelleştirmeleri yüklemeyi yoksayar ve yalnızca bir yeniden başlatma işlemi başlatır.|

Zamanlamayı yapılandırmayı tamamladığınızda, durum panosuna dönmek için **Oluştur** düğmesini seçin. **Zamanlanan** tablo, oluşturduğunuz dağıtım zamanlamasını gösterir.

> [!NOTE]
> Güncelleştirme Yönetimi, birinci taraf güncelleştirmelerini ve indirme öncesi yamalarını dağıtmayı destekler. Bu, düzeltme eki eklenen sistemlerde değişiklik yapılmasını gerektirir, sistemlerinizde bu ayarların nasıl yapılandırılacağını öğrenmek için [ilk taraf ve indirme öncesi destek](automation-configure-windows-update.md#pre-download-updates) ' e bakın.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.

Dağıtım o anda çalışıyorsa, **Sürüyor** durumundadır. Dağıtım başarıyla tamamlandıktan sonra durum **başarılı**olarak değişir.

Dağıtımdaki bir veya daha fazla güncelleştirme başarısız olursa durum **Kısmen başarısız** olur.

![Güncelleştirme dağıtımının durumu](./media/manage-update-multi/update-view-results.png)

Bir güncelleştirme dağıtımının panosunu görmek için tamamlanan dağıtımı seçin.

Güncelleştirme sonuçları bölmesi, sanal makine için toplam güncelleştirme sayısını ve dağıtım sonuçlarını gösterir. Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ve yükleme sonuçları verilmiştir. Yükleme sonuçları aşağıdaki değerlerden biri olabilir:

- `Not attempted`: Tanımlı bakım penceresine göre yetersiz süre kullanılamadığından güncelleştirme yüklenmedi.
- `Succeeded`: Güncelleştirme başarılı oldu.
- `Failed`: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef sanal makinede güncelleştirme dağıtımını yöneten runbook 'un iş akışını görmek için çıktı kutucuğunu seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Güncelleştirme günlüklerinde arama yapmanız gerekiyorsa bkz. [sorgu güncelleştirme yönetimi günlükleri](automation-update-management-query-logs.md).
