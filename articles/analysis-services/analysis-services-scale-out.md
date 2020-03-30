---
title: Azure Analiz Hizmetleri ölçeklendirme| Microsoft Dokümanlar
description: Azure Analiz Hizmetleri sunucularını ölçeklendirme ile çoğaltın. İstemci sorguları daha sonra ölçeklendirme sorgusuhavuzunda birden çok sorgu yinelemeleri arasında dağıtılabilir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/02/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3ea304d038618fc428f20e7ad72b398f593d09a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78247983"
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services ölçeğini genişletme

Ölçeklendirme ile istemci sorguları, yüksek sorgu iş yükleri sırasında yanıt sürelerini azaltarak, sorgu *havuzundaki*birden çok *sorgu yinelemesi* arasında dağıtılabilir. Ayrıca, istemci sorgularının işlem işlem işlemlerinden olumsuz etkilenmemesini sağlayarak işlemi sorgu havuzundan ayırabilirsiniz. Ölçeklendirme, Azure portalında veya Analiz Hizmetleri REST API kullanılarak yapılandırılabilir.

Ölçeklendirme, Standart fiyatlandırma katmanındaki sunucular için kullanılabilir. Her sorgu yinelemesi sunucunuzla aynı oranda faturalandırılır. Tüm sorgu yinelemeleri sunucunuzla aynı bölgede oluşturulur. Yapılandırabileceğiniz sorgu yinelemelerinin sayısı, sunucunuzun bulunduğu bölgeyle sınırlıdır. Daha fazla bilgi edinmek için bölgeye [göre Kullanılabilirlik'e](analysis-services-overview.md#availability-by-region)bakın. Ölçeklendirme, sunucunuz için kullanılabilir bellek miktarını artırmaz. Belleği artırmak için planınızı yükseltmeniz gerekir. 

## <a name="why-scale-out"></a>Neden ölçeklendir?

Tipik bir sunucu dağıtımında, bir sunucu hem işleme sunucusu hem de sorgu sunucusu olarak hizmet verir. Sunucunuzdaki modellere karşı istemci sorgularının sayısı, sunucunuzun planı için Sorgu İşlem Birimleri'ni (QPU) aşıyorsa veya model işleme yüksek sorgu iş yükleri ile aynı anda gerçekleşirse, performans düşebilir. 

Ölçeklendirme ile, en fazla yedi ek sorgu çoğaltma kaynağı *(birincil* sunucunuz da dahil olmak üzere sekiz toplam) içeren bir sorgu havuzu oluşturabilirsiniz. Sorgu havuzundaki yineleme sayısını kritik zamanlarda QPU taleplerini karşılamak üzere ölçeklendirebilir ve istediğiniz zaman bir işlem sunucusunu sorgu havuzundan ayırabilirsiniz. 

Sorgu havuzunda bulunan sorgu yinelemelerinin sayısıne bakılmaksızın, iş yüklerini işleme işlemi sorgu yinelemeleri arasında dağıtılmaz. Birincil sunucu işleme sunucusu olarak hizmet vermektedir. Sorgu yinelemeleri yalnızca birincil sunucu ile sorgu havuzundaki her yineleme arasında eşitlenen model veritabanlarına karşı sorgular kullanır. 

Ölçekleme yaparken, yeni sorgu yinelemelerinin sorgu havuzuna artımlı olarak eklenmesi beş dakika kadar sürebilir. Tüm yeni sorgu yinelemeleri çalışır durumdayken, yeni istemci bağlantıları sorgu havuzundaki kaynaklar arasında yük dengelenir. Varolan istemci bağlantıları, şu anda bağlı oldukları kaynaktan değiştirilmez. Ölçeklenirken, sorgu havuzundan kaldırılan bir sorgu havuzu kaynağına varolan istemci bağlantıları sonlandırılır. İstemciler kalan sorgu havuzu kaynağına yeniden bağlanabilir.

## <a name="how-it-works"></a>Nasıl çalışır?

İlk kez ölçeklendirme yapılandırırken, birincil sunucunuzdaki model veritabanları yeni bir sorgu havuzunda yeni yinelemelerle *otomatik olarak* eşitlenir. Otomatik eşitleme yalnızca bir kez gerçekleşir. Otomatik eşitleme sırasında, birincil sunucunun veri dosyaları (blob depolama da dinlenme şifreli) ikinci bir konuma kopyalanır, ayrıca blob depolama dinlenme şifreli. Sorgu havuzundaki yinelemeler daha sonra ikinci dosya kümesinden gelen verilerle *nemlendirilir.* 

Otomatik eşitleme yalnızca bir sunucuyu ilk kez ölçeklendirdiğinizde gerçekleştirilirken, el ile eşitleme de gerçekleştirebilirsiniz. Eşitleme, sorgu havuzundaki yinelemelerle ilgili verileri birincil sunucuyla eşleştirmeyi sağlar. Birincil sunucuda modelleri işlerken (yenileme) yaparken, işleme işlemleri *tamamlandıktan sonra* bir eşitleme gerçekleştirilmelidir. Bu eşitleme, birincil sunucunun blob depolamadaki dosyalarından ikinci dosya kümesine güncelleştirilen verileri kopyalar. Sorgu havuzundaki yinelemeler daha sonra blob depolamadaki ikinci dosya kümesinden güncelleştirilmiş verilerle nemlendirilir. 

Sonraki ölçeklendirme işlemi gerçekleştirirken, örneğin, sorgu havuzundaki yineleme sayısını ikiden beşe çıkarırken, yeni yinelemeler blob depolamadaki ikinci dosya kümesinden gelen verilerle nemlendirilir. Eşitleme yok. Daha sonra ölçeklendirildikten sonra bir eşitleme yaparsanız, sorgu havuzundaki yeni yinelemeler iki kez susuz kalır - gereksiz bir hidrasyon. Sonraki bir ölçeklendirme işlemi gerçekleştirirken, aklınızda bulundurmak önemlidir:

* Eklenen yinelemelerin gereksiz hidrasyonunu önlemek için *ölçeklendirme işleminden önce* bir eşitleme gerçekleştirin. Aynı anda çalışan eşzamanlı eşitleme ve ölçeklendirme işlemlerine izin verilmez.

* Hem işleme *hem* de ölçeklendirme işlemlerini otomatikleştirirken, önce birincil sunucudaki verileri işlemek, sonra bir eşitleme gerçekleştirmek ve ardından ölçeklendirme işlemini gerçekleştirmek önemlidir. Bu dizi, QPU ve bellek kaynakları üzerinde en az etkiyi sağlar.

* Sorgu havuzunda yineleme olmasa bile eşitleme ye izin verilir. Birincil sunucudaki bir işleme işleminden gelen yeni verilerle sıfırdan bir veya daha fazla yinelemeye ölçekleniyorsanız, önce sorgu havuzunda yineleme olmadan eşitleme işlemini gerçekleştirin ve ardından ölçeklendirin. Ölçeklendirmeden önce senkronize etmek, yeni eklenen yinelemelerin gereksiz hidrasyonlarını önler.

* Bir model veritabanını birincil sunucudan silerken, sorgu havuzundaki yinelemelerden otomatik olarak silinmez. Bu veritabanıiçin dosya/s'leri yinelemenin paylaşılan blob depolama konumundan kaldıran ve sorgu havuzundaki yinelemeler üzerindeki model veritabanını silen [Eşitleme-AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance) PowerShell komutunu kullanarak bir eşitleme işlemi gerçekleştirmeniz gerekir. Bir model veritabanı sorgu havuzunda yinelemeler üzerinde var ama birincil sunucuda değil belirlemek için, sorgu havuzu ayarı **gelen işlem sunucusu ayrı** olduğundan emin **evet**. Ardından, veritabanının var olup olmadığını `:rw` görmek için elemeyi kullanarak birincil sunucuya bağlanmak için SSMS'i kullanın. Ardından, aynı veritabanının da var olup `:rw` olmadığını görmek için niteleyici olmadan bağlanarak sorgu havuzundaki yinelemelere bağlanın. Veritabanı sorgu havuzunda yinelemeler üzerinde varsa, ancak birincil sunucuda yoksa, bir eşitleme işlemi çalıştırın.   

* Birincil sunucuda bir veritabanını yeniden adlandırırken, veritabanının tüm yinelemelere düzgün şekilde eşitlendiğinden emin olmak için ek bir adım gerekir. Yeniden adlandırmadan sonra, eski veritabanı adı ile `-Database` parametreyi belirten [Eşitleme-AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance) komutunu kullanarak bir eşitleme gerçekleştirin. Bu eşitleme, veritabanını ve eski ada sahip dosyaları tüm yinelemelerden kaldırır. Ardından, parametreyi `-Database` yeni veritabanı adı ile belirten başka bir eşitleme gerçekleştirin. İkinci eşitleme, yeni adlandırılmış veritabanını ikinci dosya kümesine kopyalar ve yinelemeleri nemlendirir. Bu eşitlemeler portaldaki Eşitleme modeli komutu kullanılarak gerçekleştirilebilir.

### <a name="synchronization-mode"></a>Eşitleme modu

Varsayılan olarak, sorgu yinelemeleri artımlı olarak değil, tam olarak rehydrated vardır. Rehidrasyon aşamalarında olur. Herhangi bir anda sorgular için en az bir yinelemenin çevrimiçi tutulmasını sağlamak için bir seferde iki kez ayrılır ve eklenirler (en az üç yineleme olduğunu varsayarsak). Bazı durumlarda, bu işlem gerçekleşirken istemcilerin çevrimiçi yinelemelerden birine yeniden bağlanması gerekebilir. (Önizleme) **ReplicaSyncMode** ayarını kullanarak, şimdi sorgu çoğaltma eşitleme paralel oluşur belirtebilirsiniz. Paralel eşitleme aşağıdaki yararları sağlar: 

- Senkronizasyon süresinde önemli azalma. 
- Eşitleme işlemi sırasında yinelemeler arasında veri tutarlı olma olasılığı daha yüksektir. 
- Veritabanları eşitleme işlemi boyunca tüm yinelemeler üzerinde çevrimiçi tutulduğundan, istemcilerin yeniden bağlanması gerekmez. 
- Bellek önbelleği, yalnızca değiştirilen verilerle artımlı olarak güncelleştirilir ve bu da modeli tamamen yeniden sulandırmaktan daha hızlı olabilir. 

#### <a name="setting-replicasyncmode"></a>ReplicaSyncMode'u Ayarlama

Gelişmiş Özellikler'de ReplicaSyncMode'u ayarlamak için SSMS'i kullanın. Olası değerler şunlardır: 

- `1`(varsayılan): Aşamalarında tam çoğaltma veritabanı rehidrasyon (artımlı). 
- `2`: Paralel olarak optimize edilmiş senkronizasyon. 

![RelicaSyncMode ayarı](media/analysis-services-scale-out/aas-scale-out-sync-mode.png)

**ReplicaSyncMode=2'yi**ayarlarken, önbelleğin ne kadarının güncelleştirilmesi gerektiğine bağlı olarak, sorgu yinelemeleri tarafından ek bellek tüketilebilir. Veritabanını çevrimiçi tutmak ve sorgular için kullanılabilir durumda tutmak için, verilerin ne kadarının değiştiğine bağlı olarak, hem eski hem de yeni kesimler aynı anda bellekte tutulduğundan, işlem yinelemedeki *belleği iki katına* çıkarmayı gerektirebilir. Yineleme düğümleri birincil düğümle aynı bellek ayırmaya sahiptir ve yenileme işlemleri için birincil düğümde normalde fazladan bellek vardır, bu nedenle yinelemelerin bellekten akması olası olmayabilir. Ayrıca, ortak senaryo veritabanı nın birincil düğümüzerinde artımlı olarak güncelleştirilmesi ve bu nedenle iki kat bellek gereksiniminin nadir olması gerektiğidir. Eşitleme işlemi bellek dışı bir hatayla karşılaşırsa, varsayılan tekniği kullanmayı yeniden dener (bir seferde iki ekleme/ayırma). 

### <a name="separate-processing-from-query-pool"></a>Sorgu havuzundan ayrı işleme

Hem işleme hem de sorgu işlemleri için maksimum performans için, işlem sunucunuzu sorgu havuzundan ayırmayı seçebilirsiniz. Ayrıldığında, yalnızca sorgu havuzundaki sorgu yinelemelerine yeni istemci bağlantıları atanır. İşleme işlemleri yalnızca kısa bir süre alıyorsa, işlem sunucunuzu yalnızca işlem ve eşitleme işlemlerini gerçekleştirmek için gereken süre için sorgu havuzundan ayırmayı ve ardından sorgu havuzuna geri eklemeyi seçebilirsiniz. İşlem sunucusunu sorgu havuzundan ayırırken veya sorgu havuzuna geri eklerken işlemin tamamlanması beş dakika kadar sürebilir.

## <a name="monitor-qpu-usage"></a>QPU kullanımını izleyin

Sunucunuz için ölçeklendirmenin gerekli olup olmadığını belirlemek için, Azure portalında Metrikler'i kullanarak [sunucunuzu izleyin.](analysis-services-monitor.md) QPU'nuz düzenli olarak maksimuma çıkarsa, modellerinize karşı sorgu sayısının planınız için QPU sınırını aştığı anlamına gelir. Sorgu havuzu iş sırası uzunluğu ölçümü, sorgu iş parçacığı havuzu kuyruğundaki sorgu sayısı kullanılabilir QPU'yu aştığında da artar. 

Izlemek için başka bir iyi metrik ServerResourceType tarafından ortalama QPU olduğunu. Bu metrik, birincil sunucunun ortalama QPU'su ile sorgu havuzunu karşılaştırır. 

![Sorgu ölçümleri ölçeklendir](media/analysis-services-scale-out/aas-scale-out-monitor.png)

**QPU'yı ServerResourceType'a göre yapılandırmak için**

1. Ölçümler satır grafiğinde **metrik ekle'yi**tıklatın. 
2. **KAYNAK'ta**sunucunuzu seçin, ardından **METRIC NAMESPACE'de** **Analiz Hizmetleri standart ölçümlerini**seçin, sonra **METRIC'de,** **QPU'da**ve ardından **AGREGASYON'da** **Avg'yi**seçin. 
3. **Splitting Uygula'yı**tıklatın. 
4. **VALUES'de** **ServerResourceType'ı**seçin.  

### <a name="detailed-diagnostic-logging"></a>Ayrıntılı tanı günlüğü

Ölçeklenmiş sunucu kaynaklarının daha ayrıntılı tanılamaları için Azure Monitör Günlükleri'ni kullanın. Günlüklerle, QPU ve bellek sunucu ve çoğaltma tarafından patlak vermek için Log Analytics sorguları kullanabilirsiniz. Daha fazla bilgi edinmek [için, Çözümleme Hizmetleri tanılama günlüğe kayden](analysis-services-logging.md#example-queries)örnek sorgulara bakın.


## <a name="configure-scale-out"></a>Ölçek genişletmeyi yapılandırma

### <a name="in-azure-portal"></a>Azure portalında

1. Portalda, **Ölçekle'yi**tıklatın. Sorgu çoğaltma sunucularının sayısını seçmek için kaydırıcıyı kullanın. Seçtiğiniz yineleme sayısı, varolan sunucunuza ek olarak verilir.  

2. **İşlem eki sunucuyu sorgu havuzundan ayırın,** işlem sunucunuzu sorgu sunucularından dışlamak için evet'i seçin. Varsayılan [connections](#connections) bağlantı dizesini (olmadan) `:rw`kullanan istemci bağlantıları sorgu havuzundaki yinelemelere yönlendirilir. 

   ![Ölçeklendir-çıkış kaydırıcısı](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Yeni sorgu çoğaltma sunucularınızı sağlamak için **Kaydet'i** tıklatın. 

İlk kez bir sunucu için ölçeklendirme yapılandırırken, birincil sunucunuzdaki modeller sorgu havuzundaki yinelemelerle otomatik olarak eşitlenir. Otomatik eşitleme, ölçeklendirmeyi ilk kez bir veya daha fazla yinelemeye yapılandırdığınızda yalnızca bir kez oluşur. Aynı sunucudaki yineleme sayısında sonraki değişiklikler *başka bir otomatik eşitleme tetiklemez.* Sunucuyu sıfır yinelemeye ayarlasanız ve sonra tekrar herhangi bir çoğaltma sayısına ölçeklendirseniz bile otomatik eşitleme tekrar oluşmaz. 

## <a name="synchronize"></a>Eşitle 

Senkronizasyon işlemleri el ile veya REST API kullanılarak gerçekleştirilmelidir.

### <a name="in-azure-portal"></a>Azure portalında

**Genel >** modeli > **Eşitle modeli.**

![Ölçeklendir-çıkış kaydırıcısı](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST API

**Eşitleme** işlemini kullanın.

#### <a name="synchronize-a-model"></a>Bir modeli eşitle   

`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Eşitleme durumu alma  

`GET https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

İade durum kodları:


|Kod  |Açıklama  |
|---------|---------|
|-1     |  Geçersiz       |
|0     | Çoğaltılıyor        |
|1     |  Rehydrating       |
|2     |   Tamamlandı       |
|3     |   Başarısız      |
|4     |    Sonlandırılıyor     |
|||


### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i kullanmadan önce [en son Azure PowerShell modüllerini yükleyin veya güncelleyin.](/powershell/azure/install-az-ps) 

Eşitleme çalıştırmak için [Eşitleme-AzAnalysisServicesInstance'ı](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance)kullanın.

Sorgu çoğaltma sayısını ayarlamak için [Set-AzAnalysisServicesServer'ı](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver)kullanın. İsteğe `-ReadonlyReplicaCount` bağlı parametreyi belirtin.

İşleme sunucusunu sorgu havuzundan ayırmak için [Set-AzAnalysisServicesServer'ı](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver)kullanın. Kullanmak için `-DefaultConnectionMode` isteğe `Readonly`bağlı parametreyi belirtin.

Daha fazla bilgi için [bkz: Az.AnalysisServices modülü ile bir hizmet sorumlusu kullanma.](analysis-services-service-principal.md#azmodule)

## <a name="connections"></a>Bağlantılar

Sunucunuzun Genel Bakış sayfasında iki sunucu adı vardır. Bir sunucu için henüz ölçeklendirme yapılandırmadıysanız, her iki sunucu adı da aynı şekilde çalışır. Bir sunucu için ölçeklendirme yapılandırdıktan sonra, bağlantı türüne bağlı olarak uygun sunucu adını belirtmeniz gerekir. 

Power BI Desktop, Excel ve özel uygulamalar gibi son kullanıcı istemci bağlantıları için **Sunucu adını**kullanın. 

PowerShell, Azure İşlev uygulamaları ve AMO'daki SSMS, Visual Studio ve bağlantı dizeleri için **Yönetim sunucu adını**kullanın. Yönetim sunucusu adı özel `:rw` (okuma-yazma) niteleyici içerir. Tüm işleme işlemleri (birincil) yönetim sunucusunda gerçekleşir.

![Sunucu adları](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="scale-up-scale-down-vs-scale-out"></a>Ölçeklendirme, Küçültme ve Küçültme

Birden çok yinelemeiçeren bir sunucudaki fiyatlandırma katmanını değiştirebilirsiniz. Aynı fiyatlandırma katmanı tüm yinelemeler için geçerlidir. Bir ölçek işlemi önce tüm kopyaları bir kerede aşağı getirecek sonra yeni fiyatlandırma katmanıtüm kopyaları getirmek.

## <a name="troubleshoot"></a>Sorun giderme

**Sorun:** Kullanıcılar hata olsun **sunucu\<bulamıyorum ' Sunucunun adı>' örneğini bağlantı modunda 'ReadOnly'.**

**Çözüm:** **İşleme sunucusunu sorgu havuzu seçeneğinden ayır'ı** seçerken, varsayılan bağlantı `:rw`dizesini (olmadan) kullanan istemci bağlantıları sorgu havuzu yinelemelerine yönlendirilir. Eşitleme henüz tamamlanmadığından sorgu havuzundaki yinelemeler henüz çevrimiçi değilse, yeniden yönlendirilen istemci bağlantıları başarısız olabilir. Başarısız bağlantıları önlemek için, eşitleme gerçekleştirirken sorgu havuzunda en az iki sunucu olmalıdır. Diğerleri çevrimiçi kalırken her sunucu ayrı ayrı eşitlenir. İşlem sırasında işlem sunucusunun sorgu havuzunda olmamasını seçerseniz, işleme için havuzdan kaldırmayı ve işleme tamamlandıktan sonra havuza yeniden eklemeyi seçebilirsiniz, ancak eşitlemeden önce. Eşitleme durumunu izlemek için Bellek ve QPU ölçümlerini kullanın.



## <a name="related-information"></a>İlgili bilgiler

[Sunucu ölçümlerini izleme](analysis-services-monitor.md)   
[Azure Analiz Hizmetlerini Yönetme](analysis-services-manage.md) 
