---
title: Azure depolama performansı ve ölçeklenebilirlik denetim listesi | Microsoft Docs
description: Performans uygulamaları geliştirmeye yönelik Azure Storage ile kullanım için kanıtlanmış uygulamaların denetim listesi.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/07/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ee216bd4d6994179e347465c30039f2f8e293c85
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68233008"
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Microsoft Azure Depolama performans ve ölçeklenebilirlik denetim listesi

Microsoft Azure Depolama hizmetlerinin yayınlanmasından bu yana, Microsoft bu Hizmetleri performans açısından kullanmaya yönelik bir dizi kanıtlanmış uygulama geliştirmiştir ve bu makale, bunların en önemlileri bir denetim listesi stili listesine konsolide eder. Bu makalenin amacı, uygulama geliştiricilerinin Azure depolama ile kendini kanıtlamış uygulamalar kullandığını ve bu uygulamaların benimsemeleri gereken diğer kanıtlanmış uygulamaları belirlemesine yardımcı olmak için tasarlanmıştır. Bu makale, olası her performans ve ölçeklenebilirlik iyileştirmesini kapsamaya çalışmaz; bu, etkileri düşük olan veya genel olarak geçerli olmayan olanları dışlar. Tasarım sırasında uygulamanın davranışının tahmin edilebileceği ölçüde, performans sorunlarından çalışacak tasarımları önlemek için bunları erken önünde tutmanız yararlı olur.  

Azure Storage kullanan her uygulama geliştiricisi, bu makaleyi okumak için zaman almalıdır ve uygulamasının aşağıda listelenen kanıtlanmış uygulamaların her birini takip edip uygulamadığı denetlenir.  

## <a name="checklist"></a>Denetim listesi

Bu makalede, kanıtlanmış uygulamalar aşağıdaki gruplar halinde düzenler. İçin geçerli kanıtlanmış uygulamalar:  

