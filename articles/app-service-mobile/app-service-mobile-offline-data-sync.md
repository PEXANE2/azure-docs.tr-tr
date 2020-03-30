---
title: Çevrimdışı Veri Eşitlemi
description: Azure Mobil Uygulamaları için çevrimdışı veri eşitleme özelliğine kavramsal başvuru ve genel bakış
author: conceptdev
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.openlocfilehash: 0cc4309fa57a29997bdd2f650634efd0723e6965
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458758"
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Azure Mobile Apps’te Çevrimdışı Veri Eşitleme

## <a name="what-is-offline-data-sync"></a>Çevrimdışı veri eşitlemi nedir?
Çevrimdışı veri eşitleme, Azure Mobile Apps'ın istemci ve sunucu SDK özelliğidir ve geliştiricilerin ağ bağlantısı olmadan işlevsel uygulamalar oluşturmasını kolaylaştırır.

Uygulamanız çevrimdışı moddayken, yerel bir mağazaya kaydedilen verileri oluşturabilir ve değiştirebilirsiniz. Uygulama çevrimiçi olduğunda, azure mobil uygulama arka ucunuzla yerel değişiklikleri senkronize edebilir. Özellik, aynı kayıt hem istemcide hem de arka uçta değiştirildiğinde çakışmaları algılamak için destek de içerir. Çakışmalar daha sonra sunucuda veya istemcide işlenebilir.

Çevrimdışı eşitlemenin çeşitli avantajları vardır:

* Sunucu verilerini aygıtta yerel olarak önbelleğe alarak uygulama yanıt verme yeteneğini geliştirin
* Ağ sorunları olduğunda yararlı olmaya devam eden sağlam uygulamalar oluşturun
* Ağ erişimi olmadığında bile son kullanıcıların veri oluşturmasına ve değiştirmesine izin vererek, çok az bağlantı içeren veya hiç bağlantı olmayan senaryoları destekleme
* Verileri birden çok aygıt arasında eşitleme ve aynı kayıt iki aygıt tarafından değiştirildiğinde çakışmaları algılama
* Yüksek gecikmeli veya tarifeli ağlarda ağ kullanımını sınırlama

Aşağıdaki öğreticiler, Azure Mobil Uygulamaları'nı kullanarak mobil müşterilerinize çevrimdışı eşitlemenin nasıl ekleyeceğinigösteriz:

* [Android: Çevrimdışı eşitleme etkinleştirme]
* [Apache Cordova: Çevrimdışı eşitleme etkinleştirme](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: Çevrimdışı eşitlemeyi etkinleştirme]
* [Xamarin iOS: Çevrimdışı eşitleme etkinleştirme]
* [Xamarin Android: Çevrimdışı eşitleme etkinleştirme]
* [Xamarin.Forms: Çevrimdışı eşitleme etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Evrensel Windows Platformu: Çevrimdışı eşitleme etkinleştirme]

## <a name="what-is-a-sync-table"></a>Eşitleme tablosu nedir?
Azure Mobil istemci SDK'ları "/tablolar" bitiş noktasına erişmek için `IMobileServiceTable` (.NET istemciSDK) veya `MSTable` (iOS istemcisi) gibi arabirimler sağlar. Bu API'ler doğrudan Azure Mobil Uygulaması arka ucuna bağlanır ve istemci aygıtın ağ bağlantısı yoksa başarısız olur.

Çevrimdışı kullanımı desteklemek için uygulamanızın bunun yerine *eşitleme* tablosu `IMobileServiceSyncTable` API'lerini (.NET istemciSi SDK) veya `MSSyncTable` (iOS istemcisi) gibi kullanmanız gerekir. Aynı CRUD işlemleri (Oluştur, Oku, Güncelle, Sil) eşitleme tablosu API'lerine karşı çalışır, ancak artık yerel bir *mağazadan*okur veya yazar. Herhangi bir eşitleme tablosu işlemleri gerçekleştirilemeden önce, yerel mağazanın başlatılması gerekir.

## <a name="what-is-a-local-store"></a>Yerel mağaza nedir?
Yerel bir depo istemci aygıtındaki veri kalıcılık katmanıdır. Azure Mobil Apps istemci SDK'ları varsayılan bir yerel mağaza uygulaması sağlar. Windows, Xamarin ve Android, Bu SQLite dayanmaktadır. iOS'ta Çekirdek Verilere dayanır.

Windows Phone veya Microsoft Mağazası'nda SQLite tabanlı uygulamayı kullanmak için bir SQLite uzantısı yüklemeniz gerekir. Daha fazla bilgi için Evrensel [Windows Platformu: Çevrimdışı eşitlemeyi etkinleştirin.] Android ve iOS cihaz işletim sisteminin kendisi SQLite bir sürümü ile gemi, bu yüzden SQLite kendi sürümü referans gerekli değildir.

Geliştiriciler kendi yerel mağazalarını da uygulayabilir. Örneğin, verileri mobil istemcide şifreli bir biçimde depolamak istiyorsanız, şifreleme için SQLCipher kullanan yerel bir depo tanımlayabilirsiniz.

## <a name="what-is-a-sync-context"></a>Eşitleme bağlamı nedir?
*Eşitleme bağlamı* mobil istemci nesnesiyle `IMobileServiceClient` (örneğin veya) `MSClient`ilişkilidir ve eşitleme tablolarıyla yapılan değişiklikleri izler. Eşitleme bağlamı, daha sonra sunucuya gönderilen CUD işlemlerinin (Oluştur, Güncelleştir, Sil) sıralı bir listesini tutan bir *işlem kuyruğu*tutar.

