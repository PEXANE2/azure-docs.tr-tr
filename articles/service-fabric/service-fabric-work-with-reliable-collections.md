---
title: Güvenilir Koleksiyonlar ile çalışma
description: Azure Hizmet Kumaşı uygulamasında Güvenilir Koleksiyonlarla çalışmak için en iyi uygulamaları öğrenin.
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 94836a37a62e3eeffb94d891980cc02694bd973e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81409799"
---
# <a name="working-with-reliable-collections"></a>Güvenilir Koleksiyonlar ile çalışma
Service Fabric, Güvenilir Koleksiyonlar aracılığıyla .NET geliştiricileri için özel bir programlama modeli sunar. Özellikle, Hizmet Kumaş güvenilir sözlük ve güvenilir kuyruk sınıfları sağlar. Bu sınıfları kullandığınızda, durumunuz (ölçeklenebilirlik için), çoğaltılır (kullanılabilirlik için) ve bir bölüm içinde (ACID semantics için) işlenir. Güvenilir bir sözlük nesnesinin tipik kullanımına bakalım ve gerçekte ne yaptığını görelim.

```csharp
try
{
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction())
   {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync isn't called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException)
{
   // choose how to handle the situation where you couldn't get a lock on the file because it was 
   // already in use. You might delay and retry the operation
}
```

Güvenilir sözlük nesneleri üzerindeki tüm işlemler (geri alınmayan ClearAsync hariç), bir ITransaction nesnesi gerektirir. Bu nesne, tek bir bölüm içinde herhangi bir güvenilir sözlük ve / veya güvenilir sıra nesneleri yapmak için çalışıyoruz herhangi bir ve tüm değişiklikleri ile ilişkili. Bölümün StateManager'ın CreateTransaction yöntemini arayarak bir ITransaction nesnesi elde edersiniz.

Yukarıdaki kodda, ITransaction nesnesi güvenilir bir sözlüğün AddAsync yöntemine geçirilir. Dahili olarak, bir anahtarı kabul eden sözlük yöntemleri, anahtarla ilişkili bir okuyucu/yazar kilidi alır. Yöntem anahtarın değerini değiştirirse, yöntem anahtara yazma kilidi alır ve yöntem yalnızca anahtarın değerinden okursa, anahtara okuma kilidi alınır. AddAsync anahtarın değerini yeni, geçirilen değere göre değiştirir, anahtarın yazma kilidi alınır. Bu nedenle, 2 (veya daha fazla) iş parçacığı aynı anda aynı anahtarla değerler eklemeyi denerse, bir iş parçacığı yazma kilidi kazanır ve diğer iş parçacıkları engeller. Varsayılan olarak, yöntemler kilidi elde etmek için 4 saniyeye kadar bloke eder; 4 saniye sonra, yöntemler bir TimeoutException atmak. Yöntem aşırı yüklemeleri, isterseniz açık bir zaman atlama değerini geçirmenize olanak sağlar.

Genellikle, bir TimeoutException'a tepki vermek için kodunuzu yakalar ve tüm işlemi yeniden deneyerek yazarsınız (yukarıdaki kodda gösterildiği gibi). Basit kodumda, task.delay'ı her seferinde 100 milisaniye yitirmek için arıyorum. Ama, gerçekte, bunun yerine üstel geri tepme gecikme bir tür kullanarak daha iyi olabilir.

Kilit alındıktan sonra AddAsync, ITransaction nesnesiyle ilişkili bir iç geçici sözlük için anahtar ve değer nesnesi başvurularını ekler. Bu okuma-your-kendi-kendi-semantik ile sağlamak için yapılır. Diğer bir tarihte, AddAsync'i aradıktan sonra, tryGetValueAsync'e daha sonra yapılan bir arama (aynı ITransaction nesnesini kullanarak) işlemi henüz işlememiş olsanız bile değeri döndürecektir. Ardından, AddAsync anahtar ve değer nesnelerinizi bayt dizilerine serileştirir ve bu bayt dizilerini yerel düğümdeki bir günlük dosyasına ekler. Son olarak, AddAsync tüm ikincil yinelemelere bayt dizilerini gönderir, böylece aynı anahtar/değer bilgilerine sahip olurlar. Anahtar/değer bilgileri bir günlük dosyasına yazılmış olsa bile, ilişkili oldukları işlem işlenene kadar bilgiler sözlüğün bir parçası olarak kabul edilmez.

