---
title: Azure Veri Gölü Depolama Gen2'yi kullanmak için en iyi uygulamalar | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen2'yi (önceden Azure Veri Gölü Deposu olarak bilinir) kullanmayla ilgili veri alımı, tarih güvenliği ve performans la ilgili en iyi uygulamaları öğrenin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: sachins
ms.openlocfilehash: ac4e126c7ecbd1fc781db74e5b19635b273bbb34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72299664"
---
# <a name="best-practices-for-using-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'yi kullanmak için en iyi uygulamalar

Bu makalede, Azure Veri Gölü Depolama Gen2 ile çalışmak için en iyi uygulamalar ve dikkat edilmesi gerekenler hakkında bilgi edinin. Bu makalede, Veri Gölü Depolama Gen2 için güvenlik, performans, esneklik ve izleme hakkında bilgi sağlar. Data Lake Storage Gen2'den önce, Azure HDInsight gibi hizmetlerde gerçekten büyük verilerle çalışmak karmaşıktı. Petabayt depolama ve bu ölçekte en iyi performans elde edilebilmek için birden çok Blob depolama hesabında veri parçaları bulmak zorunda kaldınız. Data Lake Storage Gen2, 5 TB'a kadar tek tek dosya boyutlarını destekler ve performans için sabit sınırların çoğu kaldırılmıştır. Ancak, veri gölü depolama gen2 ile en iyi performansı elde edebilirsiniz, böylece bu makalede kapsadığı bazı hususlar hala vardır.

## <a name="security-considerations"></a>Güvenlik konuları

Azure Veri Gölü Depolama Gen2, Azure Etkin Dizin (Azure AD) kullanıcıları, grupları ve hizmet ilkeleri için POSIX erişim denetimleri sunar. Bu erişim denetimleri varolan dosyalara ve dizinlere ayarlanabilir. Erişim denetimleri, yeni dosyalara veya dizinlere otomatik olarak uygulanabilen varsayılan izinler oluşturmak için de kullanılabilir. Veri Gölü Depolama Gen2 ALA'ları hakkında daha fazla bilgi [Azure Veri Gölü Depolama Gen2'de Access denetiminde](storage-data-lake-storage-access-control.md)mevcuttur.

### <a name="use-security-groups-versus-individual-users"></a>Güvenlik gruplarını tek tek kullanıcılara karşı kullanma

Veri Gölü Depolama Gen2'de büyük verilerle çalışırken, Azure HDInsight gibi hizmetlerin verilerle çalışmasına izin vermek için bir hizmet ilkesi kullanılmış olabilir. Ancak, tek tek kullanıcıların verilere erişmeye ihtiyaç duyduğu durumlar da olabilir. Her durumda, tek tek kullanıcıları dizinlere ve dosyalara atamak yerine Azure Etkin Dizin [güvenlik gruplarını](../common/storage-auth-aad.md) kullanmayı güçlü bir şekilde düşünün.

Bir güvenlik grubuna izin ler atandıktan sonra, kullanıcıekleme veya gruptan çıkarma, Veri Gölü Depolama Gen2 için herhangi bir güncelleştirme gerektirmez. Bu, erişim denetim listesi (ACL) başına maksimum erişim denetimi giriş sayısını aşmamanıza da yardımcı olur. Şu anda, bu sayı 32'dir (her zaman her dosya ve dizinle ilişkili dört POSIX tarzı APC dahil): sahibi kullanıcı, sahip olan grup, maske ve diğer. Her dizin, toplam 64 erişim denetimi girişi için iki tür ACL'ye, erişim ACL'si ve varsayılan ACL'ye sahip olabilir. Bu ACD'ler hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](data-lake-storage-access-control.md)bakın.

### <a name="security-for-groups"></a>Gruplar için güvenlik

Siz veya kullanıcılarınız hiyerarşik ad alanı etkinleştirilmiş bir depolama hesabındaki verilere erişmeye ihtiyaç duyduğunuzda, en iyisi Azure Etkin Dizin güvenlik gruplarını kullanmaktır. Bazı önerilen gruplar ile başlamak için **ReadOnlyUsers**olabilir , **WriteAccessUsers**, ve **FullAccessUsers** konteyner kökü için, ve hatta anahtar alt dizinleri için ayrı olanlar. Daha sonra eklenebilecek, ancak henüz tanımlanmamış başka beklenen kullanıcı grupları varsa, belirli klasörlere erişimi olan sahte güvenlik grupları oluşturmayı düşünebilirsiniz. Güvenlik grubunu kullanmak, binlerce dosyaya yeni izinler atarken uzun işlem sürelerinden kaçınmanızı sağlar.

