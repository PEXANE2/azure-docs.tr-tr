---
title: Azure Veri Fabrikası'nı görsel olarak izleyin
description: Azure veri fabrikalarını görsel olarak nasıl izleyeceğinizi öğrenin
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/19/2018
ms.openlocfilehash: 40b1b8d040c4b3ea76372920f88551fba35c5f26
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419451"
---
# <a name="visually-monitor-azure-data-factory"></a>Azure Veri Fabrikası'nı görsel olarak izleyin

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Veri Fabrikası'nda bir ardışık hatlar oluşturup yayımladıktan sonra, bir tetikleyiciyle ilişkilendirebilir veya geçici bir çalıştırmabaşlatabilirsiniz. Tüm ardışık hatlar çalışırınızı Azure Veri Fabrikası kullanıcı deneyiminde yerel olarak izleyebilirsiniz. İzleme deneyimini açmak için, [Azure portalının](https://portal.azure.com/)veri fabrikası bıçaklarında **Monitör & Yönet** döşemesini seçin. Zaten ADF UX'deyseniz, sol kenar çubuğundaki **Monitör** simgesine tıklayın.

Tüm veri fabrikası çalıştırmaları tarayıcının yerel saat diliminde görüntülenir. Saat dilimini değiştirirseniz, tüm tarih/saat alanları seçtiğiniz alana yapışıyor.

## <a name="monitor-pipeline-runs"></a>İşlem hattı çalıştırmalarını izleme

Varsayılan izleme görünümü, seçili zaman dilimindeki ardışık ardışık iştinlerin listesidir. Aşağıdaki sütunlar görüntülenir:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Boru Hattı Adı | Boru hattının adı |
| Eylemler | Etkinlik ayrıntılarını görüntülemenize, iptal etmenize veya ardışık hattı yeniden çalıştırmanıza olanak tanıyan simgeler |
| Çalıştır Başlangıç | Boru hattı nın çalıştırılışı için başlangıç tarihi ve saati (MM/DD/YYYY, HH:MM:SS AM/PM) |
| Süre | Çalıştırma süresi (HH:MM:SS) |
| Tarafından Tetiklenen | Boru hattını başlatan tetikleyicinin adı |
| Durum | **Başarısız**, **Başarılı**, **Devam Eden**, **İptal Edilen**veya **Sıralanan** |
| Ek Açıklamalar | Bir ardışık boru hattıyla ilişkili filtrelenebilir etiketler  |
| Parametreler | Ardışık hatlar çalışması için parametreler (ad/değer çiftleri) |
| Hata | Ardışık iş tonu başarısız olduysa, çalıştırma hatası |
| Çalıştırma kimliği | Boru hattı nın çalışma kimliği |

![Boru hattı çalıştırmalarını izlemek için liste görünümü](media/monitor-visually/pipeline-runs.png)

Boru hattı ve etkinlik çalışır listesini yenilemek için **Yenile** düğmesini el ile seçmeniz gerekir. Autorefresh şu anda desteklenmiyor.

![Yenile düğmesi](media/monitor-visually/refresh.png)

## <a name="monitor-activity-runs"></a>Etkinlik çalıştırmalarını izleme

Her bir dizi işlem için etkinlik çalışır görüntülemek **için, Eylemler** sütunun altında **Görüntüle etkinliği çalışır** simgesini seçin. Liste görünümü, her ardışık hat çalışmasına karşılık gelen etkinlik çalıştırmalarını gösterir.

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Etkinlik Adı | Boru hattı içindeki etkinliğin adı |
| Etkinlik Türü | **Copy**, **ExecuteDataFlow**veya **AzureMLExecutePipeline** gibi etkinlik türü |
| Eylemler | JSON giriş bilgilerini, JSON çıktı bilgilerini veya etkinliklere özel ayrıntılı izleme deneyimlerini görmenizi sağlayan simgeler | 
| Çalıştır Başlangıç | Faaliyetin çalıştırılışı için başlangıç tarihi ve saati (MM/DD/YYYY, HH:MM:SS AM/PM) |
| Süre | Çalıştırma süresi (HH:MM:SS) |
| Durum | **Başarısız**, **Başarılı**, **Devam Eden**veya **İptal Edilen** |
| Tümleştirme Çalışma Süresi | Etkinliğin çalıştırıldığı Tümleştirme Runtime'ı |
| Kullanıcı Özellikleri | Etkinliğin kullanıcı tanımlı özellikleri |
| Hata | Etkinlik başarısız olduysa, çalıştırma hatası |
| Çalıştırma kimliği | Etkinlik çalışmasının kimliği |

