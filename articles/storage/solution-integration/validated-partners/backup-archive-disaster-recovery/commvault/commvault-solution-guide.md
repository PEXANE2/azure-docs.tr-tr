---
title: Commkasasıyla verilerinizi Azure 'a yedekleyin
titleSuffix: Azure Blob Storage Docs
description: Web sayfası, Azure 'dan bir depolama hedefi ve kurtarma konumu olarak Azure 'dan yararlanmak için göz önünde bulundurulması gereken faktörlere ve İzlenecek adımları sağlar. yedekleme ve kurtarma
keywords: Commkasakasası, buluta yedekleme, yedekleme, Azure 'a yedekleme, olağanüstü durum kurtarma, Iş sürekliliği
author: karauten
ms.author: karauten
ms.date: 11/11/2020
ms.topic: article
ms.service: Storage
ms.subservice: Blob Storage
ms.openlocfilehash: 381775b8cdba173cc1752a45b83670c7251e50ec
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102101571"
---
# <a name="back-up-to-azure-with-commvault"></a>Commkasasıyla Azure 'a yedekleme

Bu makalede, Azure Blob depolama ile bir Commkasa altyapısını tümleştirme Kılavuzu sunulmaktadır. Önkoşulları, Azure depolama ilkelerini, uygulamayı ve operasyonel Kılavuzu içerir. Bu makale yalnızca, birincil sitenizde normal işlemi önleyen bir olağanüstü durum durumunda Azure 'u site dışı bir yedekleme hedefi ve kurtarma sitesi olarak kullanarak adresleyebilirsiniz. Ayrıca, bir Azure üretim ortamındaki kaynakların olağanüstü durum ve koruma durumunda çalışmaya hazır bir VM 'nin önyüklemesinde ve kurtarılmasına daha hızlı bir şekilde kurtarılmasını sağlamak için de bir düşük RTO çözümü, Commkasak canlı eşitlemesi de sunmaktadır. Bu yetenekler, bu belgenin kapsamı dışındadır. 

## <a name="reference-architecture-for-on-premises-to-azure-and-in-azure-deployments"></a>Şirket içinden Azure 'a ve In-Azure dağıtıma yönelik başvuru mimarisi

![Azure başvuru mimarisine yönelik commkasadan](../media/commvault-diagram.png)

Mevcut bir Azure depolama hesabı veya birden çok hesap ekleyerek bulut depolama hedefi olarak Azure ile kolayca tümleştirilebilir. Ayrıca, Azure 'da şirket içi kurtarma sitesi sunarak Azure 'daki yedeklemeleri kurtarmanızı sağlar.   

## <a name="commvault-interoperability-matrix"></a>Commkasası birlikte çalışabilirlik matrisi
| İş Yükü | GPv2 ve BLOB depolama | Cool katman desteği | Arşiv katmanı desteği | Data Box aile desteği |
|-----------------------|--------------------|--------------------|-------------------------|-------------------------|
| Şirket içi VM 'Ler/veriler | v 11,5 lik | v 11,5 lik | v 11.10 | v 11.10 |
| Azure VM’leri | v 11,5 lik | v 11,5 lik | v 11,5 lik | NA |
| Azure Blob | v 6x | v 6x | v 6x | NA |
| Azure Dosyaları | v 6x | v 6x | v 6x | NA | 

## <a name="before-you-begin"></a>Başlamadan önce

Küçük bir ön planlama, Azure 'u site dışında bir yedekleme hedefi ve kurtarma sitesi olarak kullanan birçok, çok mutlu müşterinin derecelendirmelerinin katılırsanız emin olmanızı sağlayacaktır.

### <a name="are-you-new-to-azure"></a>Azure 'da yeni misiniz?

