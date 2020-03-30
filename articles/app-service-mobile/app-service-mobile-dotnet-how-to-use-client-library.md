---
title: Yönetilen istemci kitaplığıyla çalışma
description: Windows ve Xamarin uygulamalarıyla Azure App Service Mobile Apps için .NET istemci kitaplığını nasıl kullanacağınızı öğrenin.
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 1c9fba3c13cc6e5476377d59130a95a2edaa324d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249379"
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Azure Mobile Apps için yönetilen istemci kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, Windows ve Xamarin uygulamaları için Azure App Service Mobile Apps için yönetilen istemci kitaplığını kullanarak sık karşılaşılan senaryoları nasıl gerçekleştireceklerini gösterir. Mobil Uygulamalar'da yeniyseniz, öncelikle Azure [Mobil Uygulamalar hızlı başlatma][1] eğitimini tamamlamayı düşünmelisiniz. Bu kılavuzda, müşteri tarafı yönetilen SDK odaklanmak. Mobil Uygulamalar için sunucu tarafındaki SDK'lar hakkında daha fazla bilgi edinmek için [.NET Server SDK][2] veya [Node.js Server SDK][3]belgelerine bakın.

## <a name="reference-documentation"></a>Referans belgeleri
İstemci SDK için başvuru belgeleri burada bulunur: [Azure Mobile Apps .NET istemci başvurusu.][4]
[Azure Örnekleri GitHub deposunda][5]da birkaç istemci örneği bulabilirsiniz.

## <a name="supported-platforms"></a>Desteklenen Platformlar
.NET Platformu aşağıdaki platformları destekler:

* API 19 ile 24 (Nougat ile KitKat) için Xamarin Android bültenleri
* Xamarin iOS sürümleri için 8.0 ve sonrası sürümleri
* Evrensel Windows Platformu
* Windows Phone 8.1
* Silverlight uygulamaları dışında Windows Phone 8.0

"Sunucu akışı" kimlik doğrulaması, sunulan web hizmeti için bir Web Görünümü kullanır.  Aygıt bir WebView Web Hizmeti sürümü sunamıyorsa, diğer kimlik doğrulama yöntemleri gerekir.  Bu nedenle Bu SDK, Watch tipi veya benzer şekilde kısıtlanmış aygıtlar için uygun değildir.

## <a name="setup-and-prerequisites"></a><a name="setup"></a>Kurulum ve Ön koşullar
En az bir tablo içeren Mobil Uygulama arka uç projenizi zaten oluşturduğunuzu ve yayınladığınızı varsayıyoruz.  Bu konuda kullanılan kodda, tablo `TodoItem` adlandırılmış ve aşağıdaki `Id`sütunlara sahiptir: , `Text`ve `Complete`. Bu tablo, [Azure Mobil Uygulamaları'nı quickstart'ı][1]tamamladığınızda oluşturulan tabloyla aynıdır.

C#'da karşılık gelen dakti-sİstem tarafı türü aşağıdaki sınıftır:

```csharp
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }
}
```

[JsonPropertyAttribute][6] istemci alanı ve tablo alanı arasında *PropertyName* eşleme tanımlamak için kullanılır.

Mobil Uygulamalarınızda nasıl tablo oluşturabilirsiniz öğrenmek için [.NET Server SDK konusuna][7] veya [Node.js Server SDK konusuna][8]bakın. Mobil Uygulama arka uçunuzu Azure portalında QuickStart'ı kullanarak oluşturduysanız, Azure [portalındaki] **Kolay tablolar** ayarını da kullanabilirsiniz.

