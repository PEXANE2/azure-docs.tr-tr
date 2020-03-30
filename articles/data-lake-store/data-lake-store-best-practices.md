---
title: Azure Veri Gölü Depolama Gen1'i kullanmak için en iyi uygulamalar | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 'i (önceden Azure Veri Gölü Deposu olarak bilinir) kullanmayla ilgili veri alımı, tarih güvenliği ve performans la ilgili en iyi uygulamaları öğrenin
services: data-lake-store
documentationcenter: ''
author: sachinsbigdata
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: sachins
ms.openlocfilehash: a8ca67d1ff3100aee02ed473c9cc2180de3973b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638944"
---
# <a name="best-practices-for-using-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1'i kullanmak için en iyi uygulamalar

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Bu makalede, Azure Veri Gölü Depolama Gen1 ile çalışmak için en iyi uygulamalar ve dikkat edilmesi gerekenler hakkında bilgi edinin. Bu makalede, Veri Gölü Depolama Gen1 için güvenlik, performans, esneklik ve izleme hakkında bilgi sağlar. Data Lake Storage Gen1'den önce, Azure HDInsight gibi hizmetlerde gerçekten büyük verilerle çalışmak karmaşıktı. Petabayt depolama ve bu ölçekte en iyi performans elde edilebilmek için birden çok Blob depolama hesabında veri parçaları bulmak zorunda kaldınız. Veri Gölü Depolama Gen1 ile boyut ve performans için sabit sınırların çoğu kaldırılır. Ancak, veri gölü depolama gen1 ile en iyi performansı elde edebilirsiniz, böylece bu makalede kapsadığı bazı hususlar hala vardır.

## <a name="security-considerations"></a>Güvenlik konuları

Azure Veri Gölü Depolama Gen1, Azure Etkin Dizin (Azure AD) kullanıcıları, grupları ve hizmet ilkeleri için POSIX erişim denetimleri ve ayrıntılı denetim sunar. Bu erişim denetimleri varolan dosya ve klasörlere ayarlanabilir. Erişim denetimleri, yeni dosya veya klasörlere uygulanabilecek varsayılanlar oluşturmak için de kullanılabilir. İzinler varolan klasörlere ve alt nesnelere ayarlandığında, izinlerin her nesneüzerinde özyinelemeli olarak yayılması gerekir. Çok sayıda dosya varsa, izinleri yaymak uzun zaman alabilir. Alınan süre saniyede işlenen 30-50 nesne arasında değişebilir. Bu nedenle, klasör yapısını ve kullanıcı gruplarını uygun şekilde planlayın. Aksi takdirde, verilerinizle çalışırken beklenmeyen gecikmelere ve sorunlara neden olabilir.

100.000 alt nesneiçeren bir klasörünüz olduğunu varsayalım. Saniyede işlenen 30 nesnenin alt sınırını alırsanız, tüm klasör için izni güncelleştirmek bir saat sürebilir. Veri Gölü Depolama Gen1 ALA'ları hakkında daha fazla bilgi [Azure Veri Gölü Depolama Gen1'de Access denetiminde](data-lake-store-access-control.md)mevcuttur. ALA'ları özyinelemeli olarak atama konusunda daha iyi performans için Azure Veri Gölü Komut Satırı Aracı'nı kullanabilirsiniz. Araç, milyonlarca dosyaya hızlı bir şekilde Aç'lar uygulamak için birden çok iş parçacığı ve özyinelemeli gezinme mantığı oluşturur. Araç Linux ve Windows için kullanılabilir ve bu araç için [belgeler](https://github.com/Azure/data-lake-adlstool) ve [indirme github](https://aka.ms/adlstool-download) bulunabilir. Aynı performans iyileştirmeleri, Data Lake Storage Gen1 [.NET](data-lake-store-data-operations-net-sdk.md) ve [Java](data-lake-store-get-started-java-sdk.md) SDK'ları ile yazılmış kendi araçlarınız tarafından etkinleştirilebilir.

### <a name="use-security-groups-versus-individual-users"></a>Güvenlik gruplarını tek tek kullanıcılara karşı kullanma

Veri Gölü Depolama Gen1'de büyük verilerle çalışırken, büyük olasılıkla Azure HDInsight gibi hizmetlerin verilerle çalışmasına izin vermek için bir hizmet ilkesi kullanılır. Ancak, tek tek kullanıcıların verilere erişmeye ihtiyaç duyduğu durumlar da olabilir. Bu gibi durumlarda, tek tek kullanıcıları klasörlere ve dosyalara atamak yerine Azure Etkin Dizin [güvenlik gruplarını](data-lake-store-secure-data.md#create-security-groups-in-azure-active-directory) kullanmanız gerekir.

