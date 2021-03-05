---
title: Veead ile verilerinizi Azure 'a yedekleyin
titleSuffix: Azure Blob Storage Docs
description: Web sayfası, Azure 'un bir depolama hedefi ve kurtarma konumu olarak, Veead yedeklemesi ve kurtarması için kurtarma konumu olarak ele alınması gereken faktörlere ilişkin bir genel bakış sunar.
keywords: Veead, buluta yedekleme, yedekleme, Azure 'a yedekleme, olağanüstü durum kurtarma, Iş sürekliliği
author: karauten
ms.author: karauten
ms.date: 11/11/2020
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.openlocfilehash: 6d4b005a3f9c79ff14f5ba4053dc651c1ede56e0
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102124440"
---
# <a name="backup-to-azure-with-veeam"></a>Veead ile Azure 'a yedekleme

Bu makalede, bir Veead altyapısını Azure Blob depolama ile tümleştirme Kılavuzu sunulmaktadır. Önkoşulları, Azure depolama ilkelerini, uygulamayı ve operasyonel Kılavuzu içerir. Bu makale yalnızca, birincil sitenizde normal işlemi önleyen bir olağanüstü durum durumunda Azure 'u site dışı bir yedekleme hedefi ve kurtarma sitesi olarak kullanarak adresleyebilirsiniz. Veead, bir Azure üretim ortamındaki kaynakların olağanüstü durum ve koruma durumunda önyükleme ve kurtarma için hazır bir VM 'nin önyüklenmesi ve daha hızlı kurtarılmasını sağlamak için daha düşük bir RTO çözümü de sağlar. Veead, Azure ve Office 365 kaynaklarını yedeklemeye yönelik araçlar da sağlar. Bu yetenekler, bu belgenin kapsamı dışındadır. 

## <a name="reference-architecture-for-on-premises-to-azure-and-in-azure-deployments"></a>Şirket içinden Azure 'a ve In-Azure dağıtıma yönelik başvuru mimarisi

![Azure Reference mimarisine veeaz](../media/veeam-architecture.png)

Mevcut Veead dağıtımınız, bir Azure depolama hesabı veya birden çok hesabı bir bulut yedekleme deposu olarak ekleyerek Azure ile kolayca tümleştirilebilir. Veeab Ayrıca Azure 'daki şirket içi yedeklemeleri kurtarmanızı sağlayarak Azure 'da isteğe bağlı kurtarma sitesi sağlar.

## <a name="veeam-interoperability-matrix"></a>Veead birlikte çalışabilirlik matrisi
| İş Yükü | GPv2 ve BLOB depolama | Cool katman desteği | Arşiv katmanı desteği | Data Box aile desteği |
|-----------------------|--------------------|--------------------|-------------------------|-------------------------|
| Şirket içi VM 'Ler/veriler | v10a | v10a | NA | 10A |
| Azure VM’leri | v10a | v10a | NA | 10A |
| Azure Blob | v10a | v10a | NA | 10A |
| Azure Dosyaları | v10a | v10a | NA | 10A | 

> [!Note]
Veeab yedekleme ve çoğaltma yalnızca Azure Data Box için REST API destekler, bu nedenle Azure Data Box Disk desteklenmez. Veead v11 içinde Azure Blob depolama için Arşiv katmanı desteği beklenmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Küçük bir ön planlama, Azure 'u site dışında bir yedekleme hedefi ve kurtarma sitesi olarak kullanan birçok, çok mutlu müşterinin derecelendirmelerinin katılırsanız emin olmanızı sağlayacaktır.

### <a name="are-you-new-to-azure"></a>Azure 'da yeni misiniz?

