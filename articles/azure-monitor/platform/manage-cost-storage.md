---
title: Azure Monitör Günlükleri için kullanımı ve maliyetleri yönetme | Microsoft Dokümanlar
description: Azure Monitor'daki Log Analytics çalışma alanınız için fiyatlandırma planını nasıl değiştireceğinizi ve veri hacmi ve bekletme ilkesini nasıl yöneteceğinizi öğrenin.
services: azure-monitor
documentationcenter: azure-monitor
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 6911afa5dfcd14f9e5d1068acbcb2355200c5545
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479816"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Azure Monitör Günlükleri ile kullanımı ve maliyetleri yönetme

> [!NOTE]
> Bu makalede, Azure Monitör Günlükleri için maliyetlerinizi nasıl anlayacağınızı ve denetlayacağınızı açıklanmaktadır. İlgili bir makale, [Kullanımı ve tahmini maliyetleri izleme,](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs) farklı fiyatlandırma modelleri için birden çok Azure izleme özelliği nde kullanımı niçin görüntülenebildiğini ve tahmini maliyetleri açıklar.

> [!NOTE]
> Bu makalede gösterilen tüm fiyatlar ve maliyetler yalnızca örnek amaçlar içindir. 

Azure Monitör Günlükleri, kuruluşunuzdaki veya Azure'da dağıtılan herhangi bir kaynaktan günde büyük miktarda veri toplamayı, dizine kaydetmeyi ve depolamayı ölçeklendirmek ve desteklemek üzere tasarlanmıştır.  Bu, kuruluşunuz için birincil bir sürücü olsa da, maliyet verimliliği sonuçta altta yatan sürücüdür. Bu amaçla, Log Analytics çalışma alanının maliyetinin yalnızca toplanan verilerin hacmine dayanmadığını, aynı zamanda seçilen plana ve bağlı kaynaklarınızdan oluşturulan verileri ne kadar süreyle depolamayı seçtiğinize bağlı olduğunu anlamak önemlidir.  

Bu makalede, sindirilen veri hacmini ve depolama büyümesini nasıl proaktif olarak izleyebilirsiniz ve ilişkili maliyetleri denetlemek için sınırları tanımlayabilirsiniz. 

## <a name="pricing-model"></a>Fiyatlandırma modeli

