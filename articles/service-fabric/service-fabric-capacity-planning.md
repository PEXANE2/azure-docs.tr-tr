---
title: Service Fabric uygulamaları için kapasite planlaması
description: Service Fabric uygulaması için gereken bilgi işlem düğümü sayısının nasıl belirlendirilebildiğini açıklar
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: cd5a5c55ff873e4891ac63361d0c4a0b56d70109
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75377217"
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Servis Kumaşı uygulamaları için kapasite planlaması
Bu belge, Azure Hizmet Kumaşı uygulamalarınızı çalıştırmak için gereken kaynak miktarını (CPU' lar, RAM, disk depolama) nasıl tahmin edebilirsiniz öğretir. Kaynak gereksinimlerinizin zaman içinde değişmesi yaygındır. Hizmetinizi geliştirirken/test ettikçe genellikle birkaç kaynağa ihtiyaç duyarsınız ve üretime girerken ve uygulamanız popülerlik arttıkça daha fazla kaynağa ihtiyaç duyarsınız. Uygulamanızı tasarlarken, uzun vadeli gereksinimleri gözden geçirin ve hizmetinizin yüksek müşteri talebini karşılamak üzere ölçeklendirmesine olanak tanıyan seçimler yapın.

 Bir Hizmet Kumaşı kümesi oluşturduğunuzda, kümeyi ne tür sanal makinelerin (VM'ler) oluşturduğuna karar verirsiniz. Her VM, CPU'lar (çekirdek ve hız), ağ bant genişliği, RAM ve disk depolama şeklinde sınırlı miktarda kaynakla birlikte gelir. Hizmetiniz zamanla büyüdükçe, daha fazla kaynak sunan VM'lere yükseltebilir ve/veya kümenize daha fazla VM ekleyebilirsiniz. İkincisini yapmak için, kümeye dinamik olarak eklenen yeni VM'lerden yararlanabilmesi için hizmetinizi başlangıçta tasarlamalısınız.

Bazı hizmetler, VM'ler hakkında çok az veri yönetir. Bu nedenle, bu hizmetler için kapasite planlaması öncelikle performansa odaklanmalıdır, bu da VM'lerin uygun CPU'larını (çekirdek ve hızını) seçmek anlamına gelir. Ayrıca, ağ aktarımlarının ne sıklıkta oluştuğunu ve ne kadar veri aktarıldığını da içeren ağ bant genişliğini göz önünde bulundurmalısınız. Hizmet kullanımı arttıkça hizmetinizin iyi performans göstermesi gerekiyorsa, kümeye daha fazla VM ekleyebilir ve tüm VM'lerde ağ isteklerini yük bakiyesi yükleyebilirsiniz.

VM'lerde büyük miktarda veri yöneten hizmetler için kapasite planlaması öncelikle boyuta odaklanmalıdır. Bu nedenle, VM'nin RAM ve disk depolama kapasitesini dikkatlice düşünmelisiniz. Windows'daki sanal bellek yönetim sistemi, disk alanını uygulama koduna RAM gibi gösterir. Buna ek olarak, Service Fabric çalışma süresi bellekte yalnızca sıcak verileri tutarak ve soğuk verileri diske taşıma akıllı sayfalama sağlar. Bu nedenle uygulamalar VM'de fiziksel olarak bulunandan daha fazla bellek kullanabilir. VM RAM'de daha fazla disk depolama alanı tutabildiği için daha fazla RAM'e sahip olmak performansı artırır. Seçtiğiniz VM, VM'de istediğiniz verileri depolayacak kadar büyük bir diske sahip olmalıdır. Benzer şekilde, VM istediğiniz performansı sağlamak için yeterli RAM olmalıdır. Hizmetinizin verileri zamanla büyürse, kümeye daha fazla VM ekleyebilir ve verileri tüm Sanal Taşıtlar arasında bölümlere ayırabilirsiniz.

## <a name="determine-how-many-nodes-you-need"></a>Kaç düğüme ihtiyacınız olduğunu belirleme
Hizmetinizi bölümlendirmek, hizmetinizin verilerini ölçeklendirmenize olanak tanır. Bölümleme hakkında daha fazla bilgi için [Bölümleme Hizmeti](service-fabric-concepts-partitioning.md)Kumaşı'na bakın. Her bölüm tek bir VM'ye sığmalıdır, ancak tek bir VM'ye birden çok (küçük) bölüm yerleştirilebilir. Bu nedenle, daha küçük bölümlere sahip olmak, birkaç büyük bölüme sahip olmaktan daha fazla esneklik sağlar. Bunun amacı, çok sayıda bölüme sahip olmanın Hizmet Kumaşı'nı genel yükü artırması ve bölümler arasında işlem yapılan işlemleri gerçekleştirememesidir. Hizmet kodunuz sık sık farklı bölümlerde yaşayan veri parçalarına erişmesi gerekiyorsa, daha fazla potansiyel ağ trafiği de vardır. Hizmetinizi tasarlarken, etkili bir bölümleme stratejisine ulaşmak için bu artıları ve eksileri dikkatlice düşünmelisiniz.

Uygulamanızın, bir yıl içinde DB_Size GB'ye büyümesini beklediğiniz mağaza boyutuna sahip tek bir durumlu hizmete sahip olduğunu varsayalım. O yılın ötesinde büyüme deneyimi olarak daha fazla uygulama (ve bölümler) eklemek için hazırız.  Hizmetinizin yineleme sayısını belirleyen çoğaltma faktörü (RF), toplam DB_Size etkiler. Tüm çoğaltmalar arasında toplam DB_Size çoğaltma faktörü DB_Size ile çarpılır.  Node_Size, hizmetiniz için kullanmak istediğiniz düğüm başına disk alanını/RAM'i temsil eder. En iyi performans için, DB_Size küme deki belleğe sığmalıdır ve VM'nin RAM'i etrafında bir Node_Size seçilmelidir. RAM kapasitesinden daha büyük bir Node_Size ayırarak, Service Fabric çalışma zamanı tarafından sağlanan sayfalama güveniyor. Bu nedenle, tüm verileriniz sıcak kabul edilirse (o zamandan beri veriler içeri/çıkış lı olarak tanımlanır) performansınız en iyi şekilde olmayabilir. Ancak, verilerin yalnızca bir kısmının sıcak olduğu birçok hizmet için daha uygun maliyetlidir.

Maksimum performans için gereken düğüm sayısı aşağıdaki gibi hesaplanabilir:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Büyüme için hesap
Hizmetinizin büyümesini beklediğiniz DB_Size göre düğüm sayısını, başladığınız DB_Size ek olarak hesaplamak isteyebilirsiniz. Ardından, hizmetiniz arttıkça düğüm sayısını büyüterek düğüm sayısını fazla sağlamamanızı sağlayın. Ancak bölüm sayısı, hizmetinizi maksimum büyümeyle çalıştırırken gereken düğüm sayısına dayanmalıdır.

Beklenmeyen ani artışlarla veya arızalarla başa çıkabilmeniz için herhangi bir zamanda bazı ekstra makinelerin kullanılabilir olması iyidir (örneğin, birkaç VM inerse).  Ekstra kapasite, beklenen ani artışlar kullanılarak belirlenmelidir, bir başlangıç noktası birkaç ekstra VM (yüzde 5-10 ekstra) ayırmaktır.

Önceki tek bir devlet hizmeti varsayar. Birden fazla devlet hizmetiniz varsa, denkleme diğer hizmetlerle ilişkili DB_Size eklemeniz gerekir. Alternatif olarak, her devlet hizmeti için düğüm sayısını ayrı ayrı hesaplayabilirsiniz.  Hizmetinizin bakiyesi olmayan yinelemeleri veya bölümleri olabilir. Bölümlerin diğerlerinden daha fazla veriye sahip olabileceğini unutmayın. Bölümleme hakkında daha fazla bilgi için, [en iyi uygulamalar hakkında bölümleme makalesine](service-fabric-concepts-partitioning.md)bakın. Ancak, önceki denklem bölümve çoğaltma agnostik, Hizmet Kumaş ı yinelemeler optimize edilmiş bir şekilde düğümler arasında yayılmasını sağlar çünkü.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>Maliyet hesaplaması için elektronik tablo kullanma
Şimdi formüle bazı gerçek sayılar koyalım. Örnek bir [elektronik tablo,](https://github.com/Azure/service-fabric/raw/master/docs_resources/SF_VM_Cost_calculator-NEW.xlsx) üç tür veri nesnesi içeren bir uygulamanın kapasitesinin nasıl planılacağını gösterir. Her nesne için, boyutunu ve kaç nesneye sahip olmayı beklediğimizi tahmin ediyoruz. Ayrıca, her nesne türünden kaç yineleme istediğimizi de seçeriz. Elektronik tablo kümede depolanacak toplam bellek miktarını hesaplar.

Sonra bir VM boyutu ve aylık maliyet girin. Elektronik tablo, VM boyutuna bağlı olarak, verilerinizi düğümlere fiziksel olarak sığacak şekilde bölmek için kullanmanız gereken en az bölüm sayısını bildirir. Uygulamanızın özel hesaplama ve ağ trafiği gereksinimlerini karşılamak için daha fazla sayıda bölüm isteyebilirsiniz. Elektronik tablo, kullanıcı profil nesnelerini yöneten bölüm sayısının birden altıya yükseldiğini gösterir.

Şimdi, tüm bu bilgilere dayanarak, elektronik tablo fiziksel olarak 26-düğüm küme üzerinde istenen bölümler ve yinelemeler ile tüm verileri elde edebilirsiniz gösterir. Ancak, bu küme yoğun olarak paketlenmiş olur, bu nedenle düğüm hatalarını ve yükseltmeleri barındıracak bazı ek düğümler isteyebilirsiniz. Elektronik tablo da boş düğümleri olurdu çünkü 57'den fazla düğüm leri sahip ek değer sağladığını gösterir. Yine, düğüm hataları ve yükseltmeleri karşılamak için zaten 57 düğümleri üzerinde gitmek isteyebilirsiniz. Elektronik tabloyu uygulamanızın özel gereksinimleriyle eşleşecek şekilde değiştirebilirsiniz.   

![Maliyet hesaplaması için elektronik tablo][Image1]

## <a name="next-steps"></a>Sonraki adımlar
Hizmetinizi bölümleme hakkında daha fazla bilgi edinmek için [Bölümleme Hizmeti Kumaş hizmetlerine][10] göz atın.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
