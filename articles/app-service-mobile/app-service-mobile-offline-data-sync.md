---
title: Çevrimdışı veri eşitleme
description: Azure Mobile Apps için çevrimdışı veri eşitleme özelliğine kavramsal başvuru ve genel bakış
author: conceptdev
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.openlocfilehash: 0cc4309fa57a29997bdd2f650634efd0723e6965
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458758"
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Azure Mobile Apps’te Çevrimdışı Veri Eşitleme

## <a name="what-is-offline-data-sync"></a>Çevrimdışı veri eşitleme nedir?
Çevrimdışı veri eşitleme, geliştiricilerin bir ağ bağlantısı olmadan işlevsel uygulamalar oluşturmasını kolaylaştıran bir Azure Mobile Apps istemci ve sunucu SDK özelliğidir.

Uygulamanız çevrimdışı modda olduğunda, yine de bir yerel depoya kaydedilen verileri oluşturabilir ve değiştirebilirsiniz. Uygulama yeniden çevrimiçi olduğunda, yerel değişiklikleri Azure Mobil uygulama arka ucunuz ile eşitlenebilir. Bu özellik, hem istemcide hem de arka uçta aynı kayıt değiştirildiğinde çakışmaların algılanmasının desteğini de içerir. Çakışmalar daha sonra sunucuda veya istemcide işlenebilir.

Çevrimdışı eşitlemenin çeşitli avantajları vardır:

* Sunucu verilerini yerel olarak cihazda önbelleğe alarak uygulama yanıt hızını geliştirme
* Ağ sorunları olduğunda faydalı olmaya devam eden sağlam uygulamalar oluşturun
* Son kullanıcıların, ağ erişimi olmadığında bile veri oluşturmalarına ve değiştirmesine izin ver, çok az veya hiç bağlantı olan senaryolar destekleme
* Birden çok cihaz genelinde verileri eşitleyin ve aynı kayıt iki cihaz tarafından değiştirildiğinde çakışmaları tespit edin
* Yüksek Gecikmeli veya tarifeli ağlarda ağ kullanımını sınırlayın

Aşağıdaki öğreticilerde, Azure Mobile Apps kullanarak mobil istemcilerinize nasıl çevrimdışı eşitleme ekleyeceğiniz gösterilmektedir:

* [Android: çevrimdışı eşitlemeyi etkinleştir]
* [Apache Cordova: çevrimdışı eşitlemeyi etkinleştir](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: çevrimdışı eşitlemeyi etkinleştir]
* [Xamarin iOS: çevrimdışı eşitlemeyi etkinleştir]
* [Xamarin Android: çevrimdışı eşitlemeyi etkinleştir]
* [Xamarin. Forms: çevrimdışı eşitlemeyi etkinleştir](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Evrensel Windows Platformu: çevrimdışı eşitlemeyi etkinleştir]

## <a name="what-is-a-sync-table"></a>Eşitleme tablosu nedir?
"/Tables" uç noktasına erişmek için Azure mobil istemci SDK 'Ları `IMobileServiceTable` (.NET istemci SDK) veya `MSTable` (iOS istemcisi) gibi arabirimler sağlar. Bu API 'Ler doğrudan Azure Mobil uygulama arka ucuna bağlanır ve istemci cihazının bir ağ bağlantısı yoksa başarısız olur.

Çevrimdışı kullanımı desteklemek için uygulamanızın bunun yerine `IMobileServiceSyncTable` (.NET istemci SDK) veya `MSSyncTable` (iOS istemcisi) gibi *Eşitleme tablosu* API 'lerini kullanması gerekir. Aynı CRUD işlemleri (oluşturma, okuma, güncelleştirme, silme), eşitleme tablosu API 'Lerine göre çalışarak, artık *yerel bir mağazadan*okudukları veya yazdıkları durumlar hariç. Tüm eşitleme tablosu işlemleri gerçekleştirilemeden önce, yerel deponun başlatılmış olması gerekir.

## <a name="what-is-a-local-store"></a>Yerel depo nedir?
Yerel bir depo, istemci cihazında veri Kalıcılık katmanıdır. Azure Mobile Apps istemci SDK 'Ları varsayılan bir yerel mağaza uygulamasını sağlar. Windows, Xamarin ve Android 'de, SQLite temel alır. İOS 'ta temel verileri temel alır.

Windows Phone veya Microsoft Store üzerinde SQLite tabanlı uygulama kullanmak için bir SQLite uzantısı yüklemeniz gerekir. Daha fazla bilgi için bkz. [Evrensel Windows Platformu: çevrimdışı eşitlemeyi etkinleştir]. Android ve iOS, cihaz işletim sisteminde bir SQLite sürümü ile birlikte gönderilir ve bu nedenle kendi SQLite sürümüne başvurmak gerekli değildir.

Geliştiriciler, kendi yerel mağazalarını da uygulayabilir. Örneğin, verileri mobil istemcide şifreli bir biçimde depolamak isterseniz, şifreleme için SQLCipher kullanan bir yerel depo tanımlayabilirsiniz.

## <a name="what-is-a-sync-context"></a>Eşitleme bağlamı nedir?
Bir *eşitleme bağlamı* , bir mobil istemci nesnesiyle (`IMobileServiceClient` veya `MSClient`gibi) ilişkilendirilir ve eşitleme tabloları ile yapılan değişiklikleri izler. Eşitleme bağlamı, daha sonra sunucusuna gönderilen CUD işlemlerinin sıralı bir listesini (oluşturma, güncelleştirme, silme) tutan bir *işlem kuyruğu*tutar.