### <a name="security-for-service-principals"></a>Hizmet müdürleri için güvenlik

Azure Etkin Dizin hizmet ilkeleri genellikle Veri Gölü Depolama Gen2'deki verilere erişmek için Azure Databricks gibi hizmetler tarafından kullanılır. Birçok müşteri için, tek bir Azure Etkin Dizin hizmet yöneticisi yeterli olabilir ve Veri Gölü Depolama Gen2 kapsayıcısının kökünde tam izinlere sahip olabilir. Diğer müşteriler, bir kümenin verilere tam erişimi olan farklı hizmet ilkelerine sahip birden çok küme ve yalnızca okuma erişimi olan başka bir küme gerektirebilir. 

### <a name="enable-the-data-lake-storage-gen2-firewall-with-azure-service-access"></a>Azure hizmet erişimiyle Veri Gölü Depolama Gen2 güvenlik duvarını etkinleştirme

Data Lake Storage Gen2, harici saldırıların vektörünün sınırlandırılması için önerilen bir güvenlik duvarını açma ve yalnızca Azure hizmetlerine erişimi sınırlama seçeneğini destekler. Güvenlik Duvarı, **Güvenlik Duvarı** > **Etkinleştir (ON)** > Azure hizmetlerine**erişime izin ver** aracılığıyla Azure portalındaki bir depolama hesabında etkinleştirilebilir.

Azure Databricks'ten depolama hesabınıza erişmek için Azure Databricks'i sanal ağınıza dağıtın ve ardından bu sanal ağı güvenlik duvarınıza ekleyin. Bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma.](https://docs.microsoft.com/azure/storage/common/storage-network-security)

## <a name="resiliency-considerations"></a>Dayanıklılık konuları

Veri Gölü Depolama Gen2 veya herhangi bir bulut hizmeti ile bir sistem tasarlarken, kullanılabilirlik gereksinimlerinizi ve hizmetteki olası kesintilere nasıl yanıt verilebildiğinizi göz önünde bulundurmanız gerekir. Bir sorun belirli bir örneğe veya bölge genelinde yerelleştirilebilir, bu nedenle her ikisi için de bir plana sahip olmak önemlidir. Kurtarma süresi hedefine ve iş yükünüz için kurtarma noktası hedefi SLA'lara bağlı olarak, yüksek kullanılabilirlik ve olağanüstü durum kurtarma için az ya da çok agresif bir strateji seçebilirsiniz.

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) bazen birlikte kombine edilebilir, her biri biraz farklı bir strateji olmasına rağmen, özellikle veri söz konusu olduğunda. Data Lake Storage Gen2, yerelleştirilmiş donanım hatalarına karşı korunmak için kaputun altında 3x çoğaltma işlemini zaten işler. Ayrıca, ZRS veya GZRS (önizleme) gibi diğer çoğaltma seçenekleri HA'yı geliştirirken, GRS & RA-GRS DR'yi geliştirir. HA için bir plan yaparken, bir hizmet kesintisi durumunda iş yükü, yerel olarak veya yeni bir bölgede ayrı olarak çoğaltılan bir örne geçerek en son verilere mümkün olan en kısa sürede erişmeye ihtiyaç duyar.

Bir DR stratejisinde, bir bölgenin olası bir hata olasılığına hazırlanmak için, verilerin GRS veya RA-GRS çoğaltma sını kullanarak farklı bir bölgeye çoğaltılması da önemlidir. Ayrıca, geri düşmek için periyodik anlık görüntüler oluşturmak isteyebilirsiniz veri bozulması gibi kenar durumlar için gereksinimlerinizi göz önünde bulundurmanız gerekir. Verilerin önemine ve boyutuna bağlı olarak, risk toleranslarına göre 1-, 6 ve 24 saatlik periyotların delta anlık görüntülerini göz önünde bulundurun.

Data Lake Storage Gen2 ile veri esnekliği için, HA/DR gereksinimlerinizi karşılayan GRS veya RA-GRS ile verilerinizi coğrafi olarak çoğaltmanız önerilir. Ayrıca, uygulamanın Veri Gölü Depolama Gen2'yi kullanarak, tetikleyicileri veya başarısız denemelerinin uzunluğunu izleme yoluyla ikincil bölgeye otomatik olarak başarısız olması nın veya en azından el ile müdahale için yöneticilere bildirim göndermenin yollarını göz önünde bulundurmalısınız. Bir hizmetin tekrar çevrimiçi olmasını beklemek yerine başarısız olmak gibi bir denge olduğunu unutmayın.