### <a name="how-to-install-the-managed-client-sdk-package"></a>Nasıl yapılır: Yönetilen istemci SDK paketini yükleyin
[NuGet'den][9]Mobil Uygulamalar için yönetilen istemci SDK paketini yüklemek için aşağıdaki yöntemlerden birini kullanın:

* **Görsel Stüdyo** Projenizi sağ tıklatın, **NuGet Paketlerini Yönet'i**tıklatın, `Microsoft.Azure.Mobile.Client` paketi arayın, ardından **Yükle'yi**tıklatın.
* **Xamarin Stüdyosu** Projenizi sağ tıklatın, **NuGet Paketleri** **Ekle'yi** > tıklatın, paketi arayın ve ardından **Paket Ekle'yi** `Microsoft.Azure.Mobile.Client` tıklatın.

Ana etkinlik dosyanızda aşağıdaki leri **kullanarak** ifadeeklemeyi unutmayın:

```csharp
using Microsoft.WindowsAzure.MobileServices;
```

> [!NOTE]
> Android projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olması gerektiğini unutmayın. SDK'nın `Xamarin.Android.Support.CustomTabs` Android platformuna bağımlılığı vardır, bu nedenle projenizde daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için bu paketi doğrudan gerekli sürümle yüklemeniz gerekir.

### <a name="how-to-work-with-debug-symbols-in-visual-studio"></a><a name="symbolsource"></a>Nasıl yapIlir: Visual Studio'da hata ayıklama sembolleri ile çalışma
Microsoft.Azure.Mobile ad alanı sembolleri [SymbolSource'da][10]mevcuttur.  SymbolSource'u Visual Studio ile tümleştirmek için [SymbolSource yönergelerine][11] bakın.

## <a name="create-the-mobile-apps-client"></a><a name="create-client"></a>Mobil Uygulamalar istemcisini oluşturma
Aşağıdaki kod, Mobil Uygulama arka arka ucunuza erişmek için kullanılan [MobileServiceClient][12] nesnesini oluşturur.

```csharp
var client = new MobileServiceClient("MOBILE_APP_URL");
```

Önceki kodda, Azure `MOBILE_APP_URL` [portalındaki]Mobil Uygulama arka uçunuzda bulunan Mobil Uygulama arka ucunun URL'sini değiştirin. MobileServiceClient nesnesi bir singleton olmalıdır.

## <a name="work-with-tables"></a>Tablolarla Çalışma
Aşağıdaki bölümde, kayıtların nasıl aranıp alınıp alınışları ve tablo içindeki verileri nasıl değiştirileri ayrıntılı olarak açıklanmıştır.  Aşağıdaki konular ele alınmıştır:

* [Tablo başvurusu oluşturma](#instantiating)
* [Verileri sorgulama](#querying)
* [Döndürülen verileri filtreleme](#filtering)
* [Döndürülen verileri sıralama](#sorting)
* [Sayfalarda veri döndürme](#paging)
* [Belirli sütunları seçme](#selecting)
* [Id'ye göre bir kaydı ara](#lookingup)
* [Yazılmamış sorgularla başa çıkma](#untypedqueries)
* [Veri ekleme](#inserting)
* Verileri güncelleştirme
* [Verileri silme](#deleting)
* [Uyuşmazlık Çözümü ve İyimser Eşzamanlılık](#optimisticconcurrency)
* [Windows Kullanıcı Arabirimine bağlanma](#binding)
* [Sayfa Boyutunu Değiştirme](#pagesize)

### <a name="how-to-create-a-table-reference"></a><a name="instantiating"></a>Nasıl yapılsın: Tablo başvurusu oluşturma
Arka uç tablosundaki verilere erişen veya verileri değiştiren tüm `MobileServiceTable` kod, nesnedeki işlevleri çağırır. [GetTable] yöntemini aşağıdaki gibi çağırarak tabloya bir başvuru alın:

```csharp
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

Döndürülen nesne, daktino serileştirme modelini kullanır. Yazılmamış serileştirme modeli de desteklenir. Aşağıdaki örnek, [yazılmamış bir tabloya bir başvuru oluşturur:]

```csharp
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

Yazılmamış sorgularda, altta yatan OData sorgu dizesini belirtmeniz gerekir.

### <a name="how-to-query-data-from-your-mobile-app"></a><a name="querying"></a>Nasıl kullanılır: Mobil Uygulamanızdan veri sorgulama
Bu bölümde, aşağıdaki işlevselliği içeren Mobil Uygulama arka ucuna sorgu verme nin nasıl yapılacağını açıklar:

* [Döndürülen verileri filtreleme](#filtering)
* [Döndürülen verileri sıralama](#sorting)
* [Sayfalarda veri döndürme](#paging)
* [Belirli sütunları seçme](#selecting)
* [Kimliklerine göre verilere bakın](#lookingup)

> [!NOTE]
> Tüm satırların döndürülmesini önlemek için sunucu tarafından yönlendirilen sayfa boyutu zorlanır.  Sayfalama, büyük veri kümeleri için varsayılan isteklerin hizmeti olumsuz etkilemesini sağlar.  50'den fazla satır döndürmek `Skip` için, `Take` [sayfalardaki İade verilerinde](#paging)açıklandığı gibi, bu yöntemi ve yöntemi kullanın.

### <a name="how-to-filter-returned-data"></a><a name="filtering"></a>Nasıl kullanılır: Döndürülen verileri filtreleme
Aşağıdaki kod, bir sorguya bir `Where` yan tümce ekleyerek verilerin nasıl filtreuygulanış edilebildiğini göstermektedir. Özelliği ne eşit `todoTable` `false` `Complete` olan tüm öğeleri döndürür. [Where] işlevi tabloya karşı sorguya bir satır filtreleme yüklemi uygular.

```csharp
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

Tarayıcı geliştirici araçları veya [Fiddler]gibi ileti denetim yazılımını kullanarak arka uca gönderilen isteğin URI'sini görüntüleyebilirsiniz. Uri isteğine bakarsanız, sorgu dizesinin değiştirildiğini fark edin:

```csharp
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Bu OData isteği Server SDK tarafından bir SQL sorgusuna çevrilir:

```csharp
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

`Where` Yönteme geçirilen işlev, rasgele sayıda koşula sahip olabilir.

```csharp
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

Bu örnek, Server SDK tarafından bir SQL sorgusuna çevrilebilir:

```csharp
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Bu sorgu da birden çok yan tümceye ayrılabilir:

```csharp
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

Bu iki yöntem eşdeğerdir ve birbirinin yerine kullanılabilir.  &mdash;Bir sorguda&mdash;birden çok yüklemi bağlama seçeneği daha kompakttır ve önerilir.

Yan `Where` tümce, OData alt kümesine çevrilecek işlemleri destekler. İşlemler şunlardır:

* İlişkisel işleçler (==, !=, <, <, >, >=),
* Aritmetik işleçler (+, -, /, *, %),
* Sayı hassasiyeti (Math.Floor, Math.Ceiling),
* String fonksiyonları (Uzunluk, Alt dize, değiştir, indexOf, StartsWith, EndsWith),
* Tarih özellikleri (Yıl, Ay, Gün, Saat, Dakika, İkinci),
* Bir nesnenin özelliklerine erişin ve
* Bu işlemlerden herhangi birini birleştiren ifadeler.

Sunucu SDK'nın neleri desteklediğini düşünürken, [OData v3 Dokümantasyon'u]düşünebilirsiniz.

### <a name="how-to-sort-returned-data"></a><a name="sorting"></a>Nasıl kullanılır: Döndürülen verileri sıralama
Aşağıdaki kod, sorguya bir OrderBy veya [OrderByDescending] işlevini ekleyerek verilerin nasıl sıralanabildiğini göstermektedir. [OrderBy] Alana göre `Text` `todoTable` sıralanmış artan öğeleri döndürür.

```csharp
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <a name="how-to-return-data-in-pages"></a><a name="paging"></a>Nasıl kullanılır: Sayfalardaki verileri döndürme
Varsayılan olarak, arka uç yalnızca ilk 50 satırı döndürür. [Take] yöntemini arayarak döndürülen satır sayısını artırabilirsiniz. Sorgu `Take` tarafından döndürülen toplam veri kümesinin belirli bir "sayfasını" istemek için [Atlama] yöntemiyle birlikte kullanın. Aşağıdaki sorgu, yürütüldüğünde, tablodaki en iyi üç öğeyi döndürür.

```csharp
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

Aşağıdaki gözden geçirilmiş sorgu ilk üç sonucu atlar ve sonraki üç sonucu döndürür. Bu sorgu, sayfa boyutunun üç öğe olduğu ikinci "veri sayfası"nı üretir.

```csharp
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

[IncludeTotalCount] yöntemi, belirtilen herhangi bir sayfalama/sınır yan tümcesini yok sayarak döndürülecek *tüm* kayıtlar için toplam sayımı ister:

```csharp
query = query.IncludeTotalCount();
```

Gerçek bir uygulamada, sayfalar arasında gezinmek için bir çağrı cihazı denetimi veya benzer web-yüksek ötüeden önceki örneğe benzer sorgular kullanabilirsiniz.

> [!NOTE]
> Mobil Uygulama arka ucundaki 50 satır sınırını geçersiz kılmak için, [EnableQueryAttribute'u] genel GET yöntemine uygulamalı ve çağrılama davranışını belirtmelisiniz. Yönteme uygulandığında, döndürülen en fazla satır 1000 olarak ayarlar:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="how-to-select-specific-columns"></a><a name="selecting"></a>Nasıl kullanılır: Belirli sütunları seçme
Sorgunuza bir [Seç] yan tümcesi ekleyerek sonuçlara hangi özellik kümesinin dahil edileceğini belirtebilirsiniz. Örneğin, aşağıdaki kod, yalnızca bir alanın nasıl seçilebildiğini ve birden çok alanın nasıl seçilip biçimlendirilenini gösterir:

```csharp
// Select one field -- just the Text
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => todoItem.Text);
List<string> items = await query.ToListAsync();

// Select multiple fields -- both Complete and Text info
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => string.Format("{0} -- {1}",
                    todoItem.Text.PadRight(30), todoItem.Complete ?
                    "Now complete!" : "Incomplete!"));
List<string> items = await query.ToListAsync();
```

Şu ana kadar açıklanan tüm fonksiyonlar katkı maddesi, bu yüzden onları zincirleme yedevam edebiliriz. Her zincirleme arama sorgunun daha fazlasını etkiler. Bir örnek daha:

```csharp
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="how-to-look-up-data-by-id"></a><a name="lookingup"></a>Nasıl kullanılır: Kimlikle veri arama
[LookupAsync] işlevi, belirli bir kimlikle veritabanındaki nesneleri aramak için kullanılabilir.

```csharp
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="how-to-execute-untyped-queries"></a><a name="untypedqueries"></a>Nasıl yapılsın: Yazılmamış sorguları yürütme
Yazılmamış bir tablo nesnesi kullanarak bir sorgu yürütürken, aşağıdaki örnekte olduğu gibi [ReadAsync'i]arayarak OData sorgu dizesini açıkça belirtmeniz gerekir:

```csharp
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

Bir özellik çantası gibi kullanabileceğiniz JSON değerleri geri almak. JToken ve Newtonsoft Json.NET hakkında daha fazla bilgi için [Json.NET] sitesine bakın.

### <a name="how-to-insert-data-into-a-mobile-app-backend"></a><a name="inserting"></a>Nasıl kullanılır: Mobil Uygulama arka ucuna veri ekleme
Tüm istemci türleri, varsayılan olarak bir dize olan **Id**adlı bir üye içermelidir. Bu **Kimlik** CRUD işlemleri gerçekleştirmek ve çevrimdışı eşitleme için gereklidir. Aşağıdaki kod, tabloya yeni satırlar eklemek için [InsertAsync] yönteminin nasıl kullanılacağını göstermektedir. Parametre,.NET nesnesi olarak eklenecek verileri içerir.

```csharp
await todoTable.InsertAsync(todoItem);
```

Bir ekleme `todoItem` sırasında benzersiz bir özel kimlik değeri dahil edilmezse, sunucu tarafından bir GUID oluşturulur.
Arama döndükten sonra nesneyi inceleyerek oluşturulan Kimliği alabilirsiniz.

Yazılmamış veri eklemek için Json.NET yararlanabilirsiniz:

```csharp
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Burada benzersiz bir dize kimliği olarak bir e-posta adresi kullanarak bir örnek:

```csharp
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>Kimlik değerleriyle çalışma
Mobil Uygulamalar, tablonun **kimlik** sütunu için benzersiz özel dize değerlerini destekler. Dize değeri, uygulamaların kimlik için e-posta adresleri veya kullanıcı adları gibi özel değerleri kullanmasına olanak tanır.  Dize işlikiler size aşağıdaki avantajlardan yararlanırır:

* Veritabanlarına gidiş dönüş yapmadan yapılan t.c.
* Kayıtların farklı tablolardan veya veritabanlarından birleştirilmesi daha kolaydır.
* Yanlış lar değerleri, uygulamanın mantığıyla daha iyi tümleştirilebilir.

Eklenen bir kayıtta dize kimliği değeri ayarlanmadığında, Mobil Uygulama arka ucu kimlik için benzersiz bir değer oluşturur. [Guid.NewGuid] yöntemini, istemcide veya arka uçta kendi kimlik değerlerinizi oluşturmak için kullanabilirsiniz.

```csharp
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="how-to-modify-data-in-a-mobile-app-backend"></a><a name="modifying"></a>Nasıl kullanılır: Mobil Uygulama arka ucundaki verileri değiştirme
Aşağıdaki kod, varolan bir kaydı yeni bilgilerle aynı kimlikle güncelleştirmek için [UpdateAsync] yönteminin nasıl kullanılacağını göstermektedir. Parametre,.NET nesnesi olarak güncellenecek verileri içerir.

```csharp
await todoTable.UpdateAsync(todoItem);
```

Yazılmamış verileri güncelleştirmek için aşağıdaki gibi [Json.NET] yararlanabilirsiniz:

```csharp
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

Güncelleştirme `id` yaparken bir alan belirtilmelidir. Arka uç, `id` hangi satırı güncelleştireceklerini belirlemek için alanı kullanır. Alan `id` `InsertAsync` arama sonucunda elde edilebilir. Bir `ArgumentException` öğeyi `id` değer sağlamadan güncelleştirmeye çalışırsanız, bir yükseltilir.

### <a name="how-to-delete-data-in-a-mobile-app-backend"></a><a name="deleting"></a>Nasıl kullanılır: Mobil Uygulama arka ucundaki verileri silme
Aşağıdaki kod, varolan bir örneği silmek için [DeleteAsync] yönteminin nasıl kullanılacağını göstermektedir. Örnek, `todoItem`'de `id` ayarlanan alan tarafından tanımlanır.

```csharp
await todoTable.DeleteAsync(todoItem);
```

Yazılmamış verileri silmek için aşağıdaki gibi Json.NET yararlanabilirsiniz:

```csharp
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

Silme isteğinde bulunduğunuzda, bir kimlik belirtilmelidir. Diğer özellikler hizmete geçirilir veya hizmette yoksayılır. Bir `DeleteAsync` aramanın sonucu `null`genellikle . Geçecek `InsertAsync` kimlik, arama sonucundan elde edilebilir. Alan `MobileServiceInvalidOperationException` belirtmeden `id` bir öğeyi silmeye çalıştığınızda A atılır.

### <a name="how-to-use-optimistic-concurrency-for-conflict-resolution"></a><a name="optimisticconcurrency"></a>Nasıl kullanılır: Çakışma çözümü için İyimser Eşzamanlılık'ı kullanın
İki veya daha fazla istemci aynı öğeye aynı anda değişiklik yazabilir. Çakışma algılama olmadan, son yazma önceki güncelleştirmeleri üzerine yazmak istiyorsunuz. **İyimser eşzamanlılık denetimi,** her işlemin işleyebileceğini varsayar ve bu nedenle herhangi bir kaynak kilitleme kullanmaz.  Bir işlem gerçekleştirmeden önce, iyimser eşzamanlılık denetimi başka hiçbir işlemin verileri değiştirdiğini doğrular. Veriler değiştirildiyse, işleyen işlem geri alınır.

Mobil Uygulamalar, Mobil Uygulama arka uçunuzdaki `version` her tablo için tanımlanan sistem özelliği sütununu kullanarak her öğedeki değişiklikleri izleyerek iyimser eşzamanlılık denetimini destekler. Bir kayıt her güncelleştirinde, `version` Mobil Uygulamalar bu kaydın özelliğini yeni bir değere ayarlar. Her güncelleştirme isteği `version` sırasında, istekle birlikte kaydedilen kaydın özelliği, sunucudaki kayıt için aynı özellik ile karşılaştırılır. İstekle geçirilen sürüm arka uçla eşleşmiyorsa, istemci `MobileServicePreconditionFailedException<T>` kitaplığı bir özel durum oluşturur. Özel durum la birlikte dahil edilen tür, kaydın sunucu sürümünü içeren arka uçtan gelen kayıttır. Uygulama daha sonra değişiklikleri işlemek için arka uçtan doğru `version` değerle güncelleştirme isteğini yeniden yürütüp yürütmemeye karar vermek için bu bilgileri kullanabilirsiniz.

İyimser eşzamanlılık sağlamak `version` için sistem özelliği için tablo sınıfında bir sütun tanımlayın. Örnek:

```csharp
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }

    // *** Enable Optimistic Concurrency *** //
    [JsonProperty(PropertyName = "version")]
    public string Version { set; get; }
}
```

Yazılmamış tablolar kullanan uygulamalar, tablonun `Version` `SystemProperties` üzerindeki bayrağı aşağıdaki gibi ayarlayarak iyimser eşzamanlılık sağlar.

```csharp
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

İyimser eşzamanlılığı etkinleştirmeye ek olarak, `MobileServicePreconditionFailedException<T>` [UpdateAsync'i]ararken kodunuzda ki özel durumu da yakalamanız gerekir.  Güncelleştirilmiş kayda doğru `version` uygulayarak çakışmayı çözümleyin ve çözülmüş kayıtla [UpdateAsync'i] arayın. Aşağıdaki kod, bir yazma çakışması algılandıktan sonra nasıl çözüleceğini gösterir:

```csharp
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at the remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, the item has changed since the last query
        // Resolve the conflict between the local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user to choose the resolution between versions
    MessageDialog msgDialog = new MessageDialog(
        String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
        serverItem.Text, localItem.Text),
        "CONFLICT DETECTED - Select a resolution:");

    UICommand localBtn = new UICommand("Commit Local Text");
    UICommand ServerBtn = new UICommand("Leave Server Text");
    msgDialog.Commands.Add(localBtn);
    msgDialog.Commands.Add(ServerBtn);

    localBtn.Invoked = async (IUICommand command) =>
    {
        // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while the user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

Daha fazla bilgi için [Azure Mobil Uygulamaları konusundaki Çevrimdışı Veri Eşitleme konusuna] bakın.

### <a name="how-to-bind-mobile-apps-data-to-a-windows-user-interface"></a><a name="binding"></a>Nasıl yapilir: Mobil Uygulamalar verilerini Windows kullanıcı arabirimine bağlama
Bu bölümde, bir Windows uygulamasında UI öğelerini kullanarak döndürülen veri nesnelerinin nasıl görüntülenilen görüntülenilen.  Aşağıdaki örnek kod, tamamlanmamış öğeler için bir sorgu ile listenin kaynağına bağlanır. [MobileServiceCollection,] Mobil Uygulamalara duyarlı bir bağlama koleksiyonu oluşturur.

```csharp
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound to a UI list control
IEnumerable itemsControl  = items;

// Bind this to a ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Yönetilen çalışma zamanındaki bazı denetimler [ISupportIncrementalLoading]adlı bir arabirimi destekler. Bu arabirim, kullanıcı kaydırıldığında denetimlerin ek veri istemesine olanak tanır. [MobileServiceIncrementalLoadingCollection]aracılığıyla evrensel Windows uygulamaları için bu arayüz için yerleşik destek vardır , hangi otomatik olarak kontrollerden aramaları işler. Windows `MobileServiceIncrementalLoadingCollection` uygulamalarında aşağıdaki gibi kullanın:

```csharp
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Windows Phone 8 ve "Silverlight" uygulamalarında yeni `ToCollection` koleksiyonu kullanmak `IMobileServiceTableQuery<T>` `IMobileServiceTable<T>`için, uzantı yöntemlerini kullanın ve. Veri yüklemek için. `LoadMoreItemsAsync()`

```csharp
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

Arayarak `ToCollectionAsync` oluşturulan koleksiyonu kullandığınızda `ToCollection`veya , UI denetimlerine bağlı olabilecek bir koleksiyon elde elabilirsiniz.  Bu koleksiyon sayfalama farkındadır.  Koleksiyon ağdan veri yüklediğinden, yükleme bazen başarısız olur. Bu tür hataları işlemek `OnException` `MobileServiceIncrementalLoadingCollection` için, `LoadMoreItemsAsync`''ye yapılan çağrılardan kaynaklanan özel durumları işlemek için yöntemi geçersiz kılın

Tablonuzda çok sayıda alan olup olmadığını, ancak yalnızca bazılarını denetiminizde görüntülemek isteyip istemediğiniz göz önünde bulundurun. UI'de görüntülenecek belirli sütunları seçmek için önceki bölümdeki "[Belirli sütunları seçin](#selecting)" kılavuzu kullanabilirsiniz.

### <a name="change-the-page-size"></a><a name="pagesize"></a>Sayfa boyutunu değiştirme
Azure Mobile Apps varsayılan olarak istek başına en fazla 50 öğe döndürür.  Hem istemcide hem de sunucuda maksimum sayfa boyutunu artırarak sayfa boyutunu değiştirebilirsiniz.  İstenen sayfa boyutunu artırmak `PullOptions` için `PullAsync()`şunları kullanırken belirtin:

```csharp
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Sunucu içinde 100'e `PageSize` eşit veya daha büyük bir istek yaptığınızvarsa, istek en fazla 100 öğe döndürür.

## <a name="work-with-offline-tables"></a><a name="#offlinesync"></a>Çevrimdışı Tablolarla Çalışma
Çevrimdışı tablolar, çevrimdışı olduğunda kullanmak üzere verileri depolamak için yerel bir SQLite deposu kullanır.  Tüm tablo işlemleri uzak sunucu deposu yerine yerel SQLite deposuna karşı yapılır.  Çevrimdışı bir tablo oluşturmak için önce projenizi hazırlayın:

1. Visual Studio'da, **Çözüm için NuGet Paketlerini Yönet>** çözüme sağ tıklayın... ardından çözümdeki tüm projeler için **Microsoft.Azure.Mobile.Mobile.Client.SQLiteStore** NuGet paketini arayın ve yükleyin.
2. (İsteğe bağlı) Windows aygıtlarını desteklemek için aşağıdaki SQLite çalışma zamanı paketlerinden birini yükleyin:

   * **Windows 8.1 Çalışma Zamanı:** [Windows 8.1 için SQLite'yi][3]yükleyin.
   * **Windows Phone 8.1:** [Windows Phone 8.1 için SQLite'yi][4]yükleyin.
   * **Evrensel Windows Platformu** [Evrensel Windows için SQLite yükleyin.][5]
3. (İsteğe bağlı). Windows aygıtları **için, Başvuru** > **Ekle Başvuru...** seçeneğini tıklatın, **Windows** klasörünü genişletin > **Uzantıları'nı**genişletin, ardından **Visual C++ 2013 Windows** SDK için Çalışma Süresi ile birlikte Windows SDK için uygun **SQLite'yi** etkinleştirin.
    SQLite SDK adları her Windows platformuna göre biraz farklılık gösterir.

Tablo başvurusu oluşturulmadan önce yerel mağazanın hazırlanması gerekir:

```csharp
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

Mağaza başlatma normalde istemci oluşturulduktan hemen sonra yapılır.  **ÇevrimdışıDbPath,** desteklediğiniz tüm platformlarda kullanıma uygun bir dosya adı olmalıdır.  Yol tam nitelikli bir yolsa (yani, bir eğik çizgiyle başlar), o yol kullanılır.  Yol tam olarak nitelikli değilse, dosya platforma özgü bir konuma yerleştirilir.

* iOS ve Android cihazlar için varsayılan yol "Kişisel Dosyalar" klasörüdür.
* Windows aygıtları için varsayılan yol uygulamaya özgü "AppData" klasörüdür.

Yöntem kullanılarak `GetSyncTable<>` bir tablo başvurusu elde edilebilir:

```csharp
var table = client.GetSyncTable<TodoItem>();
```

Çevrimdışı tablo kullanmak için kimlik doğrulamanız gerekmez.  Yalnızca arka uç hizmetiyle iletişim kurarken kimlik doğrulamanız gerekir.

### <a name="syncing-an-offline-table"></a><a name="syncoffline"></a>Çevrimdışı Tabloyu Eşitleme
Çevrimdışı tablolar varsayılan olarak arka uçla eşitlenmez.  Senkronizasyon iki parçaya bölünür.  Değişiklikleri yeni öğeleri indirmekten ayrı olarak itebilirsiniz.  Aşağıda tipik bir eşitleme yöntemi vereme yöntemi vereme yöntemi ve

```csharp
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
            //Use a different query name for each unique query in your program
            "allTodoItems",
            this.todoTable.CreateQuery());
    }
    catch (MobileServicePushFailedException exc)
    {
        if (exc.PushResult != null)
        {
            syncErrors = exc.PushResult.Errors;
        }
    }

    // Simple error/conflict handling. A real application would handle the various errors like network conditions,
    // server conflicts and others via the IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting to server's copy.
                await error.CancelAndUpdateItemAsync(error.Result);
            }
            else
            {
                // Discard local change.
                await error.CancelAndDiscardItemAsync();
            }

            Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
        }
    }
}
```

İlk bağımsız değişken `PullAsync` null ise, artımlı eşitleme kullanılmaz.  Her eşitleme işlemi tüm kayıtları alır.

SDK, kayıtları `PushAsync()` çekmeden önce örtük bir performans sergiliyor.

Çakışma işleme `PullAsync()` bir yöntem de olur.  Çakışmalarla çevrimiçi tablolarla aynı şekilde başa çıkabilirsiniz.  Çakışma, `PullAsync()` ekleme, güncelleştirme veya silme sırasında değil, çağrıldığında oluşur. Birden çok çakışma olursa, bunlar tek bir MobileServicePushFailedException içine paketlenir.  Her hatayı ayrı ayrı ele alın.

## <a name="work-with-a-custom-api"></a><a name="#customapi"></a>Özel bir API ile çalışma
Özel API, ekleme, güncelleştirme, silme veya okuma işlemine eş olmayan sunucu işlevselliğini ortaya çıkaran özel uç noktaları tanımlamanızı sağlar. Özel bir API kullanarak, HTTP ileti üstbilgilerini okuma ve ayarlama ve JSON dışındaki ileti gövdesi biçimini tanımlama dahil olmak üzere mesajlaşma üzerinde daha fazla denetime sahip olabilirsiniz.

İstemcideki [InvokeApiAsync] yöntemlerinden birini arayarak özel bir API çağırırsınız. Örneğin, aşağıdaki kod satırı arka uçtaki tüm API'nin **tamamına** bir POST isteği gönderir:

```javascript
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Bu form, yazılı bir yöntem çağrısıdır ve **MarkAllResult** dönüş türünün tanımlanmasını gerektirir. Hem yazılan hem de yazılmamış yöntemler desteklenir.

InvokeApiAsync() yöntemi, API '/' ile başlamadığı sürece aramak istediğiniz API'ye '/api/' hazırlar.
Örnek:

* `InvokeApiAsync("completeAll",...)`aramalar /api/completeAll arka uçta
* `InvokeApiAsync("/.auth/me",...)`arka uçta /.auth/me'yi arar

Azure Mobil Uygulamaları yla tanımlanmamış WebAPI'ler de dahil olmak üzere, invokeApiAsync'i herhangi bir WebAPI'yi aramak için kullanabilirsiniz.  InvokeApiAsync() kullandığınızda, kimlik doğrulama üstleri de dahil olmak üzere uygun üstbilgi istekle birlikte gönderilir.

## <a name="authenticate-users"></a><a name="authentication"></a>Kullanıcıların kimliklerini doğrulama
Mobil Uygulamalar, uygulama kullanıcılarının facebook, Google, Microsoft Hesabı, Twitter ve Azure Active Directory gibi çeşitli dış kimlik sağlayıcılarını kullanarak kimlik doğrulamayı ve yetkilendirmeyi destekler. Belirli işlemlere erişimi yalnızca kimlik doğrulaması yapılan kullanıcılarla sınırlamak için izinleri tablolarda ayarlayabilirsiniz. Sunucu komut dosyasında yetkilendirme kuralları nı uygulamak için kimlik doğrulaması yapılan kullanıcıların kimliğini de kullanabilirsiniz. Daha fazla bilgi için [Uygulamanıza kimlik doğrulaması ekleme] öğreticisine bakın.

İki kimlik doğrulama akışı desteklenir: *istemci tarafından yönetilen* ve sunucu tarafından *yönetilen* akış. Sunucu tarafından yönetilen akış, sağlayıcının web kimlik doğrulama arabirimine dayandığı için en basit kimlik doğrulama deneyimini sağlar. İstemci tarafından yönetilen akış, sağlayıcıya özel aygıta özgü SDK'lara dayandığı için cihaza özgü özelliklerle daha derin tümleştirme sağlar.

> [!NOTE]
> Üretim uygulamalarınızda istemci tarafından yönetilen bir akış kullanmanızı öneririz.

Kimlik doğrulamasını ayarlamak için uygulamanızı bir veya daha fazla kimlik sağlayıcısına kaydetmeniz gerekir.  Kimlik sağlayıcısı, uygulamanız için bir istemci kimliği ve bir istemci sırrı oluşturur.  Bu değerler daha sonra Azure App Service kimlik doğrulamasını/yetkilendirmesini etkinleştirmek için arka ucunuzda ayarlanır.  Daha fazla bilgi için, öğreticideki ayrıntılı yönergeleri [uygulayın Uygulamanıza kimlik doğrulama yı ekleyin.]

Aşağıdaki konular bu bölümde ele alınmıştır:

* [İstemci tarafından yönetilen kimlik doğrulaması](#clientflow)
* [Sunucu tarafından yönetilen kimlik doğrulama](#serverflow)
* [Kimlik doğrulama belirteci önbelleğe alma](#caching)

### <a name="client-managed-authentication"></a><a name="clientflow"></a>İstemci tarafından yönetilen kimlik doğrulaması
Uygulamanız kimlik sağlayıcıyla bağımsız olarak iletişim kurabilir ve giriş sırasında iade edilen jetonu arka ucunuzla sağlayabilir. Bu istemci akışı, kullanıcılar için tek bir oturum açma deneyimi sağlamanıza veya kimlik sağlayıcısından ek kullanıcı verileri almanıza olanak tanır. Kimlik sağlayıcısı SDK daha yerel bir UX hissi sağladığı ndan ve ek özelleştirmeye olanak sağladığından, istemci akışı kimlik doğrulaması sunucu akışını kullanmak için tercih edilir.

Aşağıdaki istemci akışı kimlik doğrulama desenleri için örnekler verilmiştir:

* [Etkin Dizin Kimlik Doğrulama Kitaplığı](#adal)
* [Facebook veya Google](#client-facebook)

#### <a name="authenticate-users-with-the-active-directory-authentication-library"></a><a name="adal"></a>Active Directory Authentication Library ile kullanıcıları kimlik doğrulama
Azure Etkin Dizin kimlik doğrulaması kullanarak istemciden kullanıcı kimlik doğrulaması başlatmak için Etkin Dizin Kimlik Doğrulama Kitaplığını (ADAL) kullanabilirsiniz.

1. [Active Directory giriş] için Uygulama Hizmetini yapılandırma yı izleyerek AAD oturum açma için mobil uygulama arka uçunuzu yapılandırın. Yerel istemci uygulamasını kaydetmenin isteğe bağlı adımını tamamladıktan emin olun.
2. Visual Studio veya Xamarin Studio'da projenizi açın `Microsoft.IdentityModel.Clients.ActiveDirectory` ve NuGet paketine bir referans ekleyin. Arama yaparken, ön sürüm sürümlerini ekleyin.
3. Kullandığınız platforma göre uygulamanız için aşağıdaki kodu ekleyin. Her birinde, aşağıdaki değiştirmeleri yapın:

   * **INSERT-AUTHORITY-HERE'ı** başvurunuzu yaptığınız kiracının adı ile değiştirin. Biçimi olmalıdır. https://login.microsoftonline.com/contoso.onmicrosoft.com Bu değer, [Azure portalındaki]Azure Etkin Dizininizdeki Etki Alanı sekmesinden kopyalanabilir.
   * **INSERT-RESOURCE-ID-HERE'ı** mobil uygulama arka ucunuz için istemci kimliğiyle değiştirin. Portaldaki **Azure Etkin Dizin Ayarları** altında **Gelişmiş** sekmesinden istemci kimliğini edinebilirsiniz.
   * **INSERT-CLIENT-ID-HERE'ı** yerel istemci uygulamasından kopyaladığınız istemci kimliğiyle değiştirin.
   * HTTPS şemasını kullanarak **INSERT-REDIRECT-URI-HERE'ı** sitenizin */.auth/login/done* endpoint'iyle değiştirin. Bu değer *https://contoso.azurewebsites.net/.auth/login/done*.

     Her platform için gerekli kod aşağıdaki gibidir:

     **Windows:**

     ```csharp
     private MobileServiceUser user;
     private async Task AuthenticateAsync()
     {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        while (user == null)
        {
            string message;
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await App.MobileService.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
     }
     ```

     **Xamarin.iOS**

     ```csharp
     private MobileServiceUser user;
     private async Task AuthenticateAsync(UIViewController view)
     {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(view));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
     }
     ```

     **Xamarin.Android**

     ```csharp
     private MobileServiceUser user;
     private async Task AuthenticateAsync()
     {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(this));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(ex.Message);
            builder.SetTitle("You must log in. Login Required");
            builder.Create().Show();
        }
     }
     protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
     {

        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
     }
     ```

#### <a name="single-sign-on-using-a-token-from-facebook-or-google"></a><a name="client-facebook"></a>Facebook veya Google'dan bir belirteç kullanarak Tek Oturum Açma
Bu snippet'te gösterildiği gibi müşteri akışını Facebook veya Google için kullanabilirsiniz.

```csharp
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using the Facebook or Google SDK.
token.Add("access_token", "access_token_value");

private MobileServiceUser user;
private async Task AuthenticateAsync()
{
    while (user == null)
    {
        string message;
        try
        {
            // Change MobileServiceAuthenticationProvider.Facebook
            // to MobileServiceAuthenticationProvider.Google if using Google auth.
            user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
            message = string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

### <a name="server-managed-authentication"></a><a name="serverflow"></a>Sunucu tarafından yönetilen kimlik doğrulama
Kimlik sağlayıcınızı kaydettikten sonra, sağlayıcınızın [MobileServiceAuthenticationProvider] değeriyle [MobileServiceClient] üzerindeki [LoginAsync] yöntemini arayın. Örneğin, aşağıdaki kod Facebook'u kullanarak sunucu akışı oturum açma işlemini başlatır.

```csharp
private MobileServiceUser user;
private async System.Threading.Tasks.Task Authenticate()
{
    while (user == null)
    {
        string message;
        try
        {
            user = await client
                .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
            message =
                string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, [MobileServiceAuthenticationProvider'ın] değerini sağlayıcınızın değeriyle değiştirin.

Azure App Service, sunucu akışında, seçilen sağlayıcının oturum açma sayfasını görüntüleyerek OAuth kimlik doğrulama akışını yönetir.  Kimlik sağlayıcısı döndükten sonra, Azure App Service bir Uygulama Hizmeti kimlik doğrulama belirteci oluşturur. [LoginAsync] yöntemi, hem kimliği doğrulanan kullanıcının [UserId'ini] hem de [MobileServiceAuthenticationToken'ı]JSON web jetonu (JWT) olarak sağlayan bir [MobileServiceUser'ı]döndürür. Bu belirteç önbelleğe alınabilir süresi sona erene kadar yeniden kullanılabilir. Daha fazla bilgi için kimlik [doğrulama belirteci önbelleğe alma'ya](#caching)bakın.

### <a name="caching-the-authentication-token"></a><a name="caching"></a>Kimlik doğrulama belirteci önbelleğe alma
Bazı durumlarda, kimlik doğrulama belirteci sağlayıcıdan depolayarak ilk başarılı kimlik doğrulamasından sonra oturum açma yöntemine çağrı önlenebilir.  Microsoft Store ve UWP uygulamaları, başarılı bir oturum açma sonrasında geçerli kimlik doğrulama belirteciöneğini önbelleğe almak için [PasswordVault'u] aşağıdaki gibi kullanabilir:

```csharp
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

UserId değeri, kimlik bilgisinin Kullanıcı Adı olarak depolanır ve belirteç Parola olarak depolanır. Sonraki başlangıçlarda, önbelleğe alınmış kimlik bilgileri için **Parola Vault'u** denetleyebilirsiniz. Aşağıdaki örnek, önbelleğe alınmış kimlik bilgilerini bulunduklarında kullanır ve aksi takdirde arka uçla yeniden kimlik doğrulamayı dener:

```csharp
// Try to retrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create the current user from the stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache the token as shown above.
}
```

Bir kullanıcıyı oturum dışı bettiğinizde, depolanan kimlik bilgilerini aşağıdaki gibi kaldırmanız gerekir:

```csharp
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Xamarin uygulamaları, kimlik bilgilerini bir **Hesap** nesnesinde güvenli bir şekilde depolamak için [Xamarin.Auth] API'lerini kullanır. Bu API'leri kullanma örneği için, [ContosoMoments fotoğraf paylaşım örneğindeki](https://github.com/azure-appservice-samples/ContosoMoments) [AuthStore.cs] kodu dosyasına bakın.

İstemci tarafından yönetilen kimlik doğrulamasını kullandığınızda, sağlayıcınızdan Facebook veya Twitter gibi elde edilen erişim belirteci önbelleğe de alabilirsiniz. Bu belirteç, aşağıdaki gibi arka uçtan yeni bir kimlik doğrulama belirteci istemek için sağlanabilir:

```csharp
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="push-notifications"></a><a name="pushnotifications"></a>Anında İletme Bildirimleri 
Aşağıdaki konular Push Bildirimleri'ni kapsar:

* [Anında İtme Bildirimleri için kaydolun](#register-for-push)
* [Microsoft Store paketi SID edinin](#package-sid)
* [Platform ötesi şablonlarla kaydolun](#register-xplat)

### <a name="how-to-register-for-push-notifications"></a><a name="register-for-push"></a>Nasıl Yapilir: Anında İtme Bildirimleri için kaydolun
Mobil Uygulamalar istemcisi, anında iletme bildirimleri için Azure Bildirim Hub'ları ile kaydolmanıza olanak tanır. Kayıt olurken, platforma özgü Anında Bildirim Hizmeti'nden (PNS) elde ettiğiniz bir tutamaç elde edersiniz. Daha sonra, kaydı oluştururken bu değeri herhangi bir etiketle birlikte sağlarsınız. Aşağıdaki kod, Windows Bildirim Hizmeti (WNS) ile anında iletme bildirimleri için Windows uygulamanızı kaydeder:

```csharp
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

Eğer WNS itiyorsanız, o zaman [bir Microsoft Store paketi SID elde](#package-sid)etmek gerekir.  Şablon kayıtları için nasıl kaydolunur gibi Windows uygulamaları hakkında daha fazla bilgi için [bkz.]

İstemciden etiket istenmesi desteklenmez.  Etiket İstekleri sessizce kayıttan bırakılır.
Cihazınızı etiketlerle kaydetmek istiyorsanız, kaydı sizin adınıza gerçekleştirmek için Bildirim Hub'ları API'sini kullanan bir Özel API oluşturun.  Yöntem yerine Özel API'yi `RegisterNativeAsync()` arayın.

### <a name="how-to-obtain-a-microsoft-store-package-sid"></a><a name="package-sid"></a>Nasıl yapılır: Microsoft Store paketi SID edinin
Microsoft Mağazası uygulamalarında anında iletme bildirimlerini etkinleştirmek için bir paket SID gereklidir.  Bir paket SID almak için uygulamanızı Microsoft Mağazası'na kaydedin.

Bu değeri elde etmek için:

1. Visual Studio Solution Explorer'da Microsoft Store uygulama **Store** > projesine sağ tıklayın, Mağaza**ile Ilişkilendir Uygulamasını tıklatın...**
2. Sihirbazda, **İleri'yi**tıklatın , Microsoft hesabınızla oturum açın, **Rezerv'de**uygulamanız için bir ad yazın, ardından **Rezerv'i**tıklatın.
3. Uygulama kaydı başarıyla oluşturulduktan sonra, uygulama adını seçin, **İleri'yi**tıklatın ve ardından **Ilişkilendir'i**tıklatın.
4. Microsoft Hesabınızı kullanarak [Windows Dev Center'da] oturum açın. **Uygulamalarım**altında oluşturduğunuz uygulama kaydına tıklayın.
5. **App yönetimi** > **App kimliğini**tıklatın ve ardından **Paket SID'nizi**bulmak için aşağı kaydırın.

Paket SID birçok kullanır bir URI olarak tedavi, bu durumda şema olarak *ms-app://* kullanmanız gerekir. Bu değeri önek olarak biraraya getirerek oluşturduğunuz SID paketinizin sürümünü not edin.

Xamarin uygulamaları, iOS veya Android platformlarında çalışan bir uygulamayı kaydedebilmek için bazı ek kodlara ihtiyaç duyar. Daha fazla bilgi için platformunuzun konusuna bakın:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="how-to-register-push-templates-to-send-cross-platform-notifications"></a><a name="register-xplat"></a>Nasıl yapilir: Platformlar arası bildirimler göndermek için itme şablonlarını kaydetme
Şablonları kaydetmek `RegisterAsync()` için, yöntemi şablonlarla birlikte aşağıdaki gibi kullanın:

```csharp
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

Şablonlarınız türolmalıdır `JObject` ve aşağıdaki JSON biçiminde birden çok şablon içerebilir:

```csharp
public JObject myTemplates()
{
    // single template for Windows Notification Service toast
    var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

    var templates = new JObject
    {
        ["generic-message"] = new JObject
        {
            ["body"] = template,
            ["headers"] = new JObject
            {
                ["X-WNS-Type"] = "wns/toast"
            },
            ["tags"] = new JArray()
        },
        ["more-templates"] = new JObject {...}
    };
    return templates;
}
```

**RegisterAsync()** yöntemi de İkincil Karolar kabul eder:

```csharp
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Güvenlik kaydı sırasında tüm etiketler çıkarılır. Yüklemelere veya yüklemelere etiketler eklemek için bkz: [Azure Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışın].

Bu kayıtlı şablonları kullanan bildirimler göndermek için [Bildirim Hub'ları API'lerine]bakın.

## <a name="miscellaneous-topics"></a><a name="misc"></a>Çeşitli Konular
### <a name="how-to-handle-errors"></a><a name="errors"></a>Nasıl yapılır: Hataları işleme
Arka uçta bir hata oluştuğunda, istemci SDK bir `MobileServiceInvalidOperationException`.  Aşağıdaki örnek, arka uç tarafından döndürülen bir özel durum nasıl işleyeceğini gösterir:

```csharp
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into the database. When the operation completes
    // and App Service has assigned an Id, the item is added to the CollectionView
    try
    {
        await todoTable.InsertAsync(todoItem);
        items.Add(todoItem);
    }
    catch (MobileServiceInvalidOperationException e)
    {
        // Handle error
    }
}
```

Hata koşulları ile ilgili başka bir örnek [Mobil Uygulamalar Dosyaları Örnek]bulunabilir. [LoggingHandler] örneği, arka uçta yapılan istekleri günlüğe kaydetmek için bir günlüğe kaydetme temsilcisi işleyicisi sağlar.

### <a name="how-to-customize-request-headers"></a><a name="headers"></a>Nasıl yapılacağını: İstek üstbilgileri özelleştirin
Belirli uygulama senaryonuzu desteklemek için Mobil Uygulama arka ucuyla iletişimi özelleştirmeniz gerekebilir. Örneğin, her giden isteğe özel bir üstbilgi eklemek ve hatta yanıt durum kodlarını değiştirmek isteyebilirsiniz. Aşağıdaki örnekte olduğu gibi özel bir [DelegatingHandler]kullanabilirsiniz:

```csharp
public async Task CallClientWithHandler()
{
    MobileServiceClient client = new MobileServiceClient("AppUrl", new MyHandler());
    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
    var newItem = new TodoItem { Text = "Hello world", Complete = false };
    await todoTable.InsertAsync(newItem);
}

public class MyHandler : DelegatingHandler
{
    protected override async Task<HttpResponseMessage>
        SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        // Change the request-side here based on the HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do the request
        var response = await base.SendAsync(request, cancellationToken);

        // Change the response-side here based on the HttpResponseMessage

        // Return the modified response
        return response;
    }
}
```


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: https://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: https://github.com/SymbolSource/SymbolSource
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Uygulamanıza anında iletme bildirimleri ekleme]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Register your app to use a Microsoft account login]: ../app-service/configure-authentication-provider-microsoft.md
[Active Directory girişi için Uygulama Hizmeti nasıl yapılandırılır?]: ../app-service/configure-authentication-provider-aad.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[Gettable]: https://msdn.microsoft.com/library/azure/jj554275(v=azure.10).aspx
[yazılmamış bir tabloya başvuru oluşturur]: https://msdn.microsoft.com/library/azure/jj554278(v=azure.10).aspx
[Deleteasync]: https://msdn.microsoft.com/library/azure/dn296407(v=azure.10).aspx
[Toplam Sayıyı Dahil Et]: https://msdn.microsoft.com/library/azure/dn250560(v=azure.10).aspx
[EkleAsync]: https://msdn.microsoft.com/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/library/azure/dn268343(v=azure.10).aspx
[GirişAsync]: https://msdn.microsoft.com/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/library/azure/jj871654(v=azure.10).aspx
[Orderby]: https://msdn.microsoft.com/library/azure/dn250572(v=azure.10).aspx
[Orderbydescending]: https://msdn.microsoft.com/library/azure/dn250568(v=azure.10).aspx
[Readasync]: https://msdn.microsoft.com/library/azure/mt691741(v=azure.10).aspx
[Take]: https://msdn.microsoft.com/library/azure/dn250574(v=azure.10).aspx
[Seç]: https://msdn.microsoft.com/library/azure/dn250569(v=azure.10).aspx
[Atlamak]: https://msdn.microsoft.com/library/azure/dn250573(v=azure.10).aspx
[Updateasync]: https://msdn.microsoft.com/library/azure/dn250536.(v=azure.10)aspx
[Userıd]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Nerede]: https://msdn.microsoft.com/library/azure/dn250579(v=azure.10).aspx
[Azure portalında]: https://portal.azure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: https://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows Dev Merkezi]: https://dev.windows.com/overview
[Delegatinghandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[PasswordVault]: https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: https://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Bildirim Hub'ları API'leri]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobil Uygulamalar Dosyaları Örnek]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 Dokümantasyon]: https://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: https://www.telerik.com/fiddler
[Json.NET]: https://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