Microsoft, Azure kullanmaya başlamanızı sağlamak için izlenecek bir çerçeve sunar. [Bulut benimseme çerçevesi](https://docs.microsoft.com/azure/architecture/cloud-adoption/) \( CAF, \) Kurumsal Dijital dönüştürmeye yönelik ayrıntılı bir yaklaşım ve üretim sınıfı bulutu benimseme planlaması için kapsamlı kılavuz. CAF hızlı ve güvenli bir şekilde çalışmaya başlamanıza ve [Azure portalında](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade)etkileşimli bir sürüm bulmanıza yardımcı olmak için Azure 'a yönelik adım adım [Azure kurulum kılavuzunu](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-setup-guide/) içerir. Azure uzmanlığına yol açmak için uygulama dağıtmak ve ücretsiz eğitim kaynakları dağıtmak üzere örnek mimarilere ve belirli en iyi uygulamaları bulacaksınız.

### <a name="consider-the-network-between-your-location-and-azure"></a>Konumunuz ile Azure arasında ağı değerlendirin

Bulut kaynaklarından yararlanarak üretim, test ve geliştirme veya yedekleme hedefi ve kurtarma sitesi olarak çalışır hale gelir. ilk yedekleme için bant genişliği ihtiyaçlarınızı ve şirket içi günlük aktarımları anlamak önemlidir. 

Azure Data Box, taban çizgisi aktarımının maliyetten daha uzun sürmesini sağlamak için tahmin edilebilir olması gerekmeden, ilk yedekleme temelinizi ek bant genişliği gerektirmeden Azure 'a aktarmaya yönelik bir yol sağlar. İlk yedeklemenizi aktarmak için gereken süreyi tahmin etmek üzere bir depolama hesabı oluşturduğunuzda veri aktarımı tahmin Aracı ' dan yararlanabilirsiniz.

![Azure depolama Veri Aktarımı Estimator](../media/az-storage-transfer.png)

, Üretim uygulamalarını etkilemeden gerekli aktarım penceresinde (yedekleme penceresi) günlük veri aktarımlarını desteklemek için yeterli ağ kapasitesi gerekeceğini unutmayın. Bu bölümde, ağ ihtiyaçlarınızı değerlendirmek için kullanılabilen araçların ve tekniklerin ana hatlarıyla yer alınacaktır.

#### <a name="how-can-you-determine-how-much-bandwidth-you-will-need"></a>Ne kadar bant genişliğine ihtiyacınız olacağını nasıl belirleyebilirsiniz?

-  Yedekleme yazılımınızdan raporlar. 
  Commkasa, Azure 'a ilk temel aktarım için [değişiklik oranını](https://documentation.commvault.com/commvault/v11_sp19/article?p=39699.htm) ve [Toplam yedekleme kümesi boyutunu](https://documentation.commvault.com/commvault/v11_sp19/article?p=39621.htm) belirlemede Standart raporlar sağlar.
- Yazılım bağımsız değerlendirmesi ve raporlama araçlarını yedekleme:
  - [MiTrend](https://mitrend.com/)
  - [Aptları](https://www.veritas.com/insights/aptare-it-analytics)
  - [Veri Voss](https://www.datavoss.com/)

#### <a name="how-will-i-know-how-much-headroom-i-have-with-my-current-internet-connection"></a>Geçerli Internet bağlantınızla ne kadar yer olduğunu nasıl anlarım?

Günde ne kadar fazla yer olduğunu veya genellikle unutilized, bant genişliğini bir gün içinde nasıl kullanacağınızı bilmek önemlidir. Bu, uygulamanızı ilk kez karşıya yüklemek için, çevrimdışı dengeli dağıtım için Azure Data Box kullanmazlarsa ve yukarıda tanımlanan değişiklik hızına ve yedekleme pencerenize göre günlük yedeklemeleri tamamlamaya yönelik olarak değerlendirmenizi sağlar. Aşağıda, Azure 'daki yedeklemelerinizin tükettiği bant genişliği yer odasını belirlemek için kullanabileceğiniz yöntemler yer verilmiştir.

- Mevcut bir Azure ExpressRoute müşterisi misiniz? Azure portal [devre kullanımınızı](https://docs.microsoft.com/azure/expressroute/expressroute-monitoring-metrics-alerts#circuits-metrics) görüntüleyin.
- ISS 'nize başvurabilirsiniz. Bunlar, mevcut günlük ve aylık kullanımlarınızı gösteren raporlar içermelidir.
- Ağ trafiğinizi, yönlendiricinizin/anahtar düzeyinizdeki şu şekilde izleyerek kullanımı ölçebilir birkaç araç vardır:
  - [Solarwinds bant genişliği çözümleyici paketi](https://www.solarwinds.com/network-bandwidth-analyzer-pack?CMP=ORG-BLG-DNS)
  - [Paessler PRTG](https://www.paessler.com/bandwidth_monitoring)
  - [Cisco Ağ Yardımcısı](https://www.cisco.com/c/en/us/products/cloud-systems-management/network-assistant/index.html)
  - [WhatsUp Gold](https://www.whatsupgold.com/network-traffic-monitoring)

### <a name="choosing-the-right-storage-options"></a>Doğru depolama seçeneklerini seçme

Azure 'U bir yedekleme hedefi olarak kullanırken, müşteriler [Azure Blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) 'yı kullanır\. Azure Blob depolama, Microsoft 'un nesne depolama çözümüdür. BLOB depolama, herhangi bir veri modeli veya tanımına bağlı olmayan çok büyük miktarlarda yapılandırılmamış verileri depolamak için iyileştirilmiştir. Ayrıca, Azure Storage dayanıklı, yüksek oranda kullanılabilir, güvenli ve ölçeklenebilir. Microsoft 'un platformu, iç SLA 'larınızı karşılamak için [esneklik düzeyi](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=/azure/storage/blobs/toc.json) sağlamak üzere doğru iş yükünün doğru depolama alanını seçme esnekliği sunar. BLOB depolama, kullanım başına ödeme hizmetidir. Depolanan veri miktarı, bu verilere erişim ve seyrek erişimli ve arşiv katmanları söz konusu olduğunda, gereken en düşük saklama süresi için [aylık olarak ücretlendirilirsiniz](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal#pricing-and-billing) . Yedekleme verilerine uygulanabilecek dayanıklılık ve katmanlama seçenekleri aşağıdaki tablolarda özetlenmiştir.

**Azure Blob depolama dayanıklılığı seçenekleri:**

|  |Yerel Olarak Yedekli  |Bölge yedekli  |Coğrafi olarak yedekli  |Coğrafi bölge yedekli  |
|---------|---------|---------|---------|---------|
|Geçerli kopya sayısı     | 3         | 3         | 6         | 6 |
|Kullanılabilirlik Alanları sayısı     | 1         | 3         | 2         | 4 |
|Bölge sayısı     | 1         | 1         | 2         | 2 |
|Ikincil bölgeye el ile yük devretme     | NA         | NA         | Yes         | Yes |

**Azure Blob depolama katmanları:**

|  | Etkin katman   |Cool katmanı   | Arşiv katmanı |
| ----------- | ----------- | -----------  | -----------  |
| Kullanılabilirlik | %99,9         | %99         | Çevrimdışı      |
| Kullanım Ücretleri | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyetleri | Daha düşük depolama maliyetleri, daha yüksek erişim ve işlem maliyetleri | En düşük depolama maliyetleri, en yüksek erişim ve işlem maliyetleri |
| Gerekli en düşük veri saklama | NA | 30 gün | 180 gün |
| Gecikme süresi (Ilk bayta kalan süre) | Mayacak | Mayacak | Saat |

#### <a name="sample-backup-to-azure-cost-model"></a>Azure maliyet modeline örnek yedekleme

Kullanım başına ödeme kavramı, genel buluta yeni eklenen müşterilerin kullanımına açık olabilir. Yalnızca kullanılan kapasite için ödeme yaparken, Azure Express Route Direct yerel veya Express Route 'a yönelik işlemler (okuma ve yazma) ve [Çıkış](https://azure.microsoft.com/pricing/details/bandwidth/) işlemleri için de ödeme yaparsınız. Ayrıca, Azure [Express Route Direct yerel veya Express Route sınırsız veri planı](https://azure.microsoft.com/pricing/details/expressroute/) dahil değildir. [Azure Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)'da, liste fiyatlandırmasına göre veya [Azure Storage ayrılmış kapasite fiyatlandırmasına](https://docs.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations)göre analiz 38 etme işlemini gerçekleştirebilirsiniz. Azure 'a yedekleme için aylık ücretsiz maliyeti modelleyerek örnek bir fiyatlandırma alıştırması aşağıda verilmiştir ve bu örnek yalnızca bir örnektir ve bu durum, ***burada yakalanmayan etkinlikleriniz nedeniyle fiyatlandırma farklılık gösterebilir:***


|Maliyet faktörü  |Aylık maliyet  |
|---------|---------|
|Seyrek Erişimli depolamada 100 TB yedekleme verisi     |$1556,48         |
|2 TB gün başına yazılan yeni veri x 30 gün     |işlemlerde $39          |
|Aylık tahmini toplam     |$1595,48         |
|---------|---------|
|Ortak Internet üzerinden şirket içi olarak 5 TB 'lık bir kez geri yükleme   | $491,26         |


> [!Note] 
Bu tahmin, Azure fiyatlandırma hesaplayıcısında Doğu ABD Kullandıkça Öde fiyatlandırması kullanılarak oluşturulmuştur ve 65.536 PUT Isteği üreten 32 MB 'lık alt öbek boyutunun, günde her gün yazma işlemi olan 32MB alt öbek boyutunu temel alır. Bu örnek, gereksinimlerinize uygun olmayabilir.

## <a name="implementation-and-operational-guidance"></a>Uygulama ve işlem kılavuzu

Bu bölümde, şirket içi bir iletişim dağıtımına Azure Storage eklemek için kısa bir kılavuz sunulmaktadır. Ayrıntılı kılavuz ve planlama konuları ile ilgileniyorsanız, [Azure mimari Kılavuzu](https://www.commvault.com/resources/public-cloud-architecture-guide-for-microsoft-azure-v11-sp16)' nu gözden geçirmeyi öneririz.

1. Azure portal açın ve "depolama hesapları" ifadesini arayın veya varsayılan hizmetler simgesine tıklayın.
    
    1. ![Azure Portal](../media/azure-portal.png)
  
    1. ![Azure portalındaki depolama hesapları](../media/locate-storage-account.png)

2. Hesap eklemeyi seçin ve bir kaynak grubu seçin veya oluşturun, benzersiz bir ad sağlayın, bölgeyi seçin, "standart" performans ' ı seçin, her zaman hesap türünü "depolama v2" olarak bırakın, SLA 'larınızı karşılayan çoğaltma düzeyini ve yedekleme yazılımınızın çalıştırılacağı varsayılan katmanı seçin. Azure depolama hesabı, tek bir hesap içinde kullanılabilir sık erişimli, seyrek erişimli ve arşiv katmanları, verilerinizin yaşam döngüsünü verimli bir şekilde yönetmek için birden çok katmandan yararlanmanızı sağlar. Sonraki adıma geçin. 

    ![Depolama hesabı oluşturma](../media/account-create-1.png)

3. Şimdilik varsayılan ağ seçeneklerini kontrol edin ve "veri koruma" ' ye geçin. Burada, belirlenen saklama döneminde yanlışlıkla silinen bir yedekleme dosyasını kurtarmanıza ve yanlışlıkla ya da kötü amaçlı silmeye karşı koruma sağlamasına olanak sağlayan "geçici silme" seçeneğini belirleyebilirsiniz. 
    
    ![Depolama hesabı bölümü oluşturma 2](../media/account-create-2.png)

4. Daha sonra, Azure kullanım örneklerine yedekleme için "Gelişmiş" ekranından varsayılan ayarları öneririz.

    ![Depolama hesabı Bölüm 3 oluşturma](../media/account-create-3.png) 

5. Etiketleyerek ve hesabınızı oluşturduktan sonra kuruluş için Etiketler ekleyin. Artık elden çıkarmada Petabaytlarca depolama alanına sahip olursunuz!

6. Hesabı Commkasa ortamınıza ekleyebilmeniz için hemen gerekli olan iki hızlı adım vardır. Azure portal oluşturduğunuz hesaba gidin ve Portal dikey penceresinde "blob hizmeti" menüsü altında "kapsayıcılar" seçeneğini belirleyin. Yeni bir kapsayıcı ekleyin ve anlamlı bir ad seçin. Sonra, "Ayarlar" altındaki "erişim tuşları" öğesine gidin ve "depolama hesabı adı" nı ve iki erişim anahtardan birini kopyalayın. Sonraki adımlarımızda kapsayıcı adına, hesap adına ve erişim anahtarına ihtiyacınız olacak.
    
    ![Kapsayıcı oluşturma](../media/container.png)
    
    ![Bu hesap bilgilerini alın](../media/access-key.png)

7. ***(Isteğe bağlı)*** Dağıtımınıza ek güvenlik katmanları ekleyebilirsiniz.
    
    1. Depolama hesabınızda kimlerin değişiklik yapacağına göre sınırlamak için rol tabanlı erişimi yapılandırın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/authorization-resource-provider?toc=/azure/storage/blobs/toc.json)
    1. Şirket ağınızın dışından erişim girişimlerini engellemek için, hesap erişimini [depolama güvenlik duvarıyla](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal) belirli ağ kesimleriyle kısıtlayın.

    ![Depolama güvenlik duvarı](../media/storage-firewall.png) 

    1. Depolama hesabının yanlışlıkla silinmesini engellemek için hesapta bir [silme kilidi](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) ayarlayın.

    ![Kaynak kilidi](../media/resource-lock.png)
    
    1. Ek [güvenlik en iyi yöntemlerini](https://docs.microsoft.com/azure/storage/blobs/security-recommendations)yapılandırın.
    
1. Commkasaal komut merkezinde "Yönet"--> "güvenlik"--> "Credential Manager" bölümüne gidin. "Bulut hesabı", "satıcı türü" nü seçin Microsoft Azure Depolama, Azure 'a ve Azure 'dan veri aktarabileceği "MediaAgent" öğesini seçin, depolama hesabı adını ve erişim anahtarını ekleyin.
    
    ![Commkasa kimlik bilgileri](../media/commvault-credential.png)

9. Sonra, "depolama" a gidin--> "Cloud" (Commkasate "Ekle" yi seçin. Depolama hesabı için kolay bir ad girin ve "tür" listesinden "Microsoft Azure Depolama" öğesini seçin. Yedeklemeleri Azure depolama 'ya aktarmak için kullanılacak bir medya Aracısı sunucusu seçin. Oluşturduğunuz kapsayıcıyı ekleyin, Azure Storage hesabından yararlanmak üzere depolama katmanını seçin ve #8 adımında oluşturulan kimlik bilgilerini seçin. Son olarak, yinelenenleri kaldırılmış yedeklemelerin mi yoksa yinelenenleri kaldırma veritabanı için bir konumun mi aktarılmayacağını seçin.
    
     ![Commkasasında depolama ekleme](../media/commvault-add-storage.png)

10. Son olarak, yeni Azure depolama kaynağınızı "Yönet"--> "planlar" i "yedekleme hedefi" olarak "yönetme"--"planları" aracılığıyla mevcut veya yeni bir plana ekleyin.

    ![Commkasasında depolama ekleme](../media/commvault-plan.png)

11. ***(Isteğe bağlı)*** Sunucuları ve uygulamaları Azure 'a geçirmek için Azure 'u bir kurtarma sitesi veya Commkasadan yararlanmaya çalışırsanız, Azure 'da bir VSA proxy 'Si dağıtmak en iyi uygulamadır. Ayrıntılı yönergeleri [buradan](https://documentation.commvault.com/commvault/v11/article?p=106208.htm)bulabilirsiniz.  

### <a name="azure-alerting-and-performance-monitoring"></a>Azure uyarı ve performans izleme

Yedeklemelerinizi depolamak için kullandığınız herhangi bir depolama hedefi ile yaptığınız gibi, hem Azure kaynaklarınızın hem de Commkasasının bunlardan faydalanma yeteneğini izlemeniz önerilir. Azure Izleyici ve Commkasası Komut Merkezi izlemenin bir birleşimi ortamınızı sağlıklı tutmanıza yardımcı olur.

#### <a name="microsoft-azure-portal"></a>Microsoft Azure portalı

Microsoft Azure, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource)biçiminde güçlü bir izleme çözümü sağlar. Azure Izleyici 'yi Azure depolama kapasitesini, işlemleri, kullanılabilirliği, kimlik doğrulamasını ve daha fazlasını izlemek için [yapılandırabilirsiniz](https://docs.microsoft.com/azure/storage/common/monitor-storage?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-powershell#configuration) . İzlenen ölçümlerin tam başvurusu [burada](https://docs.microsoft.com/azure/storage/common/monitor-storage-reference)bulunabilir. İzleme için birkaç faydalı ölçüm, BlobCapacity ' dir-en yüksek [depolama hesabı kapasite sınırının](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account)altında kalmakta ve Azure depolama hesabınızda okunan veri miktarını Izlemek için çıkış yapın ve Azure Storage ve mediaagent 'a giden ve giden isteklerin gidiş dönüş süresini Izlemek için SuccessE2ELatency. 

Azure depolama hizmeti durumunu izlemek ve istediğiniz zaman [Azure durum panosunu](https://status.azure.com/status) görüntülemek için [günlük uyarıları da oluşturabilirsiniz](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications) .

#### <a name="commvault-command-center"></a>Commkasala Komut Merkezi

[Bulut depolama havuzları için uyarı oluşturma](https://documentation.commvault.com/commvault/v11/article?p=100514_3.htm)

[Müşterilerin performans raporlarını görüntülemek, iş tamamlaması ve temel sorun gidermeyi başlatmak için nereden gidebileceği](https://documentation.commvault.com/commvault/v11/article?p=95306_1.htm) 

### <a name="how-to-open-support-cases"></a>Destek çalışmalarını açma

Azure çözümüne yönelik yedeklemeyle ilgili yardıma ihtiyacınız olduğunda, destek kuruluşlarımızda ortak olarak işbirliği yapabilmesi için hem Commkasadan hem de Azure ile bir servis talebi açmanız önerilir.

#### <a name="how-to-open-a-case-with-commvault"></a>Commkasasıyla bir servis talebi açma

[Commkasa destek sitesine](https://www.commvault.com/support)gidin, oturum açın ve bir servis talebi açın.

Size sunulan destek sözleşmesi seçeneklerini anlamanız gerekiyorsa lütfen bkz. [Commkasa destek seçenekleri](https://ma.commvault.com/support)

Ayrıca, bir servis talebi açmak veya e-posta yoluyla Commkasa desteğine ulaşmak için ' de çağrı yapabilirsiniz:

Ücretsiz: + 1 877-780-3077

[Dünya genelindeki destek numaraları](https://ma.commvault.com/Support/TelephoneSupport)

[E-posta Commkasası desteği](mailto:support@commvault.com)

#### <a name="how-to-open-a-case-with-the-azure-support-team"></a>Azure destek ekibi ile bir servis talebi açma

[Azure Portal](https://portal.azure.com) içinde portalın en üstündeki arama çubuğunda "destek" araması yapın ve "+ yeni destek Isteği" ni seçin. 
> [!Note]
Bir servis talebi açılırken, "Azure Backup" **değil** , "Azure Storage" veya "Azure Networking" ile ilgili yardıma ihtiyacınız olduğundan emin olun. Azure Backup, Microsoft Azure yerel bir hizmettir ve servis talebi yanlış yönlendirilir.

### <a name="links-to-relevant-commvault-documentation"></a>İlgili Commkasası belgelerinin bağlantıları

Daha ayrıntılı bilgi sağlayan commkasa belgeleri:

[Commkasakasası Kullanıcı Kılavuzu](https://documentation.commvault.com/commvault/v11/article?p=37684_1.htm)

[Commkasakasası Azure mimari Kılavuzu](https://www.commvault.com/resources/public-cloud-architecture-guide-for-microsoft-azure-v11-sp16) 

### <a name="link-to-marketplace-offering"></a>Market sunumuna bağlantı

Ayrıca, Azure üzerinde çalışan iş yüklerinizi korumak için bildiğiniz ve güvendiğiniz Commkasası çözümünü kullanmaya devam edebilirsiniz. Commkasa, çözümlerini Azure 'da dağıtmayı ve Azure sanal makinelerini ve diğer birçok Azure hizmetini korumayı kolaylaştırır.

[Azure Marketi aracılığıyla Commkasasını dağıtma](https://azuremarketplace.microsoft.com/marketplace/apps/commvault.commvault?tab=Overview)

[Azure veri sayfası](https://www.commvault.com/resources/microsoft-azure-cloud-platform-datasheet)

[Desteklenen Azure özelliklerinin ve hizmetlerinin kapsamlı listesi](https://documentation.commvault.com/commvault/v11/article?p=109795_1.htm)

[Azure 'da SAP HANA korumak için Commkasasını kullanma](https://azure.microsoft.com/resources/protecting-sap-hana-in-azure/)

## <a name="next-steps"></a>Sonraki adımlar

Özel kullanım senaryoları hakkında bilgi almak için bu dış Web sitelerindeki ek kaynakları keşfet:

[Sunucularınızı ve uygulamalarınızı Azure 'a geçirmek için Commkasasını kullanın](https://www.commvault.com/resources/demonstration-vmware-to-azure-migrations-with-commvault)

[Azure 'da SAP 'yi Commkasala koruma](https://www.youtube.com/watch?v=4ZGGE53mGVI)

[Office365 ile Commkasasıyla koruma](https://www.youtube.com/watch?v=dl3nvAacxZU)
