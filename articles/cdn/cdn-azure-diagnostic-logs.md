---
title: Azure tanılama günlükleri | Microsoft Docs
description: Müşteri, Azure CDN için günlük analizini etkinleştirebilir.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: allensu
ms.openlocfilehash: 35d028a38e6ac19f270abcc8708a532b3749eb39
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81254810"
---
# <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Azure tanılama günlükleri ile çekirdek analizlerini görüntüleyebilir ve bunları aşağıdakiler dahil olmak üzere bir veya daha fazla hedefe kaydedebilirsiniz:

 - Azure Depolama hesabınızın
 - Azure Event Hubs
 - [Log Analytics çalışma alanı](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Bu özellik tüm fiyatlandırma katmanları için CDN uç noktalarında kullanılabilir. 

Azure tanılama günlükleri, temel kullanım ölçümlerini CDN uç noktanıza çeşitli kaynaklara vererek bunları özelleştirilmiş bir şekilde kullanabilmeniz için size izin verir. Örneğin, aşağıdaki veri dışa aktarma türlerini yapabilirsiniz:

- Verileri blob depolamaya aktarın, CSV 'ye aktarın ve Excel 'de grafikler oluşturun.
- Event Hubs ve diğer Azure hizmetlerinden verilerle bağıntılı verileri dışarı aktarın.
- Azure Izleyici günlüklerine veri aktarma ve kendi Log Analytics çalışma alanınızdaki verileri görüntüleme

Aşağıdaki diyagramda, verilerin tipik bir CDN çekirdek analizi görünümü gösterilmektedir.

![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Şekil 1-CDN çekirdek analizi görünümü*

Tanılama günlükleri hakkında daha fazla bilgi için bkz. [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-logging-with-the-azure-portal"></a>Azure portalıyla günlüğe kaydetmeyi etkinleştirme

CDN çekirdek analizine sahip günlüğü etkinleştirmek için şu adımları izleyin:

[Azure Portal](https://portal.azure.com) oturum açın. İş akışınız için CDN 'yi etkinleştirdiyseniz, devam etmeden önce [bir Azure CDN profili ve uç noktası oluşturun](cdn-create-new-endpoint.md) .

1. Azure portal **CDN profili**' ne gidin.

2. Azure portal, bir CDN profili arayın veya panonuzdan bir tane seçin. Ardından, tanılama günlüklerini etkinleştirmek istediğiniz CDN uç noktasını seçin.

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Izleme bölümünde **tanılama günlükleri** ' ni seçin.

   **Tanılama günlükleri** sayfası görüntülenir.

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Azure depolama ile günlüğe kaydetmeyi etkinleştirme

Günlükleri depolamak üzere bir depolama hesabı kullanmak için şu adımları izleyin:
    
1. **Ad**için tanılama günlüğü ayarlarınıza bir ad girin.
 
2. **Bir depolama hesabına arşiv**' i seçin ve ardından **coreanalytics**' i seçin. 

2. **Bekletme (gün)** için bekletme günü sayısını seçin. Sıfır günlük bir bekletme, günlükleri süresiz olarak depolar. 

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png) 

3. **Depolama hesabı**' nı seçin.

    **Bir depolama hesabı seçin** sayfası görüntülenir.

4. Açılan listeden bir depolama hesabı seçin ve ardından **Tamam**' ı seçin.

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/cdn-select-storage-account.png)

5. Tanılama günlüğü ayarlarınızı yapmayı tamamladıktan sonra **Kaydet**' i seçin.

### <a name="logging-with-azure-monitor"></a>Azure Izleyici ile günlüğe kaydetme

Günlükleri depolamak üzere Azure Izleyici 'yi kullanmak için şu adımları izleyin:

1. **Tanılama günlükleri** sayfasından **Log Analytics gönder**' i seçin. 

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Azure Izleyici günlük kaydını yapılandırmak için **Yapılandır** ' ı seçin. 

   **Log Analytics çalışma alanları** sayfası görüntülenir.

    >[!NOTE] 
    >OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. **Yeni çalışma alanı oluştur**' u seçin.

    **Log Analytics çalışma alanı** sayfası görüntülenir.

    >[!NOTE] 
    >OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/07_Create-new.png)

4. **Log Analytics çalışma alanı**için bir Log Analytics çalışma alanı adı girin. Log Analytics çalışma alanı adı benzersiz olmalı ve yalnızca harf, rakam ve kısa çizgi içermelidir; boşluklara ve alt çizgilere izin verilmez. 

5. **Abonelik**için, açılan listeden var olan bir aboneliği seçin. 

6. **Kaynak grubu**için yeni bir kaynak grubu oluşturun veya var olan bir kaynak grubunu seçin.

7. **Konum**için listeden bir konum seçin.

8. Günlük yapılandırmasını panonuza kaydetmek istiyorsanız **panoya sabitle ' yi** seçin. 

9. Yapılandırmayı gerçekleştirmek için **Tamam ' ı** seçin.

10. Çalışma alanınız oluşturulduktan sonra, **tanılama günlükleri** sayfasına dönersiniz. Yeni Log Analytics çalışma alanınızın adını onaylayın.

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/09_Return-to-logging.png)

