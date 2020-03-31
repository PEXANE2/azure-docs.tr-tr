---
title: Güvenilir Aktörler devlet yönetimi
description: Güvenilir Aktörler durumunun yüksek kullanılabilirlik için nasıl yönetildiğini, kalıcı olduğunu ve çoğaltıldığını açıklar.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 9962d4333e458243670d1005ad2ccfbc0bb7c92a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75348909"
---
# <a name="reliable-actors-state-management"></a>Güvenilir Aktörler devlet yönetimi
Güvenilir Aktörler, hem mantığı hem de durumu kapsülleyebilen tek iş parçacığı nesnelerdir. Aktörler Güvenilir Hizmetler'de çalıştıklarından, aynı kalıcılık ve çoğaltma mekanizmalarını kullanarak durumu güvenilir bir şekilde koruyabilirler. Bu şekilde, aktörler hatalardan sonra, çöp toplamadan sonra yeniden etkinleştirildikten sonra veya kaynak dengeleme veya yükseltmeler nedeniyle kümedeki düğümler arasında taşındıklarında durumlarını kaybetmezler.

## <a name="state-persistence-and-replication"></a>Durum kalıcılığı ve çoğaltma
Her aktör örneği benzersiz bir kimlikle eşlenebildiği için tüm Güvenilir Aktörler *durumlu* olarak kabul edilir. Bu, aynı aktör kimliğine yapılan yinelenen çağrıların aynı aktör örneğine yönlendirileceği anlamına gelir. Buna karşılık, durum dışı bir sistemde, istemci çağrılarının her seferinde aynı sunucuya yönlendirilmesi garanti edilmez. Bu nedenle, aktör hizmetleri her zaman devlet hizmetleridir.

Aktörler devlet olarak kabul edilse de, bu devleti güvenilir bir şekilde depolamaları gerektiği anlamına gelmez. Aktörler, veri depolama gereksinimlerine göre durum kalıcılığı ve çoğaltma düzeyini seçebilir:

* **Kalıcı durum**: Durum diske kalıcıdır ve üç veya daha fazla yinelemeye çoğaltılır. Kalıcı durum, durum tam küme kesintisi ile devam edebilir en dayanıklı durum depolama seçeneğidir.
* **Geçici durum**: Durum üç veya daha fazla yinelemeye çoğaltılır ve yalnızca bellekte tutulur. Geçici durum düğüm hatası ve aktör hatasına karşı esneklik sağlar ve yükseltmeleri ve kaynak dengeleme sırasında. Ancak, durum diske kalıcı değildir. Yani tüm kopyalar aynı anda kaybolursa, devlet de kaybolur.
* **Kalıcı durum yok**: Durum çoğaltılmadı veya diske yazılmaz, yalnızca durumu güvenilir bir şekilde sürdürmesi gerekmeyen aktörler için kullanılır.

Her kalıcılık düzeyi, hizmetinizin farklı bir *durum sağlayıcısı* ve *çoğaltma* yapılandırmasıdır. Durum'un diske yazılıp yazılmadığı, durumu depolayan güvenilir bir hizmetteki bileşen olan devlet sağlayıcısına bağlıdır. Çoğaltma, bir hizmetin kaç çoğaltmayla dağıtılsa da değişir. Güvenilir Hizmetlerde olduğu gibi, hem devlet sağlayıcısı hem de çoğaltma sayısı el ile kolayca ayarlanabilir. Aktör çerçevesi, bir aktör üzerinde kullanıldığında, otomatik olarak varsayılan bir durum sağlayıcısı seçer ve otomatik olarak bu üç kalıcılık ayarlarından birini elde etmek için yineleme sayısı için ayarlar üreten bir öznitelik sağlar. StatePersistence özniteliği türetilmiş sınıf tarafından devralınmaz, her Aktör türü kendi Durum Kalıcılık düzeyini sağlamalıdır.

### <a name="persisted-state"></a>Kalıcı durum
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Bu ayar, verileri diskte depolayan ve hizmet çoğaltma sayısını otomatik olarak 3 olarak ayarlayan bir durum sağlayıcısı kullanır.

### <a name="volatile-state"></a>Uçucu durum
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Bu ayar yalnızca bellekte durum sağlayıcısı kullanır ve yineleme sayısını 3 olarak ayarlar.

### <a name="no-persisted-state"></a>Kalıcı durum yok
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Bu ayar yalnızca bellekte durum sağlayıcısı kullanır ve yineleme sayısını 1 olarak ayarlar.

### <a name="defaults-and-generated-settings"></a>Varsayılanlar ve oluşturulan ayarlar
Özniteliği kullanırken, `StatePersistence` aktör hizmeti başladığında çalışma zamanında sizin için otomatik olarak bir durum sağlayıcısı seçilir. Çoğaltma sayısı, ancak, Visual Studio aktör oluşturma araçları tarafından derleme zamanda ayarlanır. Yapı araçları, ApplicationManifest.xml'deki aktör hizmeti için otomatik olarak varsayılan bir *hizmet* oluşturur. Parametreler **min çoğaltma kümesi boyutu** ve **hedef çoğaltma kümesi boyutu**için oluşturulur.

