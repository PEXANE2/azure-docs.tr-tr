---
title: Azure Data Factory’yi görsel olarak izleme
description: Azure veri fabrikalarını görsel olarak izlemeyi öğrenin
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: b65b5b847712ce32c6723d725be9cf2ed1f99050
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85801576"
---
# <a name="visually-monitor-azure-data-factory"></a>Azure Data Factory’yi görsel olarak izleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Data Factory bir işlem hattı oluşturup yayımladıktan sonra, bir tetikleyici ile ilişkilendirebilir veya bir geçici çalıştırmayı el ile başlatabilirsiniz. Azure Data Factory kullanıcı deneyiminde tüm işlem hattı çalıştırmalarının yerel olarak izlenmesini sağlayabilirsiniz. İzleme deneyimini açmak için [Azure Portal](https://portal.azure.com/)Veri Fabrikası dikey penceresinde **izleyici & Yönet** kutucuğunu seçin. Zaten ADF UX kullanıyorsanız sol kenar çubuğundaki **izleyici** simgesine tıklayın.

Tüm Data Factory çalıştırmaları tarayıcının yerel saat diliminde görüntülenir. Saat dilimini değiştirirseniz, tüm tarih/saat alanları seçtiğiniz bir öğesine yastur.

## <a name="monitor-pipeline-runs"></a>İşlem hattı çalıştırmalarını izleme

Varsayılan izleme görünümü, seçilen dönemdeki işlem hattı çalıştırmaları listesidir. Aşağıdaki sütunlar görüntülenir:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem hattı adı | İşlem hattının adı |
| Eylemler | Etkinlik ayrıntılarını görüntülemenizi, iptal etmenizi veya işlem hattını yeniden çalıştırmayı sağlayan simgeler |
| Çalıştırma başlangıç | İşlem hattı çalışmasının başlangıç tarihi ve saati (AA/GG/YYYY, ss: DD: SS) |
| Süre | Çalıştırma süresi (SS: DD: SS) |
| Tetikleyen | İşlem hattını Başlatan tetikleyicinin adı |
| Durum | **Başarısız**, **başarılı**, **devam ediyor**, **iptal edildi**veya **sıraya alındı** |
| Ek Açıklamalar | İşlem hattı ile ilişkili filtrelenebilir Etiketler  |
| Parametreler | İşlem hattı çalıştırması için parametreler (ad/değer çiftleri) |
| Hata | İşlem hattı başarısız olursa, çalıştırma hatası |
| Çalıştırma kimliği | İşlem hattı çalıştırmasının KIMLIĞI |

![İzleme işlem hattı çalıştırmaları için liste görünümü](media/monitor-visually/pipeline-runs.png)

İşlem hattı ve etkinlik çalıştırmaları listesini yenilemek için **Yenile** düğmesini el ile seçmeniz gerekir. Oto yenileme şu anda desteklenmiyor.

![Yenile düğmesi](media/monitor-visually/refresh.png)

## <a name="monitor-activity-runs"></a>Etkinlik çalıştırmalarını izleme

Her Işlem hattı çalıştırması için etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütununun altındaki **etkinlik çalıştırmalarını görüntüle** simgesini seçin. Liste görünümü her bir işlem hattı çalıştırmasına karşılık gelen etkinlik çalıştırmalarını gösterir.

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Etkinlik Adı | İşlem hattının içindeki etkinliğin adı |
| Etkinlik Türü | Etkinliğin türü; örneğin **Copy**, **executedataflow**veya **AzureMLExecutePipeline** |
| Eylemler | JSON giriş bilgilerini, JSON çıktı bilgilerini veya ayrıntılı etkinliğe özgü izleme deneyimlerini görmenizi sağlayan simgeler | 
| Çalıştırma başlangıç | Etkinlik çalıştırmasının başlangıç tarihi ve saati (AA/GG/YYYY, ss: DD: SS) |
| Süre | Çalıştırma süresi (SS: DD: SS) |
| Durum | **Başarısız**, **başarılı**, **devam ediyor**veya **iptal edildi** |
| Integration Runtime | Etkinliğin çalıştırıldığı Integration Runtime |
| Kullanıcı Özellikleri | Etkinliğin Kullanıcı tanımlı özellikleri |
| Hata | Etkinlik başarısız olursa, çalıştırma hatası |
| Çalıştırma kimliği | Etkinlik çalıştırmasının KIMLIĞI |

![İzleme etkinliği çalıştırmaları için liste görünümü](media/monitor-visually/activity-runs.png)

### <a name="promote-user-properties-to-monitor"></a>Kullanıcı özelliklerini izlemeye yükselt