Yukarıdaki kodda, CommitAsync çağrısı işlemin tüm işlemlerini işler. Özellikle, yerel düğümdeki günlük dosyasına bilgi acar ve ayrıca taahhüt kaydını tüm ikincil yinelemelere gönderir. Çoğaltmaların çoğunluğu yanıtlandıktan sonra, tüm veri değişiklikleri kalıcı olarak kabul edilir ve iTransaction nesnesi üzerinden manipüle edilen anahtarlarla ilişkili kilitler serbest bırakılır, böylece diğer iş parçacıkları/işlemler aynı anahtarları ve değerlerini manipüle edebilir.

CommitAsync çağrılmazsa (genellikle bir özel durum atıldığından), ITransaction nesnesi atılır. Kaydedilmemiş bir ITransaction nesnesini devre dışı bırakırken, Service Fabric yerel düğümün günlük dosyasına iptal bilgilerini ekler ve ikincil yinelemelerin hiçbirine gönderilmesi gerekmez. Ve sonra, işlem yoluyla manipüle edilen anahtarlarla ilişkili kilitler serbest bırakılır.

## <a name="volatile-reliable-collections"></a>Uçucu Güvenilir Koleksiyonlar 
Örneğin çoğaltılmış önbellek gibi bazı iş yüklerinde, zaman zaman veri kaybı tolere edilebilir. Verilerin diske kalıcılığından kaçınmak, Güvenilir Sözlüklere yazarken daha iyi gecikmelere ve iş aralarına izin verebilir. Kalıcılık eksikliği için tradeoff çoğunluk kaybı oluşursa, tam veri kaybı oluşacak olmasıdır. Çoğunluk kaybı nadir bir olay olduğundan, artan performans bu iş yükleri için nadir veri kaybı olasılığına değer olabilir.