* Tüm Azure depolama hizmetleri (blob 'lar, tablolar, kuyruklar ve dosyalar)
* Bloblar
* Tablolar
* Kuyruklar  

| Bitti | Alan | Category | Soru |
| --- | --- | --- | --- |
| &nbsp; | Tüm Hizmetler |Ölçeklenebilirlik hedefleri |[Uygulamanız ölçeklenebilirlik hedeflerine yaklaşmamak için tasarlandı mi?](#subheading1) |
| &nbsp; | Tüm Hizmetler |Ölçeklenebilirlik hedefleri |[Adlandırma kuralınızın daha iyi yük dengelemesini sağlamak üzere tasarlandı mı?](#subheading47) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci tarafı cihazlarının gerekli performans düzeyine ulaşmak için yeterli yüksek bant genişliğine ve düşük gecikme süresine sahip olması gerekir mi?](#subheading2) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci tarafı cihazlarının yeterince yüksek kaliteli bağlantısı var mı?](#subheading3) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci uygulaması, depolama hesabının "yakınında" bulunuyor mu?](#subheading4) |
| &nbsp; | Tüm Hizmetler |İçerik Dağıtımı |[İçerik dağıtımı için CDN kullanıyor musunuz?](#subheading5) |
| &nbsp; | Tüm Hizmetler |Doğrudan Istemci erişimi |[Proxy yerine depolamaya doğrudan erişim sağlamak için SAS ve CORS kullanıyor musunuz?](#subheading6) |
| &nbsp; | Tüm Hizmetler |Önbelleğe alma |[Uygulamanız sürekli olarak kullanılan ve sık sık değişen verileriniz önbelleğe alınıyor mu?](#subheading7) |
| &nbsp; | Tüm Hizmetler |Önbelleğe alma |[Uygulamanız için güncelleştirmeleri toplu işleme (istemci tarafında önbelleğe alma ve daha büyük kümelere yükleme) midir?](#subheading8) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırması |[İstemcinizi yeterli sayıda eşzamanlı bağlantı kullanacak şekilde yapılandırdınız mı?](#subheading9) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırması |[.NET 'i yeterli sayıda iş parçacığı kullanacak şekilde yapılandırdınız mı?](#subheading10) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırması |[Gelişmiş atık toplamayı kullanan .NET 4,5 veya sonraki bir sürümü mi kullanıyorsunuz?](#subheading11) |
| &nbsp; | Tüm Hizmetler |Paralellik |[Paralellik 'in, istemci olanaklarınızı veya ölçeklenebilirlik hedeflerini aşırı yüklemeden emin misiniz?](#subheading12) |
| &nbsp; | Tüm Hizmetler |Araçlar |[Microsoft tarafından sunulan istemci kitaplıkları ve araçları 'nın en son sürümünü kullanıyor musunuz?](#subheading13) |
| &nbsp; | Tüm Hizmetler |Yeniden deneme sayısı |[Azaltma hataları ve zaman aşımları için bir üstel geri alma yeniden deneme İlkesi kullanıyor musunuz?](#subheading14) |
| &nbsp; | Tüm Hizmetler |Yeniden deneme sayısı |[Uygulamanız yeniden denenmeyen hatalara karşı yeniden denemeyi önler mi?](#subheading15) |
| &nbsp; | Bloblar |Ölçeklenebilirlik hedefleri |[Aynı anda tek bir nesneye erişen çok sayıda istemciniz var mı?](#subheading46) |
| &nbsp; | Bloblar |Ölçeklenebilirlik hedefleri |[Uygulamanız tek bir blob için bant genişliği veya işlem ölçeklenebilirliği hedefi içinde kalıyor mu?](#subheading16) |
| &nbsp; | Bloblar |Blobları kopyalama |[Blobları etkili bir şekilde kopyaladığınızı biliyor musunuz?](#subheading17) |
| &nbsp; | Bloblar |Blobları kopyalama |[Blob 'ların toplu kopyaları için AzCopy mi kullanıyorsunuz?](#subheading18) |
| &nbsp; | Bloblar |Blobları kopyalama |[Büyük hacimli verileri aktarmak için Azure Içeri/dışarı aktarma kullanıyor musunuz?](#subheading19) |
| &nbsp; | Bloblar |Meta verileri kullan |[Blob 'lar hakkında sık kullanılan meta verileri meta verilerinde depoluyorsanız mi?](#subheading20) |
| &nbsp; | Bloblar |Hızlı karşıya yükleniyor |[Bir blobu hızlıca karşıya yüklemeye çalışırken blokları paralel olarak karşıya yüklüyor musunuz?](#subheading21) |
| &nbsp; | Bloblar |Hızlı karşıya yükleniyor |[Çok sayıda blobu hızlıca yüklemeye çalışırken blob 'ları paralel olarak karşıya yüklüyor musunuz?](#subheading22) |
| &nbsp; | Bloblar |Doğru blob türü |[Uygun durumlarda sayfa Blobları mı kullanıyorsunuz, blob 'ları mi engelliyor?](#subheading23) |
| &nbsp; | Tablolar |Ölçeklenebilirlik hedefleri |[Saniye başına ölçeklenebilirlik hedeflerine yaklaşıyor musunuz?](#subheading24) |
| &nbsp; | Tablolar |Yapılandırma |[Tablo istekleriniz için JSON kullanıyor musunuz?](#subheading25) |
| &nbsp; | Tablolar |Yapılandırma |[Küçük isteklerin performansını artırmak için Nagle 'ı açtınız musunuz?](#subheading26) |
| &nbsp; | Tablolar |Tablolar ve bölümler |[Verilerinizi düzgün şekilde bölümlensin mi?](#subheading27) |
| &nbsp; | Tablolar |Sık kullanılan bölümler |[Yalnızca Append ve yalnızca bitiş desenlerinin önüne mi sahip olabilirsiniz?](#subheading28) |
| &nbsp; | Tablolar |Sık kullanılan bölümler |[Eklemi/güncelleştirmeleriniz birçok bölüme yayılıyor?](#subheading29) |
| &nbsp; | Tablolar |Sorgu kapsamı |[Şemanızı, nokta sorgularının çoğu durumda kullanılmasına izin vermek ve tablo sorguları gelişigüzel şekilde kullanılmak üzere tasarlamış musunuz?](#subheading30) |
| &nbsp; | Tablolar |Sorgu yoğunluğu |[Sorgularınız genellikle uygulamanızın kullanacağı satırları tarar ve döndürür mi?](#subheading31) |
| &nbsp; | Tablolar |Döndürülen verileri sınırlama |[Gerekli olmayan varlıkların döndürülmemek için filtreleme kullanıyor musunuz?](#subheading32) |
| &nbsp; | Tablolar |Döndürülen verileri sınırlama |[Gerekli olmayan özellikleri döndürmemek için projeksiyon kullanıyor musunuz?](#subheading33) |
| &nbsp; | Tablolar |Normalleştirilmemiş |[Verileri almaya çalışırken verimsiz sorgulardan veya birden çok okuma isteğinin oluşmasını önlemenize izin veriyor musunuz?](#subheading34) |
| &nbsp; | Tablolar |Ekle/güncelleştir/Sil |[İşlem olması gereken veya gidiş dönüş sayısını azaltmak için aynı anda yapılabilecek istekleri toplu olarak gerçekleştirebilirsiniz mu?](#subheading35) |
| &nbsp; | Tablolar |Ekle/güncelleştir/Sil |[Yalnızca ekleme veya güncelleştirme çağrısı yapılıp yapılmayacağını öğrenmek için bir varlığı alma](#subheading36) |
| &nbsp; | Tablolar |Ekle/güncelleştir/Sil |[Sık sık tek bir varlıkta birden çok varlık yerine özellikler olarak bir araya getirilen veri serisini mi saklayacaksınız?](#subheading37) |
| &nbsp; | Tablolar |Ekle/güncelleştir/Sil |[Her zaman birlikte alınacak ve toplu işlemlere (örneğin, zaman serisi verileri) yazılabilecekleri varlıklar için tablolar yerine blob 'ları kullanarak göz önünde bulundurmanız gerekir mi?](#subheading38) |
| &nbsp; | Kuyruklar |Ölçeklenebilirlik hedefleri |[Saniye başına ölçeklenebilirlik hedeflerine yaklaşıyor musunuz?](#subheading39) |
| &nbsp; | Kuyruklar |Yapılandırma |[Küçük isteklerin performansını artırmak için Nagle 'ı açtınız musunuz?](#subheading40) |
| &nbsp; | Kuyruklar |İleti Boyutu |[İletiniz, sıranın performansını geliştirmek için sıkıştırmı?](#subheading41) |
| &nbsp; | Kuyruklar |Toplu alma |[Tek bir "Get" işleminde birden çok ileti alıyor musunuz?](#subheading42) |
| &nbsp; | Kuyruklar |Yoklama sıklığı |[Uygulamanızın algılanan gecikmesini azaltmak için yeterince sık yoklanıyor musunuz?](#subheading43) |
| &nbsp; | Kuyruklar |Güncelleştirme Iletisi |[Hata oluştuğunda tüm iletiyi yeniden işlemek zorunda kalmadan, iletileri işlerken ilerlemeyi depolamak için UpdateMessage kullanıyor musunuz?](#subheading44) |
| &nbsp; | Kuyruklar |Mimari |[Uzun süre çalışan iş yüklerini kritik yoldan tutarak ve sonra bağımsız olarak ölçeklendirerek, tüm uygulamanızı daha ölçeklenebilir hale getirmek için kuyrukları kullanıyor musunuz?](#subheading45) |

## <a name="allservices"></a>Tüm hizmetler

Bu bölümde, Azure depolama hizmetlerinden (blob, tablo, kuyruk veya dosya) herhangi birinin kullanımı için geçerli olan kanıtlanmış uygulamalar listelenmektedir.  

### <a name="subheading1"></a>Ölçeklenebilirlik hedefleri

Azure Storage 'ın, her abonelik için bölge başına 250 depolama hesabı sınırlaması vardır. Bu sınıra ulaşırsanız bu abonelik/bölge bileşiminde daha fazla depolama hesabı oluşturamayacaksınız.

Azure Storage hizmetlerinin her birinde kapasite (GB), işlem hızı ve bant genişliği için ölçeklenebilirlik hedefleri vardır. Uygulamanız ölçeklenebilirlik hedeflerinin herhangi birini yaklaşırsa veya aşarsa, daha fazla işlem gecikmeleri veya azaltmasıyla karşılaşabilirler. Bir depolama hizmeti uygulamanızı kısıtsalken, hizmet bazı depolama işlemleri için "503 sunucu meşgul" veya "500 Işlem zaman aşımı" hata kodlarını döndürmeye başlar. Bu bölümde, ölçeklenebilirlik hedefleri ve bant genişliği ölçeklenebilirlik hedefleri ile ilgili genel yaklaşım, özellikle de açıklanmaktadır. Tek tek depolama hizmetleri ile ilgilenen daha sonraki bölümlerde, ölçeklenebilirlik hedefleri ilgili hizmetin bağlamında ele alınmaktadır:  

* [Blob bant genişliği ve saniye başına istek](#subheading16)
* [Saniye başına tablo varlıkları](#subheading24)
* [Saniye başına kuyruk iletisi sayısı](#subheading39)  

#### <a name="sub1bandwidth"></a>Tüm hizmetler için bant genişliği ölçeklenebilirlik hedefi

Yazma sırasında, coğrafi olarak yedekli depolama (GRS) hesabı için ABD 'deki bant genişliği hedefleri, giriş için 10 Gigabit/saniye (Gbps) (depolama hesabına gönderilen veriler) ve çıkış için 20 Gbps (depolama hesabından gönderilen veriler). Yerel olarak yedekli depolama (LRS) hesabı için sınırlar, çıkış için 30 Gbps ve çıkış için 20 Gbps daha yüksektir.  Uluslararası bant genişliği sınırları daha düşük olabilir ve [ölçeklenebilirlik hedefleri sayfamızda](https://msdn.microsoft.com/library/azure/dn249410.aspx)bulunabilir.  Depolama artıklığı seçenekleri hakkında daha fazla bilgi için aşağıdaki yararlı kaynaklarda bulunan bağlantılara bakın.  

#### <a name="what-to-do-when-approaching-a-scalability-target"></a>Ölçeklenebilirlik hedefine yaklaştığı sırada Yapılacaklar

Belirli bir abonelik/bölge birleşimi içinde sahip olduğunuz depolama hesabı sınırına yaklaşdıysanız, uygulamanızı ve depolama hesapları kullanımınızı değerlendirin ve bu koşulların herhangi birinin geçerli olup olmadığını belirleyebilirsiniz.

* Depolama hesaplarını yönetilmeyen diskler olarak kullanma ve bu diskleri sanal makinelerinize ekleme. Bu senaryoda, ayrı depolama hesapları oluşturup yönetmeniz gerekmeden, sizin için depolama disk ölçeklenebilirliğini idare ettikleri için [yönetilen diskleri](../../virtual-machines/windows/managed-disks-overview.md)kullanmanızı öneririz.
* Veri yalıtımının amacı doğrultusunda, her müşteri için tek bir depolama hesabı kullanma. Bu senaryoda, tüm depolama hesabı yerine her müşteri için depolama kapsayıcıları kullanmanızı öneririz. Azure depolama artık [kapsayıcı temelinde](storage-auth-aad-rbac-portal.md)rol tabanlı erişim denetimi belirtmenize olanak tanır.
* Giriş/çıkış/IOPS/kapasite hakkında daha fazla ölçeklenebilirlik sağlamak için birden fazla depolama hesabı kullanma. Bu senaryoda, mümkünse, iş yükünüz için gereken depolama hesabı sayısını azaltmak için standart depolama hesaplarının [artan limitlerinin](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/) avantajlarından yararlanmanızı öneririz.

Uygulamanız tek bir depolama hesabı için ölçeklenebilirlik hedeflerine yaklaşıyorsa, aşağıdaki yaklaşımlardan birini benimsede düşünün:  

* Uygulamanızın ölçeklenebilirlik hedefini yaklaşımını veya aşmasına neden olan iş yükünü yeniden değerlendirin. Daha az bant genişliği veya kapasite veya daha az işlem kullanmak için farklı bir şekilde tasarlayabilirsiniz mi?
* Bir uygulamanın ölçeklenebilirlik hedeflerinin birini aşmaması gerekiyorsa, birden çok depolama hesabı oluşturmanız ve uygulama verilerinizi bu birden çok depolama hesabı genelinde bölümlememelisiniz. Bu kalıbı kullanırsanız, daha sonra yük dengelemeye yönelik daha fazla depolama hesabı ekleyebilmeniz için uygulamanızı tasarlayadığınızdan emin olun. Yazma sırasında her bir Azure aboneliğinin bölge başına en fazla 250 depolama hesabı olabilir (Azure Resource Manager modeliyle dağıtıldığında).  Depolama hesaplarında Ayrıca, depolanan veri, işlemler veya aktarılan veriler açısından kullanımınız dışında bir ücret de yoktur.
* Uygulamanız bant genişliği hedeflerini ziyaret ediyor, verileri depolama hizmetine göndermek için gereken bant genişliğini azaltmak için istemcideki verileri sıkıştırmayı göz önünde bulundurun.  Bant genişliği tasarruf edebilir ve ağ performansını iyileştireken, bazı olumsuz etkileri de olabilir.  İstemcideki verileri sıkıştırmak ve sıkıştırmayı açmaya yönelik ek işleme gereksinimleri nedeniyle bunun performans etkisini değerlendirmelisiniz. Ayrıca, sıkıştırılmış verilerin depolanması, standart araçları kullanarak depolanan verileri görüntülemek daha zor olacağından sorunları gidermeye daha zor hale gelebilir.
* Uygulamanız ölçeklenebilirlik hedeflerini ziyaret ediyorsanız, yeniden denemeler için üstel geri alma kullandığınızdan emin olun (bkz. [yeniden deneme](#subheading14)).  Ölçeklenebilirlik hedeflerine hiçbir değişiklik yapmayın (yukarıdaki yöntemlerden birini kullanarak), ancak bu, azaltmadan daha kötüleşmeye devam etmeyeceğinden uygulamanızın hızla yeniden denenmemesini sağlar.  

#### <a name="useful-resources"></a>Yararlı kaynaklar

Aşağıdaki bağlantılar ölçeklenebilirlik hedefleri hakkında ek ayrıntılar sağlar:

* Ölçeklenebilirlik hedefleri hakkında bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) .
* Depolama artıklığı seçenekleri hakkında bilgi için bkz. [Azure Storage çoğaltma](storage-redundancy.md) ve [Azure depolama yedekliliği ve Okuma Erişimli Coğrafi olarak yedekli depolama](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) .
* Azure hizmetleri fiyatlandırması hakkında güncel bilgiler için bkz. [Azure fiyatlandırması](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>Bölüm adlandırma kuralı

Azure depolama, sistemin ölçeğini ölçeklendirmek ve yükünü dengelemek için Aralık tabanlı bölümleme şeması kullanır. Bölüm anahtarı (hesap + kapsayıcı + blob), verileri aralıklar halinde bölümlemek için kullanılır ve bu aralıklar sistem genelinde yük dengedir. Bu, sözcük temelli sıralama (örneğin, *mypayroll*, *myperformance*, *MyEmployees*, vb.) gibi adlandırma kurallarının veya zaman damgalarının (*log20160101*, *log20160102*, *log20160102*, vb.) nasıl olacağını gösterir bir yük dengeleme işlemi bunları daha küçük aralıklar halinde bölümlendirene kadar, kendisini aynı bölüm sunucusunda birlikte bulunan bölümlere da ekleyebilirsiniz. Örneğin, bir kapsayıcı içindeki tüm Bloblar, bu bloblarda yük, Bölüm aralıklarının yeniden dengelenmesini gerektirene kadar tek bir sunucu tarafından sunulabilir. Benzer şekilde, adları sözcük temelli sırada düzenlenmiş, bu hesapların bir veya tümünün birden çok bölüm sunucusuna bölünmesi gerekene kadar, adlarına sahip hafif bir şekilde yüklenmiş hesaplar grubu tek bir sunucu tarafından sunulabilir. Her yük dengeleme işlemi, işlem sırasında depolama çağrılarının gecikmesini etkileyebilir. Sistemin bir bölüme ani trafik artışını işleme yeteneği, yük dengeleme işlemi kullanıma alınana ve bölüm anahtar aralığını yeniden dengeleyene kadar tek bir bölüm sunucusunun ölçeklenebilirliği ile sınırlıdır.

Bu tür işlemlerin sıklığını azaltmak için bazı en iyi yöntemleri izleyebilirsiniz.  

* Mümkünse, yüksek verimlilik Blok Blobu (HTBB) etkinleştirmek için daha büyük put Blobu veya yerleştirme boyutları (Standart hesaplar için 4 MIB 'den büyük ve Premium hesaplar için 256 KiB 'den fazla) kullanın. HTBB, Bölüm adlandırmayla etkilenmeyen yüksek performanslı alma olanağı sağlar.
* Hesaplar, kapsayıcılar, Bloblar, tablolar ve kuyruklar için kullandığınız adlandırma kuralını yakından inceleyin. Gereksinimlerinize en uygun bir karma işlevi kullanarak hesap, kapsayıcı veya blob adlarını 3 basamaklı bir karma ile önek olarak düşünün.  
* Verilerinizi zaman damgaları veya sayısal tanımlayıcılar kullanarak düzenlediğinizde, salt bir Append (veya yalnızca sonuna kadar) trafik desenleri kullanmadığınız emin olmanız gerekir. Bu desenler, Aralık tabanlı bölümleme sistemi için uygun değildir ve tek bir bölüme giden ve sistemi etkin yük dengelemeden sınırlayan tüm trafiğe yol açabilir. Örneğin, *YYYYMMDD*gibi bir zaman damgasıyla blob nesnesi kullanan günlük işlemyaptıysanız, bu günlük işlem için tüm trafik tek bir bölüm sunucusu tarafından sunulan tek bir nesneye yönlendirilir. Blob başına limitlerin ve bölüm sınırlarının gereksinimlerinizi karşılayıp karşılamadığını ve gerekirse bu işlemi birden çok blob 'a bölmek için göz atın. Benzer şekilde, tablolarınızda zaman serisi verilerini depolarsanız, tüm trafik anahtar ad alanının son bölümüne yönlendirilebilir. Zaman damgaları veya sayısal kimlikler kullanmanız gerekiyorsa, KIMLIĞI 3 basamaklı bir karma ile önek olarak veya zaman damgalarına önek olarak *ssyyyymmdd*gibi sürenin saniye bir parçası olması gerekir. Listeleme ve sorgulama işlemleri düzenli olarak gerçekleştirilirse, sorgu sayısını sınırlayan bir karma işlev seçin. Diğer durumlarda rastgele bir ön ek yeterli olabilir.  
* Azure depolama 'da kullanılan bölümleme şeması hakkında daha fazla bilgi için bkz [. Azure Storage: Güçlü tutarlılığı](https://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf)olan yüksek oranda kullanılabilir bir bulut depolama hizmeti.

### <a name="networking"></a>Ağ

API önemli ölçüde çağırdığında, genellikle uygulamanın fiziksel ağ kısıtlamalarının performansı önemli ölçüde etkiler. Aşağıda, kullanıcıların karşılaşabileceği bazı sınırlamalar açıklanır.  

#### <a name="client-network-capability"></a>İstemci ağ özelliği

##### <a name="subheading2"></a>Trafiği

Bant genişliği için genellikle bu sorun istemcinin yeteneklerine yöneliktir. Örneğin, tek bir depolama hesabı 10 Gbps veya daha fazlasını (bkz. [bant genişliği ölçeklenebilirlik hedeflerini](#sub1bandwidth)) işleyebilir, "küçük" Azure çalışan rolü örneğindeki ağ hızı yalnızca yaklaşık 100 Mbps olabilir. Daha büyük Azure örneklerinin NIC 'Leri daha fazla kapasiteye sahip olduğundan, tek bir makineden daha yüksek ağ sınırlarına ihtiyaç duyuyorsanız daha büyük bir örnek veya daha fazla VM kullanmayı düşünmelisiniz. Bir depolama hizmetine şirket içi bir uygulamadan erişiyorsanız, aynı kural geçerlidir: istemci cihazının ağ yeteneklerini ve Azure depolama konumuna ağ bağlantısını anlayın ve bunları gerektiği şekilde geliştirir veya uygulamanın özellikleri içinde çalışması.  

##### <a name="subheading3"></a>Bağlantı kalitesi

Her türlü ağ kullanımında olduğu gibi, ağ koşullarının hatalara ve paket kaybına neden olduğunu unutmayın.  WireShark veya NetMon kullanmak bu sorunu tanılamanıza yardımcı olabilir.  

##### <a name="useful-resources"></a>Yararlı kaynaklar

Sanal makine boyutları ve ayrılan bant genişliği hakkında daha fazla bilgi için bkz. [WINDOWS VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Linux VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>Konumuna

Dağıtılmış bir ortamda, istemciyi sunucuya eklemek en iyi performansı sağlar. En düşük gecikme süresi ile Azure depolama erişimi için, istemciniz için en iyi konum aynı Azure bölgesi içindedir. Örneğin, Azure Storage kullanan bir Azure Web siteniz varsa, bunları her ikisi de tek bir bölgede (örneğin, ABD Batı veya Asya Güneydoğu) bulmanız gerekir. Bu, gecikme süresini ve maliyeti azaltır; yazma sırasında tek bir bölgedeki bant genişliği kullanımı ücretsizdir.  

İstemci uygulamalarınız Azure 'da barındırılmaz (mobil cihaz uygulamaları veya şirket içi kurumsal hizmetler gibi), daha sonra depolama hesabını ona erişecek cihazların yakınında bir bölgeye yerleştirmek, genellikle gecikmeyi azaltır. İstemcileriniz büyük ölçüde dağıtılmışsa (örneğin, bazıları Kuzey Amerika, bazıları Avrupa 'da), birden çok depolama hesabı kullanmayı göz önünde bulundurmanız gerekir: bir Kuzey Amerika bölgesinde bulunan diğeri Avrupa bölgesinde. Bu, her iki bölgedeki kullanıcılar için gecikme süresini azaltmaya yardımcı olur. Uygulamanın depoladığı veriler bireysel kullanıcılara özgü ise ve depolama hesapları arasında veri çoğaltmayı gerektirmiyorsa, bu yaklaşım daha kolay bir şekilde uygulanır.  Geniş içerik dağıtımı için CDN önerilir; daha fazla bilgi için sonraki bölüme bakın.  

### <a name="subheading5"></a>İçerik dağıtımı

Bazen, bir uygulamanın aynı içeriği aynı içeriğe (örneğin, bir Web sitesinin giriş sayfasında kullanılan bir ürün tanıtımı Videosu) aynı veya birden çok bölgede yer aldığı şekilde sunması gerekir. Bu senaryoda, Azure CDN gibi bir Content Delivery Network (CDN) kullanmanız gerekir ve CDN, verilerin kaynağı olarak Azure Storage 'ı kullanır. Tek bir bölgede bulunan ve diğer bölgelere düşük gecikme süresiyle içerik teslim edebilen bir Azure depolama hesabının aksine, Azure CDN dünyanın dört bir yanındaki birden fazla veri merkezinde sunucu kullanır. Ayrıca, bir CDN genellikle tek bir depolama hesabından çok daha yüksek çıkış limitlerini destekleyebilir.  

Azure CDN hakkında daha fazla bilgi için bkz. [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>SAS ve CORS kullanma

Bir kullanıcının Web tarayıcısında JavaScript gibi bir kodun veya bir cep telefonu uygulamasının Azure Storage 'daki verilere erişmesi için kimlik doğrulaması yapmanız gerektiğinde, bir yaklaşım, Web rolünde bir uygulamayı ara sunucu olarak kullanmalıdır: kullanıcının cihazı Web rolüyle kimliğini doğrular , bu da depolama kaynaklarına erişim yetkisi verir. Bu şekilde, depolama hesabı anahtarlarınızı güvenli olmayan cihazlarda açığa çıkarmaktan kaçınabilirsiniz. Ancak, bu, kullanıcının cihazı ile depolama hizmeti arasında aktarılan tüm verilerin web rolünden geçmesi gerektiğinden Web rolüne önemli bir ek yük koyar. Paylaşılan erişim Imzalarını (SAS) kullanarak, bazı durumlarda, bazen çıkış noktaları arası kaynak paylaşımı üstbilgileri (CORS) ile birlikte, bir Web rolünü depolama hizmeti için proxy olarak kullanmaktan kaçınabilirsiniz. SAS kullanarak, Kullanıcı cihazının sınırlı erişim belirteci aracılığıyla doğrudan bir depolama hizmetine istek yapmasına izin verebilirsiniz. Örneğin, bir Kullanıcı uygulamanıza fotoğraf yüklemek isterse, Web rolünüzün bir sonraki 30 dakika (SAS belirtecinin süresi dolduktan sonra) için belirli bir blob veya kapsayıcıya yazma izni veren bir SAS belirteci kullanıcı cihazına oluşturabilir ve gönderebilir.

Normalde, bir tarayıcı, bir etki alanındaki Web sitesi tarafından barındırılan bir sayfada JavaScript 'e, başka bir etki alanına "PUT" gibi belirli işlemleri gerçekleştirmek için izin vermez. Örneğin, "contosomarketing.cloudapp.net" konumunda bir Web rolü barındırdıysanız ve "contosoproducts.blob.core.windows.net" konumundaki depolama hesabınıza bir blob yüklemek için istemci tarafı JavaScript kullanmak istiyorsanız, "aynı kaynak ilkesi" tarayıcıları bu işlemi yeniden kullanır. CORS, hedef etki alanının (Bu durumda depolama hesabı) kaynak etki alanında (Bu durumda Web rolü) gelen isteklere güvendiği tarayıcıyla iletişim kurmasını sağlayan bir tarayıcı özelliğidir.  

Bu teknolojilerin her ikisi de Web uygulamanızda gereksiz yük (ve performans sorunlarını) önlemenize yardımcı olabilir.  

#### <a name="useful-resources"></a>Yararlı kaynaklar

SAS hakkında daha fazla bilgi için bkz [. paylaşılan erişim imzaları, 1. Bölüm: SAS modelini](../storage-dotnet-shared-access-signature-part-1.md)anlama.  

CORS hakkında daha fazla bilgi için bkz. [Azure depolama hizmetleri Için çıkış noktaları arası kaynak paylaşımı (CORS) desteği](https://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Önbelleğe alma

#### <a name="subheading7"></a>Veri alma

Genel olarak, bir hizmetten bir kez veri almak, iki kez alınması daha iyidir. Bir kullanıcıya içerik olarak hizmet vermek için depolama hizmetinden 50-MB blobu zaten alınmış bir Web rolünde çalışan bir MVC web uygulaması örneğini göz önünde bulundurun. Uygulama, bir kullanıcı tarafından her istekte aynı blobu alabilir ya da bunu diske yerel olarak önbelleğe alabilir ve sonraki Kullanıcı istekleri için önbelleğe alınmış sürümü yeniden kullanabilir. Ayrıca, bir kullanıcı verileri her istediğinde, değişiklik zaman için koşullu bir üst bilgiyle alma verebilir, bu da değişiklik yapılmadıysa tüm Blobun alınmasını önler. Tablo varlıklarıyla çalışmak için bu kalıbı de uygulayabilirsiniz.  

Bazı durumlarda, uygulamanızın blob 'u aldıktan sonra kısa bir süre için geçerli kalacağını varsaymasına ve bu süre boyunca uygulamanın Blobun değiştirilip değiştirilmediğini denetlemesi gerekmediğine karar verebilirsiniz.

Uygulama tarafından her zaman kullanılan yapılandırma, arama ve diğer veriler, önbelleğe alma için harika adaylardır.  

Koşullu indirmeler hakkında daha fazla bilgi için bkz. [BLOB hizmeti işlemleri için koşullu üstbilgiler belirtme](/rest/api/storageservices/specifying-conditional-headers-for-blob-service-operations).  

#### <a name="subheading8"></a>Toplu işlemlere veri yükleme

Bazı uygulama senaryolarında, verileri yerel olarak toplayabilir ve sonra her bir veri parçasını hemen karşıya yüklemek yerine bir toplu işe düzenli olarak yükleyebilirsiniz. Örneğin, bir Web uygulaması etkinliklerin günlük dosyasını tutabilir: uygulama, her etkinliğin ayrıntılarını bir tablo varlığı (çok sayıda depolama işlemi gerektiren) gibi) karşıya yükleyebilir ya da etkinlik ayrıntılarını yerel bir günlük dosyasına kaydedebilir ve ardından Tüm etkinlik ayrıntılarını düzenli olarak bir blob 'a ayrılmış bir dosya olarak karşıya yükleyin. Her günlük girişinin boyutu 1 KB ise, tek bir "put blobu" işleminde binlerce karşıya yükleyebilirsiniz (tek bir işlemde boyutu 64 MB 'a kadar olan bir blobu yükleyebilirsiniz). Yerel makine karşıya yüklemeden önce kilitlenirse, bazı günlük verilerini kaybedebilirsiniz: uygulama geliştiricisi, istemci cihazı veya karşıya yükleme hatalarının olasılığını tasarlaması gerekir.  Etkinlik verilerinin zaman içinde zaman bölmeleri (tek bir etkinlik değil) için indirilmesi gerekiyorsa, Bloblar tablo üzerinde önerilir.

### <a name="net-configuration"></a>.NET yapılandırması

.NET Framework kullanılıyorsa, bu bölümde önemli performans iyileştirmeleri yapmak için kullanabileceğiniz çeşitli hızlı yapılandırma ayarları listelenir.  Diğer dilleri kullanıyorsanız, seçtiğiniz dilde benzer kavramların uygulayıp uygulamamın olup olmadığını kontrol edin.  

#### <a name="subheading9"></a>Varsayılan bağlantı sınırını artır

.NET ' te aşağıdaki kod, varsayılan bağlantı sınırını (genellikle bir istemci ortamında 2 veya bir sunucu ortamında 10 ' a) 100 olarak artırır. Genellikle, değerini uygulamanız tarafından kullanılan iş parçacığı sayısı için yaklaşık olarak ayarlamanız gerekir.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Herhangi bir bağlantı açmadan önce bağlantı sınırını ayarlamanız gerekir.  

Diğer programlama dilleri için, bağlantı sınırının nasıl ayarlanacağını öğrenmek için bu dilin belgelerine bakın.  

Daha fazla bilgi için bkz. blog gönderisi [Web Hizmetleri: Eşzamanlı bağlantılar](https://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>Zaman uyumsuz görevlerle zaman uyumlu kod kullanıyorsanız, en az sayıda iş parçacığı artırın

Bu kod, iş parçacığı havuzundaki en düşük iş parçacığı sayısını artırır:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  
```

Daha fazla bilgi için bkz. [ThreadPool. SetMinThreads metodu](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>.NET 4,5 ve üzeri atık toplamanın avantajlarından yararlanın

İstemci uygulaması için, sunucu atık toplamada performans geliştirmelerinden yararlanmak üzere .NET 4,5 veya üstünü kullanın.

Daha fazla bilgi için [.net 4,5 'Deki performans Iyileştirmelerine genel bakış](https://msdn.microsoft.com/magazine/hh882452.aspx)makalesine bakın.  

### <a name="subheading12"></a>Sınırlandırılmamış paralellik

Paralellik performansı performans için harika hale getirilmiş olsa da, verileri karşıya yüklemek veya indirmek için sınırsız paralellik (iş parçacığı sayısı ve/veya paralel istekler için sınır yoktur) ve birden çok bölüme erişmek için birden fazla çalışan kullanarak (kapsayıcılar, kuyruklar veya Tablo bölümleri) aynı depolama hesabında veya aynı bölümdeki birden çok öğeye erişmek için. Paralellik sınırsız ise, uygulamanız istemci cihazının yeteneklerini aşabilir veya depolama hesabının ölçeklenebilirlik hedefleri, daha uzun gecikme süreleri ve azaltmaya neden olabilir.  

### <a name="subheading13"></a>Depolama Istemci kitaplıkları ve araçları

Her zaman en son Microsoft tarafından sunulan istemci kitaplıklarını ve araçları kullanın. Yazma sırasında, .NET, Windows Phone, Windows Çalışma Zamanı, Java ve C++diğer dillere ait önizleme kitaplıklarında kullanılabilen istemci kitaplıkları vardır. Ayrıca, Microsoft, Azure depolama ile çalışmak için PowerShell cmdlet 'leri ve Azure CLı komutları yayımlamıştır. Microsoft bu araçları göz önünde bulundurularak etkin bir şekilde geliştirir, en son hizmet sürümleriyle güncel tutar ve kendini kanıtlamış performans uygulamalarının birçoğunu dahili olarak işlemesini sağlar.  

### <a name="retries"></a>Yeniden deneme sayısı

#### <a name="subheading14"></a>Daraltma ve sunucu meşgul hataları

Bazı durumlarda, depolama hizmeti uygulamanızı kısıtlayabilir veya bazı geçici bir durum nedeniyle isteği yalnızca bir "503 sunucu meşgul" iletisi veya "500 zaman aşımı" döndürecek şekilde veremeyebilirsiniz.  Bu durum, uygulamanız ölçeklenebilirlik hedeflerinin herhangi birine yaklaşıyorsa veya sistem bölümlenmiş verilerinizi daha yüksek aktarım hızına izin verecek şekilde yeniden dengelerse meydana gelebilir.  İstemci uygulaması genellikle bu tür bir hataya neden olan işlemi yeniden denemelidir: aynı isteğin daha sonra denenmesine daha sonra başarılı olabilir. Ancak, ölçeklenebilirlik hedeflerini aştığından depolama hizmeti uygulamanızı azaltsa veya hizmet isteği başka bir nedenle hizmet veremese bile, agresif denemeler genellikle sorunu daha kötüleştir. Bu nedenle, bir üstel geri kapatma (istemci kitaplıklarının varsayılan davranışını bu davranışa) kullanmanız gerekir. Örneğin, uygulamanız 2 saniye sonra, 4 saniye, sonra 10 saniye, 30 saniye sonra yeniden deneyebilir ve ardından tamamen daha fazla verebilir. Bu davranış, uygulamanızın herhangi bir sorun exacerbating değil, hizmetin yükünü önemli ölçüde azaltmasına neden olur.  

Bağlantı hataları, azaltma sonucu olmadığı ve geçici olması beklenen için hemen yeniden denenebilir.  

#### <a name="subheading15"></a>Yeniden denenmeyen hatalar

İstemci kitaplıkları, hangi hataların yeniden deneneceğini ve hangilerinin hangilerinin olduğunu bilmez. Ancak, depolama REST API karşı kendi kodunuzu yazıyorsanız, yeniden denenmemelisiniz bazı hatalar olduğunu unutmayın: Örneğin, 400 (Hatalı Istek) yanıtı, istemci uygulamanın, olmadığı için işlenemeyen bir istek gönderdiğini gösterir beklenen bir biçimde. Bu isteği yeniden gönderme işlemi her seferinde aynı yanıtı elde eder, bu nedenle yeniden denendiğinde hiçbir nokta yoktur. Depolama REST API karşı kendi kodunuzu yazıyorsanız, hata kodlarının ne anlama geldiğini ve bunların her biri için yeniden deneme (veya Not) yolunu unutmayın.  

#### <a name="useful-resources"></a>Yararlı kaynaklar

Depolama hata kodları hakkında daha fazla bilgi için bkz. Microsoft Azure Web sitesindeki [durum ve hata kodları](https://msdn.microsoft.com/library/azure/dd179382.aspx) .  

## <a name="blobs"></a>Bloblar

Daha önce açıklanan [tüm hizmetler](#allservices) için kanıtlanmış uygulamalara ek olarak, aşağıdaki kanıtlanmış yöntemler özellikle blob hizmeti için geçerlidir.  

### <a name="blob-specific-scalability-targets"></a>Blob 'a özgü ölçeklenebilirlik hedefleri

#### <a name="subheading46"></a>Aynı anda tek bir nesneye erişen birden çok istemci

Aynı anda tek bir nesneye erişen çok sayıda istemciniz varsa, nesne ve depolama hesabı ölçeklenebilirlik hedefleri başına göz önünde bulundurmanız gerekir. Tek bir nesneye erişebilen istemcilerin tam sayısı, nesneyi aynı anda isteyen istemci sayısı, nesne boyutu, ağ koşulları vb. gibi etkenlere bağlı olarak değişir.

Nesne, bir Web sitesinden sunulan resimler veya videolar gibi bir CDN aracılığıyla dağıtılıyorsa, CDN kullanmanız gerekir. [Buraya](#subheading5)bakın.

Verilerin gizli olduğu bilimsel benzetimler gibi diğer senaryolarda iki seçeneğiniz vardır. Birincisi, nesneye aynı anda erişilmek üzere bir süre içinde erişilmek üzere iş yükünüzün erişimini şaşırtmakta. Alternatif olarak, nesneyi geçici olarak birden fazla depolama hesabına kopyalayabilir, böylece nesne başına ve depolama hesaplarında toplam ıOPS 'yi artırabilirsiniz. Sınırlı testte, 25 GB 'lık 100 bir blob 'u aynı anda paralel olarak indirecek (her VM, indirme iş parçacıkları 32 kullanılarak indirmeyi paralelleştirdi) bulduk. Nesneye erişmesi gereken 100 istemciniz varsa, önce onu ikinci bir depolama hesabına kopyalayın ve ardından ilk 50 VM 'Lerin ilk bloba erişmesini ve ikinci 50 VM 'Leri ikinci bloba erişmesini sağlayabilirsiniz. Sonuçlar uygulamalarınızın davranışına bağlı olarak farklılık gösterir, bu sayede tasarımı sırasında bunu test etmeniz gerekir. 

#### <a name="subheading16"></a>Blob başına bant genişliği ve işlemler

En fazla 60 MB/saniye olan tek bir blobu okuyabilir veya yazabilirsiniz (Bu yaklaşık 480 Mbps 'dir ve bu, çoğu istemci tarafı ağın yeteneklerini (istemci cihazdaki fiziksel NIC dahil) aşıyor. Ayrıca, tek bir blob saniyede en fazla 500 isteği destekler. Aynı blobu okuması gereken birden fazla istemciniz varsa ve bu sınırları aşarsanız, blobu dağıtmak için bir CDN kullanmayı göz önünde bulundurmanız gerekir.  

Bloblara yönelik hedef verimlilik hakkında daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Blob 'ları kopyalama ve taşıma

#### <a name="subheading17"></a>Blobu Kopyala

Depolama REST API sürüm 2012-02-12, Blobları hesaplar arasında kopyalamak için yararlı bir özelliği kullanıma sunmuştur: bir istemci uygulaması, depolama hizmetine başka bir kaynaktan (muhtemelen farklı bir depolama hesabında) bir blob kopyalamasını ve sonra da hizmetin gerçekleştirmesine izin vermek için talimat verebilir kopyayı zaman uyumsuz olarak Kopyala. Bu, verileri indirmek ve karşıya yüklemek zorunda olmadığınızdan, uygulama için gereken bant genişliğini önemli ölçüde azaltabilir.  

Ancak, depolama hesapları arasında kopyalama yaparken, kopyalamanın tamamlanmasında hiçbir zaman garantisi olmaması, ancak bir noktadır. Uygulamanızın denetiminizin altında bir blob kopyası tamamlaması gerekiyorsa, bir VM 'ye indirerek ve sonra da hedefe yükleyerek blobu kopyalamak daha iyi olabilir.  Bu durumda tam öngörülebilirlik için, kopyanın aynı Azure bölgesinde çalışan bir VM tarafından gerçekleştirildiğinden emin olun, aksi takdirde ağ koşulları da (ve muhtemelen) kopyalama performansınızı etkileyebilir.  Ayrıca, zaman uyumsuz bir kopyanın ilerlemesini programlı bir şekilde izleyebilirsiniz.  

Aynı depolama hesabı içindeki kopyaların kendisi genellikle hızlı tamamlanır.  

Daha fazla bilgi için bkz. [blobu kopyalama](https://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>AzCopy kullanma

Azure depolama ekibi, depolama hesaplarına birçok blobunun,, ve bunların tamamında toplu aktarma konusunda yardımcı olmak amacıyla "AzCopy" komut satırı aracını yayımladı.  Bu araç, bu senaryo için en iyi duruma getirilmiştir ve yüksek aktarım ücretleri elde edebilir.  Kullanımı toplu karşıya yükleme, indirme ve kopyalama senaryoları için önerilir. Bunun hakkında daha fazla bilgi edinmek ve indirmek için bkz. [AzCopy komut satırı yardımcı programıyla veri aktarma](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Azure Içeri/dışarı aktarma hizmeti

Büyük hacimde veri (1 TB 'tan fazla) için Azure depolama, sabit sürücüler sunarak blob depolamadan karşıya yükleme ve indirme olanağı sağlayan Içeri/dışarı aktarma hizmeti sunar.  Verilerinizi bir sabit sürücüye yerleştirebilir ve karşıya yükleme için Microsoft 'a gönderebilir ya da verileri indirmek için Microsoft 'a boş bir sabit sürücü gönderebilirsiniz.  Daha fazla bilgi için bkz. [Blob Depolama'ya Veri Aktarmak için Microsoft Azure İçeri/Dışarı Aktarma Hizmetini Kullanma](../storage-import-export-service.md).  Bu, ağ üzerinden bu veri hacminin karşıya yüklenmesi/indirilmesiyle çok daha verimli olabilir.  

### <a name="subheading20"></a>Meta verileri kullan

Blob hizmeti, blob hakkında meta veriler içerebilen baş isteklerini destekler. Örneğin, uygulamanız EXIF verilerini bir fotoğrafta gerektirdiğinde fotoğrafı alabilir ve ayıklayabilir. Bant genişliğini kaydetmek ve performansı artırmak için, uygulamanız fotoğrafı karşıya yüklerken uygulamanız EXIF verilerini Blobun meta verilerinde saklayabilir: yalnızca bir baş istek kullanarak EXIF verilerini meta verilerde alabilir, önemli bant genişliğini kaydedebilir ve blob her okunışında EXIF verilerini ayıklamak için gereken işleme süresi. Bu, bir Blobun tam içeriğini değil yalnızca meta verilere ihtiyaç duyduğunuz senaryolarda yararlı olabilir.  Blob başına yalnızca 8 KB 'lık meta veri depolanabilir (hizmet bundan daha fazlasını depolamak için bir istek kabul etmez), bu nedenle veriler bu boyuta uymuyorsa Bu yaklaşımı kullanmeyebilirsiniz.  

### <a name="rapid-uploading"></a>Hızlı karşıya yükleme

Blob 'ları hızlı bir şekilde yüklemek için answer 'a ilk soru: bir blob veya çok sayıda karşıya yükleniyor musunuz?  Senaryonuza bağlı olarak kullanılacak doğru yöntemi öğrenmek için aşağıdaki kılavuzu kullanın.  

#### <a name="subheading21"></a>Tek bir büyük blobu hızla karşıya yükleme

Tek bir büyük Blobun hızlı bir şekilde karşıya yüklemek için, istemci uygulamanızın blokları veya sayfalarını paralel olarak (tek blob 'lar ve depolama hesabı için bir bütün olarak ölçeklenebilirlik hedefleri) karşıya yüklemesi gerekir.  Resmi Microsoft tarafından sunulan RTM depolama Istemci kitaplıkları (.NET, Java) bunu yapma imkanına sahiptir.  Kitaplıkların her biri için, eşzamanlılık düzeyini ayarlamak için aşağıdaki belirtilen nesneyi/özelliği kullanın:  

* .NET: Kullanılacak bir BlobRequestOptions nesnesinde ParallelOperationThreadCount değerini ayarlayın.
* Java/Android: BlobRequestOptions. setConcurrentRequestCount () kullanın
* Node. js: İstek seçeneklerinde veya blob hizmetinde parallelOperationThreadCount kullanın.
* C++: Blob_request_options:: set_parallelism_factor yöntemini kullanın.

#### <a name="subheading22"></a>Birçok blob hızla karşıya yükleniyor

Birçok blobu hızlıca karşıya yüklemek için Blobları paralel olarak karşıya yükleyin. Bu, aynı anda tek blob 'ları, paralel blok yüklemeleri ile karşıya yüklemeden daha hızlıdır. bu nedenle, yükleme işlemini depolama hizmetinin birden çok bölümü arasında yayar. Tek bir blob yalnızca 60 MB/saniye aktarım hızını destekler (yaklaşık 480 Mbps). Yazma sırasında, ABD tabanlı LRS hesabı, tek bir blob tarafından desteklenen aktarım hızına göre çok daha fazla 20 Gbps girişi destekler.  [AzCopy](#subheading18) , yüklemeler için varsayılan olarak paralel olarak çalışır ve bu senaryo için önerilir.  

### <a name="subheading23"></a>Doğru blob türünü seçme

Azure depolama iki tür blobu destekler: *sayfa* Blobları ve *blok* Blobları. Belirli bir kullanım senaryosunda, blob türü seçiminiz çözümünüzün performansını ve ölçeklenebilirliğini etkileyecektir. Blok Blobları, büyük miktarlarda verileri verimli bir şekilde yüklemek istediğinizde uygundur: Örneğin, bir istemci uygulamanın, BLOB depolama alanına fotoğraf veya video yüklemesi gerekebilir. Uygulamanın verilerde rastgele yazma işlemleri yapması gerekiyorsa sayfa Blobları uygundur: Örneğin, Azure VHD 'leri sayfa Blobları olarak depolanır.  

Daha fazla bilgi için bkz. [blok bloblarını, ekleme bloblarını ve sayfa Bloblarını anlama](https://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Tablolar

Daha önce açıklanan [tüm hizmetler](#allservices) için kanıtlanmış uygulamalara ek olarak, aşağıdaki kanıtlanmış yöntemler özellikle tablo hizmeti için geçerlidir.  

### <a name="subheading24"></a>Tabloya özgü ölçeklenebilirlik hedefleri

Tüm depolama hesabının bant genişliği kısıtlamalarına ek olarak, tablolarda aşağıdaki belirli ölçeklenebilirlik limiti vardır.  Trafiğiniz arttıkça sistem yük dengelemeye başlayacaktır, ancak trafiğiniz ani bursts içeriyorsa, bu üretilen iş hacmini hemen alamıyoruz.  Düzeniniz bursts içeriyorsa, depolama hizmeti otomatik olarak tablonuzun dengeleneceği sırada, veri bloğu sırasında azaltma ve/veya zaman aşımlarını görmeyi beklemelisiniz.  Yavaş yavaş artırma işlemi, sistem zamanının uygun şekilde yük dengelemesine verdiği için genellikle daha iyi sonuçlar elde etmenizi sağlar.  

#### <a name="entities-per-second-storage-account"></a>Saniyedeki varlık sayısı (depolama hesabı)

Tablolara erişim için ölçeklenebilirlik sınırı, her bir hesap için saniyede 20.000 varlık (1 KB her) kadar olur.  Genel olarak, eklenen, güncellenen, silinen veya taranan her varlık bu hedefe doğru sayılır.  Bu nedenle 100 varlıkları içeren bir toplu ekleme, 100 varlık olarak sayılır.  1000 varlıklarını tarayan ve 5 değerini döndüren bir sorgu, 1000 varlık olarak sayılır.  

#### <a name="entities-per-second-partition"></a>Saniyedeki varlık sayısı (bölüm)

Tek bir bölümde, tablolara erişim için ölçeklenebilirlik hedefi, önceki bölümde açıklanan şekilde aynı sayımı kullanarak saniyede 2.000 varlıklardır (1 KB her).  

### <a name="configuration"></a>Yapılandırma

Bu bölümde, tablo hizmetinde önemli performans iyileştirmeleri yapmak için kullanabileceğiniz çeşitli hızlı yapılandırma ayarları listelenmektedir:  

#### <a name="subheading25"></a>JSON kullanma

Storage hizmeti sürüm 2013-08-15 ' den başlayarak tablo hizmeti, tablo verilerini aktarmak için XML tabanlı AtomPub biçimi yerine JSON kullanımını destekler. Bu, yük boyutunu% 75 kadar azaltabilir ve uygulamanızın performansını önemli ölçüde geliştirebilirler.

Daha fazla bilgi için bkz. Gönderi [Microsoft Azure tabloları: ](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) [Tablo hizmeti işlemlerine yönelik JSON ve yük biçimi](https://msdn.microsoft.com/library/azure/dn535600.aspx)ile tanışın.

#### <a name="subheading26"></a>Nagle 'ı devre dışı bırak

Nagle 'ın algoritması, TCP/IP ağlarında yaygın olarak, ağ performansının geliştirilmesi için bir yol olarak uygulanır. Ancak, tüm koşullarda en uygun değildir (yüksek oranda etkileşimli ortamlar gibi). Azure depolama için, Nagle 'ın algoritması tablo ve kuyruk hizmetlerine yönelik isteklerin performansı üzerinde olumsuz bir etkiye sahiptir ve mümkünse onu devre dışı bırakmanız gerekir.

### <a name="schema"></a>Şema

Verilerinizi nasıl temsil edersiniz ve sorgular, tablo hizmetinin performansını etkileyen en büyük tek etmendir. Her uygulama farklı olsa da, bu bölümde ilgili genel olarak kanıtlanmış bazı yöntemler özetlenmektedir:  

* Tablo tasarımı
* Etkili sorgular
* Verimli veri güncelleştirmeleri  

#### <a name="subheading27"></a>Tablolar ve bölümler

Tablolar bölümlere ayrılmıştır. Bir bölümde depolanan her varlık aynı bölüm anahtarını paylaşır ve bu bölüm içinde tanımlamak için benzersiz bir satır anahtarına sahiptir. Bölümler avantajlar sağlar, ancak ölçeklenebilirlik sınırları da getirir.  

* Avantajlar: Aynı bölümdeki varlıkları, en fazla 100 ayrı depolama işlemi (Toplam 4 MB 'lık sınır) içeren tek bir atomik, toplu işlemde güncelleştirebilirsiniz. Aynı sayıda varlık alınması varsayıldığında, tek bir bölümdeki verileri bölümlere yayılan verilerden daha verimli bir şekilde sorgulayabilirsiniz (ancak tablo verilerini sorgulama hakkında daha fazla öneri için okumaya devam edebilirsiniz).
* Ölçeklenebilirlik sınırı: Bölümler atomik toplu işlem işlemlerini desteklediği için tek bir bölümde depolanan varlıklara erişim yük dengeli olamaz. Bu nedenle, tek bir tablo bölümünün ölçeklenebilirlik hedefi tablo hizmeti 'nin tamamına göre daha düşüktür.  

Bu tablo ve bölümlerin bu özellikleri nedeniyle, aşağıdaki tasarım ilkelerini benimsemelisiniz:  

* İstemci uygulamanızın aynı mantıksal çalışma biriminde sıkça güncelleştirildiği veya sorgulandığı veriler aynı bölümde bulunmalıdır.  Bunun nedeni, uygulamanızın yazmaları birleştirme, ya da atomik toplu işlem işlemlerinden yararlanmak isteyebilirsiniz.  Ayrıca, tek bir bölümdeki veriler, bölümler genelinde verilerin tek bir sorgusunda daha verimli bir şekilde sorgulanmasını sağlar.
* İstemci uygulamanızın aynı mantıksal iş birimi (tek sorgu veya toplu güncelleştirme) içinde ekleme/güncelleştirme veya sorgu olmadığı veriler ayrı bölümlerde yer almalıdır.  Önemli bir not, tek bir tablodaki bölüm anahtarlarının sayısı için bir sınır olmaması, bu nedenle milyonlarca bölüm anahtarında sorun olmaması ve performansı etkilemeyecektir.  Örneğin, uygulamanız kullanıcı oturum açma içeren popüler bir Web sitesidir, bölüm anahtarı olarak kullanıcı KIMLIĞINI kullanmak iyi bir seçenek olabilir.  

#### <a name="hot-partitions"></a>Sık kullanılan bölümler

Etkin bir bölüm, bir hesaba giden trafiğin orantısız yüzdesini alan ve tek bir bölüm olduğundan yük dengeli bir bölümdür.  Genel olarak, sık kullanılan bölümler iki şekilde oluşturulur:  

##### <a name="subheading28"></a>Yalnızca sonuna ekle ve yalnızca önüne Ekle desenleri

"Yalnızca Append" deseninin belirli bir PK trafiğinin tümünün (veya neredeyse hepsi) geçerli saate göre arttığı veya azaldığı bir tane vardır.  Uygulamanızın günlük verileri için bir bölüm anahtarı olarak geçerli tarihi kullanbir örnek bir örnektir.  Bu, tüm eklemelerin tablonuzun son bölümüne gittiğinden oluşur ve tüm yazmam tablonuzun sonuna gittiğinden sistem yük dengelenemez.  Bu bölüme giden trafik hacmi bölüm düzeyi ölçeklenebilirlik hedefini aşarsa, azaltma ile sonuçlanır.  Trafiğin, isteklerin tablo genelinde yük dengelenmesi için birden çok bölüme gönderilmesini sağlamak daha iyidir.  

##### <a name="subheading29"></a>Yüksek trafik verileri

Bölümleme düzeniniz yalnızca diğer bölümlerden daha fazla kullanılan verilerin bulunduğu tek bir bölüm ile sonuçlanırsa, bu bölüm tek bir bölüm için ölçeklenebilirlik hedefine yaklaşırsa azaltma da görebilirsiniz.  Bölüm şemanızın ölçeklenebilirlik hedeflerine yaklaştığı tek bir bölüm içermediğinden emin olmak daha iyidir.  

#### <a name="querying"></a>Sorgulama

Bu bölümde, tablo hizmetini sorgulamak için kanıtlanmış uygulamalar açıklanmaktadır.  

##### <a name="subheading30"></a>Sorgu kapsamı

Sorgulanacak varlık aralığını belirtmek için birkaç yol vardır.  Aşağıda, her birinin kullanımları hakkında bir tartışma verilmiştir.  

Genel olarak, taramalardan kaçının (tek bir varlıktan daha büyük olan sorgular), ancak tarama yapmanız gerekirse, taramalarınızın ihtiyaç duymadığınız önemli miktarda varlığı taramadan veya döndürmeksizin ihtiyacınız olan verileri alabilmesi için verilerinizi düzenlemeyi deneyin.  

###### <a name="point-queries"></a>Nokta sorguları

Nokta sorgusu tam olarak bir varlık alır. Bunu, alınacak varlığın bölüm anahtarını ve satır anahtarını belirterek yapar. Bu sorgular verimlidir ve bunları mümkün olan yerlerde kullanmanız gerekir.  

###### <a name="partition-queries"></a>Bölüm sorguları

Bölüm sorgusu, ortak bir bölüm anahtarını paylaşan bir veri kümesini alan bir sorgudur. Genellikle sorgu, bölüm anahtarına ek olarak, bazı varlık özelliklerine ilişkin bir dizi satır anahtarı veya bir değer aralığı belirtir. Bunlar nokta sorgularından daha verimlidir ve çok dikkatli kullanılmalıdır.  

###### <a name="table-queries"></a>Tablo sorguları

Tablo sorgusu, ortak bir bölüm anahtarını paylaşmayan bir varlık kümesini alan bir sorgudur. Bu sorgular etkili değildir ve mümkünse bunları kullanmaktan kaçının.  

##### <a name="subheading31"></a>Sorgu yoğunluğu

Sorgu verimliliği ' nda başka bir anahtar faktörü, döndürülen kümeyi bulmak için taranan varlık sayısına kıyasla döndürülen varlıkların sayısıdır. Uygulamanız veri paylaşımlarının yalnızca% 1 ' i olan özellik değeri için filtre içeren bir tablo sorgusu gerçekleştirdiğinde, sorgu döndürdüğü her bir varlık için 100 varlıklarını tarar. Daha önce ele alınan tablo ölçeklenebilirliği hedefleri, döndürülen varlıkların sayısıyla değil, taranan varlık sayısıyla ilgilidir: düşük bir sorgu yoğunluğu, tablo hizmetinin uygulamanızı daha fazla sayıda varlık taraması gerektiği için kolayca genişletmesine neden olabilir. Aradığınız varlığı alın.  Bundan kaçınmak hakkında daha fazla [bilgi için aşağıdaki](#subheading34) bölüme bakın.  

##### <a name="limiting-the-amount-of-data-returned"></a>Döndürülen veri miktarını sınırlama

###### <a name="subheading32"></a>Menin

Bir sorgunun istemci uygulamada ihtiyaç duymayacak varlıkları döndürmeyeceğini Bildiğiniz yerde, döndürülen küme boyutunu azaltmak için bir filtre kullanmayı düşünün. İstemciye döndürülmediği varlıklar hala ölçeklenebilirlik sınırlarına yaklaşmaya devam ederken, daha az ağ yükü boyutu ve istemci uygulamanızın işlemesi gereken daha az sayıda varlık nedeniyle uygulama performansından iyileşecek.  Bkz. [sorgu yoğunluğu](#subheading31)üzerine yukarıdaki notta. ancak, ölçeklenebilirlik hedefleri taranan varlık sayısıyla ilgilidir, bu nedenle çok sayıda varlığı filtreleyen bir sorgu, birkaç varlık döndürülse bile yine de azaltmasına neden olabilir.  

###### <a name="subheading33"></a>Yansıtma

İstemci uygulamanız, tablonuzdaki varlıklardan yalnızca sınırlı bir özellik kümesine ihtiyaç duyuyorsa, döndürülen veri kümesinin boyutunu sınırlamak için projeksiyonu kullanabilirsiniz. Filtrelemede olduğu gibi, bu, ağ yükü ve istemci işlemesini azaltmaya yardımcı olur.  

##### <a name="subheading34"></a>Normalleştirilmemiş

İlişkisel veritabanlarıyla çalışmaktan farklı olarak, tablo verilerinin etkin bir şekilde sorgulanmasına yönelik kanıtlanmış uygulamalar, verilerinizin normalleştirilmesi için Diğer bir deyişle, bir sorgunun,, uygulamanızın verileri bulmak için çok sayıda varlığı taramak zorunda kalmadan, istemci ihtiyacı olan verileri bulmak için taraması gereken varlıkların sayısını en aza indirmek için, aynı verileri birden çok varlıkta çoğaltma (verileri bulmak için kullanabileceğiniz her anahtar için bir tane) uygulaması gerekiyor.  Örneğin, bir e-ticaret Web sitesinde, hem müşteri KIMLIĞI hem de (bu müşterinin emirlerini bana) ve tarihe göre (bir tarih için bana sipariş siparişi ver) bir sipariş bulmak isteyebilirsiniz.  Tablo depolama alanında, müşteri KIMLIĞINE göre bulmayı kolaylaştırmak için bir kez tablo adı, PK ve RK ile her iki kez bir kez (veya bir başvuru) depolamak en iyi yöntemdir.  

#### <a name="insertupdatedelete"></a>Ekle/güncelleştir/Sil

Bu bölümde, tablo hizmetinde depolanan varlıkları değiştirmeye yönelik kanıtlanmış uygulamalar açıklanmaktadır.  

##### <a name="subheading35"></a>İşlem

Toplu işlemler, Azure Storage 'da varlık grubu Işlemleri (ETG) olarak bilinir; bir ETG içindeki tüm işlemler tek bir tabloda tek bir bölümde olmalıdır. Mümkün olduğunda, toplu olarak ekleme, güncelleştirme ve silme işlemleri gerçekleştirmek için ETG 'leri kullanın. Bu, istemci uygulamanızdan sunucuya gidiş dönüş sayısını azaltır, faturalandırılabilir işlem sayısını (Faturalandırma amacıyla tek bir işlem olarak sayar ve en fazla 100 depolama işlemi içerebilir) ve atomik güncelleştirmeleri sağlar (tümü işlemler başarılı veya bir ETG içinde başarısız olur). Mobil cihazlar gibi yüksek gecikme sürelerine sahip ortamlar, ETG 'leri kullanmaktan büyük ölçüde yararlanır.  

##### <a name="subheading36"></a>Upsert

Mümkün olan her yerde tablo ön **Ekle** işlemlerini kullanın. Her ikisi de geleneksel bir **ekleme** ve **güncelleştirme** işlemlerinden daha verimli olabilecek iki tür **upsert**vardır:  

* **Insertormerge**: Varlığın özelliklerinin bir alt kümesini karşıya yüklemek istediğinizde bunu kullanın, ancak varlığın zaten var olup olmadığından emin olun. Varlık varsa, bu çağrı, **upsert** işleme dahil olan özellikleri güncelleştirir ve tüm mevcut özellikleri olduğu gibi bırakır; varlık yoksa, yeni varlığı ekler. Bu, bir sorguda projeksiyon kullanılmasına benzerdir, ancak yalnızca değişen özellikleri karşıya yüklemeniz yeterlidir.
* **Insertorreplace**: Tamamen yeni bir varlık yüklemek istediğinizde bunu kullanın, ancak zaten var olup olmadığından emin değilsiniz. Bunu yalnızca yeni yüklenen varlığın tamamen doğru olduğunu bildiğiniz durumlarda kullanmanız gerekir çünkü eski varlığın üzerine yazar. Örneğin, uygulamanın daha önce Kullanıcı için konum verilerini depoladığına bakılmaksızın kullanıcının geçerli konumunu depolayan varlığı güncelleştirmek istersiniz; Yeni konum varlığı tamamlanmıştır ve önceki varlıklardan herhangi bir bilgi almanız gerekmez.

##### <a name="subheading37"></a>Veri serisini tek bir varlıkta depolama

Bazen, bir uygulama, sıklıkla tümünü bir kez almak için ihtiyaç duyduğunu bir veri serisini depolar: Örneğin, bir uygulama, son 24 saat içindeki verilerin sıralı bir grafiğini çizmek için zaman içinde CPU kullanımını izleyebilir. Tek bir yaklaşım, her bir varlığa belirli bir saati temsil eden ve bu saat için CPU kullanımını depolayan her bir bir saat için bir adet tablo varlığına sahiptir. Bu verileri çizmek için, uygulamanın verileri en son 24 saatten tutan varlıkları alması gerekir.  

Alternatif olarak, uygulamanız her saat için CPU kullanımını tek bir varlığın ayrı bir özelliği olarak saklayabilir: her saat güncelleştirmek için, uygulamanız en son saatin değerini güncelleştirmek üzere tek bir **ınsertormerge upsert** çağrısı kullanabilir. Verilerin çizilmesi için, uygulamanın yalnızca 24 yerine tek bir varlık alması gerekir (etkin bir sorgu için bkz. [sorgu kapsamında](#subheading30)tartışmayı yukarıdaki tartışmaya bakın).

##### <a name="subheading38"></a>Yapılandırılmış verileri bloblarda depolama

Bazen benzer şekilde yapılandırılmış veriler tablolara gitmelidir, ancak varlık aralıkları her zaman birlikte alınır ve toplu olarak eklenebilir.  Bunun iyi bir örneği bir günlük dosyasıdır.  Bu durumda, birkaç dakikalık günlüğü toplu olarak çalıştırabilirsiniz, ekleyebilir ve aynı zamanda birkaç dakikalık günlüğü her zaman alabilirsiniz.  Bu durumda, performans için, yazılan/döndürülen nesne sayısını ve genellikle yapılması gereken istek sayısını önemli ölçüde azaltabileceğinizden, tablolar yerine blob 'ları kullanmak daha iyidir.  

## <a name="queues"></a>Kuyruklar

Daha önce açıklanan [tüm hizmetler](#allservices) için kanıtlanmış uygulamalara ek olarak, aşağıdaki kanıtlanmış uygulamalar özellikle kuyruk hizmeti için geçerlidir.  

### <a name="subheading39"></a>Ölçeklenebilirlik sınırları

Tek bir sıra, yaklaşık 2.000 iletiyi (her biri bir saniyede bir ileti olarak) (her AddMessage, GetMessage ve DeleteMessage Count) işleyebilir. Bu uygulama için yeterli değilse, birden çok kuyruk kullanmanız ve iletileri bunlara yaymalısınız.  

[Azure depolama ölçeklenebilirlik ve performans hedefleri '](storage-scalability-targets.md)nde geçerli ölçeklenebilirlik hedeflerini görüntüleyin.  

### <a name="subheading40"></a>Nagle 'ı devre dışı bırak

Nagle algoritmasını etkileyen tablo yapılandırması bölümüne bakın; Nagle algoritması sıra isteklerinin performansı için genellikle hatalı olur ve devre dışı bırakmanız gerekir.  

### <a name="subheading41"></a>İleti boyutu

İleti boyutu arttıkça kuyruk performansı ve ölçeklenebilirlik azalmasını azaltır. Yalnızca alıcının ihtiyacı olan bilgileri bir iletiye yerleştirmeniz gerekir.  

### <a name="subheading42"></a>Toplu iş alımı

Tek bir işlemde bir kuyruktan en fazla 32 ileti alabilirsiniz. Bu, özellikle mobil cihazlar gibi yüksek gecikme süresine sahip olan ortamlar için yararlı olan istemci uygulamasından gelen gidiş dönüş sayısını azaltabilir.  

### <a name="subheading43"></a>Sıra yoklama aralığı

Çoğu uygulama, bu uygulama için en büyük işlem kaynaklarından biri olabilen bir kuyruktaki iletileri yoklamalıdır. Yoklama sıklığından daha seyrek seçim yapın: çok sık yoklama, uygulamanızın sıraya yönelik ölçeklenebilirlik hedeflerine yaklaşımına neden olabilir. Ancak, $0,01 (yazma sırasında) için 200.000 işlem sırasında, ayda her saniye bir kez tek bir işlemci yoklaması 15 aldan düşük bir değer olduğundan, maliyet genellikle yoklama aralığı seçiminizi etkileyen bir faktör değildir.  

Güncel maliyet bilgileri için bkz. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>Güncelleştirme iletisi

**Güncelleştirme iletisi** işlemini, bir iletinin durum bilgilerini artırmak veya bir iletinin durum bilgilerini güncelleştirmek için kullanabilirsiniz. Bu güçlü bir işlem olsa da, her bir **güncelleştirme iletisi** işleminin ölçeklenebilirlik hedefine doğru olduğunu unutmayın. Ancak, işin her adımı tamamlandıktan sonra, bir işi bir kuyruktan bir sonrakine geçiren iş akışına sahip olmanın çok daha etkili bir yaklaşımı olabilir. **Iletiyi Güncelleştir** işleminin kullanılması, uygulamanızın iş durumunu iletiye kaydetmesini ve sonra bir adım her tamamlandığında işin bir sonraki adımına requeuing yerine çalışmaya devam etmesini sağlar.  

Daha fazla bilgi için bkz [. nasıl yapılır: Sıraya alınan iletinin](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message)içeriğini değiştirin.  

### <a name="subheading45"></a>Uygulama mimarisi

Uygulama mimarinizi ölçeklenebilir hale getirmek için kuyrukları kullanmanız gerekir. Aşağıda, uygulamanızı daha ölçeklenebilir hale getirmek için kuyrukları kullanabileceğiniz bazı yollar listelenmiştir:  

* Uygulamanızdaki iş yüklerini işlemek ve düzgünleştirmek için biriktirme listeleri oluşturmak üzere kuyrukları kullanabilirsiniz. Örneğin, karşıya yüklenen görüntüleri yeniden boyutlandırma gibi işlemci yoğunluklu işleri gerçekleştirmek için kullanıcılardan gelen istekleri sıraya alabilirsiniz.
* Sıralarını bağımsız olarak ölçeklendirebilmeniz için, uygulamanızın parçalarını ayırmak için kuyrukları kullanabilirsiniz. Örneğin, bir Web ön ucu, daha sonra analiz ve depolama için kullanıcılardan anket sonuçlarını bir kuyruğa yerleştirebilir. Sıra verilerini gerektiği gibi işlemek için daha fazla çalışan rolü örneği ekleyebilirsiniz.  

## <a name="conclusion"></a>Sonuç

Bu makalede, Azure Storage kullanırken performansı iyileştirmek için en yaygın ve kanıtlanmış uygulamalardan bazıları ele alınmıştır. Biz, her uygulama geliştiricisine, yukarıda belirtilen yöntemlerin her biri ile uygulamalarını değerlendirmelerini ve Azure Depolama kullanan uygulamalarından mükemmel performans alma önerilerini dikkate almalarını öneririz.
