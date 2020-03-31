---
title: Veri ardışık hatlarını izleme ve yönetme - Azure
description: Azure veri fabrikalarını ve boru hatlarını izlemek ve yönetmek için İzleme ve Yönetim uygulamasını nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: de56ba1281d0f20c8be838fa1bc9ebc24905b26c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260377"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a>İzleme ve Yönetim uygulamasını kullanarak Azure Veri Fabrikası boru hatlarını izleme ve yönetme
> [!div class="op_single_selector"]
> * [Azure portalı/Azure PowerShell'i kullanma](data-factory-monitor-manage-pipelines.md)
> * [İzleme ve Yönetim uygulamasını kullanma](data-factory-monitor-manage-app.md)
>
>

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası ardışık hatlarını şu anda izleyin ve yönetin.](../monitor-visually.md)

Bu makalede, Veri Fabrikası ardışık hatlarınızı izlemek, yönetmek ve hata ayıklamak için İzleme ve Yönetim uygulamasının nasıl kullanılacağı açıklanmaktadır. Aşağıdaki videoyu izleyerek uygulamayı kullanmaya devam edebilirsiniz:

> [!NOTE]
> Videoda gösterilen kullanıcı arabirimi, portalda gördüklerinle tam olarak eşleşmeyebilir. Biraz daha eski, ama kavramlar aynı kalır. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a>İzleme ve Yönetim uygulamasını başlatın
Monitör ve Yönetim uygulamasını başlatmak için, veri fabrikanız için **Veri Fabrikası** bıyığı üzerindeki Monitör **& Yönet'i** tıklatın.