### <a name="use-distcp-for-data-movement-between-two-locations"></a>İki konum arasındaki veri hareketi için Discp'yi kullanma

Dağıtılmış kopyanın kısaltması olan DistCp, Hadoop ile birlikte gelen ve iki konum arasında dağıtılmış veri hareketi sağlayan bir Linux komut satırı aracıdır. İki konum Data Lake Storage Gen2, HDFS veya S3 olabilir. Bu araç, tüm düğümleri ölçeklendirmek için Bir Hadoop kümesindeki (örneğin, HDInsight) MapReduce işlerini kullanır. Distcp özel ağ sıkıştırma cihazları olmadan büyük verileri taşımak için en hızlı yol olarak kabul edilir. Distcp ayrıca yalnızca iki konum arasındaki deltaları güncelleştirme seçeneği sunar, otomatik yeniden denemeleri ve bilgi işlemin dinamik ölçeklemeişlemlerini işler. Hive/Spark tabloları gibi tek bir dizinde çok büyük dosya bulunan ve yalnızca değiştirilen verileri kopyalamak isteyen şeyleri çoğaltma söz konusu olduğunda bu yaklaşım inanılmaz derecede etkilidir. Bu nedenlerden dolayı, Distcp büyük veri depoları arasında veri kopyalamak için en çok önerilen araçtır.

Kopyalama işleri, sıklık veya veri tetikleyicilerinin yanı sıra Linux cron işleri kullanarak Apache Oozie iş akışları tarafından tetiklenebilir. Yoğun çoğaltma işleri için, kopya işleri için özel olarak ayarlanabilen ve ölçeklendirilebilen ayrı bir HDInsight Hadoop kümesini döndürmesi önerilir. Bu, kopyalama işlerinin kritik işleri engellememesini sağlar. Çoğaltmayı yeterince geniş bir frekansta çalıştırıyorsanız, küme her iş arasında bile aşağı çekilebilir. İkincil bölgeye aktarılamaması durumunda, yeniden geldiğinde yeni verileri birincil Veri Gölü Depolama Gen2 hesabına çoğaltmak için ikincil bölgede başka bir kümenin de döndürüldeceğinden emin olun. Dscp kullanma örnekleri için, [Azure Depolama Blobs ve Veri Gölü Depolama Gen2 arasındaki verileri kopyalamak için Discp'yi kullanın'a](../blobs/data-lake-storage-use-distcp.md)bakın.

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Kopyalama işlerini zamanlamak için Azure Veri Fabrikası'nı kullanma

Azure Veri Fabrikası, Kopyalama Etkinliği kullanarak kopyalama işlerini zamanlamak için de kullanılabilir ve hatta Kopyalama Sihirbazı aracılığıyla bir frekansta ayarlanabilir. Azure Veri Fabrikası'nın bulut veri hareketi birimlerinin (DSU) bir sınırı olduğunu ve sonunda büyük veri iş yükleri için iş ortası/işlem kısmını kapsadığını unutmayın. Ayrıca, Azure Veri Fabrikası şu anda Veri Gölü Depolama Gen2 hesapları arasında delta güncelleştirmeleri sunmuyor, bu nedenle Hive tabloları gibi dizinler çoğaltmak için tam bir kopya gerektirecektir. Veri Fabrikası ile kopyalama hakkında daha fazla bilgi için [veri fabrikası makalesine](../../data-factory/load-azure-data-lake-storage-gen2.md) bakın.

## <a name="monitoring-considerations"></a>İzleme hususları

Data Lake Storage Gen2, Veri Gölü Depolama Gen2 hesabı nın altındaki Azure portalında ve Azure Monitor'da ölçümler sağlar. Veri Gölü Depolama Gen2 kullanılabilirliği Azure portalında görüntülenir. Bir Veri Gölü Depolama Gen2 hesabının en güncel kullanılabilirliğini elde etmek için kullanılabilirliği doğrulamak için kendi sentetik testlerinizi çalıştırmanız gerekir. Toplam depolama kullanımı, okuma/yazma istekleri ve giriş/çıkış gibi diğer ölçümler, uygulamaların izlenmesi yle kullanılabilir ve eşikler (örneğin, ortalama gecikme süresi veya dakika başına hata sayısı# gibi) aşıldığında uyarıları tetikleyebilir.

## <a name="directory-layout-considerations"></a>Dizin düzeni hususları