11. **Coreanalytics**' i seçin ve ardından **Kaydet**' i seçin.

12. Yeni Log Analytics çalışma alanını görüntülemek için CDN uç noktası sayfasından **çekirdek analizi** ' ni seçin.

    ![Portal-tanılama günlükleri](./media/cdn-diagnostics-log/cdn-core-analytics-page.png) 

    Log Analytics çalışma alanınız artık verileri günlüğe kaydetmek için hazırdır. Bu verileri kullanabilmeniz için, bu makalenin ilerleyen bölümlerinde ele alınan bir [Azure izleyici günlükleri çözümü](#consuming-diagnostics-logs-from-a-log-analytics-workspace)kullanmanız gerekir.

Günlük verisi gecikmeleri hakkında daha fazla bilgi için bkz. [günlük verileri gecikmeleri](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>PowerShell ile günlüğe kaydetmeyi etkinleştirme

Aşağıdaki örnek, Azure PowerShell cmdlet 'Leri aracılığıyla tanılama günlüklerinin nasıl etkinleştirileceğini gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="enabling-diagnostic-logs-in-a-storage-account"></a>Depolama hesabında tanılama günlüklerini etkinleştirme

1. Oturum açın ve bir abonelik seçin:

    Connect-AzAccount 

    Select-Azuyeniden gönderiliyor Scription-SubscriptionID 

2. Bir depolama hesabında tanılama günlüklerini etkinleştirmek için şu komutu girin:

    ```powershell
    Set-AzDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
    ```

3. Log Analytics çalışma alanında tanılama günlüklerini etkinleştirmek için şu komutu girin:

    ```powershell
    Set-AzDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
    ```

## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Azure Depolama'dan tanılama günlüklerini kullanma
Bu bölümde, Azure depolama hesabının içinde nasıl düzenlendiği CDN Core Analytics şeması açıklanmakta ve günlükleri bir CSV dosyasına indirmek için örnek kod sağlanmaktadır.

### <a name="using-microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini kullanma
Çekirdek analizi verilerine bir Azure Storage hesabından erişebilmek için önce, depolama hesabındaki içeriğe erişmek üzere bir araca ihtiyacınız vardır. Pazarda sunulan birkaç araç olsa da, önerdiğimiz bir Microsoft Azure Depolama Gezgini. Aracı indirmek için bkz. [Azure Depolama Gezgini](https://storageexplorer.com/). Yazılımı indirip yükledikten sonra, CDN tanılama günlüklerine hedef olarak yapılandırılmış aynı Azure Depolama hesabını kullanacak şekilde yapılandırın.

1.  **Microsoft Azure Depolama Gezgini** açın
2.  Depolama hesabını bulma
3.  Bu depolama hesabı altındaki **BLOB kapsayıcıları** düğümünü genişletin.
4.  *Öngörüler-logs-coreanalytics*adlı kapsayıcıyı seçin.
5.  Sonuçlar, *RESOURCEID =* olarak ilk düzeyden başlayarak sağ bölmedeki görünür. *PT1H. JSON*dosyasını buluncaya kadar her bir düzeyi seçmeye devam edin. Yolun açıklaması için bkz. [BLOB yol biçimi](cdn-azure-diagnostic-logs.md#blob-path-format).
6.  Her blob *PT1H. JSON* dosyası, belırlı bir CDN uç noktası veya özel etki alanı için bir saat için analiz günlüklerini temsil eder.
7.  Bu JSON dosyasının içeriğinin şeması, çekirdek analiz günlüklerinin bölüm şemasında açıklanmaktadır.


#### <a name="blob-path-format"></a>Blob yol biçimi

Çekirdek analizi günlükleri saatte bir oluşturulur ve veriler toplanır ve bir JSON yükü olarak tek bir Azure blobunun içinde depolanır. Depolama Gezgini Aracı, '/' öğesini dizin ayırıcı olarak yorumladığı ve hiyerarşiyi gösterdiği için, Azure Blob 'un yolu hiyerarşik bir yapı gibi görünür ve BLOB adını temsil eder. Blobun adı aşağıdaki adlandırma kuralına uyar:   

```resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json```

**Alanların açıklaması:**

|Değer|Açıklama|
|-------|---------|
|Abonelik Kimliği    |GUID biçiminde Azure aboneliğinin KIMLIĞI.|
|Kaynak grubu adı |CDN kaynaklarının ait olduğu kaynak grubunun adı.|
|Profil Adı |CDN profilinin adı|
|Uç nokta adı |CDN uç noktasının adı|
|Yıl|  Yılın dört basamaklı temsili, örneğin, 2017|
|Ay| Ay numarasının iki basamaklı temsili. 01 = Ocak... 12 = Aralık|
|Gün|   Ayın gününün iki basamaklı temsili|
|PT1H. JSON| Analiz verilerinin depolandığı gerçek JSON dosyası|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Temel analiz verilerini bir CSV dosyasına aktarma

Çekirdek analizine erişimi kolaylaştırmak için, bir araç için örnek kod sağlanır. Bu araç, JSON dosyalarını grafik veya diğer toplamaları oluşturmak için kullanılabilecek düz bir virgülle ayrılmış dosya biçimine indirmeyi sağlar.

Aracı şu şekilde kullanabilirsiniz:

1.  GitHub bağlantısını ziyaret edin:[https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv)
2.  Kodu indirin.
3.  Derlemek ve yapılandırmak için yönergeleri izleyin.
4.  Aracı çalıştırın.
5.  Elde edilen CSV dosyası, analiz verilerini basit bir düz hiyerarşide gösterir.

## <a name="consuming-diagnostics-logs-from-a-log-analytics-workspace"></a>Log Analytics çalışma alanından tanılama günlüklerini kullanma
Azure Izleyici, kullanılabilirliğini ve performansını korumak için bulutunuzu ve şirket içi ortamlarınızı izleyen bir Azure hizmetidir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar. 

Azure Izleyici 'yi kullanmak için, bu makalede daha önce açıklanan Azure Log Analytics çalışma alanında [günlüğe kaydetmeyi etkinleştirmeniz](#enable-logging-with-azure-storage) gerekir.

### <a name="using-the-log-analytics-workspace"></a>Log Analytics çalışma alanını kullanma

 Aşağıdaki diyagramda, deponun giriş ve çıkış mimarisi gösterilmektedir:

![Log Analytics çalışma alanı](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Şekil 3-Log Analytics deposu*

Yönetim çözümlerini kullanarak verileri çeşitli yollarla görüntüleyebilirsiniz. Yönetim çözümlerini [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions)'ten edinebilirsiniz.

İzleme çözümlerini, her çözümün en altında bulunan **Şimdi al** bağlantısını seçerek Azure Marketi 'nden yükleyebilirsiniz.

### <a name="add-an-azure-monitor-cdn-monitoring-solution"></a>Azure Izleyici CDN izleme çözümü ekleme

Azure Izleyici izleme çözümü eklemek için aşağıdaki adımları izleyin:

1.   Azure aboneliğinizi kullanarak Azure portal oturum açın ve panonuza gidin.
    ![Azure panosu](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. **Yeni** sayfada **Market**altında **izleme + yönetim**' i seçin.

    ![Market](./media/cdn-diagnostics-log/14_Marketplace.png)

3. **İzleme + yönetim** sayfasında **Tümünü göster**' i seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/15_See-all.png)

4. Arama kutusunda CDN 'yi arayın.

    ![Tümünü göster](./media/cdn-diagnostics-log/16_Search-for.png)

5. **Azure CDN çekirdek analizi**' ni seçin. 

    ![Tümünü göster](./media/cdn-diagnostics-log/17_Core-analytics.png)

6. **Oluştur**' u seçtikten sonra yeni bir Log Analytics çalışma alanı oluşturmanız veya var olan bir çalışma alanı kullanmanız istenir. 

    ![Tümünü göster](./media/cdn-diagnostics-log/18_Adding-solution.png)

7. Daha önce oluşturduğunuz çalışma alanını seçin. Daha sonra bir Otomasyon hesabı eklemeniz gerekir.

    ![Tümünü göster](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Aşağıdaki ekranda doldurmanız gereken Otomasyon hesabı formu gösterilmektedir. 

    ![Tümünü göster](./media/cdn-diagnostics-log/20_Automation.png)

9. Otomasyon hesabını oluşturduktan sonra çözümünüzü eklemeye hazırız. **Oluştur** düğmesini seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/21_Ready.png)

10. Çözümünüz artık çalışma alanınıza eklendi. Azure portal panonuza geri dönün.

    ![Tümünü göster](./media/cdn-diagnostics-log/22_Dashboard.png)

    Çalışma alanınıza gitmek için oluşturduğunuz Log Analytics çalışma alanını seçin. 

11. Yeni çözümünüzü görmek için **OMS portalı** kutucuğunu seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/23_workspace.png)

12. Portalınız Şu ekrana benzer şekilde görünmelidir:

    ![Tümünü göster](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Verilerinize ait birkaç görünümü görmek için kutucuklardan birini seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/25_Interior-view.png)

    Verilerin tek tek görünümlerini temsil eden diğer kutucukları görmek için sola veya sağa kaydırma yapabilirsiniz. 

    Verileriniz hakkında daha fazla ayrıntı görmek için kutucukların birini seçin.

     ![Tümünü göster](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Teklifler ve fiyatlandırma katmanları

Yönetim çözümleri için teklifleri ve fiyatlandırma katmanlarını [buradan](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)görebilirsiniz.

### <a name="customizing-views"></a>Görünümleri özelleştirme

Görünüm **tasarımcısını**kullanarak görünümü verilerinize özelleştirebilirsiniz. Tasarlamaya başlamak için Log Analytics çalışma alanınıza gidin ve **Görünüm Tasarımcısı** kutucuğunu seçin.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/27_Designer.png)

Grafiklerin türlerini sürükleyip bırakın ve analiz etmek istediğiniz veri ayrıntılarını girin.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Günlük verileri gecikmeleri

Aşağıdaki tabloda **Microsoft 'tan Azure CDN Standart**için günlük verisi gecikmeleri **Azure CDN, Akamai 'Ten standart**ve **Verizon 'den Standart/Premium Azure CDN**gösterilmektedir.

Microsoft günlük veri gecikmeleri | Verizon günlük verileri gecikmeleri | Akamai günlük verileri gecikmeleri
--- | --- | ---
1 saat gecikiyor. | 1 saat gecikmeli ve uç nokta yayma tamamlandıktan sonra görüntülenecek 2 saate kadar zaman alabilir. | 24 saat gecikiyor; 24 saatten uzun bir süre önce oluşturulduysa, görünün başlaması 2 saate kadar sürer. Yakın zamanda oluşturulduysa, günlüklerin görünmeye başlaması 25 saate kadar sürebilir.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>CDN çekirdek analizi için tanılama günlüğü türleri

Microsoft şu anda yalnızca CDN pop 'Ları/kenarlarından görüldüğü gibi HTTP yanıt istatistiklerini ve çıkış istatistiklerini gösteren ölçümleri içeren temel analiz günlükleri sunmaktadır.

### <a name="core-analytics-metrics-details"></a>Çekirdek analizi ölçüm ayrıntıları
Aşağıdaki tabloda **Microsoft 'tan Azure CDN Standard**, **Akamai 'ten standart Azure CDN**ve **Verizon 'ten Standart/Premium Azure CDN**için temel analiz günlüklerinde bulunan ölçümlerin bir listesi gösterilmektedir. Tüm ölçümler tüm sağlayıcılardan kullanılamaz, ancak bu farklar en az düzeydedir. Tablo aynı zamanda belirli bir metriğin sağlayıcıdan kullanılabilir olup olmadığını gösterir. Ölçümler yalnızca üzerinde trafiği bulunan CDN uç noktaları için kullanılabilir.


|Ölçüm                     | Açıklama | Microsoft | Verizon | Akamai |
|---------------------------|-------------|-----------|---------|--------|
| RequestCountTotal         | Bu süre boyunca toplam istek isabet sayısı. | Yes | Yes |Yes |
| RequestCountHttpStatus2xx | 2xx HTTP kodu ile sonuçlanan tüm isteklerin sayısı (örneğin, 200, 202). | Yes | Yes |Yes |
| RequestCountHttpStatus3xx | 3xx HTTP kodu ile sonuçlanan tüm isteklerin sayısı (örneğin, 300, 302). | Yes | Yes |Yes |
| RequestCountHttpStatus4xx | 4xx HTTP kodu ile sonuçlanan tüm isteklerin sayısı (örneğin, 400, 404). | Yes | Yes |Yes |
| RequestCountHttpStatus5xx | 5xx HTTP kodu ile sonuçlanan tüm isteklerin sayısı (örneğin, 500, 504). | Yes | Yes |Yes |
| RequestCountHttpStatusOthers | Diğer tüm HTTP kodlarının sayısı (2xx-5xx dışında). | Yes | Yes |Yes |
| RequestCountHttpStatus200 | 200 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Yes | Hayır  |Yes |
| RequestCountHttpStatus206 | 206 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Yes | Hayır  |Yes |
| RequestCountHttpStatus302 | 302 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Yes | Hayır  |Yes |
| RequestCountHttpStatus304 | 304 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Yes | Hayır  |Yes |
| RequestCountHttpStatus404 | 404 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Yes | Hayır  |Yes |
| RequestCountCacheHit | Önbellek okuması ile sonuçlanan tüm isteklerin sayısı. Varlık doğrudan POP 'tan istemciye sunulur. | Yes | Yes | Hayır  |
| Requestcountcacheisabetsizlik | Önbellek isabetsizliği ile sonuçlanan tüm isteklerin sayısı. Önbellek isabetsizliği, varlığın istemciye en yakın POP üzerinde bulunmadığı ve bu nedenle kaynaktan alındığı anlamına gelir. | Yes | Yes | Hayır |
| RequestCountCacheNoCache | Bir varlık için, uçta Kullanıcı yapılandırması nedeniyle önbelleğe alınması engellenen tüm isteklerin sayısı. | Yes | Yes | Hayır |
| RequestCountCacheUncacheable | Varlığın Cache-Control ve Expires üstbilgileri tarafından önbelleğe alınması engellenen ve bir POP 'ta veya HTTP istemcisi tarafından önbelleğe alınmamalıdır belirten varlıkların tüm isteklerinin sayısı. | Yes | Yes | Hayır |
| Requestcountcachediğerleri | Yukarıda yer almayan önbellek durumuna sahip tüm isteklerin sayısı. | Hayır | Yes | Hayır  |
| Yumurresstotal | GB cinsinden giden veri aktarımı | Yes |Yes |Yes |
| EgressHttpStatus2xx | GB cinsinden 2xx HTTP durum koduna sahip yanıtlar için giden veri aktarımı *. | Yes | Yes | Hayır  |
| EgressHttpStatus3xx | GB olarak 3xx HTTP durum koduna sahip yanıtlar için giden veri aktarımı. | Yes | Yes | Hayır  |
| EgressHttpStatus4xx | GB cinsinden 4xx HTTP durum koduna sahip yanıtlar için giden veri aktarımı. | Yes | Yes | Hayır  |
| EgressHttpStatus5xx | GB olarak 5 xx HTTP durum koduna sahip yanıtlar için giden veri aktarımı. | Yes | Yes | Hayır |
| EgressHttpStatusOthers | GB olarak diğer HTTP durum kodlarına sahip yanıtlar için giden veri aktarımı. | Yes | Yes | Hayır  |
| Yumurresscachehit | CDN pop 'Ları/kenarları üzerindeki CDN önbelleğinden doğrudan teslim edilen yanıtlar için giden veri aktarımı. | Yes | Yes | Hayır |
| Yumurresscacheisabetsizlik. | En yakın POP sunucusunda bulunmayan ve kaynak sunucudan alınan yanıtlar için giden veri aktarımı. | Yes | Yes | Hayır |
| Yumurresscachenocache | Kenarda Kullanıcı yapılandırması nedeniyle önbelleğe alınması engellenen varlıkların giden veri aktarımı. | Yes | Yes | Hayır |
| Yumurresscacheuncacheable | Varlığın Cache-Control ve/veya Expires üstbilgileri tarafından önbelleğe alınması engellenen varlıkların giden veri aktarımı. Bir POP veya HTTP istemcisi tarafından önbelleğe alınmamalıdır. | Yes | Yes | Hayır |
| Yumurresscachediğerleri | Diğer önbellek senaryoları için giden veri aktarımları. | Hayır | Yes | Hayır |

* Giden veri aktarımı, CDN POP sunucularından istemciye teslim edilen trafiğin anlamına gelir.


### <a name="schema-of-the-core-analytics-logs"></a>Çekirdek analiz günlüklerinin şeması 

Tüm Günlükler JSON biçiminde depolanır ve her girdinin aşağıdaki şemaya göre dize alanları vardır:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Burada *saat* , istatistiklerin bildirildiği saat sınırının başlangıç saatini gösterir. Bir ölçüm bir CDN sağlayıcısı tarafından, Double veya integer değeri yerine bir veya daha fazla değer olarak desteklenmiyorsa, null bir değer vardır. Bu null değer, bir ölçümün yokluğunu gösterir ve 0 değerinden farklıdır. Uç noktada yapılandırılmış etki alanı başına bu ölçümler kümesi vardır.

Örnek Özellikler:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Azure CDN ek Portal aracılığıyla temel analiz](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Azure Log Analytics REST API](https://docs.microsoft.com/rest/api/loganalytics)