Log Analytics için varsayılan fiyatlandırma, satın alınan veri hacmine ve isteğe bağlı olarak daha uzun veri saklamaya dayalı bir **Ödeme-Ardından** Ödeme modelidir. Veri hacmi, depolanacak verilerin boyutu olarak ölçülür. Her Log Analytics çalışma alanı ayrı bir hizmet olarak ücretlendirilir ve Azure aboneliğinizin faturasına katkıda bulunur. Veri alım miktarı aşağıdaki faktörlere bağlı olarak önemli olabilir: 

  - Etkin yönetim çözümlerinin sayısı ve bunların yapılandırması (örn. 
  - İzlenen VM sayısı
  - İzlenen her VM'den toplanan veri türü 
  
In addition to the Pay-As-You-Go model, Log Analytics has **Capacity Reservation** tiers which enable you to save as much as 25% compared to the Pay-As-You-Go price. Kapasite rezervasyon fiyatlandırması, 100 GB/gün'den başlayan bir rezervasyon satın almanızı sağlar. Rezervasyon seviyesinin üzerindeki tüm kullanımlar, Ödeme Niz-Kullan fiyatından faturalandırılır. Kapasite Rezervasyon katmanları 31 günlük bir taahhüt süresine sahiptir. Taahhüt süresi boyunca, daha yüksek bir Kapasite Rezervasyon katmanına (31 günlük taahhüt süresini yeniden başlatacak) değiştirebilirsiniz, ancak taahhüt süresi bitene kadar Ödeme-As-You-Go'ya veya daha düşük bir Kapasite Rezervasyon katmanına geri dönemezsiniz Bit -miş. 
Log Analytics You-You-Go Ödemesi ve Kapasite Rezervasyonu fiyatlandırması hakkında [daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/details/monitor/) 

Tüm fiyatlandırma katmanlarında, veri hacmi depolanacak şekilde hazırlanırken verilerin dize gösteriminden hesaplanır. [Tüm veri türlerinde ortak](https://docs.microsoft.com/azure/azure-monitor/platform/log-standard-properties) olan çeşitli özellikler, `_ResourceId`olay boyutunun hesaplanmasına , , `_ItemId` `_IsBillable` ve `_BilledSize`.

Ayrıca, [Azure Güvenlik Merkezi](https://azure.microsoft.com/pricing/details/security-center/) ve Azure [Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel/)gibi bazı çözümlerin kendi fiyatlandırma modeliolduğunu unutmayın. 

## <a name="estimating-the-costs-to-manage-your-environment"></a>Çevrenizi yönetmek için maliyetleri tahmin etme 

Henüz Azure Monitör Günlükleri kullanmıyorsanız, Log Analytics'i kullanmanın maliyetini tahmin etmek için [Azure Monitor fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/?service=monitor) kullanabilirsiniz. Arama kutusuna "Azure Monitörü" girerek ve ortaya çıkan Azure Monitor kutucuğuna tıklayarak başlayın. Sayfayı Azure Monitor'a kaydırın ve Tür açılır sayfasından Log Analytics'i seçin.  Buraya, her VM'den toplamayı beklediğiniz VM sayısını ve GB verisini girebilirsiniz. Genellikle 1 ila 3 GB veri ayı, tipik bir Azure VM'sinden yutulur. Azure Monitör Günlüklerini zaten değerlendiriyorsanız, kendi ortamınızdaki veri istatistiklerinizi kullanabilirsiniz. [İzlenen VM'lerin sayısını](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-nodes-sending-data) ve [çalışma alanınızın sindirimi](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume)olan veri hacmini belirlemek için aşağıya bakın. 

## <a name="understand-your-usage-and-estimate-costs"></a>Kullanımınızı ve tahmini maliyetlerinizi anlama

Şu anda Azure Monitör Günlükleri kullanıyorsanız, maliyetlerin ne kadar olacağını anlamak son kullanım alışkanlıklarına bağlı olarak kolaydır. Bunu yapmak için, veri kullanımını gözden geçirmek ve analiz etmek için **Günlük Analizi Kullanımı ve Tahmini Maliyetler'i** kullanın. Bu, her çözüm tarafından ne kadar veri toplandığını, ne kadar veri tutulduğunu ve alınan veri miktarına ve dahil edilen miktarın ötesindeki ek saklamaya bağlı olarak maliyetlerinizin tahminedildiğini gösterir.

![Kullanım ve tahmini maliyetler](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Verilerinizi daha ayrıntılı olarak keşfetmek **için, Kullanım ve Tahmini Maliyetler** sayfasındaki grafiklerden herhangi birinin sağ üst kısmındaki simgeye tıklayın. Artık kullanımınızın daha fazla ayrıntılarını keşfetmek için bu sorguyla çalışabilirsiniz.  

![Günlükgörünümü](media/manage-cost-storage/logs.png)

Kullanım **ve Tahmini Maliyetler** sayfasından ayiçin veri hacminizi gözden geçirebilirsiniz. Buna, Log Analytics çalışma alanınızda alınan ve tutulan tüm veriler dahildir.  Kaynak, bilgisayar ve teklife göre veri hacmi eğilimleri hakkındaki bilgilerle kullanım panosunu görüntülemek için sayfanın üst kısmından **Kullanım ayrıntılarını** tıklatın. Günlük kapağı görüntülemek ve ayarlamak veya bekletme süresini değiştirmek için **Veri birim yönetimini**tıklatın.
 
Azure faturanıza Günlük Analizi ücretleri eklenir. Azure faturanızın ayrıntılarını Azure portalının Faturalandırma bölümünde veya [Azure Fatura Portalı'nda](https://account.windowsazure.com/Subscriptions)görebilirsiniz.  

## <a name="viewing-log-analytics-usage-on-your-azure-bill"></a>Azure faturanızda Günlük Analizi kullanımını görüntüleme 

Azure, [Azure Maliyet Yönetimi + Faturalandırma](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json) hub'ında çok sayıda kullanışlı işlevsellik sağlar. Örneğin, "Maliyet çözümlemesi" işlevi, Azure kaynakları için harcamalarınızı görüntülemenizi sağlar. Kaynak türüne göre bir filtre eklemek (Log Analytics için microsoft.operationalinsights/çalışma alanına) harcamalarınızı izlemenize olanak sağlar.

[Kullanımınızı Azure portalından indirerek kullanımınızı](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal)daha iyi anlayabiliriz. İndirilen elektronik tabloda, azure kaynağı başına günlük kullanımı (örn. Günlük Analitik çalışma alanı) görebilirsiniz. Bu Excel elektronik tablosunda, Log Analytics çalışma alanlarınızdan kullanım, önce "İstatistikler ve Analizler" (bazı eski fiyatlandırma katmanları tarafından kullanılır) ve "Log Analytics" göstermek için "Sayaç Kategorisi" sütununa filtre uygulayarak ve ardından "Örnek"e bir filtre ekleyerek bulunabilir. "Çalışma alanı içeren" kimlik" sütunu. Kullanım "Tüketilen Miktar" sütununda, her giriş için birim "Ölçü Birimi" sütununda gösterilir.  [Microsoft Azure faturanızı anlamanıza](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)yardımcı olacak daha fazla ayrıntı mevcuttur. 

## <a name="changing-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

Çalışma alanınızın Log Analytics fiyatlandırma katmanını değiştirmek için, 

1. Azure portalında, bu çalışma alanı için kullanılabilen fiyatlandırma katmanlarının her birinin listesini göreceğiniz çalışma alanınızdan **Kullanım'ı ve tahmini maliyetleri** açın.

2. Fiyatlandırma katmanlarının her biri için tahmini maliyetleri gözden geçirin. Bu tahmin, son 31 günlük kullanıma dayanır, bu nedenle bu maliyet tahmini, tipik kullanımınızı temsil eden son 31 güne dayanır. Aşağıdaki örnekte, son 31 güne ait veri desenlerine dayanarak, bu çalışma alanının 100 GB/gün Kapasite Rezervasyon katmanına (#2) kıyasla Nasıl Daha Az Gider Siniz katmanında (#1) nasıl daha az maliyetli olacağını görebilirsiniz.  

    ![Fiyatlandırma katmanları](media/manage-cost-storage/pricing-tier-estimated-costs.png)

3. Son 31 günlük kullanıma göre tahmini maliyetleri gözden geçirdikten sonra, fiyatlandırma katmanını değiştirmeye karar verirseniz **Seç'i**tıklatın.  

Fiyatlandırma katmanını Azure Kaynak Yöneticisi aracılığıyla `sku` (Azure`pricingTier` Kaynak Yöneticisi şablonundaki) parametreyi kullanarak [da](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) ayarlayabilirsiniz. 

## <a name="legacy-pricing-tiers"></a>Eski fiyatlandırma katmanları

2 Nisan 2018 tarihinden önce log analytics çalışma alanı veya Uygulama Öngörüleri kaynağı olan veya 1 Şubat 2019'dan önce başlayan bir Kurumsal Sözleşme'ye bağlı olan abonelikler, eski fiyatlandırma katmanlarını kullanmaya erişmeye devam edecektir: **Ücretsiz**, **Bağımsız (GB Başına)** ve **Düğüm Başına (OMS)**.  Ücretsiz fiyatlandırma katmanındaki çalışma alanları günlük veri alımı 500 MB ile sınırlı olacaktır (Azure Güvenlik Merkezi tarafından toplanan güvenlik veri türleri hariç) ve veri saklama işlemi 7 günle sınırlıdır. Ücretsiz fiyatlandırma katmanı yalnızca değerlendirme amaçlıdır. Bağımsız veya Düğüm Başına fiyatlandırma katmanlarındaki çalışma alanları, 2 yıla kadar kullanıcı tarafından yapılandırılabilir saklamaya sahiptir. 

Nisan 2016'dan önce oluşturulan çalışma alanları, sırasıyla 30 ve 365 günlük sabit veri saklamasına sahip orijinal **Standart** ve **Premium** fiyatlandırma katmanlarına da erişebilir. **Standart** veya **Premium** fiyatlandırma katmanlarında yeni çalışma alanları oluşturulamaz ve bir çalışma alanı bu katmanlardan taşınırsa, geri taşınamaz. 

Fiyatlandırma katmanı sınırlamaları hakkında daha fazla bilgi [için burada](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#log-analytics-workspaces)bulabilirsiniz.

> [!NOTE]
> Sistem Merkezi için OMS E1 Suite, OMS E2 Suite veya OMS Eklentisi satın alma nın verdiği yetkileri kullanmak için, *Düğüm Başına* Log Analytics fiyatlandırma katmanını seçin.

## <a name="change-the-data-retention-period"></a>Veri saklama süresini değiştirme

Aşağıdaki adımlar, günlük verilerinin çalışma alanınızda ne kadar süreyle tutulup tutulup tutulunu açıklanır.

### <a name="default-retention"></a>Varsayılan bekletme

Çalışma alanınız için varsayılan bekletme ayarlamak için, 
 
1. Azure portalında, çalışma alanınızdan, sol bölmeden **Kullanım'ı ve tahmini maliyetleri** seçin.
2. **Kullanım ve tahmini maliyetler** sayfasının üst kısmındaki **Veri hacmi yönetimi**'ni seçin.
3. Bölmede, gün sayısını artırmak veya azaltmak için kaydırıcıyı taşıyın ve sonra **Tamam'ı**tıklatın.  *Serbest* katmandaysanız, veri saklama süresini değiştiremeniz ve bu ayarı denetlemek için ücretli katmana yükseltmeniz gerekir.

    ![Çalışma alanı veri saklama ayarını değiştirme](media/manage-cost-storage/manage-cost-change-retention-01.png)
    
Bekletme, `retentionInDays` parametre kullanılarak Azure Kaynak Yöneticisi aracılığıyla da [ayarlanabilir.](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) Ayrıca, veri saklamayı 30 güne ayarlarsanız, parametreyi `immediatePurgeDataOn30Days` kullanarak eski verilerin hemen tasfiyesini tetikleyebilirsiniz ve bu da uyumlulukla ilgili senaryolar için yararlı olabilir. Bu işlevsellik yalnızca Azure Kaynak Yöneticisi aracılığıyla ortaya çıkarır. 

İki veri `Usage` türü `AzureActivity` -ve - varsayılan olarak 90 gün boyunca saklanır ve bu 90 günlük saklama için herhangi bir ücret alınmaz. Bu veri türleri de veri alma ücretleri ücretsizdir. 

### <a name="retention-by-data-type"></a>Veri türüne göre bekletme

Tek tek veri türleri için farklı bekletme ayarları belirtmek de mümkündür. Her veri türü çalışma alanının bir alt kaynağıdır. Örneğin SecurityEvent tablosu Azure Kaynak [Yöneticisi'nde](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şu şekilde ele alınabilir:

```
/subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent
```

Veri türünün (tablo) büyük/küçük harf duyarlı olduğunu unutmayın.  Belirli bir veri türünün (bu örneksecurityEvent'de) veri türü başına geçerli tutma ayarlarını almak için şunları kullanın:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
```

Çalışma alanınızdaki tüm veri türleri için veri türü başına geçerli tutma ayarlarını almak için, örneğin belirli veri türünü atlayın:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables?api-version=2017-04-26-preview
```

Belirli bir veri türünün (bu örnekte SecurityEvent' de) saklanması için 730 gün

```JSON
    PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
    {
        "properties": 
        {
            "retentionInDays": 730
        }
    }
```

`Usage` Ve `AzureActivity` veri türleri özel bekletme ile ayarlanamaz. Bunlar varsayılan çalışma alanı bekletme veya 90 gün maksimum alır. 

Veri türüne göre saklama yı ayarlamak için doğrudan Azure Kaynak Yöneticisi'ne bağlanmak için harika bir araç OSS aracı [ARMclient'dır.](https://github.com/projectkudu/ARMClient)  [David Ebbo](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ve [Daniel Bowbyes'in](https://blog.bowbyes.co.nz/2016/11/02/using-armclient-to-directly-access-azure-arm-rest-apis-and-list-arm-policy-details/)makalelerinden ARMclient hakkında daha fazla bilgi edinin.  ArmClient kullanarak SecurityEvent verilerini 730 günlük bir saklama ya da 730 günlük saklama ya da ayarlama ya da bir örnek aşağıda verilmiştir:

```
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview "{properties: {retentionInDays: 730}}"
```

> [!NOTE]
> Tek tek veri türlerinde bekletme ayarlama, veri saklama maliyetlerinizi azaltmak için kullanılabilir.  Ekim 2019'dan itibaren toplanan veriler için (bu özellik yayımlandığında), bazı veri türleri için bekletme azaltılabilir, zaman içinde saklama maliyetinizi düşürebilir.  Daha önce toplanan veriler için, tek bir tür için daha düşük bir bekletme ayarlamak bekletme maliyetlerinizi etkilemez.  

## <a name="manage-your-maximum-daily-data-volume"></a>Maksimum günlük veri hacminizi yönetme

Günlük bir kap yapılandırabilir ve çalışma alanınız için günlük alımını sınırlandırabilirsiniz, ancak amacınız günlük limite ulaşmak olmadığı için dikkati kullanın.  Aksi takdirde, günün geri kalanında veri kaybedersiniz ve bu da işlevselliği çalışma alanında mevcut olan güncel verilere bağlı olabilecek diğer Azure hizmetlerini ve çözümlerini etkileyebilir.  Sonuç olarak, BT hizmetlerini destekleyen kaynakların sağlık koşulları etkilendiğinde uyarıları gözlemleme ve alma yeteneğiniz.  Günlük kap, yönetilen kaynaklarınızdan veri hacmindeki beklenmeyen artışı yönetmek ve sınırınız dahilinde kalmak veya çalışma alanınız için planlanmamış ücretleri sınırlamak istediğinizde kullanılmak üzere tasarlanmıştır.  

Günlük sınıra ulaşıldığında, faturalandırılabilir veri türlerinin toplanması günün geri kalanında durur. Seçili Log Analytics çalışma alanı için sayfanın üst kısmında bir uyarı başlığı görünür ve **LogManagement** kategorisi altında *İşlem* tablosuna bir işlem olayı gönderilir. Günlük limit altında tanımlanan sıfırlama süresi *'nde ayarlandıktan*sonra veri toplama devam eder. Günlük veri sınırına ulaşıldığında bildirmek üzere yapılandırılan bu işlem olayına dayalı bir uyarı kuralı tanımlamanızı öneririz. 

> [!NOTE]
> Günlük kap, Azure Güvenlik Merkezi'nin 19 Haziran 2017 tarihinden önce yüklendiği çalışma alanları dışında Azure Güvenlik Merkezi'nden veri toplanmasını durdurmaz. 

> [!NOTE]
> Günlük kapağın uygulanmasında nanca, kapağın tam olarak belirtilen günlük kap seviyesi olarak uygulanmadığı anlamına gelebilir. 

### <a name="identify-what-daily-data-limit-to-define"></a>Tanımlayacak günlük veri limitini belirleme

Veri alma eğilimini ve tanımlanması gereken günlük birim kapağının ne olduğunu anlamak için [Log Analytics Kullanımını ve tahmini maliyetleri](usage-estimated-costs.md) gözden geçirin. Sınıra ulaşıldıktan sonra kaynaklarınızı izleyemeyeceksiniz diye dikkatli bir şekilde düşünülmelidir. 

### <a name="set-the-daily-cap"></a>Günlük Kapağı Nı Ayarla

Aşağıdaki adımlar, Log Analytics çalışma alanının günde alacağı veri hacmini yönetmek için bir sınırı nasıl yapılandıracağını açıklar.  

1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. Seçili çalışma alanının **Kullanım ve tahmini maliyetler** sayfasında, sayfanın üst kısmından Veri hacmi **yönetimini** tıklatın. 
3. Günlük kap **varsayılan** olarak etkinleştirmek için **ATIK'a** tıklayın ve ardından veri hacmi sınırını GB/gün olarak ayarlayın.

    ![Günlük Analytics veri limitini yapılandırmak](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Günlük Cap'e ulaştığında uyarı

Veri sınırı eşiğiniz karşılandığında Azure portalında görsel bir ipucu sunsak da, bu davranış hemen dikkat gerektiren operasyonel sorunları yönetme biçiminize uygun değildir.  Uyarı bildirimi almak için Azure Monitor'da yeni bir uyarı kuralı oluşturabilirsiniz.  Daha fazla bilgi edinmek [için uyarıları nasıl oluşturup görüntüleyeceklerini ve yöneteceklerini](alerts-metric.md)öğrenin.

Başlamak için uyarı için önerilen ayarlar şunlardır:

- Hedef: Günlük Analizi kaynağınızı seçin
- Ölçüt: 
   - Sinyal adı: Özel günlük arama
   - Arama sorgusu: İşlem | burada Detay 'OverQuota' var
   - Based on: Sonuç sayısı
   - Koşul: Daha büyük
   - Eşik: 0
   - Süre: 5 (dakika)
   - Frekans: 5 (dakika)
- Uyarı kuralı adı: Günlük veri sınırına ulaşıldı
- Önem: Uyarı (Sev 1)

Uyarı tanımlandıktan ve sınıra ulaşıldıktan sonra, bir uyarı tetiklenir ve Eylem Grubu'nda tanımlanan yanıtı gerçekleştirir. E-posta ve kısa mesajlar yoluyla ekibinizi bilgilendirebilir veya webhooks, Otomasyon runbook'ları kullanarak veya [harici bir ITSM çözümüyle tümleştirme](itsmc-overview.md#create-itsm-work-items-from-azure-alerts)kullanarak eylemleri otomatikleştirebilir. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Kullanımın neden beklenenden daha yüksek olduğuyla ilgili sorunları giderme

Yüksek kullanımın nedeni aşağıdakilerden biri veya her ikisidir:
- Log Analytics çalışma alanına veri gönderme beklenenden daha fazla düğüm
- Log Analytics çalışma alanına beklenenden daha fazla veri gönderilmesi (belki de yeni bir çözüm kullanmaya başlama veya varolan bir çözümde yapılandırma değişikliği nedeniyle)

## <a name="understanding-nodes-sending-data"></a>Veri gönderen düğümleri anlama

Son bir ay içinde her gün aracıdan gelen kalp atışlarını bildiren düğüm sayısını anlamak için,

```kusto
Heartbeat 
| where TimeGenerated > startofday(ago(31d))
| summarize nodes = dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```
Son 24 saat içinde veri gönderen düğüm sayısı alın sorguyu kullanın: 

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodes = dcount(computerName)
```

Herhangi bir veri gönderen düğümlerin listesini (ve her biri tarafından gönderilen veri miktarı) almak için takip sorgusu kullanılabilir:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

> [!NOTE]
> Veri `union withsource = tt *` türleri arasında taramalar yürütmek için pahalı olduğundan bu sorguları tutumlu kullanın. Bu sorgu, bilgisayar başına eski bilgileri Kullanım veri türüyle sorgulama yolunun yerini alır.  

## <a name="understanding-ingested-data-volume"></a>Yutulan veri hacmini anlama

Kullanım **ve Tahmini Maliyetler** sayfasında, çözüm grafiği *başına veri alımı* gönderilen toplam veri hacmini ve her çözüm tarafından ne kadar gönderildiğini gösterir. Bu, genel veri kullanımının (veya belirli bir çözüm tarafından kullanımın) büyüyüp büyümediği, sabit kalıp azalmadığı gibi eğilimleri belirlemenize olanak tanır. 

### <a name="data-volume-for-specific-events"></a>Belirli olaylar için veri hacmi

Belirli bir olay kümesi için yutulan verilerin boyutuna bakmak için, belirli bir `Event`tabloyu sorgulayabilir (bu örnekte) ve ardından sorguyu ilgi olaylarıyla (bu örnekolay kimliği 5145 veya 5156) kısıtlayabilirsiniz:

```kusto
Event
| where TimeGenerated > startofday(ago(31d)) and TimeGenerated < startofday(now()) 
| where EventID == 5145 or EventID == 5156
| where _IsBillable == true
| summarize count(), Bytes=sum(_BilledSize) by EventID, bin(TimeGenerated, 1d)
``` 

Yan tümcenin, `where IsBillable = true` yutma ücreti olmayan belirli çözümlerden veri türlerini filtrelediğini unutmayın. 

### <a name="data-volume-by-solution"></a>Çözüme göre veri hacmi

Son bir ay içinde (son kısmi gün hariç) çözüme göre faturalandırılabilir veri hacmini görüntülemek için kullanılan sorgu:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution | render barchart
```

Yan tümce `TimeGenerated` yalnızca Azure portalındaki sorgu deneyiminin varsayılan 24 saatin ötesine bakmasını sağlamak içindir. Kullanım veri türünü `StartTime` kullanırken `EndTime` ve sonuçların sunulduğu zaman kovalarını temsil edin. 

### <a name="data-volume-by-type"></a>Türüne göre veri hacmi

Veri türüne göre veri eğilimlerini görmek için daha fazla detaya inebilirsiniz:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType | render barchart
```

Ya da çözüme göre bir tablo görmek ve son bir ay için yazın,

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) by Solution, DataType
| sort by Solution asc, DataType asc
```

### <a name="data-volume-by-computer"></a>Bilgisayara göre veri hacmi

Veri `Usage` türü tamlayıcı düzeyinde bilgi içermez. Bilgisayar başına yutulan verilerin **boyutunu** görmek `_BilledSize` için, bayt boyutunu sağlayan [özelliği](log-standard-properties.md#_billedsize)kullanın:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize BillableDataBytes = sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

`_IsBillable` [Özellik,](log-standard-properties.md#_isbillable) yutulan verilerin ücrete tabi olup olmadığını belirtir.

Bilgisayar başına faturalandırılabilir olayların **sayısını** görmek için, 

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount = count() by computerName  | sort by eventCount nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Azure kaynağına, kaynak grubuna veya aboneye göre veri hacmi

Azure'da barındırılan düğümlerden gelen veriler için bilgisayar __başına__yutulan verilerin **boyutunu** alabilirsiniz, kaynağa tam yol sağlayan _ResourceId [özelliğini](log-standard-properties.md#_resourceid)kullanabilirsiniz:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Azure'da barındırılan düğümlerden gelen veriler __için, Azure aboneliği başına__yutulan `_ResourceId` verilerin **boyutunu** alabilir ve özelliği aşağıdaki gibi ayrışdırabilirsiniz:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize BillableDataBytes = sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

Değiştirerek, `subscriptionId` `resourceGroup` Azure kaynak grubuna göre faturalandırılabilir veri hacmini gösterir. 

> [!NOTE]
> Kullanım veri türünün bazı alanları, hala şemadayken, küçümsenmiş ve değerleri artık doldurulmayacak. Bu **Bilgisayar** yanı sıra yutma ile ilgili alanlar **(TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** ve **AverageProcessingTimeMs**.

### <a name="querying-for-common-data-types"></a>Ortak veri türleri için sorgulama

Belirli bir veri türü için veri kaynağını daha derinlemesine araştırmak için, bazı yararlı örnek sorgular aşağıda verilmiştir:

+ **Güvenlik** çözümü
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **Günlük Yönetimi** çözümü
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ **Perf** veri türü
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ **Event** veri türü
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ **Syslog** veri türü
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ **AzureDiagnostics** veri türü
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>Veri hacmini azaltmak için ipuçları

Toplanan günlüklerin hacmini azaltmak için bazı öneriler şunlardır:

| Yüksek veri hacminin kaynağı | Veri hacmi nasıl azaltılır |
| -------------------------- | ------------------------- |
| Güvenlik olayları            | [Yaygın veya en az güvenlik olaylarını](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier) seçin <br> Güvenlik denetimi ilkesini yalnızca gerekli olayları toplayacak şekilde değiştirin. Özellikle, şunlarla ilgili olayları toplamak gerekip gerekmediğini gözden geçirin: <br> - [filtre platformu denetimi](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [kayıt defteri denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [dosya sistemi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [çekirdek nesnesi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [tanıtıcı değiştirme denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - çıkarılabilir depolama birimi denetimi |
| Performans sayaçları       | [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) şöyle değiştirin: <br> - Koleksiyonun sıklığını azaltın <br> - Performans sayaçlarının sayısını azaltın |
| Olay günlükleri                 | [Olay günlüğü yapılandırmasını](data-sources-windows-events.md) şöyle değiştirin: <br> - Toplanan olay günlüklerinin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneğin, *Bilgi* düzeyindeki olayları toplamayın |
| Syslog                     | [Syslog yapılandırmasını](data-sources-syslog.md) şu şekilde değiştirin: <br> - Toplanan tesislerin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneği *Bilgi* ve *Hata Ayıklama* düzeyindeki olayları toplamayın |
| AzureDiagnostics           | Aşağıdaki amaçlarla kaynak günlüğü koleksiyonunu değiştirin: <br> - Log Analytics’e günlük gönderen kaynak sayısını azaltma <br> - Yalnızca gerekli günlükleri toplama |
| Çözüm ihtiyacı olmayan bilgisayarlardan toplanan çözüm verileri | Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedeflemesini](../insights/solution-targeting.md) kullanın. |

### <a name="getting-nodes-as-billed-in-the-per-node-pricing-tier"></a>Düğüm başına fiyatlandırma katmanında faturalandırılan düğümleri alma

Çalışma alanı eski Düğüm başına fiyatlandırma katmanındaysa düğüm olarak faturalandırılacak bilgisayarların listesini almak **için, faturalı veri türleri** gönderen düğümleri arayın (bazı veri türleri ücretsizdir). Bunu yapmak için `_IsBillable` [özelliği](log-standard-properties.md#_isbillable) kullanın ve tam nitelikli alan adının en sol alanını kullanın. Bu, saat başına faturalı veriye sahip bilgisayarların sayısını döndürür (düğümlerin sayılayıp faturalandırıldıği parçalılıktır):

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

### <a name="getting-security-and-automation-node-counts"></a>Güvenlik ve Otomasyon düğüm sayılarını alma

"Düğüm Başına (OMS)" fiyatlandırma katmanındaysanız, kullandığınız düğüm ve çözüm sayısına bağlı olarak ücretlendirilirsiniz, faturalandırıldığın Öngörü ve Analiz düğümlerinin sayısı Kullanım ve **Tahmini Maliyet** sayfasındaki tabloda gösterilir.  

Farklı Güvenlik düğümlerinin sayısını görmek için sorguyu kullanabilirsiniz:

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

Farklı Otomasyon düğümlerinin sayısını görmek için sorguyu kullanın:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="create-an-alert-when-data-collection-is-high"></a>Veri toplama yüksek olduğunda bir uyarı oluşturma

Bu bölümde, aşağıdaki durumlarda nasıl uyarı oluşturulacağı açıklanır:
- Veri hacmi belirtilen bir miktarı aştığında.
- Veri hacminin belirtilen bir miktarı aşacağı tahmin edildiğinde.

Azure Uyarıları, arama sorguları kullanan [günlük uyarılarını](alerts-unified-log.md) destekler. 

Aşağıdaki sorgu, son 24 saatte 100 GB'den fazla veri toplandığında bir sonuç verir:

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1000.)) by Type 
| where DataGB > 100
```

Aşağıdaki sorgu, ne zaman bir günde 100 GB'den fazla veri toplanacağını tahmin etmek için basit bir formül kullanır: 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1000.)) by Type 
| where EstimatedGB > 100
```

Farklı bir veri hacminde uyarıda bulunmak için, sorgulardaki 100 değerini uyarılmak istediğiniz GB sayısıyla değiştirin.

Toplanan veri beklenen miktarı aştığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](alerts-metric.md) başlığı altında açıklanan adımları kullanın.

İlk sorgu için, yani 24 saat içinde 100 GB'den fazla veri toplandığında uyarı oluştururken şu ayarları yapın:  

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal Adı** bölümünde **Özel günlük araması**'nı seçin
   - **Arama sorgusu**: `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1000.)) by Type | where DataGB > 100`
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - Kullanım verileri saatte bir güncelleştirildiğinden **Süre***1440* dakika, **Uyarı sıklığı** ise *60* dakikada bir olarak belirlenmiştir.
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - **Ad**: *24 saat içinde 100 GB'den büyük veri hacmi*
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](action-groups.md) kullanın veya yeni bir tane oluşturun.

İkinci sorgu için, yani 24 saat içinde 100 GB'den fazla veri olacağı tahmin edildiğinde uyarı oluştururken şu ayarları yapın:

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal Adı** bölümünde **Özel günlük araması**'nı seçin
   - **Arama sorgusu**: `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1000.)) by Type | where EstimatedGB > 100`
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - Kullanım verileri saatte bir güncelleştirildiğinden **Süre***180* dakika, **Uyarı sıklığı** ise *60* dakikada bir olarak belirlenmiştir.
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - **Ad**: *24 saat içinde veri hacminin 100 GB'den büyük olacağı tahmin ediliyor*
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](action-groups.md) kullanın veya yeni bir tane oluşturun.

Uyarı aldığınızda, kullanımın neden beklenenden fazla olduğu konusundaki sorunları gidermek için aşağıdaki bölümde yer alan adımları kullanın.

## <a name="data-transfer-charges-using-log-analytics"></a>Log Analytics'i kullanarak veri aktarım ücretleri

Log Analytics'e veri göndermek veri bant genişliği ücretlerine neden olabilir. Azure Bant [Genişliği fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/bandwidth/)açıklandığı gibi, iki bölgede bulunan Azure hizmetleri arasındaki veri aktarımı normal hızda giden veri aktarımı olarak ücretlendirilir. Gelen veri aktarımı ücretsizdir. Ancak, bu ücret çok küçük (% birkaç) Log Analytics veri alımı maliyetleriile karşılaştırıldığında. Sonuç olarak Log Analytics'in maliyetlerini kontrol etmek için sindirilen veri hacminize odaklanması gerekir ve [burada](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume)bunu anlamamıza yardımcı olacak bir kılavuzumuz vardır.   


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Günlük Analizi'nin neden artık veri topladığını sorun giderme

Eski Ücretsiz fiyatlandırma katmanındaysanız ve bir günde 500 MB'dan fazla veri gönderdiyseniz, veri toplama günün geri kalanında durur. Günlük sınıra ulaşmak, Log Analytics'in veri toplamayı durdurmasının veya verilerin eksik görünmesinin yaygın bir nedenidir.  Log Analytics, veri toplama başlatıldığında ve durduğunda bir tür İşlem olayı oluşturur. Günlük sınıra ulaşıp ulaşmadığınızı ve verileri eksik leyip aramadığınızı kontrol etmek için aşağıdaki sorguyu aramada çalıştırın: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Veri toplama durduğunda, OperationStatus **Uyarıdır.** Veri toplama başladığında, İşlem Durumu **Başarılı**oldu. Aşağıdaki tabloda, veri toplamanın durma nedenleri ve veri toplamaya devam etmek için önerilen bir eylem açıklanmaktadır:  

|Neden toplama durur| Çözüm| 
|-----------------------|---------|
|Eski Ücretsiz fiyatlandırma katmanının günlük sınırı ulaşıldı |Koleksiyonun otomatik olarak yeniden başlatılması veya ücretli fiyatlandırma katmanına değiştirilmesi için ertesi güne kadar bekleyin.|
|Çalışma alanınızın günlük kapağına ulaşıldı|Koleksiyonun otomatik olarak yeniden başlatılmasını veya maksimum günlük veri hacmini yönetmede açıklanan günlük veri hacmi sınırını artırmasını bekleyin. Günlük kap sıfırlama süresi Veri **birimi yönetimi** sayfasında gösterir. |
|Azure aboneliği şu nedenden dolayı askıya alınmış durumdadır:<br> Ücretsiz deneme sona erdi<br> Azure geçişinin süresi doldu<br> Aylık harcama sınırına ulaşıldı (örneğin MSDN veya Visual Studio aboneliğinde)|Ücretli aboneliğe dönüştürme<br> Sınırı kaldırın veya limit sıfırlayana kadar bekleyin|

Veri toplama durduğunda haberdar olmak için, veri toplama durduğunda haberdar olmak için *günlük veri kapağı* oluştur uyarısında açıklanan adımları kullanın. Uyarı kuralı için bir e-posta, webhook veya runbook eylem yapılandırmak için [bir eylem grubu oluşturma'da](action-groups.md) açıklanan adımları kullanın. 

## <a name="limits-summary"></a>Limitler özeti

Bazı ek Log Analytics sınırları vardır ve bunların bazıları Log Analytics fiyatlandırma katmanına bağlıdır. Bunlar [burada](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#log-analytics-workspaces)belgelenmiştir.


## <a name="next-steps"></a>Sonraki adımlar

- Arama dilini nasıl kullanacağınızı öğrenmek için [Azure Monitor Günlükleri'ndeki Günlük aramalarına](../log-query/log-query-overview.md) bakın. Kullanım verilerinde başka analizler yapmak için arama sorgularını kullanabilirsiniz.
- Bir arama ölçütü karşılandığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](alerts-metric.md) başlığı altında açıklanan adımları kullanın.
- Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedeflemesini](../insights/solution-targeting.md) kullanın.
- Etkili bir olay toplama ilkesini yapılandırmak için [Azure Güvenlik Merkezi filtreleme ilkesini](../../security-center/security-center-enable-data-collection.md)gözden geçirin.
- [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) değiştirin.
- Olay toplama ayarlarınızda değişiklik yapmak için, [olay günlüğü yapılandırması](data-sources-windows-events.md) konusunu gözden geçirin.
- Syslog koleksiyonu ayarlarınızda değişiklik yapmak için, [syslog yapılandırması](data-sources-syslog.md) konusunu gözden geçirin.