Yerel bir depo, [.NET istemci SDK 'Sı]`IMobileServicesSyncContext.InitializeAsync(localstore)` gibi bir başlatma yöntemi kullanılarak eşitleme bağlamıyla ilişkilendirilir.

## <a name="how-sync-works"></a>Çevrimdışı eşitlemenin nasıl çalıştığı
Eşitleme tabloları kullanılırken, yerel değişiklikler bir Azure Mobil uygulama arka ucu ile eşitlendiğinde istemci kodunuz kontrol edilir. Yerel değişiklikleri *Gönder* çağrısı olana kadar arka uca hiçbir şey gönderilmez. Benzer şekilde, yerel depo yalnızca *çekme* verileri için bir çağrı olduğunda yeni verilerle doldurulur.

* **Gönderim**: push, eşitleme bağlamındaki bir işlemdir ve son göndermeden bu yana tüm CUD değişikliklerini gönderir. Aksi takdirde, bazı işlemler sırada gönderilemediğinden yalnızca tek bir tablonun değişikliklerini gönderemeyebilir. Gönderim, Azure Mobil uygulama arka ucunuza bir dizi REST çağrısı yürütür, bu da sunucu veritabanınızı değiştirir.
* **Çekme**: çekme, tablo başına temelinde gerçekleştirilir ve sunucu verilerinin yalnızca bir alt kümesini almak için bir sorguyla özelleştirilebilir. Azure mobil istemci SDK 'Ları, sonuçta elde edilen verileri yerel depoya ekler.
* **Örtük gönderim**: bekleyen yerel güncelleştirmeleri olan bir tabloya karşı bir çekme yürütülürse, çekme önce eşitleme bağlamında bir `push()` yürütür. Bu gönderim, zaten kuyruğa alınan değişiklikler ve sunucudan yeni veriler arasındaki çakışmaların en aza indirilmesine yardımcı olur.
* **Artımlı eşitleme**: çekme işlemine ilk parametre yalnızca istemcide kullanılan bir *sorgu adıdır* . Null olmayan bir sorgu adı kullanırsanız, Azure Mobile SDK *artımlı eşitleme*gerçekleştirir. Bir çekme işlemi bir sonuç kümesi döndürdüğünde, bu sonuç kümesinden en son `updatedAt` zaman damgası SDK yerel sistem tablolarında depolanır. Sonraki çekme işlemleri yalnızca bu zaman damgasından sonraki kayıtları alır.

  Artımlı eşitleme kullanmak için sunucunuzun anlamlı `updatedAt` değerler döndürmesi ve ayrıca bu alana göre sıralamayı desteklemesi gerekir. Ancak, SDK updatedAt alanına kendi sıralamasını eklediğinden, kendi `orderBy` yan tümcesine sahip bir çekme sorgusu kullanamazsınız.

  Sorgu adı seçtiğiniz herhangi bir dize olabilir, ancak uygulamanızdaki her bir mantıksal sorgu için benzersiz olmalıdır.
  Aksi takdirde, farklı çekme işlemleri aynı artımlı eşitleme zaman damgasının üzerine yazabilir ve sorgularınız hatalı sonuçlar döndürebilir.

  Sorguda bir parametre varsa, benzersiz bir sorgu adı oluşturmanın bir yolu parametre değerini kullanmaktır.
  Örneğin, Kullanıcı kimliği ' nde C#filtrelerken, sorgu adınız şu şekilde olabilir:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Artımlı eşitleme ' yi devre dışı bırakmak istiyorsanız, `null` sorgu KIMLIĞI olarak geçirin. Bu durumda, tüm kayıtlar, potansiyel olarak verimsiz olan `PullAsync`her çağrısıyla alınır.
* **Temizleme**: `IMobileServiceSyncTable.PurgeAsync`kullanarak yerel deponun içeriğini temizleyebilirsiniz.
  İstemci veritabanında eski veriler varsa veya bekleyen tüm değişiklikleri atmak istiyorsanız Temizleme işlemi gerekebilir.

  Bir Temizleme, bir tabloyu yerel depodan temizler. Sunucu veritabanıyla eşitleme bekleyen işlemler varsa Temizleme, *temizlemeyi zorla* parametresi ayarlanmadığı takdirde bir özel durum oluşturur.

  İstemcide eski verilerin bir örneği olarak, "Todo List" örneğinde, Device1 yalnızca tamamlanmamış öğeleri çeker. "Satın alma morijinalini" bir TodoItem, sunucuda başka bir cihaz tarafından tamamlandı olarak işaretlenir. Ancak, yalnızca tamamlandı olarak işaretlenmemiş öğeleri çektiğinden, Device1 yine de yerel depoda "mılalk" TodoItem. Bir Temizleme, bu eski öğeyi temizler.

## <a name="next-steps"></a>Sonraki adımlar
* [iOS: çevrimdışı eşitlemeyi etkinleştir]
* [Xamarin iOS: çevrimdışı eşitlemeyi etkinleştir]
* [Xamarin Android: çevrimdışı eşitlemeyi etkinleştir]
* [Evrensel Windows Platformu: çevrimdışı eşitlemeyi etkinleştir]

<!-- Links -->
[.NET istemci SDK 'Sı]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: çevrimdışı eşitlemeyi etkinleştir]: app-service-mobile-android-get-started-offline-data.md
[iOS: çevrimdışı eşitlemeyi etkinleştir]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: çevrimdışı eşitlemeyi etkinleştir]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: çevrimdışı eşitlemeyi etkinleştir]: app-service-mobile-xamarin-android-get-started-offline-data.md
[Evrensel Windows Platformu: çevrimdışı eşitlemeyi etkinleştir]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