![Veri Fabrikası ana sayfasında kiremit izleme](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

İzleme ve Yönetim uygulamasını ayrı bir pencerede açık görmelisiniz.  

![İzleme ve Yönetim uygulaması](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Web tarayıcısının "Yetkilendirme..."nde sıkışıp kaldığını görürseniz, üçüncü taraf tanımlama bilgilerini ve site veri onay kutusunu **engelleyin** veya seçili tutun, **login.microsoftonline.com**için bir özel durum oluşturun ve uygulamayı yeniden açmayı deneyin.


Orta bölmedeki Etkinlik Windows listesinde, bir etkinliğin her çalışması için bir etkinlik penceresi görürsünüz. Örneğin, etkinliğiniz beş saat boyunca saatlik olarak çalışacak şekilde zamanlanmışsa, beş veri dilimiyle ilişkili beş etkinlik penceresi görürsünüz. Listede etkinlik pencerelerini en altta görmüyorsanız, aşağıdakileri yapın:
 
- Ardınızın başlangıç ve bitiş saatlerini eşleştirmek için en üstteki **başlangıç ve** **bitiş saati** filtrelerini güncelleştirin ve ardından **Uygula** düğmesini tıklatın.  
- Etkinlik Windows listesi otomatik olarak yenilenmez. **Etkinlik Windows** listesindeki araç çubuğundaki **Yenile** düğmesini tıklatın.  

Bu adımları test etmek için bir Veri Fabrikası uygulamanız yoksa, öğreticiyi yapın: [Verileri Veri Fabrikası'nı kullanarak Blob Storage'dan SQL Veritabanı'na kopyalayın.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

## <a name="understand-the-monitoring-and-management-app"></a>İzleme ve Yönetim uygulamasını anlama
Solda üç sekme vardır: **Kaynak Gezgini**, **İzleme Görünümleri**ve **Uyarılar.** İlk sekme **(Kaynak Gezgini)** varsayılan olarak seçilir.

### <a name="resource-explorer"></a>Kaynak Gezgini
Aşağıdakileri görüyorsunuz:

* Sol bölmedeki Kaynak Gezgini **ağaç görünümü.**
* Orta bölmenin üst **kısmındaki Diyagram Görünümü.**
* Orta bölmenin altındaki **Etkinlik Windows** listesi.
* **Özellikler**, **Etkinlik Penceresi Gezgini**ve Komut **Dosyası** sekmeleri sağ bölmede.

Kaynak Gezgini'nde, veri fabrikasındaki tüm kaynakları (ardışık hatlar, veri kümeleri, bağlantılı hizmetler) bir ağaç görünümünde görürsünüz. Kaynak Gezgini'nde bir nesne seçtiğinizde:

* İlişkili Veri Fabrikası varlığı Diyagram Görünümü'nde vurgulanır.
* [İlişkili etkinlik pencereleri](data-factory-scheduling-and-execution.md) en alttaki Etkinlik Windows listesinde vurgulanır.  
* Seçili nesnenin özellikleri sağ bölmedeki Özellikler penceresinde gösterilir.
* Seçili nesnenin JSON tanımı, varsa gösterilir. Örneğin: bağlantılı bir hizmet, veri kümesi veya bir ardışık hat.

![Kaynak Gezgini](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Etkinlik [pencereleri](data-factory-scheduling-and-execution.md) hakkında ayrıntılı kavramsal bilgiler için Zamanlama ve Yürütme makalesine bakın.

### <a name="diagram-view"></a>Diyagram Görünümü
Bir veri fabrikasının Diyagram Görünümü, bir veri fabrikasını ve varlıklarını izlemek ve yönetmek için tek bir cam bölmesağlar. Diyagram Görünümü'nde bir Veri Fabrikası varlığı (veri kümesi/ardışık kaynak) seçtiğinizde:

* Veri fabrikası varlığı ağaç görünümünde seçilir.
* İlişkili etkinlik pencereleri Etkinlik Windows listesinde vurgulanır.
* Seçili nesnenin özellikleri Özellikler penceresinde gösterilir.

Ardışık düzen hattı etkinleştirildiğinde (duraklatılmış durumda değil), yeşil bir çizgiyle gösterilir:

![Boru hattı çalışıyor](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Diyagram görünümünde seçerek ve komut çubuğundaki düğmeleri kullanarak bir ardışık hattı duraklatabilir, devam ettirebilir veya sonlandırabilirsiniz.

![Komut çubuğunda duraklatma/devam ettirme](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Diyagram Görünümü'nde ardışık hatlar için üç komut çubuğu düğmesi vardır. Ardışık mayıda duraklatmak için ikinci düğmeyi kullanabilirsiniz. Duraklatma, şu anda çalışan etkinlikleri sonlandırmaz ve tamamlanmalarına izin verir. Üçüncü düğme ardışık lığı duraklatabilir ve varolan yürütme etkinliklerini sonlandırır. İlk düğme ardışık devam eder. Ardışık hatlar duraklatıldığında, ardışık hattın rengi değişir. Örneğin, duraklatılmış bir ardışık iş neşrisi aşağıdaki resimde aşağıdaki gibi görünür: 

![Boru hattı duraklatıldı](./media/data-factory-monitor-manage-app/PipelinePaused.png)

Ctrl tuşunu kullanarak iki veya daha fazla ardışık hattı birden çok seçebilirsiniz. Aynı anda birden çok ardışık hattı duraklatmak/devam ettirmek için komut çubuğu düğmelerini kullanabilirsiniz.

Ayrıca bir ardışık hattı sağ tıklatabilir ve bir ardışık hattı askıya alma, devam ettirme veya sonlandırma seçeneklerini seçebilirsiniz. 

![Ardışık iş için bağlam menüsü](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Boru hattındaki tüm etkinlikleri görmek için **Açık ardışık hatlar** seçeneğini tıklatın. 

![İşlem hattı menüsünü açma](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

Açılan ardışık hat lar görünümünde, ardışık ardışık alandaki tüm etkinlikleri görürsünüz. Bu örnekte, yalnızca bir etkinlik vardır: Etkinliği Kopyala. 

![Açılan boru hattı](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

Önceki görünüme geri dönmek için, üstteki ekmek kırıntısı menüsündeki veri fabrikası adını tıklatın.

Ardışık pencere görünümünde, bir çıktı veri kümesi seçtiğinizde veya farenizi çıktı veri kümesi üzerinde hareket ettirdiğinizde, bu veri kümesi için Etkinlik Windows açılır penceresini görürsünüz.

![Etkinlik Windows açılır penceresi](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Sağ bölmedeki **Özellikler** penceresinde ayrıntılarını görmek için bir etkinlik penceresini tıklatabilirsiniz.

![Etkinlik penceresi özellikleri](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Sağ bölmede, daha fazla ayrıntı görmek için **Etkinlik Penceresi Gezgini** sekmesine geçin.

![Etkinlik Penceresi Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

Ayrıca, **Denemeler** bölümünde bir etkinlik için her çalıştırma girişimi için **çözümlenmiş değişkenler** de görürsünüz.

![Çözülmüş değişkenler](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Seçili nesnenin JSON komut dosyası tanımını görmek için **Komut Dosyası** sekmesine geçin.   

![Komut dosyası sekmesi](./media/data-factory-monitor-manage-app/ScriptTab.png)

Etkinlik pencerelerini üç yerde görebilirsiniz:

* Diyagram Görünümü'nde (orta bölme) Etkinlik Windows açılır penceresi.
* Sağ bölmedeki Etkinlik Penceresi Gezgini.
* Alt bölmedeki Etkinlik Windows listesi.

Etkinlik Windows açılır penceresinde ve Etkinlik Penceresi Gezgini'nde, sol ve sağ okları kullanarak önceki haftaya ve sonraki haftaya kaydırabilirsiniz.

![Etkinlik Penceresi Gezgini sol/sağ oklar](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Diyagram Görünümü'nün alt kısmında şu düğmeleri görürsünüz: Yakınlaştırma, Yakınlaştırma, Yakınlaştırmaya Yakınlaştırma, Yakınlaştırma%100, Düzeni Kilitle. **Kilit düzen** düğmesi, Diyagram Görünümü'nde tabloları ve ardışık hatları yanlışlıkla taşımanızı engeller. Varsayılan olarak açıktır. Kapatabilir ve diyagramdaki varlıkları hareket ettirebilirsiniz. Kapattığınızda, tabloları ve ardışık hatları otomatik olarak konumlandırmak için son düğmeyi kullanabilirsiniz. Ayrıca fare tekerleği kullanarak yakınlaştırabilir veya uzaklaştırabilirsiniz.

![Diyagram Yakınlaştırma komutlarını görüntüle](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>Etkinlik Pencereleri listesi
Orta bölmenin altındaki Etkinlik Windows listesi, Kaynak Gezgini'nde veya Diyagram Görünümü'nde seçtiğiniz veri kümesiiçin tüm etkinlik pencerelerini görüntüler. Varsayılan olarak, liste azalan sıradadır, bu da en son etkinlik penceresini en üstte gördüğünüz anlamına gelir.

![Etkinlik Pencereleri listesi](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Bu liste otomatik olarak yenilenmez, bu nedenle araç çubuğundaki yenileme düğmesini kullanarak el ile yenileyin.  

Etkinlik pencereleri aşağıdaki durumlardan birinde olabilir:

<table>
<tr>
    <th align="left">Durum</th><th align="left">Alt durum</th><th align="left">Açıklama</th>
</tr>
<tr>
    <td rowspan="8">Bekleniyor</td><td>Zaman Çizelgesi</td><td>Etkinlik penceresinin çalışma zamanı gelmedi.</td>
</tr>
<tr>
<td>DatasetBağımlılıklar</td><td>Yukarı bağımlılıklar hazır değil.</td>
</tr>
<tr>
<td>HesaplamaKaynakları</td><td>İşlem kaynakları kullanılamıyor.</td>
</tr>
<tr>
<td>EşzamanlılıkLimit</td> <td>Tüm etkinlik örnekleri diğer etkinlik pencerelerini çalıştırmakla meşgul.</td>
</tr>
<tr>
<td>EtkinlikDevamı</td><td>Etkinlik duraklatıldı ve devam edene kadar etkinlik pencerelerini çalıştıramıyor.</td>
</tr>
<tr>
<td>Yeniden Dene</td><td>Etkinlik yürütme yeniden denenmektedir.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama henüz başlamadı.</td>
</tr>
<tr>
<td>DoğrulamaRetry</td><td>Doğrulama yeniden denenmeyi bekliyor.</td>
</tr>
<tr>
<tr>
<td rowspan="2">Devam Ediyor</td><td>Doğrulama</td><td>Doğrulama devam ediyor.</td>
</tr>
<td>-</td>
<td>Etkinlik penceresi işleniyor.</td>
</tr>
<tr>
<td rowspan="4">Başarısız</td><td>TimedOut</td><td>Etkinlik yürütme, etkinlik tarafından izin verilenden daha uzun sürdü.</td>
</tr>
<tr>
<td>İptal edildi</td><td>Etkinlik penceresi kullanıcı eylemi tarafından iptal edildi.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama başarısız oldu.</td>
</tr>
<tr>
<td>-</td><td>Etkinlik penceresi oluşturulamadı veya doğrulanmadı.</td>
</tr>
<td>Hazır</td><td>-</td><td>Etkinlik penceresi tüketime hazırdır.</td>
</tr>
<tr>
<td>Atlandı</td><td>-</td><td>Etkinlik penceresi işlenmedi.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Farklı bir durumla var olan bir etkinlik penceresi kullanılır, ancak sıfırlanmıştır.</td>
</tr>
</table>


Listedeki bir etkinlik penceresini tıklattığınızda, etkinlik **Windows Gezgini'nde** veya sağdaki **Özellikler** penceresinde bununla ilgili ayrıntıları görürsünüz.

![Etkinlik Penceresi Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Etkinlik pencerelerini yenileme
Ayrıntılar otomatik olarak yenilenmez, bu nedenle etkinlik pencereleri listesini el ile yenilemek için komut çubuğundaki yenileme düğmesini (ikinci düğme) kullanın.  

### <a name="properties-window"></a>Özellik penceresi
Özellikler penceresi, İzleme ve Yönetim uygulamasının en sağ bölmesindedir.

![Özellik penceresi](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Kaynak Gezgini (ağaç görünümü), Diyagram Görünümü veya Etkinlik Windows listesinde seçtiğiniz öğenin özelliklerini görüntüler.

### <a name="activity-window-explorer"></a>Etkinlik Penceresi Gezgini
**Etkinlik Penceresi Gezgini** penceresi, İzleme ve Yönetim uygulamasının en sağ bölmesindedir. Etkinlik Windows açılır penceresinde veya Etkinlik Windows listesinde seçtiğiniz etkinlik penceresi yle ilgili ayrıntıları görüntüler.

![Etkinlik Penceresi Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

En üstteki takvim görünümünde tıklatarak başka bir etkinlik penceresine geçebilirsiniz. Ayrıca, önceki haftaveya sonraki haftaaktivite pencerelerini görmek için üstteki sol ok/sağ ok düğmelerini de kullanabilirsiniz.

Etkinlik penceresini yeniden çalıştırmak veya bölmedeki ayrıntıları yenilemek için alt bölmedeki araç çubuğu düğmelerini kullanabilirsiniz.

### <a name="script"></a>Betik
Seçili Veri **Script** Fabrikası varlığının (bağlantılı hizmet, veri kümesi veya ardışık ad.

![Komut dosyası sekmesi](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Sistem görünümlerini kullanma
İzleme ve Yönetim uygulaması, veri fabrikanız için en son/başarısız/devam eden etkinlik pencerelerini görüntülemenize olanak tanıyan önceden oluşturulmuş sistem görünümlerini **(Son etkinlik pencereleri,** **Başarısız etkinlik pencereleri,** Devam Eden **Etkinlik pencereleri)** içerir.

Tıklatarak soldaki **İzleme Görünümleri** sekmesine geçin.

![Görünümleri İzleme sekmesi](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Şu anda, desteklenen üç sistem görünümü vardır. Etkinlik Windows listesinde (orta bölmenin alt kısmında) son etkinlik pencerelerini, başarısız etkinlik pencerelerini veya devam eden etkinlik pencerelerini görmek için bir seçenek seçin.

Son etkinlik **pencereleri** seçeneğini seçtiğinizde, tüm son etkinlik pencerelerini **son deneme süresinin**azalan sırasına göre görürsünüz.

Listedeki tüm başarısız etkinlik pencerelerini görmek için **Başarısız etkinlik windows** görünümünü kullanabilirsiniz. **Özellikler** penceresinde veya Etkinlik Penceresi Gezgini'nde bununla ilgili **Activity Window Explorer**ayrıntıları görmek için listede başarısız bir etkinlik penceresi seçin. Başarısız bir etkinlik penceresi için tüm günlükleri de indirebilirsiniz.

## <a name="sort-and-filter-activity-windows"></a>Etkinlik pencerelerini sıralama ve filtreleme
Etkinlik pencerelerini filtrelemek için komut çubuğundaki **başlangıç saati** ve **bitiş saati** ayarlarını değiştirin. Başlangıç saatini ve bitiş saatini değiştirdikten sonra, Etkinlik Windows listesini yenilemek için bitiş saatinin yanındaki düğmeyi tıklatın.

![Başlangıç ve bitiş Saatleri](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> Şu anda, tüm zamanlar İzleme ve Yönetim uygulamasında UTC formatındadır.
>
>

Etkinlik **Windows listesinde,** bir sütunun adını tıklatın (örneğin: Durum).

![Etkinlik Windows listesi sütun menüsü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Şunları yapabilirsiniz:

* Artan sırayla sıralayın.
* Azalan sırayla sıralayın.
* Bir veya daha fazla değere göre filtre uygulayın (Hazır, Bekletme vb.).

Sütunda bir filtre belirttiğiniz zaman, sütundaki değerlerin filtrelenmiş değerler olduğunu gösteren bu sütun için etkinleştirilmiş filtre düğmesini görürsünüz.

![Etkinlik Windows listesinin bir sütununa filtre uygulayın](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Filtreleri temizlemek için aynı açılır pencereyi kullanabilirsiniz. Etkinlik Windows listesinin tüm filtrelerini temizlemek için komut çubuğundaki filtreyi temizle düğmesini tıklatın.

![Etkinlik Windows listesi için tüm filtreleri temizle](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Toplu iş eylemleri gerçekleştirme
### <a name="rerun-selected-activity-windows"></a>Seçili etkinlik pencerelerini yeniden çalıştırın
Etkinlik penceresi seçin, ilk komut çubuğu düğmesi için aşağı ok'u tıklatın ve**ardışık akışla Yeniden Çalıştırma'yı** **Rerun** / seçin. **Ardışık düzen de upstream ile Rerun'u seçtiğinizde,** tüm yukarı akış etkinlik pencerelerini de yeniden çalıştırın.
    ![Etkinlik pencereni yeniden yürütme](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Ayrıca listede birden çok etkinlik penceresi seçebilir ve bunları aynı anda yeniden çalıştırabilirsiniz. Etkinlik pencerelerini duruma göre filtrelemek isteyebilirsiniz (örneğin: **Başarısız**)--ve etkinlik pencerelerinde başarısızlığa neden olan sorunu düzeltten sonra başarısız etkinlik pencerelerini yeniden çalıştırmak isteyebilirsiniz. Listedeki etkinlik pencerelerini filtreleme yle ilgili ayrıntılar için aşağıdaki bölüme bakın.  

### <a name="pauseresume-multiple-pipelines"></a>Birden çok ardışık hattı duraklatma/devam ettirme
Ctrl tuşunu kullanarak iki veya daha fazla ardışık noktayı birden çok seçebilirsiniz. Duraklatmak/devam ettirmek için komut çubuğu düğmelerini (aşağıdaki resimdeki kırmızı dikdörtgende vurgulanan) kullanabilirsiniz.

![Komut çubuğunda duraklatma/devam ettirme](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