Şu anda, geçici destek yalnızca Güvenilir Sözlükler ve Güvenilir Kuyruklar için kullanılabilir ve Güvenilir Eşzamanlı Kuyruklar için kullanılamaz. Geçici koleksiyonları kullanıp kullanmayacağınıza karar vermek için lütfen [Uyarılar](service-fabric-reliable-services-reliable-collections-guidelines.md#volatile-reliable-collections) listesine bakın.

Hizmetinizde geçici desteği etkinleştirmek ```HasPersistedState``` için, ```false```hizmet türü bildirimindeki bayrağı ( aşağıdaki gibi) ayarlayın:
```xml
<StatefulServiceType ServiceTypeName="MyServiceType" HasPersistedState="false" />
```

>[!NOTE]
>Varolan kalıcı hizmetler geçici hale getirilemez ve bunun tersi de geçerlidir. Bunu yapmak isterseniz, varolan hizmeti silmeniz ve ardından hizmeti güncelleştirilmiş bayrakla dağıtmanız gerekir. Bu, ```HasPersistedState``` bayrağı değiştirmek istiyorsanız tam veri kaybına maruz kalmak istediğiniz anlamına gelir. 

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Ortak tuzaklar ve nasıl önlemek için
Artık güvenilir koleksiyonların dahili olarak nasıl çalıştığını anladığınıza göre, bunların bazı yaygın yanlış kullanımlarına bir göz atalım. Aşağıdaki koda bakın:

```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property's value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

Normal bir .NET sözlüğüyle çalışırken, sözlüğe bir anahtar/değer ekleyebilir ve ardından bir özelliğin değerini (LastLogin gibi) değiştirebilirsiniz. Ancak, bu kod güvenilir bir sözlük ile doğru çalışmaz. Önceki tartışmadan, AddAsync çağrısının anahtar/değer nesnelerini bayt dizilerine serileştirip dizileri yerel bir dosyaya kaydedip ikincil yinelemelere gönderdiğini unutmayın. Daha sonra bir özelliği değiştirirseniz, bu özellik yalnızca bellekteki değerini değiştirir; yerel dosyayı veya yinelemelere gönderilen verileri etkilemez. İşlem çökerse, bellekteki ler atılır. Yeni bir işlem başlatıldığında veya başka bir yineleme birincil olduğunda, kullanılabilir olan eski özellik değeridir.

Yukarıda gösterilen hatayı yapmak ne kadar kolay olduğunu yeterince vurgulayamıyorum. Ve, sadece hata hakkında öğreneceksiniz eğer / süreç aşağı giderse. Kodu yazmanın doğru yolu, iki satırı tersine çevirmektir:


```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Burada ortak bir hata gösteren başka bir örnektir:

```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   // Use the user's name to look up their data
   ConditionalValue<User> user = await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue)
   {
      // The line below updates the property's value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

Yine, normal .NET sözlükleri ile, yukarıdaki kod iyi çalışır ve ortak bir desendir: geliştirici bir değer aramak için bir anahtar kullanır. Değer varsa, geliştirici bir özelliğin değerini değiştirir. Ancak, güvenilir koleksiyonları ile, bu kod zaten tartışılan aynı sorunu sergiler: **güvenilir bir koleksiyona verdikten sonra bir nesne değiştirmeniz gerekir.**

Güvenilir bir koleksiyondaki bir değeri güncelleştirmenin doğru yolu, varolan değere bir başvuru almak ve bu başvuru tarafından atıfta bulunulan nesneyi değişmez olarak değerlendirmektir. Ardından, özgün nesnenin tam kopyası olan yeni bir nesne oluşturun. Şimdi, bu yeni nesnenin durumunu değiştirebilir ve yeni nesneyi koleksiyona yazarak yerel dosyaya eklenen ve yinelemelere gönderilen bayt dizilerine seri hale getirebilirsiniz. Değişiklik(ler) işledikten sonra, bellek nesneleri, yerel dosya ve tüm yinelemeler aynı tam duruma sahiptir. Her şey yolunda!

Aşağıdaki kod, güvenilir bir koleksiyondaki bir değeri güncelleştirmenin doğru yolunu gösterir:

```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   // Use the user's name to look up their data
   ConditionalValue<User> currentUser = await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue)
   {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key's value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);
      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Programcı hatasını önlemek için değişmez veri türlerini tanımlama
İdeal olarak, derleyicinin, değişmez olarak düşünmeniz gereken bir nesnenin durumunu mutasyona uğratması gereken bir kod yanlışlıkla ürettiğinizde hataları bildirmesini isteriz. Ancak, C# derleyicisinin bunu yapma yeteneği yoktur. Bu nedenle, olası programcı hatalarından kaçınmak için, güvenilir koleksiyonlarla kullandığınız türleri değişmez türler olarak tanımlamanızı şiddetle öneririz. Özellikle, bu temel değer türlerine (sayılar [Int32, UInt64, vb], DateTime, Guid, TimeSpan ve benzeri gibi sopa anlamına gelir. String'i de kullanabilirsiniz. Toplama özelliklerini serihale ve deserializing sık sık performansa zarar verebilir gibi önlemek için en iyisidir. Ancak, toplama özelliklerini kullanmak istiyorsanız, 'nin kullanımını şiddetle öneririz. NET değişmez koleksiyonları kitaplığı ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Bu kitaplık' tan https://nuget.orgindirilebilir. Ayrıca, sınıflarınızı mühürlemenizi ve alanları mümkün olduğunca yalnızca okunabilir hale getirmenizi öneririz.

Aşağıdaki UserInfo türü, yukarıda belirtilen önerilerden yararlanarak değişmez bir türün nasıl tanımlanabildiğini gösterir.

```csharp
[DataContract]
// If you don't seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo
{
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) 
   {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context)
   {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId)
   {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

ItemId türü de burada gösterildiği gibi değişmez bir türüdür:

```csharp
[DataContract]
public struct ItemId
{
   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName)
   {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a>Şema sürüm (yükseltmeleri)
Dahili olarak, Güvenilir Koleksiyonlar kullanarak nesneleri serihale. NET'in DataContractSerializer. Serileştirilmiş nesneler birincil yinelemenin yerel diskine kalıcı olarak sürülür ve ikincil yinelemelere de aktarılır. Hizmetiniz olgunlaştıkça, büyük olasılıkla hizmetinizin gerektirdiği veri türünü (şema) değiştirmek isteyeceksiniz. Verilerinizin sürümüne büyük bir özenle yaklaşın. Her şeyden önce, her zaman eski verileri deserialize gerekir. Özellikle, bu deserialization kodu sonsuz geri uyumlu olması gerektiği anlamına gelir: Sürüm 333 hizmet kodunun 5 yıl önce sürüm 1 tarafından güvenilir bir koleksiyona yerleştirilen veriler üzerinde çalışması gerekir.

Ayrıca, hizmet kodu bir defada bir yükseltme etki alanı yükseltilir. Bu nedenle, yükseltme sırasında servis kodunuzu aynı anda çalışan iki farklı sürümü vardır. Hizmet kodunun yeni sürümünün yeni şemayı kullanmasından kaçınmanız gerekir, çünkü hizmet kodunuzu eski sürümleri yeni şemayı kaldıramayabilir. Mümkün olduğunda, hizmetinizin her sürümünü tek bir sürümle uyumlu olacak şekilde tasarlamanız gerekir. Özellikle, bu, hizmet kodunuzdaki V1'in açıkça işlemediği şema öğelerini yok sayabilmesi gerektiği anlamına gelir. Ancak, açıkça bilmediği verileri kaydedebilmeli ve sözlük anahtarını veya değerini güncellerken yeniden yazabilmelidir.

> [!WARNING]
> Bir anahtarın şemasını değiştirebilirsiniz, ancak anahtarınızın karma kodu ve eşit algoritmalarının kararlı olduğundan emin olmalısınız. Bu algoritmalardan herhangi birinin çalışma şeklini değiştirirseniz, anahtarı güvenilir sözlükiçinde bir daha asla aramazsınız.
> .NET Strings bir anahtar olarak kullanılabilir, ancak dize kendisini anahtar olarak kullanır--String.GetHashCode sonucunu anahtar olarak kullanmayın.

Alternatif olarak, genellikle iki yükseltme olarak adlandırılan gerçekleştirebilirsiniz. İki aşamalı yükseltme ile hizmetinizi V1'den V2'ye yükseltirsiniz: V2, yeni şema değişikliğiyle nasıl başa çıkacağını bilen kodu içerir, ancak bu kod yürütülmez. V2 kodu V1 verilerini okuduğunda, üzerinde çalışır ve V1 verileri yazar. Daha sonra, yükseltme tüm yükseltme etki alanlarında tamamlandıktan sonra, bir şekilde çalışan V2 örneklerine yükseltmenin tamamladığını işaret edebilirsiniz. (Bunu sinyal vermenin bir yolu bir yapılandırma yükseltmesi yapmaktır; bu iki aşamalı bir yükseltme yapar.) Şimdi, V2 örnekleri V1 verilerini okuyabilir, V2 verilerine dönüştürebilir, üzerinde çalışabilir ve V2 verisi olarak yazabilir. Diğer örnekler V2 verilerini okuduğunda, dönüştürmelerine gerek yoktur, sadece üzerinde çalışırlar ve V2 verilerini yazarlar.

## <a name="next-steps"></a>Sonraki adımlar
İleriye uyumlu veri sözleşmeleri oluşturma hakkında bilgi edinmek için [bkz.](https://msdn.microsoft.com/library/ms731083.aspx)

Veri sözleşmelerini sürümleme konusunda en iyi uygulamaları öğrenmek için [bkz.](https://msdn.microsoft.com/library/ms731138.aspx)

Sürüm toleranslı veri sözleşmelerinin nasıl uygulanacağını öğrenmek için, [Sürüm Toleranslı Serileştirme Geri Aramaları'na](https://msdn.microsoft.com/library/ms733734.aspx) bakın

Birden çok sürümde birlikte çalışabilen bir veri yapısının nasıl sağlayabileceğini öğrenmek için [Bkz. IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx)
