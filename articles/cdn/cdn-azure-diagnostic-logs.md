---
title: Azure tanı günlükleri | Microsoft Dokümanlar
description: Müşteri Azure CDN için günlük çözümlemesini etkinleştirebilir.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: magattus
ms.openlocfilehash: 86696ed6715b4e43a9d02232c013eb64feb61f67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67594201"
---
# <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Azure tanı günlükleri ile temel analitiği görüntüleyebilir ve aşağıdakiler dahil olmak üzere bir veya daha fazla noktaya kaydedebilirsiniz:

 - Azure Depolama hesabınızın
 - Azure Event Hubs
 - [Log Analytics çalışma alanı](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Bu özellik, tüm fiyatlandırma katmanları için CDN uç noktalarında kullanılabilir. 

Azure tanılama günlükleri, cdn bitiş noktanızdan çeşitli kaynaklara temel kullanım ölçümlerini dışa aktarmanıza olanak tanır, böylece bunları özelleştirilmiş bir şekilde tüketebilirsiniz. Örneğin, aşağıdaki veri dışa aktarma türlerini yapabilirsiniz:

- Verileri blob depolamasına aktarın, CSV'ye dışa aktarın ve Excel'de grafikler oluşturun.
- Verileri Olay Hub'larına aktarın ve diğer Azure hizmetlerinden gelen verilerle ilişkilendirin.
- Verileri Azure Monitor günlüklerine aktarın ve verileri kendi Log Analytics çalışma alanınızda görüntüleyin

Aşağıdaki diyagram, verilerin tipik bir CDN çekirdek analizi görünümünü gösterir.

![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Şekil 1 - CDN çekirdek analizi görünümü*

Tanılama günlükleri hakkında daha fazla bilgi için [Tanılama Günlükleri'ne](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)bakın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-logging-with-the-azure-portal"></a>Azure portalıyla günlüğe kaydetmeyi etkinleştirme

CDN çekirdek analitiği ile günlüğe kaydetmeyi etkinleştiren aşağıdaki adımları izleyin:

[Azure portalında](https://portal.azure.com)oturum açın. İş akışınız için CDN'yi etkinleştirmediyseniz, devam etmeden önce [bir Azure CDN profili ve bitiş noktası oluşturun.](cdn-create-new-endpoint.md)

1. Azure portalında **CDN profiline**gidin.

2. Azure portalında, bir CDN profili arayın veya panonuzdan birini seçin. Ardından, tanılama günlüklerini etkinleştirmek istediğiniz CDN bitiş noktasını seçin.

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. İzleme bölümünde **Tanılama günlüklerini** seçin.

   **Tanılama günlükleri** sayfası görüntülenir.

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Azure Depolama ile günlüğe kaydetmeyi etkinleştirme

Günlükleri depolamak için bir depolama hesabı kullanmak için aşağıdaki adımları izleyin:
    
1. **Ad**için, tanılama günlüğü ayarlarınız için bir ad girin.
 
2. **Bir depolama hesabına Arşiv'i**seçin, ardından **CoreAnalytics'i**seçin. 

2. **Bekletme (gün)** için bekletme gün sayısını seçin. Sıfır gün bekletme günlükleri süresiz olarak depolar. 

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png) 

3. **Depolama hesabı'nı**seçin.

    **Depolama hesabı seç** sayfası görüntülenir.

4. Açılan listeden bir depolama hesabı seçin ve **ardından Tamam'ı**seçin.

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/cdn-select-storage-account.png)

5. Tanılama günlüğü ayarlarınızı tamamladıktan sonra **Kaydet'i**seçin.

### <a name="logging-with-azure-monitor"></a>Azure Monitörü ile Günlüğe Kaydetme

Günlükleri depolamak için Azure Monitor'u kullanmak için aşağıdaki adımları izleyin:

1. **Tanılama günlükleri** sayfasından, **Günlük Analitiğine Gönder'i**seçin. 

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Azure Monitor günlüğe kaydetmeyi yapılandırmak için **Yapılandırma'yı** seçin. 

   **Günlük Analizi çalışma alanları** sayfası görüntülenir.

    >[!NOTE] 
    >OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. **Yeni Çalışma Alanı Oluştur'u**seçin.

    **Günlük Analizi çalışma alanı** sayfası görüntülenir.

    >[!NOTE] 
    >OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/07_Create-new.png)

4. **Log Analytics çalışma alanı**için bir Log Analytics çalışma alanı adı girin. Log Analytics çalışma alanı adı benzersiz olmalı ve yalnızca harfler, sayılar ve tireler içermelidir; boşluklara ve alt çizere izin verilmez. 

5. **Abonelik**için açılan listeden varolan bir abonelik seçin. 

6. **Kaynak grubu**için yeni bir kaynak grubu oluşturun veya varolan bir tane seçin.

7. **Konum**için, listeden bir konum seçin.

8. Günlük yapılandırmasını panonuza kaydetmek istiyorsanız **panoya Pin'i** seçin. 

9. Yapılandırmayı tamamlamak için **Tamam'ı** seçin.

10. Çalışma alanınız oluşturulduktan sonra Tanılama **günlükleri** sayfasına döndürülür. Yeni Log Analytics çalışma alanınızın adını onaylayın.

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/09_Return-to-logging.png)

