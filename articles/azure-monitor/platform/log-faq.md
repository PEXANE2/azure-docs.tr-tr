---
title: Log Analytics SSS | Microsoft Docs
description: Azure Izleyici günlükleri Analizi hizmeti hakkında sık sorulan soruların yanıtları.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/01/2019
ms.openlocfilehash: 77159e0fa73a1f56688c867c55ae46f28016992c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75394794"
---
# <a name="log-analytics-faq"></a>Log Analytics SSS

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu Microsoft SSS, Azure Izleyici Log Analytics çalışma alanı hakkında sık sorulan sorulardan oluşan bir listesidir. Log Analytics hakkında ek sorularınız varsa, Git [tartışma forumuna](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) ve sorularınızı gönderin. Sık sorulan bir soru, böylece hızla ve kolayca bulunabilir, bu makaleye ekleriz.


## <a name="new-logs-experience"></a>Yeni günlükleri deneyimi

### <a name="q-whats-the-difference-between-the-new-logs-experience-and-log-analytics"></a>S: yeni günlükleri deneyimi ve Log Analytics arasındaki fark nedir?

Y: Bunlar aynı şeydir. [Log Analytics'e tümleşik Azure İzleyici'de bir özellik olarak](../../azure-monitor/azure-monitor-rebrand.md) daha birleşik izleme deneyimi sağlamak. Azure İzleyici'de yeni günlükleri deneyimini tam olarak birçok müşteri zaten kullanmakta olduğunuz Log Analytics sorguları ile aynıdır.

### <a name="q-can-i-still-use-log-search"></a>Günlük araması kullanabilir miyim? 

Y: günlük araması hala kullanılabilir OMS portalında ve adla Azure portalında şu anda **günlükleri (Klasik)** . OMS portalında resmi olarak 15 Ocak 2019 üzerinde kullanımdan kaldırılacaktır. Azure portalında Klasik günlükleri deneyimi yavaş yavaş kullanımdan kaldırılacak ve yerini yeni günlükleri deneyimi. 

### <a name="q-can-i-still-use-advanced-analytics-portal"></a>S. Gelişmiş analiz portalını kullanabilir miyim? 
Azure portalında yeni günlükleri deneyimi Gelişmiş analiz portalında bağlıdır, ancak yine de Azure portal dışında erişilebilir. Bu dış portal devre dışı bırakma için yol haritası yakında duyurulacaktır.

### <a name="q-why-cant-i-see-query-explorer-and-save-buttons-in-the-new-logs-experience"></a>S. Neden olamaz miyim sorgu Gezgini görebilir ve yeni günlükleri deneyiminde düğmeleri kaydetmek mi?