Microsoft, Azure kullanmaya başlamanızı sağlamak için izlenecek bir çerçeve sunar. [Bulut benimseme çerçevesi](https://docs.microsoft.com/azure/architecture/cloud-adoption/) \( CAF, \) Kurumsal Dijital dönüştürmeye yönelik ayrıntılı bir yaklaşım ve üretim sınıfı bulutu benimseme planlaması için kapsamlı kılavuz. CAF, hızlı ve güvenli bir şekilde çalışmaya başlamanıza ve [Azure Portal](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade)etkileşimli bir sürüm bulmanıza yardımcı olmak için Azure 'a yönelik adım adım [Azure kurulum kılavuzunu](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-setup-guide/) içerir. Azure uzmanlığına yol açmak için uygulama dağıtmak ve ücretsiz eğitim kaynakları dağıtmak üzere örnek mimarilere ve belirli en iyi uygulamaları bulacaksınız.

### <a name="consider-the-network-between-your-location-and-azure"></a>Konumunuz ile Azure arasında ağı değerlendirin

Bulut kaynaklarından yararlanarak üretim, test ve geliştirme veya yedekleme hedefi ve kurtarma sitesi olarak çalışır hale gelir. ilk yedekleme için bant genişliği ihtiyaçlarınızı ve şirket içi günlük aktarımları anlamak önemlidir.

Azure Data Box, taban çizgisi aktarımının maliyetten daha uzun sürmesini sağlamak için tahmin edilebilir olması gerekmeden, ilk yedekleme temelinizi ek bant genişliği gerektirmeden Azure 'a aktarmaya yönelik bir yol sağlar. İlk yedeklemenizi aktarmak için gereken süreyi tahmin etmek üzere bir depolama hesabı oluşturduğunuzda veri aktarımı tahmin Aracı ' dan yararlanabilirsiniz.

![Azure depolama Veri Aktarımı Estimator](../media/az-storage-transfer.png)

, Üretim uygulamalarını etkilemeden gerekli aktarım penceresinde (yedekleme penceresi) günlük veri aktarımlarını desteklemek için yeterli ağ kapasitesi gerekeceğini unutmayın. Bu bölümde, ağ ihtiyaçlarınızı değerlendirmek için kullanılabilen araçların ve tekniklerin ana hatlarıyla yer alınacaktır.

#### <a name="how-can-you-determine-how-much-bandwidth-you-will-need"></a>Ne kadar bant genişliğine ihtiyacınız olacağını nasıl belirleyebilirsiniz?

Azure 'a ilk temel aktarım için değişiklik oranını ve toplam yedekleme kümesi boyutunu tespit etmek üzere birden çok değerlendirme seçeneği kullanılabilir. Aşağıda aşağıdakiler gibi değerlendirme ve raporlama araçlarının örnekleri verilmiştir:
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
|2 TB gün başına yazılan yeni veri x 30 gün     |işlemlerde $72          |
|Aylık tahmini toplam     |$1628,48         |
|---------|---------|
|Ortak Internet üzerinden şirket içi olarak 5 TB 'lık bir kez geri yükleme   | $527,26         |

> [!Note]
Bu tahmin, Azure fiyatlandırma hesaplayıcıda Doğu ABD Kullandıkça Öde fiyatlandırması kullanılarak oluşturulmuştur ve WAN aktarımları için varsayılan olarak 256. Bu örnek, gereksinimlerinize uygun olmayabilir.

## <a name="implementation-and-operational-guidance"></a>Uygulama ve işlem kılavuzu

Bu bölümde, şirket içi Veead dağıtımına Azure Storage eklemek için kısa bir kılavuz sunulmaktadır. Ayrıntılı kılavuz ve planlama konuları ile ilgileniyorsanız, [Veeak bulutu Connect Backup kılavuzunu](https://helpcenter.veeam.com/docs/backup/cloud/cloud_backup.html?ver=100)gözden geçirmeyi öneririz.

1. Azure portal açın ve "depolama hesapları" ifadesini arayın veya varsayılan hizmetler simgesine tıklayın.

      ![Azure Portal](../media/azure-portal.png)

      ![Azure portalındaki depolama hesapları](../media/locate-storage-account.png)

2. Hesap eklemeyi seçin ve bir kaynak grubu seçin veya oluşturun, benzersiz bir ad sağlayın, bölgeyi seçin, "standart" performans ' ı seçin, her zaman hesap türünü "depolama v2" olarak bırakın, SLA 'larınızı karşılayan çoğaltma düzeyini ve yedekleme yazılımınızın çalıştırılacağı varsayılan katmanı seçin. Azure depolama hesabı, tek bir hesap içinde sık erişimli, seyrek erişimli ve arşiv katmanları sağlar ve Veeab ilkeleri verilerinizin yaşam döngüsünü verimli bir şekilde yönetmek için birden çok katmandan yararlanmanızı sağlar. Sonraki adıma geçin. 
    
      ![Depolama hesabı oluşturma](../media/account-create-1.png)

3. Şimdilik varsayılan ağ seçeneklerini kontrol edin ve "veri koruma" ' ye geçin. Burada, belirlenen saklama döneminde yanlışlıkla silinen bir yedekleme dosyasını kurtarmanıza ve yanlışlıkla ya da kötü amaçlı silmeye karşı koruma sağlamasına olanak sağlayan "geçici silme" seçeneğini belirleyebilirsiniz. 
    ![Depolama hesabı bölümü oluşturma 2](../media/account-create-2.png)

4. Daha sonra, Azure kullanım örneklerine yedekleme için "Gelişmiş" ekranından varsayılan ayarları öneririz.

    ![Depolama hesabı Bölüm 3 oluşturma](../media/account-create-3.png) 

5. Etiketleyerek ve hesabınızı oluşturduktan sonra kuruluş için Etiketler ekleyin. Artık elden çıkarmada Petabaytlarca depolama alanına sahip olursunuz!

6. Hesabı Veead ortamınıza ekleyebilmeniz için hemen gerekli olan iki hızlı adım vardır. Azure portalında oluşturduğunuz hesaba gidin ve Portal dikey penceresinde "blob hizmeti" menüsünde "kapsayıcılar" ı seçin. Yeni bir kapsayıcı ekleyin ve anlamlı bir ad seçin. Sonra, "Ayarlar" altındaki "erişim tuşları" öğesine gidin ve "depolama hesabı adı" nı ve iki erişim anahtardan birini kopyalayın. Sonraki adımlarımızda kapsayıcı adına, hesap adına ve erişim anahtarına ihtiyacınız olacak.

    ![Kapsayıcı oluşturma](../media/container-b.png)
    
    ![Bu hesap bilgilerini alın](../media/access-key.png)
    
    > [!Note]
Veeab yedekleme ve çoğaltma, Azure 'a bağlanmak için ek seçenekler sunar. Bu makalenin kullanım durumu için, bir yedekleme hedefi olarak Microsoft Azure Blob depolama özelliğinden yararlanarak, yukarıdaki yöntemin kullanılması önerilen en iyi uygulamadır.

7. ***(Isteğe bağlı)*** Dağıtımınıza ek güvenlik katmanları ekleyebilirsiniz.

     1. Depolama hesabınızda kimlerin değişiklik yapacağına göre sınırlamak için rol tabanlı erişimi yapılandırın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/authorization-resource-provider?toc=/azure/storage/blobs/toc.json)

    1. Şirket ağınızın dışından erişim girişimlerini engellemek için, hesap erişimini [depolama güvenlik duvarıyla](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal) belirli ağ kesimleriyle kısıtlayın.

    ![Depolama güvenlik duvarı](../media/storage-firewall.png) 

    1. Depolama hesabının yanlışlıkla silinmesini engellemek için hesapta bir [silme kilidi](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) ayarlayın.

    ![Kaynak kilidi ](../media/resource-lock.png) 1.) Ek [güvenlik en iyi yöntemlerini](https://docs.microsoft.com/azure/storage/blobs/security-recommendations)yapılandırın. 
8. Veahar yedekleme ve Çoğaltma Yönetimi konsolunda, "Yedekleme altyapısı" ' na gidin, > genel bakış bölmesine sağ tıklayın ve "yedekleme deposu Ekle" yi seçerek yapılandırma sihirbazını açın. İletişim kutusunda, nesne depolama--> blob Storage Microsoft Azure > Azure Blob Depolama ' yı seçin.
    
    ![Veeaa deposu Sihirbazı ekran a](../media/veeam-repo-a.png)

    ![Veeam deposu Sihirbazı ekran b](../media/veeam-repo-b.png)

    ![Veeam deposu Sihirbazı ekran c](../media/veeam-repo-c.png)

9. Ardından, yeni Microsoft Azure Blob deponuzu bir ad ve açıklama belirtin.
    
    ![Veeam deposu Sihirbazı ekran d](../media/veeam-repo-d.png)

10. Sonraki adımda, Azure depolama hesabınıza erişmek için kimlik bilgilerini ekleyin. Bulut kimlik bilgileri Yöneticisi 'nde "Microsoft Azure Depolama hesap" seçeneğini belirleyin, depolama hesabınızın adını ve erişim anahtarınızı girin. Bölge seçicisindeki Azure Global ' i ve varsa ağ geçidi sunucusunu seçin.
    
    ![Veeam deposu Sihirbazı ekran e](../media/veeam-repo-e.png)

> [!Note]
Veead Gateway sunucusu kullanmayı tercih ederseniz, tüm genişleme depo kapsamlarının doğrudan internet erişimine sahip olduğundan emin olun.

11. Kapsayıcı kaydı ' nda, Azure depolama kapsayıcınızı seçin ve yedeklemelerinizi depolamak için bir klasör seçin veya oluşturun. Ayrıca, Veead (önerilir) tarafından kullanılacak genel depolama kapasitesi üzerinde bir geçici sınır tanımlayabilirsiniz. Özet bölümünde görünen bilgileri gözden geçirin ve yapılandırma aracını doldurun. Yeni depo daha sonra yedekleme işi yapılandırmanızda seçilebilir.

    ![Veeab deposu Sihirbazı ekran f](../media/veeam-repo-f.png)
    
    ![Veeam deposu Sihirbazı ekran g](../media/veeam-repo-g.png)

### <a name="azure-alerting-and-performance-monitoring"></a>Azure uyarı ve performans izleme

Yedeklemelerinizi depolamak için kullandığınız herhangi bir depolama hedefi ile yaptığınız gibi, hem Azure kaynaklarınızın hem de aynı olanlarınızın onlardan faydalanma yeteneğini izlemeniz önerilir. Azure Izleyici ve Veeam 'nin izleme özellikleri (örneğin, Veeam yönetim konsolunun işler düğümündeki istatistikler sekmesi veya Veeam ONE Reporter gibi daha gelişmiş seçenekler), ortamınızı sağlıklı tutmanıza yardımcı olur.

#### <a name="microsoft-azure-portal"></a>Microsoft Azure portalı

Microsoft Azure, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource)biçiminde güçlü bir izleme çözümü sağlar. Azure Izleyici 'yi Azure depolama kapasitesini, işlemleri, kullanılabilirliği, kimlik doğrulamasını ve daha fazlasını izlemek için [yapılandırabilirsiniz](https://docs.microsoft.com/azure/storage/common/monitor-storage?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-powershell#configuration) . İzlenen ölçümlerin tam başvurusu [burada](https://docs.microsoft.com/azure/storage/common/monitor-storage-reference)bulunabilir. İzleme için birkaç faydalı ölçüm, BlobCapacity ' dir. Azure depolama hesabınıza yazılan ve Azure Storage hesabından gelen ve giden isteklerin gidiş dönüş süresini izlemek için en fazla [depolama hesabı kapasitesi sınırı](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account), giriş ve çıkış-altında kaldığınızdan emin olun. 