Verileri bir veri gölüne indirerken, güvenliğin, bölümlemenin ve işlemenin etkin bir şekilde kullanılabileceğini bilmek için verilerin yapısını önceden planlamak önemlidir. Aşağıdaki önerilerin çoğu tüm büyük veri iş yükleri için geçerlidir. Her iş yükü, verilerin nasıl tüketildigine dair farkli gereksinimlere sahiptir, ancak aşağıda IoT ve toplu iş senaryolarıyla çalışırken göz önünde bulundurulması gereken bazi ortak mizanpajlar yer almaktadır.

### <a name="iot-structure"></a>IoT yapısı

IoT iş yüklerinde, çok sayıda ürün, cihaz, kuruluş ve müşteriye yayılan veri deposuna büyük miktarda veri indirilebilir. Akış aşağı tüketiciler için verilerin organizasyon, güvenlik ve verimli bir şekilde işlenmesi için dizin düzenini önceden planlamak önemlidir. Göz önünde bulundurulması gereken genel bir şablon aşağıdaki düzen olabilir:

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/

Örneğin, İngiltere'de bir uçak motoru için iniş telemetri aşağıdaki yapı gibi görünebilir:

    UK/Planes/BA1293/Engine1/2017/08/11/12/

Tarihi dizin yapısının sonuna koymak için önemli bir neden var. Belirli bölgeleri kilitlemek veya konuları kullanıcılara/gruplara vermek istiyorsanız, posix izinleriyle bunu kolayca yapabilirsiniz. Aksi takdirde, belirli bir güvenlik grubunun yalnızca Birleşik Krallık verilerini veya belirli düzlemlerini görüntülemesini kısıtlamak gerekirse, önündeki tarih yapısı yla her saat dizininin altındaki çok sayıda dizin için ayrı bir izin gerekir. Ayrıca, önde tarih yapısına sahip olmak, zaman geçtikçe dizin sayısını katlanarak artıracaktır.

### <a name="batch-jobs-structure"></a>Toplu iş yapısı

Üst düzey bir yaklaşımdan, toplu işlemede yaygın olarak kullanılan bir yaklaşım, verileri "in" dizinine yerhaline getirmektir. Ardından, veriler işlendikten sonra, yeni verileri alt akış işlemlerinin tüketilmesi için bir "çıkış" dizinine koyun. Bu dizin yapısı bazen tek tek dosyalarüzerinde işlenmesi gereken işler için görülür ve büyük veri kümeleri üzerinde büyük ölçüde paralel işleme gerektirmeyebilir. Yukarıda önerilen IoT yapısı gibi, iyi bir dizin yapısı bölge ve konu konuları (örneğin, organizasyon, ürün/üretici) gibi şeyler için üst düzey dizinlere sahiptir. Bu yapı, kuruluşunuz genelinde verilerin güvenliğinin sağlanmasına ve iş yüklerinizdeki verilerin daha iyi yönetilmesine yardımcı olur. Ayrıca, işleme de daha iyi organizasyon, filtrelenmiş aramalar, güvenlik ve otomasyon sağlamak için yapıdaki tarih ve saati düşünün. Tarih yapısı için parçalılık düzeyi, verilerin saatlik, günlük ve hatta aylık gibi yüklendiği veya işlendiği aralıkla belirlenir.

Bazen veri bozulması veya beklenmeyen biçimler nedeniyle dosya işleme başarısız olabilir. Bu gibi durumlarda, dizin yapısı daha fazla inceleme için dosyaları taşımak için bir **/ kötü** klasör yararlanabilir. Toplu iş, bu *kötü* dosyaların el ile müdahale için raporlanmasını veya bildirimini de işleyebilir. Aşağıdaki şablon yapısını göz önünde bulundurun:

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/

Örneğin, bir pazarlama firması Kuzey Amerika'daki müşterilerinden müşteri güncelleştirmelerinin günlük veri özleri alır. İşlemedilmeden önce ve sonra aşağıdaki parçacık gibi görünebilir:

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv

Toplu iş verilerinin doğrudan Hive veya geleneksel SQL veritabanları gibi veritabanlarına işlenmesi durumunda, çıktı zaten Hive tablosu veya dış veritabanı için ayrı bir klasöre gittiğinden, **/giriş** **veya/çıkış** klasörüne gerek yoktur. Örneğin, müşterilerden gelen günlük ekstreler kendi klasörlerine iner ve Azure Veri Fabrikası, Apache Oozie veya Apache Airflow gibi bir şey tarafından düzenlenmesi, verileri işlemek ve bir Kovan tablosuna yazmak için günlük hive veya Spark işini tetikler.