**Sorgu Gezgini**, **Kaydet** ve **ayarlamak uyarı** düğmeleri kullanılamıyor günlükleri belirli bir kaynağa bağlamında keşfederken. Kaydetme uyarıları oluşturmak veya bir sorgu yüklemek için bir çalışma alanına günlükleri kapsamlandırılmalıdır. Günlükleri çalışma bağlamında açmak için seçmeniz **tüm hizmetleri** > **İzleyici** > **günlükleri**. Son kullanılan çalışma alanına seçtiğiniz, ancak diğer çalışma alanı seçebilirsiniz. Bkz: [görüntüleme ve Log analytics'te verileri analiz etme](../log-query/portals.md) daha fazla bilgi için.

### <a name="q-how-do-i-extract-custom-fields-in-the-new-logs-experience"></a>S. Özel alanları yeni günlükleri deneyiminde nasıl çıkarabilirim? 

Y: özel alanlar ayıklama günlükleri deneyimi Klasik şu anda desteklenmiyor. 

### <a name="q-where-do-i-find-list-view-in-the-new-logs"></a>S. Liste Görünümü'ndeki yeni günlükleri nerede bulabilirim? 

Y: Liste Görünümü'ndeki yeni günlükleri kullanılamaz. Sonuçları tablodaki her bir kaydın sol ok yoktur. Belirli bir kaydın ayrıntılarını açmak için bu oka tıklayın. 

### <a name="q-after-running-a-query-a-list-of-suggested-filters-are-available-how-can-i-see-filters"></a>S. Bir sorgu çalıştırdıktan sonra önerilen filtrelerin bir listesi kullanılabilir. Filtreleri nasıl görebilirim? 

Y: yeni filtreler uygulamasının önizlemesini görmek için sol bölmedeki ' Filtreler ' seçeneğine tıklayın. Bu artık UI 10.000 kaydı sınırına göre sınırlı kalmak yerine tam Sonuç kümenizi temel alır. Şu anda en popüler filtreleri ve her filtre için en yaygın 10 değerleri listesi budur. 

### <a name="q-why-am-i-getting-the-error-register-resource-provider-microsoftinsights-for-this-subscription-to-enable-this-query-in-logs-after-drilling-in-from-vm"></a>S. Neden iletisi alıyorum hata: "Kayıt kaynak Sağlayıcısı 'Bu sorgu etkinleştirmek için Microsoft.Insights' Bu abonelik için" günlüklere sonrası ayrıntılara VM'den? 

Y: varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir, ancak, bazı kaynak sağlayıcıları elle kaydetmeniz gerekebilir. Bu, aboneliğinizin kaynak sağlayıcısı ile çalışacak şekilde yapılandırır. Kayıt için kapsam her zaman aboneliktir. Daha fazla bilgi için bkz. [Kaynak sağlayıcıları ve türleri](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

### <a name="q-why-am-i-am-getting-no-access-error-message-when-accessing-logs-from-a-vm-page"></a>S. Ben neden günlükleri bir VM sayfasından erişirken erişim hata iletisi alıyorum? 

Y: VM günlüklerini görüntülemek için VM günlüklerine depolayan okuma izni çalışma alanları verilmesi gerekir. Bu gibi durumlarda yöneticinize ile azure'da izinlerini sağlamanız gerekir.

### <a name="q-why-can-i-can-access-my-workspace-in-oms-portal-but-i-get-the-error-you-have-no-access-in-the-azure-portal"></a>S. OMS portalında çalışma Alanım erişebilirim, ancak hata alıyorum neden "Azure portalında erişiminiz yok"?  

Y: azure'da bir çalışma alanına erişmek için atanan Azure izinleri olmalıdır. Burada uygun erişim izni olmayabilir bazı durumlar vardır. Bu gibi durumlarda yöneticinize Azure.See de izinlerle sağlamanız gerekir [Azure'a taşıyarak OMS portalında](oms-portal-transition.md) daha fazla bilgi için.

### <a name="q-why-cant-i-cant-see-view-designer-entry-in-logs"></a>S. Neden Görünüm Tasarımcısı giriş günlüklerinde göremiyorum olamaz?

Y: Görünüm Tasarımcısı yalnızca katkıda bulunan izinlerine sahip ya da daha yüksek atanan kullanıcılar için günlüklerde kullanılabilir.

### <a name="q-can-i-still-use-the-analytics-portal-outside-of-azure"></a>S. Azure dışındaki Analytics portalını kullanabilir miyim?

A. Evet, Azure ve Gelişmiş analiz portalını günlükleri sayfasında aynı koda bağlıdır. Log Analytics, Azure İzleyici'nın daha birleşik bir izleme deneyimi sağlamak için bir özellik olarak tümleştiriliyor. Şu URL 'YI kullanarak analiz portalına erişmeye devam edebilirsiniz: https:\/\/Portal. loganalytics. IO/abonelikler/{SubscriptionID}/ResourceGroups/{resourceGroupName}/çalışma alanları/{Çalışmaadı}.



## <a name="general"></a>Genel

### <a name="q-how-can-i-see-my-views-and-solutions-in-azure-portal"></a>S. My görünümler ve çözümler Azure portalında nasıl görebilirim? 

A: Azure portalında görünümleri ve yüklü çözümleri bir listesi bulunur. **Tüm hizmetler**’e tıklayın. Kaynak listesinde seçin **İzleyici**, ardından **... Daha fazla**. Son kullanılan çalışma alanına seçtiğiniz, ancak diğer çalışma alanı seçebilirsiniz. 

### <a name="q-does-log-analytics-use-the-same-agent-as-azure-security-center"></a>S. Log Analytics aracı aynı zamanda Azure Güvenlik Merkezi olarak kullanıyor mu?

Y: erken Haziran 2017'de, veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanarak Azure Güvenlik Merkezi başladı. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi Platform geçişi ile ilgili SSS](../../security-center/security-center-enable-data-collection.md).

### <a name="q-what-checks-are-performed-by-the-ad-and-sql-assessment-solutions"></a>S. Hangi denetimleri AD tarafından gerçekleştirilir ve SQL değerlendirmesi çözümleri?

Y: aşağıdaki sorgu, şu anda gerçekleştirilen tüm denetimleri açıklamasını gösterir:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Sonuçlar daha sonra daha fazla inceleme için Excel'e aktarılabilir.

### <a name="q-why-do-i-see-something-different-than-oms-in-the-system-center-operations-manager-console"></a>S. System Center Operations Manager konsolunda OMS farklı bir şey neden görüyorum?

Y: hangi güncelleştirme paketi Operations bulunduğunuz Manager'ın yapılandırmanıza bağlı olarak, bir düğüm görebilirsiniz *System Center Advisor*, *operasyonel İçgörüler*, veya *Log Analytics*.

Metin dizesi güncelleştirmeye *OMS* el ile içeri aktarılması gereken bir yönetim paketinde bulunur. Geçerli metin ve işlevleri görmek için en son System Center Operations Manager güncelleştirme paketi KB makale yönergeleri izleyin ve konsolu yenileyin.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>S: bir Log Analytics şirket içi sürümü var mı?

C: Hayır. Log Analytics'e işleyen ve büyük miktarlarda veri depolayan bir ölçeklenebilir bir bulut hizmetidir. 

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>S. Veri toplama durduğunda nasıl uyarılabilirim?

Y: açıklanan adımları kullanın [yeni günlük uyarısı oluşturma](../../azure-monitor/platform/alerts-metric.md) veri toplamayı durdurur bildirilmesini sağlamak için.

Veri toplama durduğunda için uyarı oluştururken şu ayarları yapın:

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal adı** seçin **özel günlük araması**.
   - **Arama sorgusu**: `Heartbeat | summarize LastCall = max(TimeGenerated) by Computer | where LastCall < ago(15m)`
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - **Süre** , *30* dakika ve **uyarı sıklığı** her *10* dakika
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - **Adı** için *veri toplama durduruldu*
   - **Önem derecesi**: *Uyarı*

Mevcut bir belirtin veya yeni bir [eylem grubu](../../azure-monitor/platform/action-groups.md) zaman günlüğü uyarısı ölçütleri ile eşleşen böylece 15 dakikadan fazla bir süre için eksik sinyal varsa size bildirilir.

## <a name="configuration"></a>Yapılandırma

### <a name="q-can-i-change-the-name-of-the-tableblob-container-used-to-read-from-azure-diagnostics-wad"></a>S. Azure tanılama (WAD) okumak için kullanılan tablo/blob kapsayıcının adını değiştirebilirim?

A. Hayır, rastgele bir tablo veya Azure depolamadaki kapsayıcılar okumak şu anda mümkün değildir.

### <a name="q-what-ip-addresses-does-the-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-to-the-log-analytics-service"></a>S. Hangi IP adresleri Log Analytics hizmeti tarafından kullanılır? Güvenlik duvarımı yalnızca Log Analytics hizmetine trafiğine izin verir nasıl emin olabilirim?

A. Log Analytics hizmeti, Azure üzerinde oluşturulmuştur. Log Analytics IP adresleridir içinde [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

Hizmet dağıtımları yaptığınız gibi Log Analytics hizmetine gerçek IP adreslerini değiştirin. DNS adları, güvenlik duvarından geçmesine izin bölümünde belgelendirilen [ağ gereksinimleri](../../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements).

### <a name="q-i-use-expressroute-for-connecting-to-azure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>S. Azure'a bağlamak için ExpressRoute kullanıyorum. My Log Analytics trafik, ExpressRoute bağlantım kullanıyor mu?

A. ExpressRoute trafik farklı türde açıklanan [ExpressRoute belgeleri](../../expressroute/expressroute-faqs.md#supported-services).

Log analytics'e trafiği, genel eşleme ExpressRoute bağlantı hattı kullanır.

### <a name="q-is-there-a-simple-and-easy-way-to-move-an-existing-log-analytics-workspace-to-another-log-analytics-workspaceazure-subscription"></a>S. Mevcut bir Log Analytics çalışma alanını başka bir Log Analytics çalışma alanı/Azure aboneliğine taşımak için basit ve kolay bir yolu var mı?

A. `Move-AzResource` Cmdlet'i bir Log Analytics çalışma alanı ve Otomasyon hesabı bir Azure aboneliğine ait taşıma olanak tanır. Daha fazla bilgi için bkz. [Move-AzResource](https://msdn.microsoft.com/library/mt652516.aspx).

Bu değişiklik, ayrıca Azure portalında yapılabilir.

Verileri bir Log Analytics çalışma alanından diğerine taşıma veya Log Analytics verilerini depolanan bölgeyi değiştirin.

### <a name="q-how-do-i-add-log-analytics-to-system-center-operations-manager"></a>S: System Center Operations Manager için Log Analytics nasıl ekleyebilirim?

A: en son güncelleştirme paketi için güncelleştiriliyor ve yönetim paketlerini içeri aktarma, Operations Manager'ı Log Analytics'e bağlamak sağlar.

>[!NOTE]
>Log Analytics için Operations Manager bağlantısı, yalnızca System Center Operations Manager 2012 SP1 ve sonraki sürümlerinde kullanılabilir.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-to-communicate-with-log-analytics"></a>S: bir aracının Log Analytics ile iletişim kuramıyor, nasıl doğrulayabilirim?

Y: aracının Log Analytics çalışma alanıyla iletişim kurabildiğinden emin olmak Için şuraya gidin: Denetim Masası, güvenlik & ayarları **Microsoft Monitoring Agent**.

Altında **Azure Log Analytics (OMS)** sekmesinde, yeşil bir onay işareti için bakın. Yeşil onay işareti simgesine, aracıyı Azure hizmeti ile iletişim kurabildiğini olduğunu doğrular.

Sarı bir uyarı simgesi, aracının Log Analytics ile iletişimi sorunları yaşıyor anlamına gelir. Yaygın nedenlerinden biri, Microsoft Monitoring Agent hizmeti durduruldu ' dir. Hizmeti yeniden başlatmak için Hizmet Denetimi Yöneticisi'ni kullanın.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>S: nasıl Log Analytics ile iletişim kurmasını aracıyı durdurulsun mu?

Y: System Center Operations Manager, bilgisayarı Log Analytics yönetilen bilgisayarlar listesinden kaldırın. Operations Manager artık rapor Log analytics'e aracının yapılandırmasını güncelleştirir. Doğrudan log Analytics'e bağlı aracılar için bunları aracılığıyla iletişim kurmasını durdurabilirsiniz: Denetim Masası, güvenlik ve ayarları **Microsoft Monitoring Agent**.
Altında **Azure Log Analytics (OMS)** , listelenen tüm çalışma alanlarını kaldırın.

### <a name="q-why-am-i-getting-an-error-when-i-try-to-move-my-workspace-from-one-azure-subscription-to-another"></a>S: neden çalışma Alanım'ı bir Azure aboneliğine ait diğerine taşımak çalıştığınızda bir hata alıyorum?

A: bir çalışma alanı farklı bir abonelik veya kaynak grubuna taşımak için önce çalışma alanındaki Otomasyon hesabının bağlantısını kaldırmanız gerekir. Bir Otomasyon hesabının bağlantısını gerektiriyorsa Bu çözümleri kaldırılmasını çalışma alanınızda yüklü: güncelleştirme yönetimi, değişiklik izleme veya Vm'leri çalışma saatleri dışında başlatma/durdurma kaldırılır. Bu çözümleri kaldırdıktan sonra seçerek Otomasyon hesabının bağlantısını kaldırmanız **bağlı çalışma alanları** tıklayın ve sol bölmede Otomasyon hesabı kaynak **çalışma alanının bağlantısını Kaldır** Şerit üzerindeki.
 > Çözümler çalışma alanında yüklenmesi gerek kaldırıldı ve çalışma alanı Otomasyon bağlantısını taşımadan sonra açıklandı gerekir.

Hem Azure aboneliklerinde izninizin olduğundan emin olun.

### <a name="q-why-am-i-getting-an-error-when-i-try-to-update-a-savedsearch"></a>S: neden bir SavedSearch güncelleştirmeye çalıştığınızda bir hata alıyorum?

A: 'etag' API'si veya Azure Resource Manager şablonu özellikleri gövdesinde eklemeniz gerekir:
```
"properties": {
   "etag": "*",
   "query": "SecurityEvent | where TimeGenerated > ago(1h) | where EventID == 4625 | count",
   "displayName": "An account failed to log on",
   "category": "Security"
}
```

## <a name="agent-data"></a>Aracı verileri
### <a name="q-how-much-data-can-i-send-through-the-agent-to-log-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>S. Ne kadar veri Log Analytics'e Aracısı üzerinden gönderebilirim? En fazla bir müşteri başına veri miktarını var mı?
A. Karşıya yüklenen veri miktarı için bir sınır yoktur, bu, seçtiğiniz kapasite rezervasyonu veya kullandıkça öde fiyatlandırma seçeneğine dayanır. Bir Log Analytics çalışma alanı, bir müşteriden gelen hacmi işlemek üzere otomatik olarak ölçeklendirilecek şekilde tasarlanmıştır. Bu, gün başına terabayt olsa bile. Daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/monitor/).

Log Analytics aracısını, küçük ayak izine sahip olmak için tasarlanmıştır. Veri hacmi sağlayan çözümlere göre değişir. Veri hacmi hakkındaki ayrıntılı bilgileri bulmak ve çözümde tarafından dökümünü görmek [kullanım](../../azure-monitor/platform/data-usage.md) sayfası.

Daha fazla bilgi için, Log Analytics aracısının kaynak kullanımını (parmak izi) değerlendirdikten sonra sonuçlarını gösteren bir [Müşteri blogunu](https://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) okuyabilirsiniz.

### <a name="q-how-much-network-bandwidth-is-used-by-the-microsoft-management-agent-mma-when-sending-data-to-log-analytics"></a>S. Ne kadar ağ bant genişliği, verilerini Log Analytics'e gönderirken Microsoft Yönetim Aracısı (MMA) tarafından kullanılır?

A. Bant genişliği, gönderilen veri miktarı için kullanılan bir işlevdir. Ağ üzerinden gönderilen verilerin sıkıştırılır.

### <a name="q-how-much-data-is-sent-per-agent"></a>S. Aracı ne kadar veri gönderiliyor?

A. Her aracı gönderilen veri miktarını bağlıdır:

* Etkinleştirdiğiniz çözümleri
* Toplanmakta olan günlükleri ve performans sayaçları
* Günlüklerde veri hacmi

Genel kullanım gösterilir [kullanım](../../azure-monitor/platform/data-usage.md) sayfası.

İletilen veriler aracı çalışacak şekilde bilgisayarlar için ne kadar veri gönderildiğini görmek için aşağıdaki sorguyu kullanın:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```

## <a name="next-steps"></a>Sonraki adımlar

Log Analytics hakkında daha fazla bilgi edinmek için [Azure izleyici 'yi kullanmaya](../../azure-monitor/overview.md) başlayın ve dakikalar içinde çalışmaya başlayın.