Herhangi bir işlem hattı etkinlik özelliğini Kullanıcı özelliği olarak yükselterek izlediğiniz bir varlık haline gelir. Örneğin, işlem hattınızdaki kopyalama etkinliğinin **kaynak** ve **hedef** özelliklerini Kullanıcı özellikleri olarak yükseltebilirsiniz. Bir kopyalama etkinliğinin **kaynak** ve **hedef** Kullanıcı özelliklerini oluşturmak için **Otomatik oluştur** ' u seçin.

![Kullanıcı Özellikleri oluştur](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Yalnızca Kullanıcı özellikleri olarak en fazla beş ardışık düzen etkinliği özelliği yükseltebilirsiniz.

Kullanıcı özelliklerini oluşturduktan sonra bunları izleme listesi görünümlerinde izleyebilirsiniz. Kopyalama etkinliğinin kaynağı bir tablo adı ise, kaynak tablo adını etkinlik çalıştırmaları için liste görünümünde bir sütun olarak izleyebilirsiniz.

![Kullanıcı özellikleri olmadan etkinlik çalıştırmaları listesi](media/monitor-visually/monitor-user-properties-image2.png)

![Etkinlik çalıştırmaları listesine kullanıcı özellikleri için sütun ekleme](media/monitor-visually/monitor-user-properties-image3.png)

![Kullanıcı özellikleri için sütunları olan etkinlik çalıştırmaları listesi](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="configure-the-list-view"></a>Liste görünümünü yapılandırma

### <a name="order-and-filter"></a>Sırala ve filtrele

İşlem hattının çalışmasının, çalışma başlangıç zamanına göre azalan veya artan şekilde olacağını değiştirin. Aşağıdaki sütunları kullanarak filtre işlem hattı çalıştırmaları:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem hattı adı | İşlem hattının adına göre filtreleyin. |
| Çalıştırma başlangıç |  Gösterilecek işlem hattı çalıştırmalarının zaman aralığını belirleme. Seçenekler, **son 24 saat**, **son hafta**ve **son 30 gün** için hızlı filtreler içerir veya özel bir tarih ve saat seçer. |
| Çalışma durumu | **Şu**durum ile filtre çalıştırmaları: **başarılı**, başarısız, **sıraya alınmış**, **iptal edildi**veya **devam ediyor**. |
| Ek Açıklamalar | Her bir ardışık düzene uygulanan etiketlere göre filtrele |
| Çalıştırmalar | İşlem hatlarını yeniden yönlendirme görmek isteyip istemediğinizi filtreleyin |

![Filtreleme seçenekleri](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Sütun ekleme veya kaldırma
Liste görünümü başlığına sağ tıklayın ve liste görünümünde görünmesini istediğiniz sütunları seçin.

![Sütun seçenekleri](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Sütun genişliklerini ayarla
Sütun üst bilgisinin üzerine gelerek liste görünümündeki sütun genişliklerini artırın ve azaltın.

## <a name="rerun-activities-inside-a-pipeline"></a>Etkinlikleri bir işlem hattı içinde yeniden çalıştırma

Etkinlikleri bir işlem hattı içinde yeniden çalıştırabilirsiniz. **Etkinlik çalıştırmalarını görüntüle**' yi seçin ve ardından işlem hattınızda etkinlik hattınızı yeniden çalıştırmak istediğiniz noktayı seçin.

![Etkinlik çalıştırmalarını görüntüleme](media/monitor-visually/rerun-activities-image1.png)

![Etkinlik çalıştırması seçin](media/monitor-visually/rerun-activities-image2.png)

### <a name="rerun-from-failed-activity"></a>Başarısız etkinlikten yeniden çalıştır

Bir etkinlik başarısız olursa, zaman aşımına uğrar veya iptal edilirse, **başarısız etkinlikten yeniden çalıştır**' ı seçerek bu başarısız etkinlikten ardışık düzeni yeniden çalıştırabilirsiniz.

![Başarısız etkinliği yeniden çalıştır](media/monitor-visually/rerun-failed-activity.png)

### <a name="view-rerun-history"></a>Yeniden çalıştırma geçmişini görüntüle

Liste görünümündeki tüm işlem hattı çalıştırmaları için yeniden çalıştırma geçmişini görüntüleyebilirsiniz.

![Geçmişi görüntüleme](media/monitor-visually/rerun-history-image1.png)

Ayrıca, belirli bir işlem hattı çalıştırması için yeniden çalıştırma geçmişini görüntüleyebilirsiniz.

![İşlem hattı çalıştırmasının geçmişini görüntüleme](media/monitor-visually/rerun-history-image2.png)

## <a name="monitor-consumption"></a>Tüketimi izleme

Bir işlem hattı çalıştırması tarafından tüketilen kaynakları, çalıştırmanın yanındaki tüketim simgesine tıklayarak görebilirsiniz. 

![Tüketimi izleme](media/monitor-visually/monitor-consumption-1.png)

Simgeye tıkladığınızda, bu işlem hattı çalıştırması tarafından kullanılan kaynakların tüketim raporu açılır. 

![Tüketimi izleme](media/monitor-visually/monitor-consumption-2.png)

İşlem hattı çalıştırmasının maliyetini tahmin etmek için bu değerleri [Azure fiyatlandırma hesaplayıcıya](https://azure.microsoft.com/pricing/details/data-factory/) takabilirsiniz. Azure Data Factory fiyatlandırması hakkında daha fazla bilgi için bkz. [fiyatlandırmayı anlama](pricing-concepts.md).

> [!NOTE]
> Fiyatlandırma Hesaplayıcı tarafından döndürülen bu değerler bir tahmindir. Azure Data Factory, faturalandırılacaksınız tam miktarı yansıtmaz 

## <a name="gantt-views"></a>Gantt görünümleri

Ardışık düzenleri ve etkinlik çalıştırmalarını hızlıca görselleştirmek için Gantt görünümlerini kullanın.

![Gantt grafiğinin örneği](media/monitor-visually/gantt1.png)

İşlem hatlarında oluşturduğunuz ek açıklamaların/etiketlerin başına, ardışık düzen veya grup için Gantt görünümüne bakabilirsiniz.

![Gantt grafiği ek açıklamaları](media/monitor-visually/gantt2.png)

Çubuğun uzunluğu, işlem hattının süresini bilgilendirir. Daha fazla ayrıntı görmek için çubuğu da seçebilirsiniz.

![Gantt grafiği süresi](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Kılavuzlu Turlar
Sol alt köşedeki **bilgi** simgesini seçin. Ardından, işlem hattınızı ve etkinlik çalıştırmalarını nasıl izleyecağınız hakkında adım adım yönergeleri almak için **Kılavuzlu Turları** seçin.

![Kılavuzlu Turlar](media/monitor-visually/guided-tours.png)

## <a name="alerts"></a>Uyarılar

Data Factory içinde desteklenen ölçümler üzerinde uyarı oluşturabilirsiniz. Başlamak **Monitor**  >  için Data Factory izleme sayfasında,**uyarıları & ölçümleri** izleyin ' ı seçin.

![Data Factory Izleyici sayfası](media/monitor-visually/start-page.png)

Bu özelliğin yedi dakikalık bir girişi ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Uyarı oluşturma

1.  Yeni bir uyarı oluşturmak için **Yeni uyarı kuralı** ' nı seçin.

    ![Yeni uyarı kuralı düğmesi](media/monitor-visually/new-alerts.png)

1.  Kural adını belirtin ve uyarı önem derecesini seçin.

    ![Kural adı ve önem derecesi için kutular](media/monitor-visually/name-and-severity.png)

1.  Uyarı ölçütlerini seçin.

    ![Hedef ölçütü kutusu](media/monitor-visually/add-criteria-1.png)

    ![Ölçüt listesi](media/monitor-visually/add-criteria-2.png)

    ![Ölçüt listesi](media/monitor-visually/add-criteria-3.png)

    ADF varlık sayısı/boyutu, etkinlik/ardışık düzen/tetikleyici çalıştırmaları, Integration Runtime (IR) CPU kullanımı/belleği/düğüm sayısı/kuyruğu ve SSIS paket yürütmeleri ve SSIS IR başlatma/durdurma işlemleri dahil olmak üzere çeşitli ölçümler üzerinde uyarılar oluşturabilirsiniz.

1.  Uyarı mantığını yapılandırın. Tüm işlem hatları ve ilgili etkinlikler için seçili ölçüm için bir uyarı oluşturabilirsiniz. Ayrıca belirli bir etkinlik türünü, etkinlik adını, işlem hattı adını veya hata türünü de seçebilirsiniz.

    ![Uyarı mantığını yapılandırma seçenekleri](media/monitor-visually/alert-logic.png)

1.  Uyarı için e-posta, SMS, push ve sesli bildirimleri yapılandırın. Uyarı bildirimleri için bir eylem grubu oluşturun veya mevcut olanı seçin.

    ![Bildirimleri yapılandırma seçenekleri](media/monitor-visually/configure-notification-1.png)

    ![Bildirim ekleme seçenekleri](media/monitor-visually/configure-notification-2.png)

1.  Uyarı kuralını oluşturun.

    ![Uyarı kuralı oluşturma seçenekleri](media/monitor-visually/create-alert-rule.png)

## <a name="next-steps"></a>Sonraki adımlar

İşlem hatlarını izleme ve yönetme hakkında bilgi edinmek için işlem [hatlarını programlama yoluyla izleme ve yönetme](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) makalesine bakın.
