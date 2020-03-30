---
title: Blob depolama için performans ve ölçeklenebilirlik denetim listesi - Azure Depolama
description: Yüksek performanslı uygulamalar geliştirmede Blob depolama ile kullanılmak üzere kanıtlanmış uygulamaların bir kontrol listesi.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: e4103f8360f6fa80470b0f8002a61f8ac903bd8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255437"
---
# <a name="performance-and-scalability-checklist-for-blob-storage"></a>Blob depolama için performans ve ölçeklenebilirlik kontrol listesi

Microsoft, Blob depolama ile yüksek performanslı uygulamalar geliştirmek için kanıtlanmış bir dizi uygulama geliştirmiştir. Bu denetim listesi, geliştiricilerin performansı en iyi duruma getirmek için izleyebilirsiniz önemli uygulamaları tanımlar. Uygulamanızı tasarlarken ve süreç boyunca bu uygulamaları aklınızda bulundurun.

Azure Depolama kapasite, işlem hızı ve bant genişliği için ölçeklenebilirlik ve performans hedefleri vardır. Azure Depolama ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için, [standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ve [Blob depolama için ölçeklenebilirlik ve performans hedefleri](scalability-targets.md)ne bakın.

## <a name="checklist"></a>Denetim Listesi

Bu makalede, Blob depolama uygulamanızı geliştirirken izleyebileceğiniz bir denetim listesinde performans için kanıtlanmış uygulamaları düzenler.

| Bitti | Kategori | Tasarım değerlendirmesi |
| --- | --- | --- |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Uygulamanızı maksimum depolama hesabı sayısından daha fazla kullanacak şekilde tasarlayabilir misiniz?](#maximum-number-of-storage-accounts) |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Yaklaşan kapasite ve işlem limitlerinden kaçınıyor musunuz?](#capacity-and-transaction-targets) |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Çok sayıda istemci aynı anda tek bir blob'a erişiyor mu?](#multiple-clients-accessing-a-single-blob-concurrently) |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Uygulamanız tek bir blob için ölçeklenebilirlik hedefleri içinde kalıyor mu?](#bandwidth-and-operations-per-blob) |
| &nbsp; |Bölümleme |[Adlandırma kuralınız daha iyi yük dengelemesi sağlamak için tasarlandı mı?](#partitioning) |
| &nbsp; |Ağ Oluşturma |[İstemci tarafındaki aygıtlar, gereken performansı elde etmek için yeterince yüksek bant genişliğine ve düşük gecikme adabına sahip mi?](#throughput) |
| &nbsp; |Ağ Oluşturma |[İstemci tarafındaki aygıtların yüksek kaliteli bir ağ bağlantısı var mı?](#link-quality) |
| &nbsp; |Ağ Oluşturma |[İstemci uygulaması depolama hesabıyla aynı bölgede mi?](#location) |
| &nbsp; |Doğrudan istemci erişimi |[Azure Depolama'ya doğrudan erişimi etkinleştirmek için paylaşılan erişim imzalarını (SAS) ve orijinler arası kaynak paylaşımını (CORS) mi kullanıyorsunuz?](#sas-and-cors) |
| &nbsp; |Önbelleğe alma |[Uygulamanız sık erişilen ve nadiren değiştirilen verileri önbelleğe mi alıç?](#reading-data) |
| &nbsp; |Önbelleğe alma |[Uygulamanız güncelleştirmeleri istemciye önbelleğe alıp daha büyük kümeler halinde yükleyerek toplu hale mi getiriyor?](#uploading-data-in-batches) |
| &nbsp; |.NET yapılandırması |[Optimum performans için .NET Core 2.1 veya daha sonra kullanıyor musunuz?](#use-net-core) |
| &nbsp; |.NET yapılandırması |[İstemcinizi yeterli sayıda eşzamanlı bağlantı kullanacak şekilde yapılandırıldınız mı?](#increase-default-connection-limit) |
| &nbsp; |.NET yapılandırması |[.NET uygulamaları için yeterli sayıda iş parçacığı kullanacak şekilde .NET'i yapılandırdın mı?](#increase-minimum-number-of-threads) |
| &nbsp; |Parallelism |[Paralelliğin, müşterinizin yeteneklerini aşırı yüklememeniz veya ölçeklenebilirlik hedeflerine yaklaşmamanız için uygun şekilde sınırlandırılmanızı sağladınız mı?](#unbounded-parallelism) |
| &nbsp; |Araçlar |[Microsoft tarafından sağlanan istemci kitaplıklarının ve araçlarının en son sürümlerini mi kullanıyorsunuz?](#client-libraries-and-tools) |
| &nbsp; |Yeniden deneme sayısı |[Hataları ve zaman ekmelerini azaltma için üstel geri leme içeren bir yeniden deneme ilkesi mi kullanıyorsunuz?](#timeout-and-server-busy-errors) |
| &nbsp; |Yeniden deneme sayısı |[Uygulamanız yeniden denemeyen hatalar için yeniden denemelerden kaçınıyor mu?](#non-retryable-errors) |
| &nbsp; |Lekeleri kopyalama |[Lekeleri en verimli şekilde mi kopyalıyorsun?](#blob-copy-apis) |
| &nbsp; |Lekeleri kopyalama |[Toplu kopyalama işlemleri için AzCopy'nin en son sürümünü mü kullanıyorsunuz?](#use-azcopy) |
| &nbsp; |Lekeleri kopyalama |[Büyük hacimli veri almak için Azure Veri Kutusu ailesini mi kullanıyorsunuz?](#use-azure-data-box) |
| &nbsp; |İçerik dağıtımı |[İçerik dağıtımı için CDN mi kullanıyorsunuz?](#content-distribution) |
| &nbsp; |Meta verileri kullanma |[Sık kullanılan meta verileri meta verilerinde lekeler hakkında mı depolayabiliyorsunuz?](#use-metadata) |
| &nbsp; |Hızlı yükleme |[Hızlı bir şekilde bir blob yüklemeye çalışırken, paralel blokları yükleme mi?](#upload-one-large-blob-quickly) |
| &nbsp; |Hızlı yükleme |[Hızlı bir şekilde birçok blobs yüklemeye çalışırken, paralel lekeler yükleme mi?](#upload-many-blobs-quickly) |
| &nbsp; |Blob türü |[Uygun olduğunda sayfa lekeleri mi kullanıyorsunuz yoksa blobs engelliyor musunuz?](#choose-the-correct-type-of-blob) |

## <a name="scalability-targets"></a>Ölçeklenebilirlik hedefleri

Uygulamanız ölçeklenebilirlik hedeflerinden herhangi birini yaklaşır veya aşarsa, işlem gecikmesi veya azaltma da artabilir. Azure Depolama uygulamanızı daralttığında, hizmet 503 (Sunucu meşgul) veya 500 (İşlem zaman) hata kodu döndürmeye başlar. Ölçeklenebilirlik hedeflerinin sınırları içinde kalarak bu hatalardan kaçınmak, uygulamanızın performansını artırmanın önemli bir parçasıdır.

Kuyruk hizmetinin ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için Azure [Depolama ölçeklenebilirliği ve performans hedefleri'ne](/azure/storage/queues/scalability-targets#scale-targets-for-queue-storage)bakın.

### <a name="maximum-number-of-storage-accounts"></a>Maksimum depolama hesabı sayısı

Belirli bir abonelik/bölge birleşimi için izin verilen maksimum depolama hesabı sayısına yaklaşıyorsanız, senaryonuzu değerlendirin ve aşağıdaki koşullardan herhangi birinin geçerli olup olmadığını belirleyin:

- Yönetilmeyen diskleri depolamak ve bu diskleri sanal makinelerinize (VM) eklemek için depolama hesapları mı kullanıyorsunuz? Bu senaryo için Microsoft yönetilen diskler kullanmanızı önerir. Yönetilen diskler sizin için otomatik olarak ve tek tek depolama hesapları oluşturma ve yönetmeye gerek kalmadan ölçeklendirilir. Daha fazla bilgi için [bkz.](../../virtual-machines/windows/managed-disks-overview.md)
- Veri yalıtımı amacıyla müşteri başına bir depolama hesabı mı kullanıyorsunuz? Bu senaryo için Microsoft, tüm depolama hesabı yerine her müşteri için bir blob kapsayıcısı kullanılmasını önerir. Azure Depolama artık kapsayıcı başına rol tabanlı erişim denetimi (RBAC) rolleri atamanıza olanak tanır. Daha fazla bilgi için, [Azure portalında RBAC ile Azure blob ve kuyruk verilerine Erişim Izni'ne](../common/storage-auth-aad-rbac-portal.md)bakın.
- Girişi, çıkış, saniyede G/Ç işlemlerini (IOPS) veya kapasiteyi artırmak için birden fazla depolama hesabı mı kullanıyorsunuz? Bu senaryoda Microsoft, mümkünse iş yükünüz için gereken depolama hesabı sayısını azaltmak için depolama hesapları için artırılmış sınırlardan yararlanmanızı önerir. Depolama hesabınız için daha fazla sınır istemek için [Azure Desteği'ne](https://azure.microsoft.com/support/options/) başvurun. Daha fazla bilgi için bkz: [Daha büyük, daha yüksek ölçekli depolama hesaplarını duyur.](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/)

### <a name="capacity-and-transaction-targets"></a>Kapasite ve işlem hedefleri

Uygulamanız tek bir depolama hesabı için ölçeklenebilirlik hedeflerine yaklaşıyorsa, aşağıdaki yaklaşımlardan birini benimsemeyi düşünün:  

- Uygulamanız hareket hedefine uarsa, yüksek işlem oranları ve düşük ve tutarlı gecikme süresi için optimize edilmiş blok blob depolama hesaplarını kullanmayı düşünün. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).
- Uygulamanızın ölçeklenebilirlik hedefine yaklaşmasına veya aşılmasına neden olan iş yükünü yeniden gözden geçirin. Daha az bant genişliği veya kapasite veya daha az işlem kullanmak için farklı tasarlayabilir misiniz?
- Uygulamanız ölçeklenebilirlik hedeflerinden birini aşması gerekiyorsa, birden çok depolama hesabı oluşturun ve uygulama verilerinizi bu birden çok depolama hesabı arasında bölün. Bu deseni kullanıyorsanız, gelecekte yük dengeleme için daha fazla depolama hesabı ekleyebilmeniz için uygulamanızı tasarladığınızdan emin olun. Depolama hesaplarının, depolanan veriler, yapılan işlemler veya aktarılan veriler açısından kullanımınızdan başka bir maliyeti yoktur.
- Uygulamanız bant genişliği hedeflerine yaklaşıyorsa, verileri Azure Depolama'ya göndermek için gereken bant genişliğini azaltmak için istemci tarafında ki verileri sıkıştırmayı düşünün.
    Verileri sıkıştırmak bant genişliğini kaydedebilir ve ağ performansını artırabilir, ancak performans üzerinde olumsuz etkileri de olabilir. Müşteri tarafında veri sıkıştırma ve dekompresyon için ek işleme gereksinimlerinin performans etkisini değerlendirin. Sıkıştırılmış verileri depolamanın sorun gidermeyi zorlaştırabileceğini unutmayın, çünkü verileri standart araçları kullanarak görüntülemek daha zor olabilir.
- Uygulamanız ölçeklenebilirlik hedeflerine yaklaşıyorsa, yeniden denemeler için üstel bir geri tepme kullandığınızdan emin olun. Bu makalede açıklanan önerileri uygulayarak ölçeklenebilirlik hedeflerine ulaşmaktan kaçınmak en iyisidir. Ancak, yeniden denemeler için üstel bir geri dönüş kullanmak, uygulamanızın hızla yeniden denemesini önler ve bu da azaltmayı daha kötü hale getirebilir. Daha fazla bilgi için [Zaman Önce ve Sunucu Meşgul hataları](#timeout-and-server-busy-errors)başlıklı bölüme bakın.

### <a name="multiple-clients-accessing-a-single-blob-concurrently"></a>Aynı anda tek bir blob erişen birden çok istemci

Aynı anda tek bir blob erişen istemcilerin çok sayıda varsa, hem blob başına ve depolama hesabı ölçeklenebilirlik hedefleri başına dikkate almanız gerekir. Tek bir blob erişebilen istemcilerin tam sayısı aynı anda blob isteyen istemcilerin sayısı, blob boyutu ve ağ koşulları gibi faktörlere bağlı olarak değişir.

Blob, bir web sitesinden sunulan resimler veya videolar gibi bir CDN aracılığıyla dağıtılabiliyorsa, CDN kullanabilirsiniz. Daha fazla bilgi için [İçerik dağıtımı](#content-distribution)başlıklı bölüme bakın.

Verilerin gizli olduğu bilimsel simülasyonlar gibi diğer senaryolarda iki seçeneğiniz vardır. Birincisi, iş yükünüze erişiminizi, blob'a aynı anda erişilen bir süre içinde erişilen vs. olacak şekilde şaşırttı. Alternatif olarak, blob başına ve depolama hesapları arasında toplam IOPS'yi artırmak için blob'u geçici olarak birden çok depolama hesabına kopyalayabilirsiniz. Sonuçlar uygulamanızın davranışına bağlı olarak değişir, bu nedenle tasarım sırasında eşzamanlılık desenlerini test etmeyi unutmayın.

### <a name="bandwidth-and-operations-per-blob"></a>Bant genişliği ve blob başına işlemler

Tek bir blob saniyede 500'e kadar isteği destekler. Aynı blob okumak için gereken birden fazla istemcivarsa ve bu sınırı aşabilir, o zaman bir blok blob depolama hesabı kullanmayı düşünün. Blok blob depolama hesabı daha yüksek bir istek oranı veya saniyede G/Ç işlemleri (IOPS) sağlar.

Ayrıca, blob'daki işlemleri dağıtmak için Azure CDN gibi bir içerik dağıtım ağı (CDN) de kullanabilirsiniz. Azure CDN hakkında daha fazla bilgi için [Azure CDN genel bakış](../../cdn/cdn-overview.md)bilgisine bakın.  

## <a name="partitioning"></a>Bölümleme

Azure Depolama'nın blob verilerinizi nasıl bölümleye bildiğini anlamak, performansı artırmak için yararlıdır. Azure Depolama, birden çok bölüme yayılan verilerden daha hızlı bir şekilde tek bir bölümdeki verilere hizmet verebilir. Lekelerinizi uygun şekilde adlandırarak, okuma isteklerinin verimliliğini artırabilirsiniz.

Blob depolama ölçekleme ve yük dengeleme için bir dizi tabanlı bölümleme düzeni kullanır. Her blob tam blob adı (hesap + konteyner + blob) oluşan bir bölüm anahtarı vardır. Bölme tuşu, blob verilerini aralıklara bölmek için kullanılır. Aralıklar daha sonra Blob depolama boyunca yük dengeli.

Aralık tabanlı bölümleme, sözlük sıralama (örneğin, *mypayroll,* *myperformance,* *myemployees,* vb.) veya zaman damgaları *(log20160101*, *log20160102*, *log20160102*, vb.) kullanan kuralları adlandırmanın bölümlerin aynı sunucuda birlikte bulunmasıyla sonuçlanmasının daha olası olduğu anlamına gelir. , artan yük kadar onlar daha küçük aralıklara ayrılır gerektirir. Aynı bölüm sunucusundaki lekelerin birlikte bulunması performansı artırır, bu nedenle performans geliştirmenin önemli bir bölümü lekeleri en etkili şekilde düzenleyecek şekilde adlandırmayı içerir.

Örneğin, bu blobs üzerindeki yük bölüm aralıkları daha fazla yeniden dengeleme gerektirene kadar bir kapsayıcı içinde tüm lekeler tek bir sunucu tarafından sunulabilir. Benzer şekilde, adlarını sözlü sırada düzenlenmiş hafif yüklü bir hesap grubu, bu hesaplardan birinin veya tümünün yükü birden çok bölüm sunucusuna bölünmesini gerektirene kadar tek bir sunucu tarafından sunulabilir.

Her yük dengeleme işlemi, işlem sırasında depolama çağrılarının gecikmesini etkileyebilir. Hizmetin bir bölüme ani bir trafik patlamasını işleme yeteneği, yük dengeleme işlemi devreye girip bölüm anahtar aralığını yeniden dengeleyene kadar tek bir bölüm sunucusunun ölçeklenebilirliğiyle sınırlıdır.

Bu tür işlemlerin sıklığını azaltmak için bazı en iyi uygulamaları izleyebilirsiniz.  

- Mümkünse, standart depolama hesapları için 4 MiB'den büyük blob veya blok boyutları ve premium depolama hesapları için 256 KiB'den büyük kullanın. Daha büyük blob veya blok boyutları yüksek iş lenme bloğu lekelerini otomatik olarak etkinleştirir. Yüksek iş bölümü blok lekeleri, bölüm adlandırmadan etkilenmeyen yüksek performanslı yutma sağlar.
- Hesaplar, kapsayıcılar, blob'lar, tablolar ve kuyruklar için kullandığınız adlandırma kuralını inceleyin. İhtiyaçlarınıza en uygun karma işlevi kullanarak hesap, kapsayıcı veya blob adlarını üç basamaklı karma ile önceden ele almayı düşünün.
- Verilerinizi zaman damgaları veya sayısal tanımlayıcılar kullanarak düzenliyorsanız, yalnızca ek (veya yalnızca prepend) trafik deseni kullanmadığınızdan emin olun. Bu desenler aralık tabanlı bölümleme sistemi için uygun değildir. Bu desenler, tüm trafiğin tek bir bölüme giden ve sistemin etkili bir şekilde yük dengelemesini sınırlamasına yol açabilir.

    Örneğin, *yyyymmdd*gibi bir zaman damgası ile bir blob kullanan günlük işlemler varsa, o günlük işlem için tüm trafik tek bir bölüm sunucusu tarafından sunulan tek bir blob, yönlendirilir. Blob başına sınırların ve bölüm başına sınırların gereksinimlerinizi karşılayıp karşılamadığını düşünün ve gerekirse bu işlemi birden çok blob'a bölmeyi düşünün. Benzer şekilde, zaman serisi verilerini tablolarınızda depolarsanız, tüm trafik anahtar ad alanının son bölümüne yönlendirilebilir. Sayısal kimlikler kullanıyorsanız, kimliği üç basamaklı karma ile önen. Zaman damgaları kullanıyorsanız, zaman damgasını saniye değeriyle önek, örneğin, *ssyyyymmdd*. Uygulamanız düzenli olarak giriş ve sorgu işlemleri gerçekleştiriyorsa, sorgu sayınızı sınırlayacak bir karma işlevi seçin. Bazı durumlarda, rasgele bir önek yeterli olabilir.
  
- Azure Depolama'da kullanılan bölümleme şeması hakkında daha fazla bilgi için Azure [Depolama: Güçlü Tutarlılığa Sahip Yüksek Kullanılabilirlik Li Bulut Depolama Hizmeti'ne](https://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf)bakın.

## <a name="networking"></a>Ağ Oluşturma

Uygulamanın fiziksel ağ kısıtlamaları performans üzerinde önemli bir etkiye sahip olabilir. Aşağıdaki bölümlerde, kullanıcıların karşılaşabileceği bazı sınırlamalar açıklanabilir.  

### <a name="client-network-capability"></a>İstemci ağ özelliği

Bant genişliği ve ağ bağlantısının kalitesi, aşağıdaki bölümlerde açıklandığı gibi uygulama performansında önemli roller oynar.

#### <a name="throughput"></a>Aktarım hızı

Bant genişliği için sorun genellikle istemcinin yetenekleridir. Daha büyük Azure örneklerinin daha fazla kapasiteye sahip NIC'leri vardır, bu nedenle tek bir makineden daha yüksek ağ sınırlarına ihtiyacınız varsa daha büyük bir örnek veya daha fazla VM kullanmayı düşünmelisiniz. Azure Depolama'ya şirket içi bir uygulamadan erişiyorsanız, aynı kural geçerlidir: istemci aygıtının ağ özelliklerini ve Azure Depolama konumuna ağ bağlantısını anlamak ve gerektiğinde bunları geliştirmek veya tasarımı kendi yetenekleri dahilinde çalışmak için uygulama.

#### <a name="link-quality"></a>Bağlantı kalitesi

Her ağ kullanımında olduğu gibi, hatalara ve paket kaybına neden olan ağ koşullarının etkili iş verime yavaşlayacaktır.  WireShark veya NetMon kullanarak bu sorunu tanılamayardımcı olabilir.  

### <a name="location"></a>Konum

Dağıtılmış herhangi bir ortamda, istemciyi sunucunun yakınına yerleştirmek en iyi performansı sunar. Azure Depolama'ya en düşük gecikme gecikmesiyle erişmek için istemciniz için en iyi konum aynı Azure bölgesi içindedir. Örneğin, Azure Depolama'yı kullanan bir Azure web uygulamanız varsa, her ikisini de ABD Batı veya Asya Güneydoğu gibi tek bir bölgede bulun. Tek bir bölgedeki bant genişliği kullanımı ücretsiz olduğundan, kaynakların birlikte bulunması gecikme süresini ve maliyeti azaltır.  

İstemci uygulamaları Azure Depolama'ya erişecek ancak mobil aygıt uygulamaları veya şirket içi kurumsal hizmetler gibi Azure'da barındırılamıyorsa, depolama hesabını bu istemcilere yakın bir bölgede bulmak gecikme süresini azaltabilir. Müşterileriniz geniş bir şekilde dağıtılıyorsa (örneğin, bazıları Kuzey Amerika'da, bazıları Avrupa'da), bölge başına bir depolama hesabı kullanmayı düşünün. Uygulamanın depolayan verileri tek tek kullanıcılara özelse ve depolama hesapları arasında veri çoğaltmayı gerektirmeyecekse, bu yaklaşımın uygulanması daha kolaydır.

Blob içeriğinin geniş dağıtımı için Azure CDN gibi bir içerik teslim ağı kullanın. Azure CDN hakkında daha fazla bilgi için [Azure CDN'ye](../../cdn/cdn-overview.md)bakın.  

## <a name="sas-and-cors"></a>SAS ve CORS

Azure Depolama'daki verilere erişmek için kullanıcının web tarayıcısında veya bir cep telefonu uygulamasında çalışan JavaScript gibi kodları yetkilendirmeniz gerektiğini varsayalım. Bir yaklaşım, proxy gibi davranan bir hizmet uygulaması oluşturmaktır. Kullanıcının aygıtı, hizmetle ilgili olarak kimlik doğrulaması verir ve bu da Azure Depolama kaynaklarına erişimyetkisi verir. Bu şekilde, depolama hesabı anahtarlarınızı güvenli olmayan aygıtlarda açığa çıkarmaktan kaçınabilirsiniz. Ancak, kullanıcının aygıtı ile Azure Depolama arasında aktarılan tüm verilerin hizmet uygulamasından geçmesi gerektiğinden, bu yaklaşım hizmet uygulamasına önemli bir ek yük yerleştirir.

Paylaşılan erişim imzalarını (SAS) kullanarak bir hizmet uygulamasını Azure Depolama için proxy olarak kullanmaktan kaçınabilirsiniz. SAS'ı kullanarak, sınırlı bir erişim belirteci kullanarak kullanıcınızın cihazının doğrudan Azure Depolama'ya istekte bulunmasını sağlayabilirsiniz. Örneğin, bir kullanıcı uygulamanıza bir fotoğraf yüklemek isterse, hizmet uygulamanız bir SAS oluşturabilir ve kullanıcının aygıtına gönderebilir. SAS belirteci, belirli bir süre için bir Azure Depolama kaynağına yazma izni verebilir ve bundan sonra SAS belirteci sona erer. SAS hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](../common/storage-sas-overview.md)ver.  

Genellikle, bir web tarayıcısı, bir etki alanında bir web sitesi tarafından barındırılan bir sayfada JavaScript'in yazma işlemleri gibi belirli işlemleri başka bir etki alanına gerçekleştirmesine izin vermez. Aynı kaynak ilkesi olarak bilinen bu ilke, bir sayfadaki kötü amaçlı bir komut dosyasının başka bir web sayfasındaki verilere erişmesini engeller. Ancak, bulutta bir çözüm oluştururken aynı kaynak ilkesi bir sınırlama olabilir. Orijinler arası kaynak paylaşımı (CORS), hedef etki alanının kaynak etki alanından kaynaklanan isteklere güvendiği tarayıcıyla iletişim kurmasını sağlayan bir tarayıcı özelliğidir.

Örneğin, Azure'da çalışan bir web uygulamasının bir Azure Depolama hesabına kaynak isteği nde bulunduğunu varsayalım. Web uygulaması kaynak etki alanıdır ve depolama hesabı hedef etki alanıdır. Kaynak etki alanından gelen isteklerin Azure Depolama tarafından güvenilen web tarayıcısına iletişim kurmak için Azure Depolama hizmetlerinden herhangi biri için CORS'ü yapılandırabilirsiniz. CORS hakkında daha fazla bilgi için [Azure Depolama için başlangıçlar arası kaynak paylaşımı (CORS) desteğine](/rest/api/storageservices/Cross-Origin-Resource-Sharing--CORS--Support-for-the-Azure-Storage-Services)bakın.  
  
Hem SAS hem de CORS, web uygulamanızda gereksiz yükleri önlemenize yardımcı olabilir.  

## <a name="caching"></a>Önbelleğe alma

Önbelleğe alma performansta önemli bir rol oynar. Aşağıdaki bölümlerde önbelleğe alma en iyi uygulamaları tartışır.

### <a name="reading-data"></a>Verileri okuma

Genel olarak, verileri bir kez okumak iki kez okumak için tercih edilir. Bir kullanıcıya içerik olarak hizmet vermek için Azure Depolama'dan 50 MiB blob alan bir web uygulaması örneğini düşünün. İdeal olarak, uygulama blob'u yerel olarak diske önbelleğe alır ve ardından sonraki kullanıcı istekleri için önbelleğe alınan sürümü alır.

Önbelleğe alındığı günden beri değiştirilmemişse bir blob almaktan kaçınmanın bir yolu, GET işlemini değişiklik süresi için koşullu bir üstbilgiyle nitelemektir. Değiştirilen son süre, blob'un önbelleğe alındığı andan sonraysa, blob alınır ve yeniden önbelleğe alınır. Aksi takdirde, önbelleğe alınan blob en iyi performans için alınır.

Ayrıca, hatanızı aldıktan sonra kısa bir süre için değişmeden kalır varsaymak için uygulama tasarlamaya karar verebilirsiniz. Bu durumda, uygulamanın bu aralıkta blob'un değiştirilip değiştirilmediğini denetlemesi gerekmez.

Yapılandırma verileri, arama verileri ve uygulama tarafından sık kullanılan diğer veriler önbelleğe almak için iyi adaylardır.  

Koşullu üstbilgileri kullanma hakkında daha fazla bilgi için [bkz.](/rest/api/storageservices/specifying-conditional-headers-for-blob-service-operations)  

### <a name="uploading-data-in-batches"></a>Toplu olarak veri yükleme

Bazı senaryolarda, verileri yerel olarak toplayabilir ve ardından her veri parçasını hemen yüklemek yerine düzenli aralıklarla toplu olarak yükleyebilirsiniz. Örneğin, bir web uygulamasının etkinliklerin günlük dosyasini tuttuğunu varsayalım. Uygulama, bir tabloya (çok sayıda depolama işlemi gerektiren) olur gibi her etkinliğin ayrıntılarını yükleyebilir veya etkinlik ayrıntılarını yerel bir günlük dosyasına kaydedebilir ve ardından tüm etkinlik ayrıntılarını düzenli olarak bir blob'a sınırlı bir dosya olarak yükleyebilir. Her günlük girişi 1 KB boyutundaysa, tek bir işlemde binlerce giriş yükleyebilirsiniz. Tek bir işlem, 64 MiB boyutuna kadar bir blob yüklemeyi destekler. Uygulama geliştiricisi istemci aygıtı veya yükleme hataları olasılığı için tasarlamalıdır. Etkinlik verilerinin tek bir etkinlik yerine bir süre için karşıdan yüklenmeleri gerekiyorsa, Tablo depolama üzerinde Blob depolama alanı kullanılması önerilir.

## <a name="net-configuration"></a>.NET yapılandırması

.NET Framework kullanıyorsanız, bu bölümde önemli performans iyileştirmeleri yapmak için kullanabileceğiniz birkaç hızlı yapılandırma ayarı listelenir.  Diğer dilleri kullanıyorsanız, seçtiğiniz dilde benzer kavramların geçerli olup olmadığını kontrol edin.  

### <a name="use-net-core"></a>.NET Core kullanın

Performans geliştirmelerinden yararlanmak için Azure Depolama uygulamalarınızı .NET Core 2.1 veya sonraki lerle geliştirin. Mümkün olduğunda .NET Core 3.x kullanılması önerilir.

.NET Core'daki performans iyileştirmeleri hakkında daha fazla bilgi için aşağıdaki blog gönderilerine bakın:

- [.NET Core 3.0'da Performans Geliştirmeleri](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core-3-0/)
- [.NET Core 2.1'de Performans Geliştirmeleri](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core-2-1/)

### <a name="increase-default-connection-limit"></a>Varsayılan bağlantı sınırını artırma

.NET'te, aşağıdaki kod varsayılan bağlantı sınırını (genellikle istemci ortamında iki veya sunucu ortamında on) 100'e yükseltir. Genellikle, değeri uygulamanız tarafından kullanılan iş parçacığı sayısına ayarlamanız gerekir. Bağlantıları açmadan önce bağlantı sınırını ayarlayın.

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Diğer programlama dilleri için bağlantı sınırının nasıl ayarlan olduğunu belirlemek için belgelere bakın.  

Daha fazla bilgi için [web](https://blogs.msdn.microsoft.com/darrenj/2005/03/07/web-services-concurrent-connections/)hizmetleri: Eşzamanlı Bağlantılar adlı blog gönderisini görün.  

### <a name="increase-minimum-number-of-threads"></a>En az iş parçacığı sayısını artırma

Eşzamanlı aramaları eşzamanlı görevlerle birlikte kullanıyorsanız, iş parçacığı havuzundaki iş parçacığı sayısını artırmak isteyebilirsiniz:

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  
```

Daha fazla bilgi için [ThreadPool.SetMinThreads](/dotnet/api/system.threading.threadpool.setminthreads) yöntemine bakın.  

## <a name="unbounded-parallelism"></a>Sınırsız paralellik

Paralellik performans için harika olsa da, sınırlanmamış paralellik kullanırken dikkatli olun, yani iş parçacığı veya paralel istek sayısına uygulanan bir sınır yoktur. Veri yüklemek veya indirmek, aynı depolama hesabında birden çok bölüme erişmek veya aynı bölümdeki birden çok öğeye erişmek için paralel istekleri sınırlamayı unutmayın. Paralellik sınırlandırılmamışsa, uygulamanız istemci aygıtının yeteneklerini veya depolama hesabının ölçeklenebilirlik hedeflerini aşarak daha uzun gecikmeler ve daralmayla sonuçlanabilir.  

## <a name="client-libraries-and-tools"></a>İstemci kitaplıkları ve araçları

En iyi performans için her zaman Microsoft tarafından sağlanan en son istemci kitaplıklarını ve araçlarını kullanın. Azure Depolama istemci kitaplıkları çeşitli diller için kullanılabilir. Azure Depolama, PowerShell ve Azure CLI'yi de destekler. Microsoft, bu istemci kitaplıklarını ve araçlarını performans göz önünde bulundurarak etkin bir şekilde geliştirir, en son hizmet sürümleriyle güncel tutar ve kanıtlanmış performans uygulamalarının çoğunu dahili olarak ele almalarını sağlar. Daha fazla bilgi için [Azure Depolama başvuru belgelerine](/azure/storage/#reference)bakın.

## <a name="handle-service-errors"></a>Hizmet hatalarını işleme

Hizmet bir isteği işleyemediğinde Azure Depolama bir hata döndürür. Belirli bir senaryoda Azure Depolama tarafından döndürülebilecek hataları anlamak, performansı optimize etmek için yararlıdır.

### <a name="timeout-and-server-busy-errors"></a>Zaman Ara ve Sunucu Meşgul hataları

Azure Depolama, ölçeklenebilirlik sınırlarına yaklaşırsa uygulamanızı azaltabilir. Bazı durumlarda, Azure Depolama bazı geçici koşullar nedeniyle bir isteği işleyemeyebilir. Her iki durumda da, hizmet 503 (Sunucu Meşgul) veya 500 (Timeout) hatası döndürebilir. Hizmet, daha yüksek iş elde edilebilmesi için veri bölümlerini yeniden dengeliyorsa, bu hatalar da oluşabilir. İstemci uygulaması genellikle bu hatalardan birine neden olan işlemi yeniden denemelidir. Ancak, Azure Depolama ölçeklenebilirlik hedeflerini aştığı için uygulamanızı daraltıyorsa veya hizmet isteği başka bir nedenle yanıtveremese bile, agresif yeniden denemeler sorunu daha da kötüleştirebilir. Üstel geri leme ilkesini kullanman önerilir ve istemci kitaplıkları bu davranış için varsayılan olarak. Örneğin, uygulamanız 2 saniye, sonra 4 saniye, sonra 10 saniye, sonra 30 saniye sonra yeniden deneyebilir ve sonra tamamen vazgeçebilir. Bu şekilde, uygulamanız, daralmaya yol açabilecek davranışları şiddetlendirmek yerine hizmet üzerindeki yükünü önemli ölçüde azaltır.  

Bağlantı hataları, azaltmanın sonucu olmadığından ve geçici olması beklendiğiiçin hemen yeniden denenebilir.  

### <a name="non-retryable-errors"></a>Yeniden denemeyen hatalar

İstemci kitaplıkları, hangi hataların yeniden denenebileceğini ve hangilerinin yapamayacağına farkında olarak yeniden denemeleri yönetir. Ancak, Azure Depolama REST API'sini doğrudan arıyorsanız, yeniden denememeniz gereken bazı hatalar vardır. Örneğin, 400 (Kötü İstek) hatası, istemci uygulamasının beklenen formda olmadığı için işlenmeyen bir istek gönderdiğini gösterir. Bu isteği yeniden göndermek her seferinde aynı yanıtı verir, bu nedenle yeniden denemenin bir anlamı yoktur. Azure Depolama REST API'sini doğrudan arıyorsanız, olası hatalar ve bunların yeniden denenip denenmemesi gerektiğini unutmayın.

Azure Depolama hata kodları hakkında daha fazla bilgi için [Durum ve hata kodlarına](/rest/api/storageservices/status-and-error-codes2)bakın.

## <a name="copying-and-moving-blobs"></a>Lekelerin kopyalanması ve taşınması

Azure Depolama, bir depolama hesabı içindeki, depolama hesapları arasında ve şirket içi sistemler ve bulut arasında blob'ları kopyalamak ve taşımak için bir dizi çözüm sağlar. Bu bölümde, performans üzerindeki etkileri açısından bu seçeneklerden bazıları açıklanmaktadır. Verileri Blob depolama alanına veya Blob depolamadan verimli [bir](../common/storage-choose-data-transfer-solution.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)şekilde aktarma hakkında bilgi için bkz.

### <a name="blob-copy-apis"></a>Blob kopya API'leri

Depolama hesapları arasında blobs kopyalamak için [URL'den Blok La](/rest/api/storageservices/put-block-from-url) işlemini kullanın. Bu işlem, verileri herhangi bir URL kaynağından eşzamanlı olarak bir blok blob içine kopyalar. İşlemi `Put Block from URL` kullanmak, depolama hesapları arasında veri aktarırken gerekli bant genişliğini önemli ölçüde azaltabilir. Kopyalama işlemi hizmet tarafında gerçekleştiğinden, verileri karşıdan yüklemeniz ve yeniden yüklemeniz gerekmez.

Aynı depolama hesabındaki verileri kopyalamak için [Blob Kopyala](/rest/api/storageservices/Copy-Blob) işlemini kullanın. Aynı depolama hesabındaki verilerin kopyalanması genellikle hızlı bir şekilde tamamlanır.  

### <a name="use-azcopy"></a>AzCopy’yi kullanma

AzCopy komut satırı yardımcı programı, blob'ların depolama hesaplarına, den ve depolama hesapları arasında toplu olarak aktarılması için basit ve verimli bir seçenektir. AzCopy bu senaryo için optimize edilebiyi ve yüksek aktarım hızları elde edebilirsiniz. AzCopy sürüm 10, blob verilerini depolama hesapları arasında kopyalamak için `Put Block From URL` işlemi kullanır. Daha fazla bilgi için [AzCopy v10 kullanarak verileri Kopyala veya Azure Depolama'ya taşıyın.](/azure/storage/common/storage-use-azcopy-v10)  

### <a name="use-azure-data-box"></a>Azure Veri Kutusu'ni kullanma

Blob depolama alanına büyük hacimli veri almak için, çevrimdışı aktarımlar için Azure Veri Kutusu ailesini kullanmayı düşünün. Microsoft tarafından sağlanan Veri Kutusu aygıtları, zaman, ağ kullanılabilirliği veya maliyetlerle sınırlı olduğunuzda büyük miktarda veriyi Azure'a taşımak için iyi bir seçimdir. Daha fazla bilgi için [Azure Veri Kutusu Belgeleri'ne](/azure/databox/)bakın.

## <a name="content-distribution"></a>İçerik dağıtımı

Bazen bir uygulamanın aynı veya birden çok bölgede bulunan birçok kullanıcıya (örneğin, bir web sitesinin ana sayfasında kullanılan bir ürün demo videosu) aynı içeriği sunması gerekir. Bu senaryoda, blob içeriğini coğrafi olarak dağıtmak için Azure CDN gibi bir İçerik Dağıtım Ağı (CDN) kullanın. Azure CDN, tek bir bölgede bulunan ve düşük gecikme süreyle içerik teslim edemeyen bir Azure Depolama hesabının aksine, dünyanın dört bir yanındaki birden fazla veri merkezinde sunucukullanır. Ayrıca, CDN genellikle tek bir depolama hesabından çok daha yüksek çıkış sınırlarını destekleyebilir.  

Azure CDN hakkında daha fazla bilgi için [Azure CDN'ye](../../cdn/cdn-overview.md)bakın.

## <a name="use-metadata"></a>Meta verileri kullanma

Blob hizmeti, blob özellikleri veya meta veriler içerebilen HEAD isteklerini destekler. Örneğin, uygulamanızın bir fotoğraftan Exif (exchangable image format) verilerine ihtiyacı varsa, fotoğrafı alabilir ve ayıklayabilir. Bant genişliğini kaydetmek ve performansı artırmak için, uygulama fotoğrafı yüklediğinde uygulama exif verilerini blob'un meta verilerinde depolayabilir. Daha sonra yalnızca bir HEAD isteği kullanarak meta verilerdeki Exif verilerini alabilirsiniz. Blob'un tam içeriğini değil, yalnızca meta verileri almak önemli bant genişliğinden tasarruf sağlar ve Exif verilerini ayıklamak için gereken işlem süresini azaltır. 8 KiB meta veri blob başına depolanabilir unutmayın.  

## <a name="upload-blobs-quickly"></a>Blob'ları hızlı bir şekilde yükleyin

Blobs'u hızlı bir şekilde yüklemek için, önce bir blob veya çok yükleyip yüklemeyeceğinizi belirleyin. Senaryonuza bağlı olarak kullanılacak doğru yöntemi belirlemek için aşağıdaki kılavuzu kullanın.  

### <a name="upload-one-large-blob-quickly"></a>Hızlı bir şekilde büyük bir blob yükleyin

Tek bir büyük blob'u hızlı bir şekilde yüklemek için, istemci uygulaması bloklarını veya sayfalarını paralel olarak yükleyebilir ve tek tek lekeler için ölçeklenebilirlik hedeflerine ve bir bütün olarak depolama hesabına dikkat edebilir. Azure Depolama istemci kitaplıkları paralel olarak yüklemeyi destekler. Örneğin, .NET veya Java'da izin verilen eşzamanlı istek sayısını belirtmek için aşağıdaki özellikleri kullanabilirsiniz. Desteklenen diğer diller için istemci kitaplıkları da benzer seçenekler sunar.

- .NET için [BlobRequestOptions.ParallelOperationThreadCount](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.paralleloperationthreadcount) özelliğini ayarlayın.
- Java/Android için [BlobRequestOptions.setConcurrentRequestCount (son Integer eşzamanlıRequestCount)](/java/api/com.microsoft.azure.storage.blob._blob_request_options.setconcurrentrequestcount) yöntemini arayın.

### <a name="upload-many-blobs-quickly"></a>Birçok blob'u hızlı bir şekilde yükleyin

Birçok blob'u hızlı bir şekilde yüklemek için, lekeleri paralel olarak yükleyin. Paralel olarak yüklemek, yüklemeyi depolama hizmetinin birden çok bölümüne yaydığı için paralel blok yüklemeleriyle aynı anda tek blob yüklemekten daha hızlıdır. AzCopy varsayılan olarak paralel yüklemeler gerçekleştirir ve bu senaryo için önerilir. Daha fazla bilgi için [Bkz. AzCopy ile başlayın.](../common/storage-use-azcopy-v10.md)  

## <a name="choose-the-correct-type-of-blob"></a>Doğru blob türünü seçin

Azure Depolama blok lekeleri, ek lekeler ve sayfa lekeleri destekler. Belirli bir kullanım senaryosu için, blob türü seçiminiz çözümünüzün performansını ve ölçeklenebilirliğini etkiler.

Büyük miktarda veriyi verimli bir şekilde yüklemek istediğinizde blob'ları engelleyin uygundur. Örneğin, Blob depolama alanına fotoğraf veya video yükleyen bir istemci uygulaması blok lekelerini hedefler.

Ek lekeler bloklardan oluşan blobs bloklar benzer. Bir ek blob değiştirdiğinizde, bloklar yalnızca blob sonuna eklenir. Uygulamanın varolan bir blob'a veri eklemesi gerektiğinde, ekleme blobları günlüğe kaydetme gibi senaryolar için yararlıdır.

Uygulamanın verilere rasgele yazma yapması gerekiyorsa, sayfa lekeleri uygundur. Örneğin, Azure sanal makine diskleri sayfa lekeleri olarak depolanır. Daha fazla bilgi için [bkz.](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)  

## <a name="next-steps"></a>Sonraki adımlar

- [Blob depolama için ölçeklenebilirlik ve performans hedefleri](scalability-targets.md)
- [Standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [Durum ve hata kodları](/rest/api/storageservices/Status-and-Error-Codes2)