Yerel bir [mağaza,.NET istemcisi SDK] `IMobileServicesSyncContext.InitializeAsync(localstore)` gibi bir başlatma yöntemi ni kullanarak eşitleme bağlamıyla ilişkilidir.

## <a name="how-offline-synchronization-works"></a><a name="how-sync-works"></a>Çevrimdışı eşitleme nasıl çalışır?
Eşitleme tablolarını kullanırken, yerel değişiklikler Azure Mobil Uygulaması arka uçla eşitlendiğinde istemci kodunuz denetler. Yerel değişiklikleri *zorlamak* için bir çağrı olana kadar hiçbir şey arka uca gönderilmez. Benzer şekilde, yerel depo yalnızca veri *çekme* çağrısı olduğunda yeni verilerle doldurulur.

* **Push**: Push eşitleme bağlamında bir işlemdir ve son itme den bu yana tüm CUD değişiklikleri gönderir. Aksi takdirde işlemler sıradışı gönderilebildiği için yalnızca tek bir tablonun değişikliklerini göndermenin mümkün olmadığını unutmayın. Push, Azure Mobil Uygulama arka uçunuzda bir dizi REST çağrısı yürütür ve bu da sunucu veritabanınızı değiştirir.
* **Pull**: Pull, her tablo bazında gerçekleştirilir ve sunucu verilerinin yalnızca bir alt kümesini almak için bir sorguyla özelleştirilebilir. Azure Mobile istemci SDK'ları, elde edilen verileri yerel mağazaya ekler.
* **Örtük İtmeler**: Çekme bekleyen yerel güncelleştirmeleri olan bir tabloya `push()` karşı yürütülürse, çekme önce eşitleme bağlamında bir yürütme yi yürütür. Bu itme, zaten sıralanmış olan değişikliklerle sunucudan gelen yeni veriler arasındaki çakışmaları en aza indirmeye yardımcı olur.
* **Artımlı Eşitleme**: çekme işleminin ilk parametresi yalnızca istemcide kullanılan bir *sorgu adıdır.* Null olmayan bir sorgu adı kullanıyorsanız, Azure Mobil SDK *artımlı*bir eşitleme gerçekleştirir. Bir çekme işlemi bir dizi sonuç döndürderken, bu sonuç kümesinden en son `updatedAt` zaman damgası SDK yerel sistem tablolarında depolanır. Sonraki çekme işlemleri yalnızca bu zaman damgası sonra kayıtları alır.

  Artımlı eşitleme kullanmak için sunucunuzun anlamlı `updatedAt` değerler döndürmesi ve bu alana göre sıralamayı desteklemesi gerekir. Ancak, SDK güncelleştirilmişAt alanına kendi sıralamasını eklediklerinden, kendi `orderBy` yan tümcesi olan bir çekme sorgusu kullanamazsınız.

  Sorgu adı seçtiğiniz herhangi bir dize olabilir, ancak uygulamanızdaki her mantıksal sorgu için benzersiz olmalıdır.
  Aksi takdirde, farklı çekme işlemleri aynı artımlı eşitleme zaman damgasının üzerine yazabilir ve sorgularınız yanlış sonuçlar döndürebilir.

  Sorgunun bir parametresi varsa, benzersiz bir sorgu adı oluşturmanın bir yolu parametre değerini birleştirmektir.
  Örneğin, userid üzerinde filtreleme iseniz, sorgu adınız aşağıdaki gibi olabilir (C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Artımlı eşitlemeyi devre dışı bırakmak `null` istiyorsanız, sorgu kimliği olarak geçirin. Bu durumda, tüm kayıtlar, potansiyel olarak `PullAsync`verimsiz olan her çağrıda alınır.
* **Tasfiye**: Yerel mağazanın içeriğini kullanarak `IMobileServiceSyncTable.PurgeAsync`temizleyebilirsiniz.
  İstemci veritabanında eski verileriniz varsa veya bekleyen tüm değişiklikleri atmak istiyorsanız temizleme gerekebilir.

  Bir tasfiye yerel mağazadan bir tablo temizler. Sunucu veritabanıyla eşitlemeyi bekleyen işlemler varsa, *zorlama temizleme* parametresi ayarlanmadığı sürece temizleme bir özel durum oluşturur.

  İstemcideki eski verilere örnek olarak, "yapılacaklar listesi" örneğinde, Device1 yalnızca tamamlanmamış öğeleri çeker. Sunucuda başka bir aygıt tarafından tamamlanan "Süt satın al" todoitem işaretlenir. Ancak, Device1 hala yerel mağazada "Süt Satın Al" todoitem vardır, çünkü yalnızca tam olarak işaretlenmemiş öğeleri çekiyor. Bir temizleme bu bayat öğeyi temizler.

## <a name="next-steps"></a>Sonraki adımlar
* [iOS: Çevrimdışı eşitlemeyi etkinleştirme]
* [Xamarin iOS: Çevrimdışı eşitleme etkinleştirme]
* [Xamarin Android: Çevrimdışı eşitleme etkinleştirme]
* [Evrensel Windows Platformu: Çevrimdışı eşitleme etkinleştirme]

<!-- Links -->
[.NET istemci SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Çevrimdışı eşitleme etkinleştirme]: app-service-mobile-android-get-started-offline-data.md
[iOS: Çevrimdışı eşitlemeyi etkinleştirme]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: Çevrimdışı eşitleme etkinleştirme]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Çevrimdışı eşitleme etkinleştirme]: app-service-mobile-xamarin-android-get-started-offline-data.md
[Evrensel Windows Platformu: Çevrimdışı eşitleme etkinleştirme]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