Bir güvenlik grubuna izin ler atandıktan sonra, kullanıcıekleme veya gruptan çıkarma, Veri Gölü Depolama Gen1 için herhangi bir güncelleştirme gerektirmez. Bu aynı zamanda [32 Erişim ve Varsayılan ALA'ların](../azure-resource-manager/management/azure-subscription-service-limits.md#data-lake-store-limits) sınırını aşmamanıza yardımcı olur (bu, her zaman her dosya ve klasörle ilişkili olan dört POSIX tarzı ALA'yı içerir: [sahip olan kullanıcı,](data-lake-store-access-control.md#the-owning-user) [sahip olan grup,](data-lake-store-access-control.md#the-owning-group) [maske](data-lake-store-access-control.md#the-mask)ve diğer).

### <a name="security-for-groups"></a>Gruplar için güvenlik

Tartışıldığı gibi, kullanıcıların Veri Gölü Depolama Gen1'e erişmeleri gerektiğinde, en iyisi Azure Active Directory güvenlik gruplarını kullanmaktır. Bazı önerilen gruplar ile başlamak için **ReadOnlyUsers**olabilir , **WriteAccessUsers**, ve **FullAccessUsers** hesabın kökü için, ve hatta anahtar alt klasörler için ayrı olanlar. Daha sonra eklenebilecek, ancak henüz tanımlanmamış başka beklenen kullanıcı grupları varsa, belirli klasörlere erişimi olan sahte güvenlik grupları oluşturmayı düşünebilirsiniz. Güvenlik grubunu kullanmak, daha sonra binlerce dosyaya yeni izinler atamak için uzun bir işlem süresine gerek duymamasını sağlar.

### <a name="security-for-service-principals"></a>Hizmet müdürleri için güvenlik

Azure Active Directory hizmet ilkeleri genellikle Veri Gölü Depolama Gen1'deki verilere erişmek için Azure HDInsight gibi hizmetler tarafından kullanılır. Birden çok iş yükü ndeki erişim gereksinimlerine bağlı olarak, kuruluşun içinde ve dışında güvenliği sağlamak için bazı hususlar olabilir. Birçok müşteri için, tek bir Azure Etkin Dizin hizmet ilkesi yeterli olabilir ve Veri Gölü Depolama Gen1 hesabının kökünde tam izinlere sahip olabilir. Diğer müşteriler, bir kümenin verilere tam erişimi olan farklı hizmet ilkelerine sahip birden çok küme ve yalnızca okuma erişimi olan başka bir küme gerektirebilir. Güvenlik gruplarında olduğu gibi, veri gölü depolama gen1 hesabı oluşturulduktan sonra beklenen her senaryo için bir hizmet ilkesi oluşturmayı düşünebilirsiniz (okuma, yazma, tam).

### <a name="enable-the-data-lake-storage-gen1-firewall-with-azure-service-access"></a>Azure hizmet erişimiyle Veri Gölü Depolama Gen1 güvenlik duvarını etkinleştirme

Data Lake Storage Gen1, dışarıdan gelen saldırılardan daha küçük bir saldırı vektörü için önerilen bir güvenlik duvarını açma ve yalnızca Azure hizmetlerine erişimi sınırlama seçeneğini destekler. Güvenlik duvarı, **Güvenlik Duvarı** > **Etkinleştir (ON)** > Azure hizmetleri**seçeneklerine erişime izin vererek** Azure portalındaki Veri Gölü Depolama Gen1 hesabında etkinleştirilebilir.

![Veri Gölü Depolama Gen1'de güvenlik duvarı ayarları](./media/data-lake-store-best-practices/data-lake-store-firewall-setting.png "Veri Gölü Depolama Gen1'de güvenlik duvarı ayarları")

Güvenlik duvarı etkinleştirildikten sonra, yalnızca HDInsight, Veri Fabrikası, SQL Veri Ambarı gibi Azure hizmetleri Veri Gölü Depolama Gen1'e erişebilir. Azure tarafından kullanılan dahili ağ adresi çevirisi nedeniyle, Veri Gölü Depolama Gen1 güvenlik duvarı belirli hizmetlerin IP tarafından kısıtlanmasını desteklemez ve yalnızca şirket içi gibi Azure dışındaki uç noktaların kısıtlamaları için tasarlanmıştır.

## <a name="performance-and-scale-considerations"></a>Performans ve ölçek hususları

Veri Gölü Depolama Gen1'in en güçlü özelliklerinden biri, veri veri aktarışlarına ilişkin sabit sınırları kaldırmasidır. Sınırların kaldırılması, müşterilerin verileri parçalamalarına gerek kalmadan veri boyutlarını ve performans gereksinimlerini büyütmelerine olanak tanır. Veri Gölü Depolama Gen1 performansını optimize etmek için en önemli hususlardan biri, paralellik verildiğinde en iyi performansı sergilemesidir.

### <a name="improve-throughput-with-parallelism"></a>Paralellik ile iş elde ini geliştirin

En iyi okuma/yazma iş parçacığı için çekirdek başına 8-12 iş parçacığı vermeyi düşünün. Bunun nedeni, tek bir iş parçacığı üzerinde okuma/yazmanın engellenmesidir ve daha fazla iş parçacığı VM'de daha yüksek eşzamanlılık sağlar. Düzeylerin sağlıklı olduğundan ve paralelliğin artırılabilmesini sağlamak için VM'nin CPU kullanımını izlediğinden emin olun.

### <a name="avoid-small-file-sizes"></a>Küçük dosya boyutlarından kaçının

POSIX izinleri ve Veri Gölü Depolama Gen1 denetim çok sayıda küçük dosyaları ile çalışırken belirgin hale gelir bir ek yükü ile birlikte gelir. En iyi yöntem olarak, verilerinizi daha büyük dosyalara toplu olarak eklemeniz gerekir ve binlerce veya milyonlarca küçük dosyayı Data Lake Storage Gen1'e yazmanız gerekir. Küçük dosya boyutlarından kaçınmak, örneğin birden çok avantajı olabilir:

* Birden çok dosya arasında kimlik doğrulama denetimlerini düşürme
* Azaltılmış açık dosya bağlantıları
* Daha hızlı kopyalama/çoğaltma
* Veri Gölü Depolama Gen1 POSIX izinlerini güncellerken işlenmesi gereken daha az dosya

Verileri hangi hizmetlerin ve iş yüklerinin kullandığına bağlı olarak, dosyalar için göz önünde bulundurulması gereken iyi bir boyut 256 MB veya daha büyüktür. Veri Gölü Depolama Gen1'e inerken dosya boyutları toplu olarak toplu olarak alınamıyorsa, bu dosyaları daha büyük dosyalarla birleştiren ayrı bir sıkıştırma işiniz olabilir. Dosya boyutları hakkında daha fazla bilgi ve öneri ve Veri Gölü Depolama Gen1'deki verileri düzenlemek için [bkz.](data-lake-store-performance-tuning-guidance.md#structure-your-data-set)

### <a name="large-file-sizes-and-potential-performance-impact"></a>Büyük dosya boyutları ve olası performans etkisi

Data Lake Storage Gen1, en iyi performans için ve verileri okuma işlemine bağlı olarak büyük dosyaları petabayt boyutuna kadar desteklese de, ortalama 2 GB'ın üzerine çıkmak ideal olmayabilir. Örneğin, konumlar veya farklı depolama hesapları arasındaki verileri kopyalamak için **Dscp** kullanırken, dosyalar harita görevlerini belirlemek için kullanılan en iyi ayrıntı düzeyidir. Yani, her biri 1 TB olan 10 dosyayı kopyalıyorsanız, en fazla 10 mapper sayılsın. Ayrıca, mappers atanmış dosyaları çok varsa, başlangıçta mappers büyük dosyaları taşımak için paralel olarak çalışır. Ancak, iş aşağı rüzgar başlar gibi sadece birkaç mappers ayrılmış kalır ve büyük bir dosyaya atanan tek bir mapper ile sıkışmış olabilir. Microsoft, gelecekteki Hadoop sürümlerinde bu sorunu gidermek için Distcp'ye geliştirmeler göndermiştir.

Göz önünde bulundurulması gereken bir diğer örnek de, Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizi'ni kullanırken. Ayıklayıcı tarafından yapılan işleme bağlı olarak, bölünemeyen bazı dosyalar (örneğin, XML, JSON) 2 GB'dan büyük olduğunda performansta zarar görebilir. Dosyaların bir çıkarıcı (örneğin, CSV) tarafından bölünebildiği durumlarda, büyük dosyalar tercih edilir.

### <a name="capacity-plan-for-your-workload"></a>İş yükünüz için kapasite planı

Azure Veri Gölü Depolama Gen1, Blob depolama hesaplarına yerleştirilen zorlu IO azaltma sınırlarını kaldırır. Ancak, hala dikkate alınması gereken yumuşak sınırlar vardır. Varsayılan giriş/çıkış azaltma sınırları çoğu senaryonun gereksinimlerini karşılar. İş yükünüzün sınırları artırması gerekiyorsa, Microsoft desteğiyle çalışın. Ayrıca, io azaltma limitleri üretim sırasında isabet değil, böylece kavram kanıtı aşamasında sınırları bakmak. Bu durumda, Microsoft mühendislik ekibinden el ile artış beklemesi gerekebilir. IO azaltma oluşursa, Azure Veri Gölü Depolama Gen1 429'luk bir hata kodu döndürür ve ideal olarak uygun bir üstel geri dönüş ilkesiyle yeniden denenmelidir.

### <a name="optimize-writes-with-the-data-lake-storage-gen1-driver-buffer"></a>Data Lake Storage Gen1 sürücü arabelleği yle "yazma" optimize edin

Performansı optimize etmek ve Hadoop'tan Data Lake Storage Gen1'e yazarken IOPS'yi azaltmak için, Veri Gölü Depolama Gen1 sürücü arabelleği boyutuna mümkün olduğunca yakın yazma işlemleri gerçekleştirin. Apache Storm veya Spark akış iş yüklerini kullanarak akış yaparken olduğu gibi, yıkamadan önce arabellek boyutunu aşmamaya çalışın. HDInsight/Hadoop'tan Data Lake Storage Gen1'e yazarken, Data Lake Storage Gen1'in 4 MB arabelleğe sahip bir sürücüsü olduğunu bilmek önemlidir. Birçok dosya sistemi sürücüsü gibi, bu arabellek 4 MB boyutuna ulaşmadan önce el ile temizlenebilir. Değilse, bir sonraki yazı arabelleğenin maksimum boyutunu aşarsa hemen depolamaya atılır. Mümkün olduğunda, sayım veya zaman penceresine göre eşitleme/yıkama ilkesini eşitlerken arabelleğe bir taşmayı veya önemli bir alt çalışmadan kaçınmalısınız.

## <a name="resiliency-considerations"></a>Dayanıklılık konuları

Veri Gölü Depolama Gen1 veya herhangi bir bulut hizmeti yle bir sistem tasarlarken, kullanılabilirlik gereksinimlerinizi ve hizmetteki olası kesintilere nasıl yanıt verilebildiğinizi göz önünde bulundurmanız gerekir. Bir sorun belirli bir örneğe veya bölge genelinde yerelleştirilebilir, bu nedenle her ikisi için de bir plana sahip olmak önemlidir. Kurtarma süresi **hedefine** ve iş yükünüz için **kurtarma noktası hedefi** SLA'lara bağlı olarak, yüksek kullanılabilirlik ve olağanüstü durum kurtarma için az ya da çok agresif bir strateji seçebilirsiniz.

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) bazen birlikte kombine edilebilir, her biri biraz farklı bir strateji olmasına rağmen, özellikle veri söz konusu olduğunda. Veri Gölü Depolama Gen1 zaten yerelleştirilmiş donanım hatalarına karşı korumak için kaputun altında 3x çoğaltma işler. Ancak, bölgeler arasında çoğaltma yerleşik olmadığından, bunu kendiniz yönetmeniz gerekir. HA için bir plan yaparken, bir hizmet kesintisi durumunda iş yükü, yerel olarak veya yeni bir bölgede ayrı olarak çoğaltılan bir örne geçerek en son verilere mümkün olan en kısa sürede erişmeye ihtiyaç duyar.

Bir DR stratejisinde, bir bölgenin olası bir felaket başarısızlığı olayına hazırlanmak için, verilerin farklı bir bölgeye çoğaltılması da önemlidir. Bu veriler başlangıçta çoğaltılan HA verileriyle aynı olabilir. Ancak, geri düşmek için periyodik anlık görüntüler oluşturmak isteyebilirsiniz veri bozulması gibi kenar durumları için gereksinimlerinizi de göz önünde bulundurmanız gerekir. Verilerin önemine ve boyutuna bağlı olarak, risk toleranslarına göre yerel ve/veya ikincil depoda 1-, 6 ve 24 saatlik dönemlerin delta anlık görüntülerini göz önünde bulundurun.

Veri Gölü Depolama Gen1 ile veri esnekliği için, ideal olarak her saat, HA / DR gereksinimlerini karşılayan bir frekans ile ayrı bir bölgeye verilerinizi coğrafi çoğaltmak için tavsiye edilir. Bu çoğaltma sıklığı, ana sistem ve daha iyi bir kurtarma noktası hedefi (RPO) ile rekabet iş hacmi gereksinimlerine sahip olabilecek büyük veri hareketlerini en aza indirir. Ayrıca, uygulamanın Veri Gölü Depolama Gen1'i kullanarak, tetikleyicileri veya başarısız denemelerinin uzunluğunu izleme yoluyla ikincil hesaba otomatik olarak başarısız olması veya en azından el ile müdahale için yöneticilere bir bildirim göndermeyollarını göz önünde bulundurmalısınız. Bir hizmetin tekrar çevrimiçi olmasını beklemek yerine başarısız olmak gibi bir denge olduğunu unutmayın. Veriler çoğaltmayı tamamlamadıysa, bir hata, olası veri kaybına, tutarsızlığa veya verilerin karmaşık birşekilde birleştirilmesine neden olabilir.

Aşağıda, Veri Gölü Depolama Gen1 hesapları arasında çoğaltma yıcama düzenlemek için önerilen ilk üç seçenek ve bunların her biri arasındaki temel farklar yer almaktadır.

|  |Discp  |Azure Data Factory  |AdlCopy  |
|---------|---------|---------|---------|
|**Ölçek sınırları**     | İşçi düğümleri ile sınırlanmış        | Max Cloud Veri Hareketi birimleri ile sınırlıdır        | Analitik birimleri ile bağlı        |
|**Deltaların kopyalanmasına destek**     |   Evet      | Hayır         | Hayır         |
|**Dahili orkestrasyon**     |  Hayır (Oozie Airflow veya cron işleri kullanın)       | Evet        | Hayır (Azure Otomasyonu veya Windows Görev Zamanlayıcısı kullanın)         |
|**Desteklenen dosya sistemleri**     | ADL, HDFS, WASB, S3, GS, CFS        |Çok sayıda, [Connectors](../data-factory/connector-azure-blob-storage.md)bakın.         | ADL'den ADL'ye, WASB'den ADL'ye (yalnızca aynı bölge)        |
|**İşletim sistemi desteği**     |Hadoop çalışan herhangi bir işletim sistemi         | Yok          | Windows 10         |

### <a name="use-distcp-for-data-movement-between-two-locations"></a>İki konum arasındaki veri hareketi için Discp'yi kullanma

Dağıtılmış kopyanın kısaltması olan Distcp, Hadoop ile birlikte gelen ve iki konum arasında dağıtılmış veri hareketi sağlayan bir Linux komut satırı aracıdır. İki konum Data Lake Storage Gen1, HDFS, WASB veya S3 olabilir. Bu araç, tüm düğümleri ölçeklendirmek için Bir Hadoop kümesindeki (örneğin, HDInsight) MapReduce işlerini kullanır. Distcp özel ağ sıkıştırma cihazları olmadan büyük verileri taşımak için en hızlı yol olarak kabul edilir. Distcp ayrıca yalnızca iki konum arasındaki deltaları güncelleştirme seçeneği sunar, otomatik yeniden denemeleri ve bilgi işlemin dinamik ölçeklemeişlemlerini işler. Hive/Spark tabloları gibi tek bir dizinde çok büyük dosya bulunan ve yalnızca değiştirilen verileri kopyalamak isteyen şeyleri çoğaltma söz konusu olduğunda bu yaklaşım inanılmaz derecede etkilidir. Bu nedenlerden dolayı, Distcp büyük veri depoları arasında veri kopyalamak için en çok önerilen araçtır.

Kopyalama işleri, sıklık veya veri tetikleyicilerinin yanı sıra Linux cron işleri kullanarak Apache Oozie iş akışları tarafından tetiklenebilir. Yoğun çoğaltma işleri için, kopya işleri için özel olarak ayarlanabilen ve ölçeklendirilebilen ayrı bir HDInsight Hadoop kümesini döndürmesi önerilir. Bu, kopyalama işlerinin kritik işleri engellememesini sağlar. Çoğaltmayı yeterince geniş bir frekansta çalıştırıyorsanız, küme her iş arasında bile aşağı çekilebilir. İkincil bölgeye aktarılamaması durumunda, yeniden geldiğinde yeni verileri birincil Veri Gölü Depolama Gen1 hesabına çoğaltmak için ikincil bölgede başka bir kümenin de döndürüldeceğinden emin olun. Dscp kullanma örnekleri için, [Azure Depolama Blobs ve Veri Gölü Depolama Gen1 arasındaki verileri kopyalamak için Discp'yi kullanın'a](data-lake-store-copy-data-wasb-distcp.md)bakın.

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Kopyalama işlerini zamanlamak için Azure Veri Fabrikası'nı kullanma

Azure Veri Fabrikası, **Kopyalama Etkinliği**kullanarak kopyalama işlerini zamanlamak için de kullanılabilir ve hatta **Kopyalama Sihirbazı**aracılığıyla bir frekansta ayarlanabilir. Azure Veri Fabrikası'nın bulut veri hareketi birimlerinin (DSU) bir sınırı olduğunu ve sonunda büyük veri iş yükleri için iş ortası/işlem kısmını kapsadığını unutmayın. Ayrıca, Azure Veri Fabrikası şu anda Veri Gölü Depolama Gen1 hesapları arasında delta güncelleştirmeleri sunmuyor, bu nedenle Hive tabloları gibi klasörlerin çoğaltılması için tam bir kopya gerektireceği için. Veri Fabrikası ile kopyalama hakkında daha fazla bilgi için [Kopyalama Etkinliği aparat kılavuzuna](../data-factory/copy-activity-performance.md) bakın.

### <a name="adlcopy"></a>AdlCopy

AdlCopy, yalnızca aynı bölgede bulunan iki Veri Gölü Depolama Gen1 hesabı arasındaki verileri kopyalamanızı sağlayan bir Windows komut satırı aracıdır. AdlCopy aracı, kopya işinizi çalıştırmak için bağımsız bir seçenek veya bir Azure Veri Gölü Analizi hesabı kullanma seçeneği sağlar. Başlangıçta sağlam bir çoğaltma aksine isteğe bağlı kopyalar için inşa edilmiş olsa da, aynı bölge içinde Veri Gölü Depolama Gen1 hesapları arasında dağıtılmış kopyalama yapmak için başka bir seçenek sağlar. Güvenilirlik için, herhangi bir üretim iş yükü için premium Data Lake Analytics seçeneğini kullanmanız önerilir. Bağımsız sürüm meşgul yanıtları döndürebilir ve sınırlı ölçek ve izleme vardır.

Distcp gibi AdlCopy'nin de Azure Otomasyonu veya Windows Görev Zamanlayıcısı gibi bir şey tarafından düzenlenmesi gerekir. Data Factory'de olduğu gibi, AdlCopy yalnızca güncelleştirilen dosyaların kopyalanmasını desteklemez, ancak varolan dosyaların kopyalanmasını ve üzerine yazılır. AdlCopy'i kullanma yla ilgili daha fazla bilgi ve örnekler için azure [depolama blobs'undan Veri Gölü Depolama Gen1'e veri kopyalama bölümüne](data-lake-store-copy-data-azure-storage-blob.md)bakın.

## <a name="monitoring-considerations"></a>İzleme hususları

Data Lake Storage Gen1 ayrıntılı tanı günlükleri ve denetim sağlar. Veri Gölü Depolama Gen1, Veri Gölü Depolama Gen1 hesabı nın altındaki Azure portalında ve Azure Monitor'da bazı temel ölçümler sağlar. Veri Gölü Depolama Gen1 kullanılabilirliği Azure portalında görüntülenir. Ancak, bu metrik her yedi dakikada bir yenilenir ve genel olarak açıklanmış bir API aracılığıyla sorgulanamaz. Bir Veri Gölü Depolama Gen1 hesabının en güncel kullanılabilirliğini elde etmek için kullanılabilirliği doğrulamak için kendi sentetik testlerinizi çalıştırmanız gerekir. Toplam depolama alanı kullanımı, okuma/yazma istekleri ve giriş/çıkış gibi diğer ölçümlerin yenilenmesi 24 saat kadar sürebilir. Bu nedenle, hadoop komut satırı araçları veya günlük bilgilerini toplama yoluyla daha güncel ölçümler el ile hesaplanmalıdır. En son depolama kullanımını elde etmenin en hızlı yolu, bu HDFS komutunu hadoop küme düğümünden (örneğin, kafa düğümü) çalıştırmaktır:

    hdfs dfs -du -s -h adl://<adlsg1_account_name>.azuredatalakestore.net:443/

### <a name="export-data-lake-storage-gen1-diagnostics"></a>İhracat Veri Göl Depolama Gen1 tanılama

Data Lake Storage Gen1'den aranabilir günlüklere erişmenin en hızlı yollarından biri, Veri Gölü Depolama Gen1 hesabı için **Tanılama** bıçağı altında **Log Analytics'e** günlük gönderimi etkinleştirmektir. Bu, 15 dakikalık aralıklarla tetiklenen uyarı seçeneklerinin (e-posta/webhook) yanı sıra zaman ve içerik filtreleriyle gelen günlüklere anında erişim sağlar. Talimatlar için bkz: [Azure Veri Gölü Depolama Gen1 için tanı günlüklerine erişim.](data-lake-store-diagnostic-logs.md)

Günlüklerin nereye ineceğiniz konusunda daha fazla gerçek zamanlı uyarı ve daha fazla denetim için, bir kuyruğa gerçek zamanlı bildirimler göndermek için içeriğin tek tek veya bir zaman penceresi üzerinden analiz edilebildiği Azure EventHub'a günlükleri dışa aktarmayı düşünün. [Mantık Uygulaması](../connectors/connectors-create-api-azure-event-hubs.md) gibi ayrı bir uygulama daha sonra uyarıları tüketebilir ve ilgili kanala iletebilir ve NewRelic, Datadog veya AppDynamics gibi izleme araçlarına ölçümler gönderebilir. Alternatif olarak, ElasticSearch gibi bir üçüncü taraf aracı kullanıyorsanız, günlükleri Blob Depolama'ya dışa aktarabilir ve verileri Elasticsearch, Kibana ve Logstash (ELK) yığınınıza aktarmak için [Azure Logstash eklentisini](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob) kullanabilirsiniz.

### <a name="turn-on-debug-level-logging-in-hdinsight"></a>HDInsight'ta hata ayıklama düzeyinde günlük açmayı açma

Veri Gölü Depolama Gen1 günlük gönderimi açık değilse, Azure HDInsight, log4j üzerinden [Veri Gölü Depolama Gen1 için istemci tarafındaki günlüğü](data-lake-store-performance-tuning-mapreduce.md) açmanın bir yolunu da sağlar. **Ambari** > **YARN** > **Config** > Advanced**iplik-log4j yapılandırmalarında**aşağıdaki özelliği ayarlamanız gerekir:

    log4j.logger.com.microsoft.azure.datalake.store=DEBUG

Özellik ayarlandıktan ve düğümler yeniden başlatıldıktan sonra, Veri Gölü Depolama Gen1 tanılama düğümleri (/tmp/\<kullanıcı\>/iplik.log) üzerindeki İplik günlüklerine yazılır ve hata veya azaltma (HTTP 429 hata kodu) gibi önemli ayrıntılar izlenebilir. Aynı bilgiler Azure Monitor günlüklerinde veya günlüklerin Veri Gölü Depolama Gen1 hesabının [Tanılama](data-lake-store-diagnostic-logs.md) bıçağında gönderildiği her yerde de izlenebilir. En azından istemci tarafı günlüğünün açık olması veya operasyonel görünürlük ve daha kolay hata ayıklama için Data Lake Storage Gen1 ile günlük gönderim seçeneğini kullanmanız önerilir.

### <a name="run-synthetic-transactions"></a>Sentetik işlemleri çalıştırın

Şu anda, Azure portalındaki Veri Gölü Depolama Gen1 için hizmet kullanılabilirlik ölçümü 7 dakikalık yenileme penceresine sahiptir. Ayrıca, genel olarak açıklanmış bir API kullanılarak sorgulanamaz. Bu nedenle, veri gölü depolama gen1 için dakika kullanılabilirlik sağlayabilir sentetik işlemler yapan temel bir uygulama oluşturmak için tavsiye edilir. Veri Gölü Depolama Gen1'e karşı okuma, oluşturma ve güncelleştirme gerçekleştirmek ve sonuçları izleme çözümünüze göndermek için bir WebJob, Logic App veya Azure İşi Uygulaması oluşturmak bir örnek olabilir. İşlemler geçici bir klasörde yapılabilir ve gereksinimlere bağlı olarak her 30-60 saniyede bir çalıştırılabilecek testten sonra silinebilir.

## <a name="directory-layout-considerations"></a>Dizin düzeni hususları

Verileri bir veri gölüne indirerken, güvenliğin, bölümlemenin ve işlemenin etkin bir şekilde kullanılabileceğini bilmek için verilerin yapısını önceden planlamak önemlidir. Aşağıdaki önerilerin çoğu, Azure Veri Gölü Depolama Gen1, Blob Depolama veya HDFS ile birlikte kullanılabilir. Her iş yükü, verilerin nasıl tüketildigine dair farkli gereksinimlere sahiptir, ancak aşağıda IoT ve toplu iş senaryolarıyla çalışırken göz önünde bulundurulması gereken bazi ortak mizanpajlar yer almaktadır.

### <a name="iot-structure"></a>IoT yapısı

IoT iş yüklerinde, çok sayıda ürün, cihaz, kuruluş ve müşteriye yayılan veri deposuna büyük miktarda veri indirilebilir. Akış aşağı tüketiciler için verilerin organizasyon, güvenlik ve verimli bir şekilde işlenmesi için dizin düzenini önceden planlamak önemlidir. Göz önünde bulundurulması gereken genel bir şablon aşağıdaki düzen olabilir:

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/

Örneğin, İngiltere'de bir uçak motoru için iniş telemetri aşağıdaki yapı gibi görünebilir:

    UK/Planes/BA1293/Engine1/2017/08/11/12/

Tarihi klasör yapısının sonuna koymak için önemli bir neden vardır. Belirli bölgeleri kilitlemek veya konuları kullanıcılara/gruplara vermek istiyorsanız, posix izinleriyle bunu kolayca yapabilirsiniz. Aksi takdirde, belirli bir güvenlik grubunun yalnızca Birleşik Krallık verilerini veya belirli düzlemlerini görüntülemesini kısıtlamak gerekirse, önündeki tarih yapısı yla her saat klasörü altında çok sayıda klasör için ayrı bir izin gerekir. Ayrıca, tarih yapısının önünde olması, zaman geçtikçe klasör sayısını katlanarak artırır.

### <a name="batch-jobs-structure"></a>Toplu iş yapısı

Üst düzey bir yaklaşımdan, toplu iş işlemede yaygın olarak kullanılan bir yaklaşım, verileri "içeri" klasörüne yerlebir etmektir. Ardından, veriler işlendikten sonra, yeni verileri alt akış işlemlerinin tüketilmesi için bir "çıkış" klasörüne koyun. Bu dizin yapısı bazen tek tek dosyalarüzerinde işlenmesi gereken işler için görülür ve büyük veri kümeleri üzerinde büyük ölçüde paralel işleme gerektirmeyebilir. Yukarıda önerilen IoT yapısı gibi, iyi bir dizin yapısı bölge ve konu konuları (örneğin, organizasyon, ürün/üretici) gibi şeyler için üst düzey klasörlere sahiptir. Bu yapı, kuruluşunuz genelinde verilerin güvenliğinin sağlanmasına ve iş yüklerinizdeki verilerin daha iyi yönetilmesine yardımcı olur. Ayrıca, işleme de daha iyi organizasyon, filtrelenmiş aramalar, güvenlik ve otomasyon sağlamak için yapıdaki tarih ve saati düşünün. Tarih yapısı için parçalılık düzeyi, verilerin saatlik, günlük ve hatta aylık gibi yüklendiği veya işlendiği aralıkla belirlenir.

Bazen veri bozulması veya beklenmeyen biçimler nedeniyle dosya işleme başarısız olabilir. Bu gibi durumlarda, dizin yapısı daha fazla inceleme için dosyaları taşımak için bir **/ kötü** klasör yararlanabilir. Toplu iş, bu *kötü* dosyaların el ile müdahale için raporlanmasını veya bildirimini de işleyebilir. Aşağıdaki şablon yapısını göz önünde bulundurun:

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/

Örneğin, bir pazarlama firması Kuzey Amerika'daki müşterilerinden müşteri güncelleştirmelerinin günlük veri özleri alır. İşlemedilmeden önce ve sonra aşağıdaki parçacık gibi görünebilir:

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv

Toplu iş verilerinin doğrudan Hive veya geleneksel SQL veritabanları gibi veritabanlarına işlenmesi durumunda, çıktı zaten Hive tablosu veya dış veritabanı için ayrı bir klasöre gittiğinden, **/giriş** **veya/çıkış** klasörüne gerek yoktur. Örneğin, müşterilerden gelen günlük ekstreler kendi klasörlerine iner ve Azure Veri Fabrikası, Apache Oozie veya Apache Airflow gibi bir şey tarafından düzenlenmesi, verileri işlemek ve bir Kovan tablosuna yazmak için günlük hive veya Spark işini tetikler.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
* [Azure Veri Gölü Depolama Gen1'de Erişim Denetimi](data-lake-store-access-control.md)
* [Azure Veri Gölü Depolama Gen1'de Güvenlik](data-lake-store-security-overview.md)
* [Performans için Azure Veri Gölü Depolama Gen1'i ala](data-lake-store-performance-tuning-guidance.md)
* [Azure Veri Gölü Depolama Gen1 ile HDInsight Spark'ı kullanmak için performans alamı kılavuzu](data-lake-store-performance-tuning-spark.md)
* [Azure Veri Gölü Depolama Gen1 ile HDInsight Hive'ı kullanmak için performans alamı kılavuzu](data-lake-store-performance-tuning-hive.md)
* [Data Lake Storage Gen1 ile HDInsight kümeleri oluşturun](data-lake-store-hdinsight-hadoop-use-portal.md)
