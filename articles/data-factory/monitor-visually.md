---
title: Azure veri fabrikalarını görsel olarak izleme | Microsoft Docs
description: Azure veri fabrikalarını görsel olarak izlemeyi öğrenin
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: shlo
ms.openlocfilehash: 53ead1caed47ae442670f0b6bcd54cd84956a759
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720611"
---
# <a name="visually-monitor-azure-data-factories"></a>Azure veri fabrikalarını görsel olarak izleme
Azure Data Factory, bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir. Azure Data Factory’yi kullanarak, farklı veri depolarından veri alabilen, Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyebilen/dönüştürebilen ve çıktı verilerini iş zekası (BI) uygulamaları tarafından kullanılabilmesi için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilen veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturup zamanlayabilirsiniz.

Bu hızlı başlangıçta, tek bir kod satırı yazmadan Data Factory işlem hatlarını görsel olarak nasıl izleyebileceğinizi öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="monitor-data-factory-pipelines"></a>Data Factory işlem hatlarını izleme

Bir basit liste görünümü arabirimiyle işlem hattını ve etkinlik çalıştırmalarını izleyin. Tüm çalıştırmalar yerel tarayıcı saat diliminde görüntülenir. Saat dilimini değiştirebilir ve tüm tarih saat alanları seçili saat dilimine ek olarak değişiklik yapabilir.  

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. [Azure Portal](https://portal.azure.com/)oturum açın.
3. Azure portal ' de oluşturulan veri fabrikası dikey penceresine gidin ve Data Factory görsel izleme deneyimini başlatmak için ' Izleme & Yönet ' kutucuğuna tıklayın.

## <a name="monitor-pipeline-runs"></a>İşlem hattı çalıştırmalarını izleme
Data Factory v2 işlem hatlarınız için her işlem hattı çalıştırmasının gösterildiği liste görünümü. Dahil edilen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem Hattı Adı | İşlem hattının adı. |
| Eylemler | Etkinlik çalıştırmalarını görüntülemek için kullanılabilen tek eylem. |
| Çalıştırma başlangıç | İşlem hattı çalıştırma başlangıç tarihi saati (AA/GG/YYYY, ss: DD: SS) |
| Duration | Çalıştırma süresi (SS: DD: SS) |
| Tetikleyen | El ile tetikleyici, zamanlama tetikleyicisi |
| Durum | Başarısız, başarılı, devam ediyor |
| Parametreler | İşlem hattı çalıştırma parametreleri (ad, değer çiftleri) |
| Hata | İşlem hattı çalıştırma hatası (if/any) |
| Çalışma Kimliği | İşlem hattı çalıştırmasının KIMLIĞI |

![İşlem hattı çalıştırmalarını izleme](media/monitor-visually/pipeline-runs.png)

## <a name="monitor-activity-runs"></a>Etkinlik çalıştırmalarını izleme
Her işlem hattı çalıştırmasına karşılık gelen etkinlik çalıştırmalarının gösterildiği liste görünümü. Her Işlem hattı çalıştırması için etkinlik çalıştırmalarını görüntülemek için **' Actions '** sütununun altındaki **' etkinlik çalıştırmaları '** simgesine tıklayın. Dahil edilen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Etkinlik Adı | İşlem hattının içindeki etkinliğin adı. |
| Etkinlik Türü | Etkinliğin türü, örneğin Copy, HDInsightSpark, Hdınsighthive, vb. |
| Çalıştırma başlangıç | Etkinlik çalıştırma başlangıç tarihi saati (AA/GG/YYYY, ss: DD: SS) |
| Duration | Çalıştırma süresi (SS: DD: SS) |
| Durum | Başarısız, başarılı, devam ediyor |
| Girdi | Etkinlik girişlerini açıklayan JSON dizisi |
| Output | Etkinlik çıkışlarını açıklayan JSON dizisi |
| Hata | Etkinlik çalıştırma hatası (IF/any) |

![Etkinlik çalıştırmalarını izleme](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> İşlem hattı ve etkinlik çalıştırmaları listesini yenilemek için üstteki **' Yenile '** simgesine tıklamak gerekir. Otomatik yenileme şu anda desteklenmiyor.

![Yenile](media/monitor-visually/refresh.png)

## <a name="select-a-data-factory-to-monitor"></a>İzlenecek bir veri fabrikası seçin
Sol üstteki **Data Factory** simgesine gelin. İzleyebilmeniz gereken Azure aboneliklerinin ve veri fabrikalarının listesini görmek için ' ok ' simgesine tıklayın.

![Veri fabrikası seçme](media/monitor-visually/select-datafactory.png)

## <a name="configure-the-list-view"></a>Liste görünümünü yapılandırma

### <a name="apply-rich-ordering-and-filtering"></a>Zengin sıralama ve filtreleme uygulama

Sıra işlem hattı, sırasıyla Çalıştır ve filtrele işlem hattı çalıştırmaları ile aşağıdaki sütunlar tarafından çalışır:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem Hattı Adı | İşlem hattının adı. Seçenekler arasında ' son 24 saat ', ' son hafta ', ' son 30 gün ' için hızlı filtreler veya özel bir tarih saati seçebilirsiniz. |
| Çalıştırma başlangıç | İşlem hattı çalıştırma başlangıç tarihi saati |
| Çalışma durumu | Durum başarıyla tamamlandı, başarısız, devam ediyor |

![Filtre](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Sütun ekleme veya kaldırma
Liste görünümü başlığına sağ tıklayıp liste görünümünde görünmesini istediğiniz sütunları seçin

![Sütunlar](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Sütun genişliklerini ayarla
Sütun üst bilgisinin üzerine gelerek liste görünümündeki sütun genişliklerini artırın ve azaltın

## <a name="promote-user-properties-to-monitor"></a>Kullanıcı özelliklerini izlemeye yükselt

Herhangi bir işlem hattı etkinlik özelliğini, izleyebileceğiniz bir varlık olacak şekilde Kullanıcı özelliği olarak yükseltebilirsiniz. Örneğin, işlem hattınızdaki kopyalama etkinliğinin **kaynak** ve **hedef** özelliklerini Kullanıcı özellikleri olarak yükseltebilirsiniz. Ayrıca, bir kopyalama etkinliğinin **kaynak** ve **hedef** Kullanıcı özelliklerini oluşturmak için **Otomatik oluştur** ' u da seçebilirsiniz.

![Kullanıcı Özellikleri oluştur](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Kullanıcı özellikleri olarak yalnızca 5 adede kadar işlem hattı etkinliği özelliği yükseltebilirsiniz.

Kullanıcı özelliklerini oluşturduktan sonra bunları izleme listesi görünümlerinde izleyebilirsiniz. Kopyalama etkinliğinin kaynağı bir tablo adı ise, kaynak tablo adını etkinlik çalıştırmaları liste görünümünde bir sütun olarak izleyebilirsiniz.

![Kullanıcı özellikleri olmadan etkinlik çalıştırmaları listesi](media/monitor-visually/monitor-user-properties-image2.png)

![Etkinlik çalıştırmaları listesine kullanıcı özellikleri için sütun ekleme](media/monitor-visually/monitor-user-properties-image3.png)

![Kullanıcı özellikleri için sütunları olan etkinlik çalıştırmaları listesi](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="rerun-activities-inside-a-pipeline"></a>Etkinlikleri bir işlem hattı içinde yeniden çalıştırma

Artık etkinlikleri bir işlem hattı içinde yeniden çalıştırabilirsiniz. **Etkinlik çalıştırmalarını görüntüle** ' ye tıklayın ve işlem hattınızda, işlem hattınızı yeniden çalıştırmak istediğiniz noktada etkinliği seçin.

![Etkinlik çalıştırmalarını görüntüleme](media/monitor-visually/rerun-activities-image1.png)

![Etkinlik çalıştırması seçin](media/monitor-visually/rerun-activities-image2.png)

### <a name="view-rerun-history"></a>Yeniden çalıştırma geçmişini görüntüle

Liste görünümündeki tüm işlem hattı çalıştırmaları için yeniden çalıştırma geçmişini görüntüleyebilirsiniz.

![Geçmişi görüntüleme](media/monitor-visually/rerun-history-image1.png)

Ayrıca, belirli bir işlem hattı çalıştırması için yeniden çalıştırma geçmişini görüntüleyebilirsiniz.

![İşlem hattı çalıştırmasının geçmişini görüntüleme](media/monitor-visually/rerun-history-image2.png)

## <a name="gantt-views"></a>Gantt görünümleri

Ardışık düzenleri ve etkinlik çalıştırmalarını hızlıca görselleştirmek için Gantt görünümlerini kullanın. İşlem hatlarında oluşturduğunuz ek açıklamaların/etiketlerin başına, ardışık düzen veya grup için Gantt görünümüne bakabilirsiniz.

![Gantt grafiği](media/monitor-visually/gantt1.png)

![Gantt grafiği ek açıklamaları](media/monitor-visually/gantt2.png)

Çubuğun uzunluğu, işlem hattının süresini bilgilendirir. Daha fazla ayrıntı görmek için çubuğa de tıklayabilirsiniz.

![Gantt grafiği süresi](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Kılavuzlu Turlar
Ardışık düzenin ve etkinlik çalıştırmalarının nasıl izleneceği hakkında adım adım yönergeler almak için sol alt köşedeki ' bilgi simgesi ' seçeneğine tıklayın ve ' Kılavuzlu Turlar ' seçeneğine tıklayın.

![Kılavuzlu Turlar](media/monitor-visually/guided-tours.png)

## <a name="feedback"></a>Geri Bildirim
Çeşitli özelliklerle ilgili geri bildirimde bulunmak için ' geri bildirim ' simgesine tıklayın.

![Geri Bildirim](media/monitor-visually/feedback.png)

## <a name="alerts"></a>Uyarılar

Data Factory içinde desteklenen ölçümler üzerinde uyarı oluşturabilirsiniz. Başlamak için Data Factory İzleyici sayfasında bulunan **Monitor-> Alerts & ölçümleri** ' ni seçin.

![](media/monitor-visually/alerts01.png)

Bu özelliğin yedi dakikalık bir girişi ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Uyarı Oluştur

1.  Yeni uyarı oluşturmak için **Yeni uyarı kuralı** ' na tıklayın.

    ![](media/monitor-visually/alerts02.png)

1.  Kural adını belirtin ve uyarı **önem derecesini**seçin.

    ![](media/monitor-visually/alerts03.png)

1.  Uyarı ölçütlerini seçin.

    ![](media/monitor-visually/alerts04.png)

    ![](media/monitor-visually/alerts05.png)

1.  Uyarı mantığını yapılandırın. Tüm işlem hatları ve ilgili etkinlikler için seçili ölçüm için bir uyarı oluşturabilirsiniz. Ayrıca belirli bir etkinlik türünü, etkinlik adını, işlem hattı adını veya hata türünü de seçebilirsiniz.

    ![](media/monitor-visually/alerts06.png)

1.  Uyarı için **e-posta/SMS/Push/sesli** bildirimleri yapılandırın. Uyarı bildirimleri için mevcut bir **eylem grubu** oluşturun veya seçin.

    ![](media/monitor-visually/alerts07.png)

    ![](media/monitor-visually/alerts08.png)

1.  Uyarı kuralını oluşturun.

    ![](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Sonraki adımlar

İşlem hatlarını izleme ve yönetme hakkında bilgi edinmek için işlem [hatlarını programlama yoluyla izleme ve yönetme](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) makalesine bakın.