![İzleme etkinliği çalıştırmaları için liste görünümü](media/monitor-visually/activity-runs.png)

### <a name="promote-user-properties-to-monitor"></a>İzlemek için kullanıcı özelliklerini tanıtın

İzlediğiniz bir varlık haline gelmesi için herhangi bir boru hattı etkinlik özelliğini kullanıcı özelliği olarak tanıtın. Örneğin, ardışık ardışık alandaki kopyalama etkinliğinin **Kaynak** ve **Hedef** özelliklerini kullanıcı özellikleri olarak tanıtabilirsiniz. Kopyalama etkinliği için **Kaynak** ve **Hedef** kullanıcı özelliklerini oluşturmak için **Otomatik Oluştur'u** seçin.

![Kullanıcı özellikleri oluşturma](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Kullanıcı özellikleri olarak en fazla beş ardışık ardışık etkinlik özelliğini tanıtabilirsiniz.

Kullanıcı özelliklerini oluşturduktan sonra, bunları izleme listesi görünümlerinde izleyebilirsiniz. Kopyalama etkinliğinin kaynağı bir tablo adıysa, etkinlik çalışır için liste görünümünde kaynak tablo adını sütun olarak izleyebilirsiniz.

![Etkinlik, kullanıcı özellikleri olmadan listeyi çalıştırDı](media/monitor-visually/monitor-user-properties-image2.png)

![Etkinlik çalışır listesine kullanıcı özellikleri için sütun ekleme](media/monitor-visually/monitor-user-properties-image3.png)

![Etkinlik, kullanıcı özellikleri için sütunlu listeyi çalıştırDı](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="configure-the-list-view"></a>Liste görünümünü yapılandırma

### <a name="order-and-filter"></a>Sipariş ve filtre

Ardışık hatlar çalışır çalışan başlangıç saatine göre azalan veya artan olup olmayacağını geçiş. Filtre ardışık hattı aşağıdaki sütunları kullanarak çalışır:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Boru Hattı Adı | Boru hattının adına göre filtreuygulayın. |
| Çalıştır Başlangıç |  Görüntülenen ardışık hatlar çalışır zaman aralığını belirleyin. Seçenekler arasında Son **24 saat**, **Geçen hafta**ve **Son 30 gün** için veya özel bir tarih ve saat seçmek için hızlı filtreler yer alıyor. |
| Çalışma Durumu | Filtre duruma göre çalışır: **Başarılı**, **Başarısız**, **Sıraya ,** İptal **edilen**veya **Devam Ediyor**. |
| Ek Açıklamalar | Her ardışık ardışık boru hattına uygulanan etiketlere göre filtreleme |
| Çalıştırmalar | Reran ardışık hatlarını görmek isteyip istemediğiniz ifiltreedin |

![Filtreleme seçenekleri](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Sütun ekleme veya kaldırma
Liste görünümü üstbilgisini sağ tıklatın ve liste görünümünde görünmesini istediğiniz sütunları seçin.

![Sütun seçenekleri](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Sütun genişliklerini ayarlama
Sütun üstbilgisinin üzerine gezinerek liste görünümündeki sütun genişliklerini artırın ve azaltın.

## <a name="rerun-activities-inside-a-pipeline"></a>Bir boru hattı içindeki etkinlikleri yeniden çalıştırma

Bir ardışık boru hattı içinde etkinlikleri yeniden çalıştırabilirsiniz. **Etkinlik çalışır görüntüle'yi**seçin ve ardından ardışık hattınızı yeniden çalıştırmak istediğiniz noktadan gelen etkinliği seçin.

![Etkinlik çalıştırmalarını görüntüleme](media/monitor-visually/rerun-activities-image1.png)

![Etkinlik çalıştır'ı seçme](media/monitor-visually/rerun-activities-image2.png)

### <a name="rerun-from-failed-activity"></a>Başarısız olan etkinlikten yeniden çalıştırma

Bir etkinlik başarısız olursa, kez geri lanırsa veya iptal edilirse, başarısız olan **etkinlikten Yeniden Çalıştır'ı**seçerek bu başarısız etkinlikten ardışık ardışık alanı yeniden çalıştırabilirsiniz.

![Başarısız etkinliği yeniden çalıştırma](media/monitor-visually/rerun-failed-activity.png)

### <a name="view-rerun-history"></a>Yeniden çalıştırma geçmişini görüntüleme

Liste görünümünde tüm ardışık ardışık çalışır için yeniden çalışma geçmişini görüntüleyebilirsiniz.

![Geçmişi görüntüleme](media/monitor-visually/rerun-history-image1.png)

Ayrıca, belirli bir ardışık hat çalışması için yeniden çalışma geçmişini görüntüleyebilirsiniz.

![Bir ardışık hat çalışması için geçmişi görüntüleme](media/monitor-visually/rerun-history-image2.png)

## <a name="gantt-views"></a>Gantt görünümleri

Boru hatlarınızı ve etkinlik çalışanlarınızı hızlı bir şekilde görselleştirmek için Gantt görünümlerini kullanın.

![Gantt grafiği örneği](media/monitor-visually/gantt1.png)

Ardışık hatlarınızda oluşturduğunuz ek açıklamalara/etiketlere göre ardışık alan başına Gantt görünümüne veya grup görünümüne bakabilirsiniz.

![Gantt grafik ek açıklamaları](media/monitor-visually/gantt2.png)

Çubuğun uzunluğu boru hattının süresini bildirir. Daha fazla ayrıntı görmek için çubuğu da seçebilirsiniz.

![Gantt grafik süresi](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Rehberli turlar
Sol alttaki **Bilgi** simgesini seçin. Ardından, boru hattınızı ve etkinlik çalışanlarınızı nasıl izleyeceğiniz hakkında adım adım talimatlar almak için **Rehberli** Turlar'ı seçin.

![Rehberli turlar](media/monitor-visually/guided-tours.png)

## <a name="alerts"></a>Uyarılar

Veri Fabrikası'nda desteklenen ölçümlerle ilgili uyarıları yükseltebilirsiniz. Başlamak için Veri Fabrikası izleme sayfasındaki &**Ölçümleri** **İzle'yi** > seçin.

![Veri fabrikası Monitör sayfası](media/monitor-visually/alerts01.png)

Yedi dakikalık bir tanıtım ve bu özelliğin tanıtımı için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Uyarı oluşturma

1.  Yeni bir uyarı oluşturmak için **Yeni Uyarı Kuralı'nı** seçin.

    ![Yeni Uyarı Kuralı düğmesi](media/monitor-visually/alerts02.png)

1.  Kural adını belirtin ve uyarı nın önem derecesini seçin.

    ![Kural adı ve önem derecesi için kutular](media/monitor-visually/alerts03.png)

1.  Uyarı ölçütlerini seçin.

    ![Hedef ölçütler için kutu](media/monitor-visually/alerts04.png)

    ![Ölçütler listesi](media/monitor-visually/alerts05.png)

1.  Uyarı mantığını yapılandırın. Tüm ardışık hatlar ve ilgili etkinlikler için seçili metrik için bir uyarı oluşturabilirsiniz. Belirli bir etkinlik türünü, etkinlik adını, ardışık adveya hata türünü de seçebilirsiniz.

    ![Uyarı mantığını yapılandırma seçenekleri](media/monitor-visually/alerts06.png)

1.  Uyarı için e-posta, SMS, itme ve sesli bildirimleri yapılandırın. Uyarı bildirimleri için bir eylem grubu oluşturun veya varolan bir grup seçin.

    ![Bildirimleri yapılandırma seçenekleri](media/monitor-visually/alerts07.png)

    ![Bildirim ekleme seçenekleri](media/monitor-visually/alerts08.png)

1.  Uyarı kuralını oluşturun.

    ![Uyarı kuralı oluşturma seçenekleri](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Sonraki adımlar

Boru hatlarını izleme ve yönetme hakkında bilgi edinmek için Monitör'e bakın [ve ardışık olarak boru hatları makalesini yönetin.](https://docs.microsoft.com/azure/data-factory/monitor-programmatically)