Azure depolama hizmeti durumunu izlemek ve istediğiniz zaman [Azure durum panosunu](https://status.azure.com/status) görüntülemek için [günlük uyarıları da oluşturabilirsiniz](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications) .

#### <a name="veeam-reporting"></a>Veead raporlama

[Veead bir raporlamayı yapılandırma](https://helpcenter.veeam.com/docs/one/reporter/configure_reporter.html?ver=100)

[Veeab yedekleme ve çoğaltma alarmları](https://helpcenter.veeam.com/docs/one/monitor/backup_alarms.html?ver=100) 

### <a name="how-to-open-support-cases"></a>Destek çalışmalarını açma

Azure çözümüne yönelik yedeklemeyle ilgili yardıma ihtiyacınız olduğunda, destek kuruluşlarımızda ortak olarak işbirliği yapabilmesi için hem Veeak hem de Azure ile bir servis talebi açmanız önerilir.

#### <a name="how-to-open-a-case-with-veeam"></a>Veead ile bir servis talebi açma

[Veeae müşteri destek portalına](https://www.veeam.com/support.html)gidin, oturum açın ve bir servis talebi açın.

Veead tarafından sunulan destek seçeneklerini anlamanız gerekiyorsa, bkz. [Veeae müşteri destek ilkesi](https://www.veeam.com/veeam_software_support_policy_ds.pdf)

Ayrıca, bir servis talebi açmak için ' de çağrı yapabilirsiniz:

[Dünya genelindeki destek numaraları](https://www.veeam.com/contacts.html?ad=in-text-link#support-numbers)

#### <a name="how-to-open-a-case-with-the-azure-support-team"></a>Azure destek ekibi ile bir servis talebi açma

[Azure Portal](https://portal.azure.com) içinde portalın en üstündeki arama çubuğunda "destek" araması yapın ve "+ yeni destek Isteği" ni seçin. 
> [!Note]
Bir servis talebi açılırken, lütfen "Azure Backup" **değil** , "Azure Storage" veya "Azure Networking" ile ilgili yardıma ihtiyacınız olduğundan emin olun. Azure Backup, Microsoft Azure yerel bir hizmettir ve servis talebi yanlış yönlendirilir.

### <a name="links-to-relevant-veeam-documentation"></a>İlgili Veead belgelerinin bağlantıları

Daha ayrıntılı bilgi sağlayan veead belgeleri:

[Veeab Kullanıcı Kılavuzu](https://helpcenter.veeam.com/docs/backup/hyperv/overview.html?ver=100)

[Veead mimari Kılavuzu](https://helpcenter.veeam.com/docs/backup/vsphere/backup_architecture.html?ver=100)

### <a name="link-to-marketplace-offering"></a>Market sunumuna bağlantı

Ayrıca, Azure üzerinde çalışan iş yüklerinizi korumak için bildiğiniz ve güvendiğiniz Veeam çözümünü kullanmaya devam edebilirsiniz. Veead, çözümlerini Azure 'a dağıtmayı ve Azure sanal makinelerini ve diğer birçok Azure hizmetini korumanıza yardımcı oldu.

[Azure Marketi aracılığıyla Veeam B&R 'yi dağıtma](https://azuremarketplace.microsoft.com/marketplace/apps/veeam.veeam-backup-replication?tab=overview)

[Azure veri sayfası](https://www.veeam.com/backup-azure.html?ad=menu-products)


## <a name="next-steps"></a>Sonraki adımlar

Özel kullanım senaryoları hakkında bilgi almak için bu dış Web sitelerindeki ek kaynakları keşfet:

[Veead videoları](https://www.veeam.com/how-to-videos.html?ad=menu-resources)

[Veeab teknik belgeleri](https://www.veeam.com/documentation-guides-datasheets.html?ad=menu-resources)

[Veeab Bilgi Bankası ve SSS](https://www.veeam.com/knowledge-base.html?ad=menu-resources)