Bu parametreleri el ile değiştirebilirsiniz. Ancak öznitelik `StatePersistence` her değiştirilse, parametreler seçili `StatePersistence` öznitelik için varsayılan yineleme kümesi boyut değerlerine ayarlanır ve önceki değerler geçersiz kılınz. Başka bir deyişle, ServiceManifest.xml'de belirlediğiniz değerler *yalnızca* öznitelik değerini `StatePersistence` değiştirdiğinizde yapı zamanında geçersiz kılınır.

```xml
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Eyalet müdürü
Her aktör örneğinin kendi durum yöneticisi vardır: anahtar/değer çiftlerini güvenilir bir şekilde depolayan sözlük benzeri bir veri yapısı. Devlet müdürü bir devlet sağlayıcı etrafında bir sarmalayıcı olduğunu. Hangi kalıcılık ayarı kullanılırsa kullanılsın verileri depolamak için kullanabilirsiniz. Çalışan bir aktör hizmetinin, verileri korurken yuvarlanan yükseltme yoluyla kalıcı bir durum ayarına geçici (yalnızca bellek) durum ayarından değiştirilebilen herhangi bir garanti sağlamaz. Ancak, çalışan bir hizmet için yineleme sayısını değiştirmek mümkündür.

Devlet yöneticisi anahtarları dizeleri olmalıdır. Değerler geneldir ve özel türler de dahil olmak üzere her türde olabilir. Çoğaltma sırasında ağ üzerinden diğer düğümlere aktarılabileceği nden ve aktörün durum kalıcılığı ayarına bağlı olarak diske yazılabileceğinden, durum yöneticisinde depolanan değerler veri sözleşmesi nin seri olarak geçirilebiliyor olması gerekir.

Devlet yöneticisi, Güvenilir Sözlük'te bulunanlara benzer şekilde durumu yönetmek için ortak sözlük yöntemlerini ortaya çıkarır.

Aktör durumunu yönetme örnekleri [için, Güvenilir Aktörler durumunu Eriş' i okuyun, kaydedin ve kaldırın.](service-fabric-reliable-actors-access-save-remove-state.md)

## <a name="best-practices"></a>En iyi uygulamalar
Aşağıda, aktör durumunuzu yönetmek için önerilen bazı uygulamalar ve sorun giderme ipuçları verilmiştir.

### <a name="make-the-actor-state-as-granular-as-possible"></a>Aktör durumunu olabildiğince ayrıntılı hale getirin
Bu, uygulamanızın performansı ve kaynak kullanımı için çok önemlidir. Bir aktörün "adlandırılmış durumuna" herhangi bir yazma/güncelleştirme olduğunda, bu "adlandırılmış duruma" karşılık gelen tüm değer seri hale getirilir ve ağ üzerinden ikincil yinelemelere gönderilir.  İkinciler yerel diske yazar ve birincil yinelemeye yanıt lar. Birincil ikincil yinelemeler bir çoğunluk bildirimleri aldığında, kendi yerel diske durumu yazar. Örneğin, değerin 20 üyesi ve 1 MB boyutuolan bir sınıf olduğunu varsayalım. Sınıf üyelerinden yalnızca birini 1 KB boyutuna göre değiştirmiş olsanız bile, serileştirme ve ağ ve disk yazma maliyetini tam 1 MB olarak ödentirsiniz. Benzer şekilde, değer bir koleksiyonsa (liste, dizi veya sözlük gibi), üyelerinden birini değiştirseniz bile koleksiyonun tamamının maliyetini ödenebilirsiniz. Aktör sınıfının StateManager arabirimi sözlük gibidir. Bu sözlüğün üstünde aktör durumunu temsil eden veri yapısını her zaman modellemeniz gerekir.
 
### <a name="correctly-manage-the-actors-life-cycle"></a>Aktörün yaşam döngüsünü doğru yönetin
Bir aktör hizmetinin her bölünmesinde durum boyutunu yönetme konusunda net bir ilke niz olmalıdır. Aktör hizmetiniz sabit sayıda aktöre sahip olmalı ve bunları mümkün olduğunca yeniden kullanmalıdır. Sürekli olarak yeni aktörler oluşturursanız, işlerini bitirdikten sonra bunları silmeniz gerekir. Aktör çerçevesi, var olan her aktör hakkında bazı meta verileri depolar. Bir aktörün tüm durumunu silerken, o aktörle ilgili meta verileri kaldırmaz. Sistemde depolanan tüm bilgileri kaldırmak için aktörü silmeniz gerekir [(bkz. aktörleri ve durumlarını silmek).](service-fabric-reliable-actors-lifecycle.md#manually-deleting-actors-and-their-state) Ek bir denetim olarak, aktör sayısının beklenen aralıkta olduğundan emin olmak için arada bir aktör hizmetini sorgulamalısınız [(numaralandırma aktörleri](service-fabric-reliable-actors-enumerate.md)görün).
 
Bir Aktör Hizmetinin veritabanı dosya boyutunun beklenen boyutun üzerinde arttığını görürseniz, önceki yönergeleri izlediğinden emin olun. Bu yönergeleri takip ediyorsanız ve hala veritabanı dosya boyutu sorunları iseniz, yardım almak için ürün ekibi ile [bir destek bileti açmanız](service-fabric-support.md) gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Güvenilir Aktörler'de depolanan durum, diske yazılmadan ve yüksek kullanılabilirlik için çoğaltılmadan önce seri hale getirilmelidir. [Aktör türü serileştirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)hakkında daha fazla bilgi edinin.

Ardından, Aktör [tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)hakkında daha fazla bilgi edinin.