11. **CoreAnalytics'i**seçin, ardından **Kaydet'i**seçin.

12. Yeni Log Analytics çalışma alanını görüntülemek için CDN bitiş noktası sayfanızdan **Çekirdek analizini** seçin.

    ![portal - Tanılama günlükleri](./media/cdn-diagnostics-log/cdn-core-analytics-page.png) 

    Log Analytics çalışma alanınız artık verileri günlüğe kaydetmeye hazır. Bu verileri tüketmek için, bu makalenin ilerleyen aylarında kapsanan bir [Azure Monitor günlükleri çözümünü](#consuming-diagnostics-logs-from-a-log-analytics-workspace)kullanmanız gerekir.

Günlük veri gecikmeleri hakkında daha fazla bilgi için veri [gecikmelerini günlüğe](#log-data-delays)bürün.

## <a name="enable-logging-with-powershell"></a>PowerShell ile günlüğe kaydetmeyi etkinleştirme

Aşağıdaki örnek, Azure PowerShell Cmdlets üzerinden tanıgünlüklerinin nasıl etkinleştirilen gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="enabling-diagnostic-logs-in-a-storage-account"></a>Depolama hesabındatanı günlüklerini etkinleştirme

1. Giriş yapın ve bir abonelik seçin:

    Bağlan-AzHesap 

    Select-AzureSubscription -SubscriptionId 

2. Depolama hesabında Tanılama Günlükleri etkinleştirmek için şu komutu girin:

    ```powershell
    Set-AzDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
    ```

3. Log Analytics çalışma alanında tanılama günlemelerini etkinleştirmek için şu komutu girin:

    ```powershell
    Set-AzDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
    ```

## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Azure Depolama'dan tanılama günlüklerini kullanma
Bu bölümde CDN çekirdek analitiğinin şeması, azure depolama hesabının içinde nasıl düzenlendiği açıklanır ve bir CSV dosyasındaki günlükleri indirmek için örnek kod sağlar.

### <a name="using-microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini'ni kullanma
Azure depolama hesabından temel analiz verilerine erişebilmeniz için önce bir depolama hesabındaki içeriklere erişmek için bir araca ihtiyacınız vardır. Piyasada çeşitli araçlar olsa da, önerdiğimiz araç Microsoft Azure Depolama Gezgini'dir. Aracı indirmek için [Azure Depolama Gezgini'ne](https://storageexplorer.com/)bakın. Yazılımı indirip yükledikten sonra, CDN Diagnostics Logs'a hedef olarak yapılandırılan aynı Azure depolama hesabını kullanacak şekilde yapılandırın.

1.  **Microsoft Azure Depolama Gezgini'ni** aç
2.  Depolama hesabını bulma
3.  **Blob Kapsayıcılar** düğümlerini bu depolama hesabının altında genişletin.
4.  *Insights-logs-coreanalytics*adlı kapsayıcıyı seçin.
5.  Sonuçlar sağ bölmede, birinci düzeyden başlayarak *resourceId= olarak*gösterir. *PT1H.json*dosyasını bulana kadar her düzeyi seçmeye devam edin. Yolun açıklaması için [Blob yol biçimine](cdn-azure-diagnostic-logs.md#blob-path-format)bakın.
6.  Her blob *PT1H.json* dosyası, belirli bir CDN bitiş noktası veya özel etki alanı için bir saat boyunca analiz günlüklerini temsil eder.
7.  Bu JSON dosyasının içeriğinin şeması, çekirdek analiz günlüklerinin kesitinde açıklanmıştır.


#### <a name="blob-path-format"></a>Blob yol biçimi

Çekirdek analiz günlükleri her saat başı oluşturulur ve veriler JSON yükü olarak tek bir Azure blob'unda toplanır ve depolanır. Depolama gezgini aracı '/' dizin ayırıcısı olarak yorumladığı ve hiyerarşiyi gösterdiğinden, Azure blob'una giden yol hiyerarşik bir yapı varmış gibi görünür ve blob adını temsil eder. Blob adı aşağıdaki adlandırma kuralı izler:   

```resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json```

**Alanların açıklaması:**

|Değer|Açıklama|
|-------|---------|
|Abonelik Kimliği    |Azure aboneliğinin Guid biçimindeki kimliği.|
|Kaynak Grup Adı |CDN kaynaklarının ait olduğu kaynak grubunun adı.|
|Profil Adı |CDN Profilinin Adı|
|Bitiş Noktası Adı |CDN Bitiş Noktası'nın adı|
|Yıl|  Yılın dört basamaklı temsili, örneğin, 2017|
|Ay| Ay sayısının iki basamaklı gösterimi. 01=Ocak ... 12=Aralık|
|Gün|   Ayın gününün iki basamaklı gösterimi|
|PT1H.json| Analitik verilerin depolandığı gerçek JSON dosyası|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Temel analitik verilerini bir CSV dosyasına dışa aktarma

Çekirdek analitiğine erişebilmek için bir araç için örnek kod sağlanır. Bu araç, JSON dosyalarını grafikler veya diğer toplamalar oluşturmak için kullanılabilecek, virgülle ayrılmış düz bir dosya biçimine indirmenize olanak tanır.

Aracı şu şekilde kullanabilirsiniz:

1.  GitHub bağlantısını ziyaret edin:[https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv)
2.  Kodu indirin.
3.  Derlemek ve yapılandırmak için yönergeleri izleyin.
4.  Aracı çalıştırın.
5.  Ortaya çıkan CSV dosyası, analiz verilerini basit bir düz hiyerarşide gösterir.

## <a name="consuming-diagnostics-logs-from-a-log-analytics-workspace"></a>Log Analytics çalışma alanından tanılama günlüklerini kullanma
Azure Monitor, kullanılabilirliklerini ve performanslarını korumak için bulutve şirket içi ortamlarınızı izleyen bir Azure hizmetidir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar. 

Azure Monitor'u kullanmak için, bu makalede daha önce tartışılan Azure Günlük Analizi çalışma alanına [günlüğe kaydetmeyi etkinleştirmeniz](#enable-logging-with-azure-storage) gerekir.

### <a name="using-the-log-analytics-workspace"></a>Günlük Analizi çalışma alanını kullanma

 Aşağıdaki diyagram, deponun girdi ve çıktılarının mimarisini gösterir:

![Log Analytics çalışma alanı](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Şekil 3 - Log Analytics Deposu*

Yönetim Çözümleri'ni kullanarak verileri çeşitli şekillerde görüntüleyebilirsiniz. [Azure](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions)Marketi'nden Yönetim Çözümleri alabilirsiniz.

Her çözümün altındaki **Şimdi Al** bağlantısını seçerek Azure pazar noktasından izleme çözümleri yükleyebilirsiniz.

### <a name="add-an-azure-monitor-cdn-monitoring-solution"></a>Azure MonitörCDN izleme çözümü ekleme

Azure Monitörizleme çözümü eklemek için aşağıdaki adımları izleyin:

1.   Azure aboneliğinizi kullanarak Azure portalında oturum açın ve panonuza gidin.
    ![Azure panosu](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. **Yeni** sayfada, **Marketplace**altında, İzleme **+ yönetimi**seçin.

    ![Market](./media/cdn-diagnostics-log/14_Marketplace.png)

3. İzleme **+ yönetim** sayfasında **Tümünü Gör'ü**seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/15_See-all.png)

4. Arama kutusunda CDN'yi arayın.

    ![Tümünü göster](./media/cdn-diagnostics-log/16_Search-for.png)

5. **Azure CDN Çekirdek Analizi'ni**seçin. 

    ![Tümünü göster](./media/cdn-diagnostics-log/17_Core-analytics.png)

6. **Oluştur'u**seçtikten sonra, yeni bir Log Analytics çalışma alanı oluşturmanız veya varolan bir çalışma alanı kullanmanız istenir. 

    ![Tümünü göster](./media/cdn-diagnostics-log/18_Adding-solution.png)

7. Daha önce oluşturduğunuz çalışma alanını seçin. Daha sonra bir otomasyon hesabı eklemeniz gerekir.

    ![Tümünü göster](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Aşağıdaki ekranda doldurmanız gereken otomasyon hesap formunu gösterilmektedir. 

    ![Tümünü göster](./media/cdn-diagnostics-log/20_Automation.png)

9. Otomasyon hesabını oluşturduktan sonra, çözümünüzü eklemeye hazırsınız. **Oluştur** düğmesini seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/21_Ready.png)

10. Çözümünüz artık çalışma alanınıza eklendi. Azure portal panonuza geri dönün.

    ![Tümünü göster](./media/cdn-diagnostics-log/22_Dashboard.png)

    Çalışma alanınıza gitmek için oluşturduğunuz Günlük Analizi çalışma alanını seçin. 

11. Yeni çözümünüzü görmek için **OMS Portal** döşemesini seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/23_workspace.png)

12. Portalınız artık aşağıdaki ekrana benzemeli:

    ![Tümünü göster](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Verilerinizin çeşitli görünümlerini görmek için kutucuklardan birini seçin.

    ![Tümünü göster](./media/cdn-diagnostics-log/25_Interior-view.png)

    Verilere tek tek görünümleri temsil eden diğer döşemeleri görmek için sola veya sağa kaydırabilirsiniz. 

    Verileriniz hakkında daha fazla ayrıntı görmek için kutucuklardan birini seçin.

     ![Tümünü göster](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Teklifler ve fiyatlandırma katmanları

Yönetim çözümleri için teklifleri ve fiyatlandırma katmanlarını [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)görebilirsiniz.

### <a name="customizing-views"></a>Görünümleri özelleştirme

**Görünüm Tasarımcısı'nı**kullanarak görünümü verilerinize özelleştirebilirsiniz. Tasarıma başlamak için Log Analytics çalışma alanınıza gidin ve **Görünüm Tasarımcısı** döşemesini seçin.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/27_Designer.png)

Grafik türlerini sürükleyip bırakın ve çözümlemek istediğiniz veri ayrıntılarını doldurun.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Günlük veri gecikmeleri

Aşağıdaki tablo, **Microsoft'tan Azure CDN Standard,** **Akamai'den Azure CDN Standard**ve **Verizon'dan Azure CDN Standard/Premium**için günlük veri gecikmelerini gösterir.

Microsoft günlük veri gecikmeleri | Verizon günlük veri gecikmeleri | Akamai günlük veri gecikmeleri
--- | --- | ---
1 saat gecikti. | 1 saat gecikti ve bitiş noktası yayılımı tamamlandıktan sonra görünmeye başlaması 2 saat kadar sürebilir. | 24 saat gecikti; 24 saatten daha önce oluşturulduysa, görünmeye başlaması 2 saat kadar sürer. Yakın zamanda oluşturulduysa, günlüklerin görünmeye başlaması 25 saat kadar sürebilir.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>CDN çekirdek analitiği için tanılama günlüğü türleri

Microsoft şu anda yalnızca, CDN'lerden/kenarlarından görüldüğü gibi HTTP yanıt istatistiklerini ve çıkış istatistiklerini gösteren ölçümleri içeren temel analiz günlükleri sunmaktadır.

### <a name="core-analytics-metrics-details"></a>Temel analitik ölçümleri ayrıntıları
Aşağıdaki tablo, **Microsoft'tan Azure CDN Standardı,** **Akamai'den Azure CDN Standardı**ve **Verizon'dan Azure CDN Standard/Premium**için temel analiz günlüklerinde bulunan ölçümlerin bir listesini gösterir. Bu tür farklılıklar en az olmasına rağmen, tüm ölçümler tüm sağlayıcılar tarafından kullanılamaz. Tablo, belirli bir ölçümün sağlayıcıdan kullanılabilir olup olmadığını da görüntüler. Ölçümler yalnızca üzerinde trafik olan CDN uç noktaları için kullanılabilir.


|Ölçüm                     | Açıklama | Microsoft | Verizon | Akamai |
|---------------------------|-------------|-----------|---------|--------|
| İstekSayısıToplam         | Bu dönemdeki toplam istek isabet sayısı. | Evet | Evet |Evet |
| İstek SayısıHttpStatus2xx | 2xx HTTP koduyla sonuçlanan tüm isteklerin sayısı (örneğin, 200, 202). | Evet | Evet |Evet |
| İstek SayısıHttpStatus3xx | 3xx HTTP koduyla sonuçlanan tüm isteklerin sayısı (örneğin, 300, 302). | Evet | Evet |Evet |
| İstek SayısıHttpStatus4xx | 4xx HTTP koduyla sonuçlanan tüm isteklerin sayısı (örneğin, 400, 404). | Evet | Evet |Evet |
| İstek SayısıHttpStatus5xx | 5xx HTTP koduyla sonuçlanan tüm isteklerin sayısı (örneğin, 500, 504). | Evet | Evet |Evet |
| İstek SayısıhttpstatusOthers | Diğer tüm HTTP kodları (2xx-5xx dışında) sayısı. | Evet | Evet |Evet |
| İstek SayısıhttpStatus200 | 200 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| İstek SayısıhttpStatus206 | 206 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| İstek SayısıhttpStatus302 | 302 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| İstek SayısıhttpStatus304 | 304 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| İstek SayısıhttpStatus404 | 404 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| İstekCountÖnbellekHit | Önbellek isabeti ile sonuçlanan tüm isteklerin sayısı. Varlık doğrudan POP'tan istemciye sunuldu. | Evet | Evet | Hayır  |
| İstekCountCacheMiss | Önbellek kaçamaz ile sonuçlanan tüm isteklerin sayısı. Önbellek miss, varlığın istemciye en yakın POP'ta bulunmadığı ve bu nedenle Origin'den alındığı anlamına gelir. | Evet | Evet | Hayır |
| İstekCountÖnbellek | Kenardaki bir kullanıcı yapılandırması nedeniyle önbelleğe alınmaması engellenen bir varlığa yapılan tüm isteklerin sayısı. | Evet | Evet | Hayır |
| İstekCountÖnbelleğe | Varlığın Önbellek Denetimi ve Süresi Dolan üstbellekleri tarafından önbelleğe alınması engellenen varlıklara yönelik tüm isteklerin sayısı, bunun bir POP'ta veya HTTP istemcisi tarafından önbelleğe alınmaması gerektiğini gösterir. | Evet | Evet | Hayır |
| İstekCountÖnbellekDiğerleri | Önbellek durumu yukarıda yer almayan tüm isteklerin sayısı. | Hayır | Evet | Hayır  |
| ÇıkışToplam | GB'de giden veri aktarımı | Evet |Evet |Evet |
| ÇıkışHttpStatus2xx | GB'de 2xx HTTP durum kodları içeren yanıtlar için giden veri aktarımı* | Evet | Evet | Hayır  |
| ÇıkışHttpStatus3xx | GB 3xx HTTP durum kodları ile yanıtlar için giden veri aktarımı. | Evet | Evet | Hayır  |
| ÇıkışHttpStatus4xx | GB 4xx HTTP durum kodları ile yanıtlar için giden veri aktarımı. | Evet | Evet | Hayır  |
| ÇıkışHttpStatus5xx | GB 5xx HTTP durum kodları ile yanıtlar için giden veri aktarımı. | Evet | Evet | Hayır |
| ÇıkışHttpStatusOthers | GB'deki diğer HTTP durum kodlarıyla yanıtlar için giden veri aktarımı. | Evet | Evet | Hayır  |
| ÇıkışCacheHit | CDN'ler/Kenarlar üzerindeki CDN önbelleğinden doğrudan teslim edilen yanıtlar için giden veri aktarımı. | Evet | Evet | Hayır |
| ÇıkışCacheMiss. | En yakın POP sunucusunda bulunmayan ve kaynak sunucudan alınan yanıtlar için giden veri aktarımı. | Evet | Evet | Hayır |
| ÇıkışCacheNoCache | Kenardaki bir kullanıcı yapılandırması nedeniyle önbelleğe alınmaları engellenen varlıklar için giden veri aktarımı. | Evet | Evet | Hayır |
| ÇıkışCacheUncacheable | Varlığın Önbellek Denetimi ve/veya Sona Erer üstbilgileri tarafından önbelleğe alınmaları engellenen varlıklar için giden veri aktarımı. Pop'ta veya HTTP istemcisi tarafından önbelleğe alınmaması gerektiğini gösterir. | Evet | Evet | Hayır |
| ÇıkışCacheOthers | Diğer önbellek senaryoları için giden veri aktarımları. | Hayır | Evet | Hayır |

*Giden veri aktarımı, CDN POP sunucularından istemciye teslim edilen trafiği ifade eder.


### <a name="schema-of-the-core-analytics-logs"></a>Çekirdek analiz günlüklerinin şeması 

Tüm günlükler JSON biçiminde depolanır ve her girişte aşağıdaki şemaya göre dize alanları vardır:

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

*Zaman,* istatistiklerin raporlandığı saat sınırının başlangıç saatini temsil eder. Bir metrik bir CDN sağlayıcısı tarafından desteklenmiyorsa, çift veya tamsayı değeri yerine, null bir değer vardır. Bu null değeri bir metnin yokluğunu gösterir ve 0 değerinden farklıdır. Bitiş noktasında yapılandırılan etki alanı başına bu ölçümlerden bir küme vardır.

Örnek özellikler:

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

* [Azure Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Azure CDN ek portalı üzerinden temel analitik](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Azure Monitör günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Azure Günlük Analizi REST API](https://docs.microsoft.com/rest/api/loganalytics)







