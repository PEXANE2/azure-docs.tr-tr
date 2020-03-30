---
title: Android için SDK nasıl kullanılır?
description: Android için Azure Mobil Uygulamalar SDK nasıl kullanılır?
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 52e91d900ce0f22862904695ba8adf463219c469
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249392"
---
# <a name="how-to-use-the-azure-mobile-apps-sdk-for-android"></a>Android için Azure Mobil Uygulamalar SDK nasıl kullanılır?

Bu kılavuz, aşağıdakiler gibi yaygın senaryoları uygulamak için Mobil Uygulamalar için Android istemcisi SDK'sını nasıl kullanacağınızı gösterir:

* Veri sorgulama (ekleme, güncelleştirme ve silme).
* Kimlik doğrulaması.
* Hataları işleme.
* İstemciyi özelleştirme.

Bu kılavuz, istemci tarafı Android SDK üzerinde duruluyor.  Mobil Uygulamalar için sunucu tarafındaki SDK'lar hakkında daha fazla bilgi edinmek için [.NET arka uç SDK ile çalışma][10] veya [Node.js backend SDK'yı nasıl kullanacağız][11]bölümüne bakın.

## <a name="reference-documentation"></a>Başvuru Belgeleri

GitHub'daki Android istemci kitaplığı için [Javadocs API başvurularını][12] bulabilirsiniz.

## <a name="supported-platforms"></a>Desteklenen Platformlar

Android için Azure Mobil Uygulamalar SDK, telefon ve tablet form faktörleri için API düzeyleri 19 ile 24 (Nougat üzerinden KitKat) destekler.  Kimlik doğrulama, özellikle, kimlik bilgilerini toplamak için ortak bir web çerçevesi yaklaşımı kullanır.  Sunucu akışı kimlik doğrulaması saatler gibi küçük form faktörü aygıtlarıyla çalışmaz.

## <a name="setup-and-prerequisites"></a>Kurulum ve Ön koşullar

Mobil [Uygulamalar hızlı başlatma](app-service-mobile-android-get-started.md) eğitimini tamamlayın.  Bu görev, Azure Mobil Uygulamaları geliştirmek için tüm ön koşulların karşılanmasını sağlar.  Quickstart ayrıca hesabınızı yapılandırmanıza ve ilk Mobil Uygulama arka uçunuzu oluşturmanıza da yardımcı olur.

Quickstart öğreticisini tamamlamamaya karar verirseniz, aşağıdaki görevleri tamamlayın:

* Android uygulamanızla kullanmak üzere [bir Mobil Uygulama arka ucu oluşturun.][13]
* Android Studio'da [Gradle build dosyalarını güncelleştirin.](#gradle-build)
* [İnternet iznini etkinleştirin.](#enable-internet)

### <a name="update-the-gradle-build-file"></a><a name="gradle-build"></a>Gradle yapı dosyasını güncelleştirme

Her iki **build.gradle** dosyayı değiştirin:

1. Bu kodu *Project* level **build.gradle** dosyasına ekleyin:

    ```gradle
    buildscript {
        repositories {
            jcenter()
            google()
        }
    }

    allprojects {
        repositories {
            jcenter()
            google()
        }
    }
    ```

2. Bu *kodu, bağımlılıklar* etiketinin içindeki *Modül uygulama* düzeyi **build.gradle** dosyasına ekleyin:

    ```gradle
    implementation 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    ```

    Şu anda en son sürümü 3.4.0 olduğunu. Desteklenen sürümler [bintray'de][14]listelenir.

### <a name="enable-internet-permission"></a><a name="enable-internet"></a>İnternet iznini etkinleştirme

Azure'a erişmek için uygulamanızın INTERNET izninin etkinleştirilmiş olması gerekir. Zaten etkinleştirilemediyse, **AndroidManifest.xml** dosyanıza aşağıdaki kod satırını ekleyin:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>İstemci Bağlantısı Oluşturma

Azure Mobile Apps mobil uygulamanız için dört işlev sağlar:

* Azure Mobil Uygulamalar Hizmeti ile Veri Erişimi ve Çevrimdışı Eşitleme.
* Azure Mobile Apps Server SDK ile yazılmış Özel API'leri arayın.
* Azure Uygulama Hizmeti Kimlik Doğrulaması ve Yetkilendirmesi ile kimlik doğrulama.
* Bildirim Hub'ları ile Anında Bildirim Kaydı.

Bu işlevlerin her biri ilk `MobileServiceClient` önce bir nesne oluşturmanızı gerektirir.  Mobil `MobileServiceClient` istemcinizde yalnızca bir nesne oluşturulmalıdır (diğer bir şey, singleton deseni olmalıdır).  Bir `MobileServiceClient` nesne oluşturmak için:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with the Site URL
    this);                  // Your application Context
```

Bu, `<MobileAppUrl>` mobil arka uçunuzu işaret eden bir dize veya URL nesnesidir.  Mobil arka arka uçunuzu barındırmak için Azure Uygulama Hizmeti `https://` kullanıyorsanız, URL'nin güvenli sürümünü kullandığınızdan emin olun.

İstemci ayrıca Etkinlik veya Bağlam `this` 'a erişim gerektirir - örnekteki parametre.  MobileServiceClient `AndroidManifest.xml` yapısı, dosyada `onCreate()` başvurulan Etkinlik yöntemi içinde gerçekleşmelidir.

En iyi uygulama olarak, sunucu iletişimini kendi (singleton-desen) sınıfına soyutlamalısınız.  Bu durumda, hizmeti uygun şekilde yapılandırmak için oluşturucu içindeki Etkinliği geçmeniz gerekir.  Örnek:

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public class AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

Artık ana `AzureServiceAdapter.Initialize(this);` etkinliğinizin `onCreate()` yöntemini arayabilirsiniz.  Hizmet bağdaştırıcısına başvuru `AzureServiceAdapter.getInstance();` almak için istemci kullanımına erişilmesi gereken diğer yöntemler.

## <a name="data-operations"></a>Veri İşlemleri

Azure Mobil Uygulamalar SDK'nın temeli, Mobil Uygulama arka ucunda SQL Azure'da depolanan verilere erişim sağlamaktır.  Bu verilere güçlü bir şekilde yazılan sınıfları (tercih edilen) veya yazılmamış sorguları (önerilmez) kullanarak erişebilirsiniz.  Bu bölümün büyük bölümü, güçlü bir şekilde yazılan sınıfların kullanılmasıyla ilgilidir.

### <a name="define-client-data-classes"></a>İstemci veri sınıflarını tanımlama

SQL Azure tablolarından verilere erişmek için, Mobil Uygulama arka ucundaki tablolara karşılık gelen istemci veri sınıflarını tanımlayın. Bu konudaki örnekler, aşağıdaki sütunlara sahip **MyDataTable**adlı bir tablo varsayar:

* id
* metin
* tamamlama

Karşılık gelen yazılı istemci tarafı **nesnemyDataTable.java**adlı bir dosyada bulunur:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Eklediğiniz her alan için getter ve ayarlayıcı yöntemleri ekleyin.  SQL Azure tablonuz daha fazla sütun içeriyorsa, bu sınıfa karşılık gelen alanları eklersiniz.  Örneğin, DTO'nun (veri aktarım nesnesi) tamsayı Öncelik sütununa sahipse, bu alanı alıcı ve ayarlayıcı yöntemleriyle birlikte ekleyebilirsiniz:

```java
private Integer priority;

/**
* Returns the item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets the item priority
*
* @param priority
*            priority to set
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

Mobil Uygulamalarınızda nasıl ek tablolar oluşturabilirsiniz öğrenmek [için bkz: Nasıl bir tablo denetleyicisi tanımlayın][15] (.NET arka uç) veya Dinamik Şema (Node.js backend) [kullanarak Tabloları Tanımla.][16]

Azure Mobil Uygulamalar arka uç tablosu, dördü istemciler tarafından kullanılabilen beş özel alan tanımlar:

* `String id`: Kayıt için genel olarak benzersiz kimlik.  En iyi uygulama olarak, bir [UUID][17] nesnesinin String temsilini yapın.
* `DateTimeOffset updatedAt`: Son güncelleştirmenin tarihi/saati.  GüncelleştirilmişAt alanı sunucu tarafından ayarlanır ve istemci kodunuz tarafından asla ayarlanamamalıdır.
* `DateTimeOffset createdAt`: Nesnenin oluşturulduğu tarih/saat.  createdAt alanı sunucu tarafından ayarlanır ve istemci kodunuz tarafından asla ayarlanamamalıdır.
* `byte[] version`: Normalde bir dize olarak temsil edilir, sürüm de sunucu tarafından ayarlanır.
* `boolean deleted`: Kaydın silindiğini ancak henüz silinmediğini gösterir.  Sınıfınızda `deleted` bir özellik olarak kullanmayın.

`id` alanı gereklidir.  Alan `updatedAt` ve `version` alan çevrimdışı eşitleme (sırasıyla artımlı eşitleme ve çakışma çözümü için) için kullanılır.  Alan `createdAt` bir başvuru alanıdır ve istemci tarafından kullanılmaz.  Adlar özelliklerin "tel ötesi" adlarıdır ve ayarlanabilir değildir.  Ancak, [gson][3] kitaplığını kullanarak nesneniz ile "tel ötesi" adları arasında bir eşleme oluşturabilirsiniz.  Örnek:

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getCreatedAt() { return mCreatedAt; }
    protected void setCreatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected void setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getVersion() { return mVersion; }
    public final void setVersion(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a>Tablo Başvurusu Oluşturma

Bir tabloya erişmek için önce [MobileServiceClient'da][9] **getTable** yöntemini arayarak bir [MobileServiceTable][8] nesnesi oluşturun.  Bu yöntemin iki aşırı yüklemesi vardır:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

Aşağıdaki **kodda, mClient** MobileServiceClient nesnenize bir başvurudur.  İlk aşırı yükleme, sınıf adı ve tablo adının aynı olduğu ve Quickstart'ta kullanılan aşağıdakiler için kullanılır:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

İkinci aşırı yükleme, tablo adı sınıf adından farklı olduğunda kullanılır: ilk parametre tablo adıdır.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query-a-backend-table"></a><a name="query"></a>Arka Uç Tablosunu Sorgula

İlk olarak, bir tablo referansı edinin.  Ardından tablo referansında bir sorgu çalıştırın.  Sorgu herhangi bir kombinasyonudur:

* Bir `.where()` [filtre yan tümcesi](#filtering).
* Bir `.orderBy()` [sipariş yan tümcesi.](#sorting)
* Alan `.select()` [seçim yan tümcesi.](#selection)
* A `.skip()` `.top()` ve [çağrılı sonuçlar](#paging)için.

Yan tümceler önceki sırada sunulmalıdır.

### <a name="filtering-results"></a><a name="filter"></a>Sonuçları Filtreleme

Bir sorgunun genel biçimi:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts the async into a sync result
```

Önceki örnek tüm sonuçları döndürür (sunucu tarafından ayarlanan maksimum sayfa boyutuna kadar).  Yöntem `.execute()` arka uçtaki sorguyu yürütür.  Sorgu, Mobil Uygulamalar arka ucuna iletilmeden önce [OData v3][19] sorgusuna dönüştürülür.  Alındıktan sonra, Mobil Uygulamalar arka ucu, sql azure örneğinde yürütmeden önce sorguyu bir SQL deyimine dönüştürür.  Ağ etkinliği biraz zaman `.execute()` aldığından, [`ListenableFuture<E>`][18]yöntem bir .

### <a name="filter-returned-data"></a><a name="filtering"></a>Döndürülen verileri filtreleme

Aşağıdaki sorgu **yürütme, tam'ın** **false**eşit olduğu **ToDoItem** tablosundaki tüm öğeleri döndürür.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable,** daha önce oluşturduğumuz mobil servis tablosuna yapılan başvurudur.

Tablo başvurusu **üzerinde** nerede yöntemi çağrısı nı kullanarak bir filtre tanımlayın. **Nerede** yöntemi mantıksal yüklem belirten bir yöntem ardından bir **alan** yöntemi takip edilir. Olası yüklem yöntemleri **eq** (eşittir), **ne** (eşit değil), **gt** (büyük), **ge** (büyük veya eşittir), **lt** (az), **le** (az veya eşit) içerir. Bu yöntemler, sayı ve dize alanlarını belirli değerlerle karşılaştırmanızı sağlar.

Tarihlere filtre uygulayabilirsiniz. Aşağıdaki yöntemler tüm tarih alanı veya tarih bölümlerini karşılaştırmak sağlar: **yıl**, **ay**, **gün**, **saat**, **dakika**, ve **ikinci**. Aşağıdaki örnek, *vade tarihi* 2013'e eşit olan öğeler için bir filtre ekler.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

Aşağıdaki yöntemler dize alanlarında karmaşık filtreleri destekler: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **replace**, **toLower**, **toUpper**, **trim**, ve **uzunluk**. Metin *sütunun* "PRI0" ile başladığı tablo satırları için aşağıdaki örnek filtrelemeolur.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

Aşağıdaki operatör yöntemleri sayı alanlarında desteklenir: **eklemek**, **alt**, **mul**, **div**, **mod**, **zemin**, **tavan**, ve **yuvarlak**. Aşağıdaki örnek, **sürenin** çift sayı olduğu tablo satırları için filtreler.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

Yüklemleri bu mantıksal yöntemlerle birleştirebilirsiniz: **ve**, **veya** **değil.** Aşağıdaki örnek, önceki örneklerden ikisini birleştirir.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Mantıksal işleçleri grupla ve iç içe:

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

Daha ayrıntılı tartışma ve filtreleme örnekleri için Android [istemci sorgu modelinin zenginliğini keşfetme bölümüne][20]bakın.

### <a name="sort-returned-data"></a><a name="sorting"></a>Döndürülen verileri sıralama

Aşağıdaki kod, *metin* alanına göre artan **ToDoItems** tablosundaki tüm öğeleri döndürür. *mToDoTable,* daha önce oluşturduğunuz arka uç tablosuna yapılan başvurudur:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

**orderBy** yönteminin ilk parametresi, sıralamak için alanın adına eşit bir dizedir. İkinci parametre, artan veya azalan sıralamaolup olmadığını belirtmek için **QueryOrder** numaralandırmasını kullanır.  ***Where*** yöntemini kullanarak filtreleme yapıyorsunuz, ***nerede*** yöntemi ***orderBy*** yönteminden önce çağrılmalıdır.

### <a name="select-specific-columns"></a><a name="selection"></a>Belirli sütunları seçme

Aşağıdaki kod, **ToDoItems**tablosundaki tüm öğelerin nasıl döndürüleceklerini gösterir, ancak yalnızca **tam** ve **metin** alanlarını görüntüler. **mToDoTable,** daha önce oluşturduğumuz arka uç tablosuna yapılan başvurudur.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

Seçme işlevinin parametreleri, döndürmek istediğiniz tablo sütunlarının dize adlarıdır.  **Select** yöntemi **nin nerede** ve **orderBy**gibi yöntemleri izlemesi gerekir. Bu **atlama** ve **üst**gibi sayfalama yöntemleri ile takip edilebilir.

### <a name="return-data-in-pages"></a><a name="paging"></a>Sayfalarda veri döndürme

Veriler **HER ZAMAN** sayfalarda döndürülür.  Döndürülen en fazla kayıt sayısı sunucu tarafından ayarlanır.  İstemci daha fazla kayıt isterse, sunucu en fazla kayıt sayısını döndürür.  Varsayılan olarak, sunucudaki en büyük sayfa boyutu 50 kayıttır.

İlk örnek, bir tablodan ilk beş öğenin nasıl seçilebildiğini gösterir. Sorgu, **Öğeleri ToDoItems**tablosundan döndürür. **mToDoTable,** daha önce oluşturduğunuz arka uç tablosuna yapılan başvurudur:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

Burada, ilk beş öğeyi atlayan ve sonraki beş öğeyi döndüren bir sorgu ve ardından:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Tüm kayıtları bir tabloda almak istiyorsanız, tüm sayfalarda yineleyecek kod uygulayın:

```java
List<MyDataModel> results = new ArrayList<>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

Bu yöntemi kullanan tüm kayıtlar için bir istek, Mobil Uygulamalar arka ucuna en az iki istek oluşturur.

> [!TIP]
> Doğru sayfa boyutunu seçmek, istek olurken bellek kullanımı, bant genişliği kullanımı ve verilerin tamamen alınmasındaki gecikme arasında bir dengedir.  Varsayılan (50 kayıt) tüm aygıtlar için uygundur.  Yalnızca daha büyük bellek aygıtlarında çalışıyorsanız, 500'e kadar artırın.  Sayfa boyutunun 500 kaydın üzerinde artırılmasının kabul edilemez gecikmelere ve büyük bellek sorunlarına neden olduğunu gördük.

### <a name="how-to-concatenate-query-methods"></a><a name="chaining"></a>Nasıl yapılır: Sorgu yöntemlerini eşleştirin

Arka uç tablolarının sorgulanmasında kullanılan yöntemler kısıtlanabilir. Zincirleme sorgu yöntemleri, sıralanmış ve çağrılı filtre uygulanmış belirli sütunları seçmenize olanak tanır. Karmaşık mantıksal filtreler oluşturabilirsiniz.  Her sorgu yöntemi bir Sorgu nesnesi döndürür. Yöntem serisini sona erdirmek ve sorguyu fiilen çalıştırmak için **yürütme** yöntemini arayın. Örnek:

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

Zincirleme sorgu yöntemleri aşağıdaki gibi sıralanmalıdır:

1. Filtreleme (**nerede**) yöntemleri.
2. Sıralama **(orderBy**) yöntemleri.
3. Seçim (**select**) yöntemleri.
4. (atlama**skip** ve **üst**) yöntemleri.

## <a name="bind-data-to-the-user-interface"></a><a name="binding"></a>Verileri kullanıcı arabirimine bağlama

Veri bağlama üç bileşen içerir:

* Veri kaynağı
* Ekran düzeni
* İkisini birbirine bağlayan bağdaştırıcı.

Örnek kodumuzda, Mobil Uygulamalar SQL Azure tablosu **ToDoItem'deki** verileri bir diziye döndüreriz. Bu etkinlik, veri uygulamaları için ortak bir desendir.  Veritabanı sorguları genellikle istemcinin bir liste veya dizide aldığı satırlar koleksiyonunu döndürür. Bu örnekte, dizi veri kaynağıdır.  Kod, aygıtta görüntülenen verilerin görünümünü tanımlayan bir ekran düzeni belirtir.  Bu kodda **ArrayAdapter&lt;ToDoItem&gt; ** sınıfının bir uzantısı olan bir bağdaştırıcı ile bağlanır.

#### <a name="define-the-layout"></a><a name="layout"></a>Düzeni Tanımla

Düzen, XML kodunun birkaç kesiti ile tanımlanır. Varolan bir düzen göz önüne alındığında, aşağıdaki kod sunucu verilerimizle doldurmak istediğimiz **ListView'ı** temsil eder.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

Önceki kodda, *listitem* özniteliği, listedeki tek bir satırın düzeninin kimliğini belirtir. Bu kod bir onay kutusu ve ilişkili metni belirtir ve listedeki her öğe için bir kez anında alır. Bu düzen **id** alanını görüntülemez ve daha karmaşık bir düzen ekranda ek alanlar belirtir. Bu kod **row_list_to_do.xml** dosyasında dır.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <a name="define-the-adapter"></a><a name="adapter"></a>Bağdaştırıcıyı tanımlayın
Görünümümüzün veri kaynağı bir dizi **ToDoItem**olduğundan, adaptörümüzü **ArrayAdapter&lt;ToDoItem&gt; ** sınıfından alt sınıfa sınıyoruz. Bu alt **sınıf, row_list_to_do** düzenini kullanarak her **ToDoItem** için bir Görünüm üretir.  Kodumuzda, **ArrayAdapter&lt;E&gt; ** sınıfının bir uzantısı olan aşağıdaki sınıfı tanımlarız:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Bağdaştırıcıları **getView** yöntemini geçersiz kılın. Örnek:

```java
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

Etkinliğimizde bu sınıfın bir örneğini aşağıdaki gibi oluştururuz:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

ToDoItemAdapter oluşturucuiçin ikinci parametre düzene bir başvurudur. Artık **ListView'ı** anında anlayabilir ve bağdaştırıcıyı **ListView'a**atayabiliriz.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-the-adapter-to-bind-to-the-ui"></a><a name="use-adapter"></a>Uyarlayıcıyı UI'ye Bağlamak için kullanma

Artık veri bağlama kullanmaya hazırsınız. Aşağıdaki kod, maddelerin tabloya nasıl alındığını gösterir ve yerel bağdaştırıcıyı döndürülen öğelerle doldurur.

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

**ToDoItem** tablosunu her değiştirdiğinizde bağdaştırıcıyı arayın. Değişiklikler kayıt bazında kayıt bazında yapıldığından, koleksiyon yerine tek bir satırı işlersiniz. Bir öğe eklediğinizde, bağdaştırıcıdaki **ekle** yöntemini çağırın; silerken **kaldırma** yöntemini arayın.

[Android Quickstart Projesi'nde][21]tam bir örnek bulabilirsiniz.

## <a name="insert-data-into-the-backend"></a><a name="inserting"></a>Arka uca veri ekleme

*ToDoItem* sınıfının bir örneğini anında belirleyin ve özelliklerini ayarlayın.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Sonra bir nesne eklemek için **insert()** kullanın:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

Döndürülen varlık, arka uç tablosuna eklenen verilerle eşleşir, kimliği ve arka `createdAt` `updatedAt`uçta `version` ayarlanan diğer değerleri (, , ve alanlar gibi) dahil eder.

Mobil Uygulamalar **tabloları, id**adlı birincil anahtar sütun gerektirir. Bu sütun bir dize olmalıdır. Kimlik sütununun varsayılan değeri bir GUID'dir.  E-posta adresleri veya kullanıcı adları gibi diğer benzersiz değerleri sağlayabilirsiniz. Eklenen bir kayıt için dize kimliği değeri sağlanmadığında, arka uç yeni bir GUID oluşturur.

String ID değerleri aşağıdaki avantajları sağlar:

* Veritabanlarına gidiş-dönüş yapmadan, iYe'ler oluşturulabilir.
* Kayıtların farklı tablolardan veya veritabanlarından birleştirilmesi daha kolaydır.
* Kimlik değerleri, uygulamanın mantığıyla daha iyi tümleşir.

String ID değerleri çevrimdışı eşitleme desteği için **GEREKLIDIR.**  Arka uç veritabanında depolandıktan sonra kimliği değiştiremezsiniz.

## <a name="update-data-in-a-mobile-app"></a><a name="updating"></a>Mobil uygulamadaki verileri güncelleştirme

Tablodaki verileri güncelleştirmek için yeni nesneyi **güncelleştirme()** yöntemine geçirin.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

Bu örnekte, *madde* *ToDoItem* tablosunda bazı değişiklikler yapılmış bir satıra bir başvurudur.  Aynı **kimlikle** satır güncelleştirilir.

## <a name="delete-data-in-a-mobile-app"></a><a name="deleting"></a>Mobil uygulamadaki verileri silme

Aşağıdaki kod, veri nesnesini belirterek tablodaki verilerin nasıl silinir olduğunu gösterir.

```java
mToDoTable
    .delete(item);
```

Ayrıca, silmek için satırın **id** alanını belirterek bir öğeyi silebilirsiniz.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="look-up-a-specific-item-by-id"></a><a name="lookup"></a>Belirli bir öğeyi Id'ye göre arama

**LookUp()** yöntemiyle belirli bir **kimlik** alanına sahip bir öğeyi arayın:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="how-to-work-with-untyped-data"></a><a name="untyped"></a>Nasıl kullanılır: Yazılmamış verilerle çalışma

Yazılmamış programlama modeli, JSON serileştirme üzerinde tam denetim sağlar.  Yazılmamış bir programlama modeli kullanmak isteyebileceğiniz bazı yaygın senaryolar vardır. Örneğin, arka uç tablonuz çok sayıda sütun içeriyorsa ve yalnızca sütunların bir alt kümesine başvurmanız gerekir.  Yazılan model, veri sınıfınızda Mobil Uygulamalar arka ucunda tanımlanan tüm sütunları tanımlamanızı gerektirir.  Verilere erişim için YAPıLAN API çağrılarının çoğu, yazılan programlama çağrılarına benzer. Temel fark, yazılmamış modelde **MobileServiceJsonTable** nesnesi yerine **MobileServiceJsonTable** nesnesi üzerinde yöntemleri çağırmak olmasıdır.

### <a name="create-an-instance-of-an-untyped-table"></a><a name="json_instance"></a>Yazılmamış tablo örneği oluşturma

Yazılan modele benzer şekilde, bir tablo başvurusu alarak başlarsınız, ancak bu durumda bu bir **MobileServicesJsonTable** nesnesidir. İstemcinin bir örneğinde **getTable** yöntemini arayarak başvuruyu alın:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

Bir kez **MobileServiceJsonTable**bir örnek oluşturduk , bu dakti-yazılmış programlama modeli ile hemen hemen aynı API vardır. Bazı durumlarda, yöntemler, yazılı bir parametre yerine yazılmamış bir parametre alır.

### <a name="insert-into-an-untyped-table"></a><a name="json_insert"></a>Yazılmamış tabloya ekleme
Aşağıdaki kod, eklemenin nasıl yapılacağını gösterir. İlk adım, [gson][1]kitaplığın bir parçası olan bir [JsonObject][3] oluşturmaktır.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Ardından, yazılmamış nesneyi tabloya eklemek için **ekle()** kullanın.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Eklenen nesnenin kimliğini almanız gerekiyorsa, **getAsJsonPrimitive()** yöntemini kullanın.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="delete-from-an-untyped-table"></a><a name="json_delete"></a>Yazılmamış bir tablodan silme
Aşağıdaki kod, bu durumda, önceki *ekleme* örneğinde oluşturulan **jsonObject'in** aynı örneğinde bir örneğin nasıl silindiğini gösterir. Kod, yazılan servis talebiyle aynıdır, ancak **jsonObject'e**başvuruldığından yöntemin farklı bir imzası vardır.

```java
mToDoTable
    .delete(insertedItem);
```

Ayrıca, bir örneği doğrudan kimliğini kullanarak silebilirsiniz:

```java
mToDoTable.delete(ID);
```

### <a name="return-all-rows-from-an-untyped-table"></a><a name="json_get"></a>Yazılmamış tablodaki tüm satırları döndürme
Aşağıdaki kod, tüm tablonun nasıl alınır olduğunu gösterir. JSON Tablosu kullandığınızdan, tablonun sütunlarından yalnızca bazılarını seçarak alabilirsiniz.

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

Daktio model için kullanılabilen aynı filtreleme, filtreleme ve sayfalama yöntemleri kümesi, yazılmamış model için kullanılabilir.

## <a name="implement-offline-sync"></a><a name="offline-sync"></a>Çevrimdışı Eşitleme uygulama

Azure Mobil Apps İstemci SDK, sunucu verilerinin bir kopyasını yerel olarak depolamak için bir SQLite veritabanı kullanarak verilerin çevrimdışı eşitlemesi de uygular.  Çevrimdışı bir tabloda gerçekleştirilen işlemler, çalışmak için mobil bağlantı gerektirmez.  Çakışma çözümü için daha karmaşık mantık pahasına esneklik ve performans çevrimdışı eşitleme yardımcıları.  Azure Mobil Apps İstemci SDK aşağıdaki özellikleri uygular:

* Artımlı Eşitleme: Bant genişliği ve bellek tüketimi kaydedilen, yalnızca güncelleştirilmiş ve yeni kayıtlar indirilir.
* İyimser Eşzamanlılık: Operasyonların başarılı olduğu varsayılır.  Çakışma Çözümlemesi, güncelleştirmeler sunucuda gerçekleştirilene kadar ertelenir.
* Çakışma Çözümü: SDK, sunucuda çakışan bir değişiklik yapıldığında algılar ve kullanıcıyı uyarmak için kancalar sağlar.
* Yumuşak Silme: Silinen kayıtlar silinerek işaretlenir ve diğer aygıtların çevrimdışı önbelleğini güncelleştirmelerine olanak tanır.

### <a name="initialize-offline-sync"></a>Çevrimdışı Eşitleme başlatma

Her çevrimdışı tablo, kullanmadan önce çevrimdışı önbellekte tanımlanmalıdır.  Normalde, tablo tanımı istemcioluşturulduktan hemen sonra yapılır:

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with the model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define the table in the local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize the local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-to-the-offline-cache-table"></a>Çevrimdışı Önbellek Tablosuna başvuru alma

Çevrimiçi bir tablo için. `.getTable()`  Çevrimdışı bir tablo için `.getSyncTable()`şunları kullanın:

```java
MobileServiceSyncTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

Çevrimiçi tablolar için kullanılabilen tüm yöntemler (filtreleme, sıralama, sayfalama, veri ekleme, verileri güncelleştirme ve verileri silme dahil) çevrimiçi ve çevrimdışı tablolarda eşit derecede iyi çalışır.

### <a name="synchronize-the-local-offline-cache"></a>Yerel Çevrimdışı Önbelleği eşitle

Eşitleme uygulamanızın denetimi ndedir.  Aşağıda bir örnek eşitleme yöntemi verilmiştir:

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

`.pull(query, queryname)` Yönteme bir sorgu adı sağlanırsa, yalnızca başarıyla tamamlanan çekmeden bu yana oluşturulan veya değiştirilen kayıtları döndürmek için artımlı eşitleme kullanılır.

### <a name="handle-conflicts-during-offline-synchronization"></a>Çevrimdışı Eşitleme sırasında Çakışmaları işleme

Bir `.push()` işlem sırasında bir çakışma olursa, bir `MobileServiceConflictException` atılır.   Sunucu tarafından verilen öğe özel durum gömülüdür ve `.getItem()` özel durum tarafından alınabilir.  MobileServiceSyncContext nesnesindeki aşağıdaki öğeleri arayarak itmeyi ayarlayın:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

Tüm çakışmalar istediğiniz gibi `.push()` işaretlendikten sonra, tüm çakışmaları çözmek için yeniden arayın.

## <a name="call-a-custom-api"></a><a name="custom-api"></a>Özel bir API çağırın

Özel API, ekleme, güncelleştirme, silme veya okuma işlemine eş olmayan sunucu işlevselliğini ortaya çıkaran özel uç noktaları tanımlamanızı sağlar. Özel bir API kullanarak, HTTP ileti üstbilgilerini okuma ve ayarlama ve JSON dışındaki ileti gövdesi biçimini tanımlama dahil olmak üzere mesajlaşma üzerinde daha fazla denetime sahip olabilirsiniz.

Bir Android istemcisinden, özel API bitiş noktasını aramak için **invokeApi** yöntemini çağırırsınız. Aşağıdaki örnek, **MarkAllResult**adlı bir toplama sınıfı döndüren **completeAll**adlı bir API bitiş noktasının nasıl çağrılmasını gösterir.

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

**InvokeApi** yöntemi, yeni özel API'ye BIR POST isteği gönderen istemciye çağrılır. Özel API tarafından döndürülen sonuç, hatalar gibi bir ileti iletişim kutusunda görüntülenir. **invokeApi'nin** diğer sürümleri isteğe bağlı olarak istek gövdesine bir nesne göndermenize, HTTP yöntemini belirtmenize ve istekle birlikte sorgu parametrelerini göndermenize izin verebiliyor. **InvokeApi'nin** yazılmamış sürümleri de sağlanır.

## <a name="add-authentication-to-your-app"></a><a name="authentication"></a>Uygulamanıza kimlik doğrulaması ekleme

Öğreticiler bu özelliklerin nasıl ekleyeceğini ayrıntılı olarak açıklar.

Uygulama Hizmeti, [uygulama kullanıcılarının kimlik doğrulamalarını](app-service-mobile-android-get-started-users.md) çeşitli dış kimlik sağlayıcılarını kullanarak destekler: Facebook, Google, Microsoft Account, Twitter ve Azure Active Directory. Belirli işlemlere erişimi yalnızca kimlik doğrulaması yapılan kullanıcılarla sınırlamak için izinleri tablolarda ayarlayabilirsiniz. Arka uçunuzda yetkilendirme kuralları nı uygulamak için kimlik doğrulaması yapılan kullanıcıların kimliğini de kullanabilirsiniz.

İki kimlik doğrulama akışı desteklenir: **sunucu** akışı ve **istemci** akışı. Sunucu akışı, kimlik sağlayıcıları web arabirimine dayandığı için en basit kimlik doğrulama deneyimini sağlar.  Sunucu akışı kimlik doğrulaması uygulamak için ek SDK'lar gerekmez. Sunucu akışı kimlik doğrulaması mobil aygıta derin bir tümleştirme sağlamaz ve yalnızca kavram senaryolarının kanıtı için önerilir.

İstemci akışı, kimlik sağlayıcısı tarafından sağlanan SDK'lara dayandığı için tek oturum açma gibi cihaza özgü özelliklerle daha derin tümleştirme sağlar.  Örneğin, Facebook SDK'yı mobil uygulamanıza entegre edebilirsiniz.  Mobil istemci Facebook uygulamasına dönüşür ve mobil uygulamanıza geri dönmeden önce oturum açmanızı onaylar.

Uygulamanızda kimlik doğrulamayı etkinleştirmek için dört adım gereklidir:

* Uygulamanızı kimlik doğrulama için bir kimlik sağlayıcısına kaydedin.
* Uygulama Hizmeti arka uçunuzu yapılandırın.
* Tablo izinlerini yalnızca App Service arka ucundaki kimliği doğrulanmış kullanıcılarla sınırlandırın.
* Uygulamanıza kimlik doğrulama kodu ekleyin.

Belirli işlemlere erişimi yalnızca kimlik doğrulaması yapılan kullanıcılarla sınırlamak için izinleri tablolarda ayarlayabilirsiniz. İstekleri değiştirmek için kimliği doğrulanmış bir kullanıcının SID'sini de kullanabilirsiniz.  Daha fazla bilgi için kimlik [doğrulama] ve Server SDK HOWTO belgelerini gözden geçirin.

### <a name="authentication-server-flow"></a><a name="caching"></a>Kimlik Doğrulama: Sunucu Akışı

Aşağıdaki kod, Google sağlayıcısını kullanarak bir sunucu akışı giriş işlemini başlatır.  Google sağlayıcısının güvenlik gereksinimleri nedeniyle ek yapılandırma gereklidir:

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

Buna ek olarak, ana Etkinlik sınıfına aşağıdaki yöntemi ekleyin:

```java
// You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check the request code matches the one we send in the login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check the error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

Ana `GOOGLE_LOGIN_REQUEST_CODE` Aktivitenizde tanımlanan `login()` yöntem ve `onActivityResult()` yöntem içinde kullanılır.  `login()` Yöntem ve `onActivityResult()` yöntem içinde aynı sayı kullanıldığı sürece, herhangi bir benzersiz sayı seçebilirsiniz.  İstemci kodunu bir hizmet bağdaştırıcısına soyutlarsanız (daha önce gösterildiği gibi), hizmet bağdaştırıcısı üzerinde uygun yöntemleri aramalısınız.

Ayrıca özel sekmeler için projeyi yapılandırmanız gerekir.  Önce bir yönlendirme URL'si belirtin.  Aşağıdaki snippet `AndroidManifest.xml`ekleyin:

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

Uygulamanız için dosyaya `build.gradle` **yönlendirmeUriScheme** ekleyin:

```gradle
android {
    buildTypes {
        release {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
        debug {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
    }
}
```

Son olarak, dosyadaki bağımlılıklar `build.gradle` listesine ekleyin: `com.android.support:customtabs:28.0.0`

```gradle
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.google.code.gson:gson:2.3'
    implementation 'com.google.guava:guava:18.0'
    implementation 'com.android.support:customtabs:28.0.0'
    implementation 'com.squareup.okhttp:okhttp:2.5.0'
    implementation 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

**getUserId** yöntemini kullanarak bir **MobileServiceUser'dan** oturum açan kullanıcının kimliğini alın. Asynchronous giriş API'lerini aramak için Futures'ın nasıl kullanılacağına bir örnek için [bkz.]

> [!WARNING]
> Belirtilen URL Düzeni büyük/küçük harf duyarlıdır.  `{url_scheme_of_you_app}` Eşmatch case tüm oluşumları emin olun.

### <a name="cache-authentication-tokens"></a><a name="caching"></a>Önbellek kimlik doğrulama belirteçleri

Kimlik doğrulama belirteçlerini önbelleğe almak, Kullanıcı Kimliğini ve kimlik doğrulama belirtecisini aygıtta yerel olarak saklamanızı gerektirir. Uygulama bir sonraki başlatışta önbelleği kontrol elabilirsiniz ve bu değerler varsa, oturum açma yordamını atlayabilir ve istemciyi bu verilerle yeniden sulendirebilirsiniz. Ancak bu veriler hassastır ve telefonun çalınması durumunda güvenlik için şifreli olarak saklanmalıdır.  [Önbellek kimlik][7]belirteçleri bölümünde kimlik doğrulama belirteçlerinin nasıl önbelleğe alındığının tam bir örneğini görebilirsiniz.

Süresi dolmuş bir belirteci kullanmaya çalıştığınızda, *401 yetkisiz* yanıt alırsınız. Filtreleri kullanarak kimlik doğrulama hatalarını işleyebilirsiniz.  Filtreler, App Service arka ucuna gelen istekleri yakalar. Filtre kodu bir 401 yanıtını sınar, oturum açma işlemini tetikler ve ardından 401'i oluşturan isteği devam ettirer.

### <a name="use-refresh-tokens"></a><a name="refresh"></a>Belirteçleri Yenile'yi Kullanma

Azure App Service Authentication and Authorization tarafından döndürülen belirteç, tanımlanmış bir kullanım ömrüne sahiptir.  Bu dönemden sonra, kullanıcının yeniden kimlik doğrulaması gerekir.  İstemci akışı kimlik doğrulaması yoluyla aldığınız uzun ömürlü bir belirteci kullanıyorsanız, aynı belirteci kullanarak Azure Uygulama Hizmeti Kimlik Doğrulaması ve Yetkilendirme ile yeniden kimlik doğrulaması yapabilirsiniz.  Başka bir Azure Uygulama Hizmeti belirteci yeni bir kullanım ömrüyle oluşturulur.

Ayrıca, Yenile Belirteçleri'ni kullanmak için sağlayıcıyı da kaydedebilirsiniz.  Yenile Belirteci her zaman kullanılamaz.  Ek yapılandırma gereklidir:

* **Azure Etkin Dizin için,** Azure Etkin Dizin Uygulaması için bir istemci sırrını yapılandırın.  Azure Etkin Dizin Kimlik Doğrulaması'nı yapılandırırken Azure Uygulama Hizmeti'nde istemci sırrını belirtin.  Ararken, `.login()` `response_type=code id_token` parametre olarak geçirin:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* **Google**için, `access_type=offline` bir parametre olarak geçmek:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* **Microsoft Hesabı**için `wl.offline_access` kapsamı seçin.

Bir belirteci yenilemek için: `.refreshUser()`

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

En iyi yöntem olarak, sunucudan 401 yanıtı algılayan ve kullanıcı belirteciyenilemeye çalışan bir filtre oluşturun.

## <a name="log-in-with-client-flow-authentication"></a>İstemci akışı Kimlik Doğrulaması ile giriş yapın

İstemci akışı kimlik doğrulaması ile oturum açmak için genel işlem aşağıdaki gibidir:

* Azure Uygulama Hizmeti Kimlik Doğrulaması ve Yetkilendirme'yi sunucu akışı kimlik doğrulaması gibi yapılandırın.
* Bir erişim belirteci oluşturmak için kimlik doğrulama sağlayıcısı SDK'yı kimlik doğrulama için tümleştirin.
* `.login()` Metodu aşağıdaki`result` gibi çağırın ( olmalıdır): `AuthenticationResult`

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

Bir sonraki bölümdeki kod örneğinin tamamına bakın.

`onSuccess()` Yöntemi, başarılı bir girişte kullanmak istediğiniz kodla değiştirin.  Dize `{provider}` geçerli bir sağlayıcıdır: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, veya **twitter**.  Özel kimlik doğrulaması uyguladıysanız, özel kimlik doğrulama sağlayıcısı etiketini de kullanabilirsiniz.

### <a name="authenticate-users-with-the-active-directory-authentication-library-adal"></a><a name="adal"></a>Active Directory Authentication Library (ADAL) ile kullanıcıları kimlik doğrulaması

Azure Active Directory kullanarak kullanıcıları uygulamanızda oturum haline getirmek için Etkin Dizin Kimlik Doğrulama Kitaplığını (ADAL) kullanabilirsiniz. Bir istemci akışı giriş kullanarak genellikle daha `loginAsync()` yerel ux hissediyorum sağlar ve ek özelleştirme sağlar gibi yöntemleri kullanarak tercih edilir.

1. [Active Directory giriş][22] için Uygulama Hizmetini yapılandırma yı izleyerek AAD oturum açma için mobil uygulama arka uçunuzu yapılandırın. Yerel istemci uygulamasını kaydetmenin isteğe bağlı adımını tamamladıktan emin olun.
2. build.gradle dosyanızı aşağıdaki tanımları içerecek şekilde değiştirerek ADAL'ı yükleyin:

    ```gradle
    repositories {
        mavenCentral()
        flatDir {
            dirs 'libs'
        }
        maven {
            url "YourLocalMavenRepoPath\\.m2\\repository"
        }
    }
    packagingOptions {
        exclude 'META-INF/MSFTSIG.RSA'
        exclude 'META-INF/MSFTSIG.SF'
    }
    dependencies {
        implementation fileTree(dir: 'libs', include: ['*.jar'])
        implementation('com.microsoft.aad:adal:1.16.1') {
            exclude group: 'com.android.support'
        } // Recent version is 1.16.1
        implementation 'com.android.support:support-v4:28.0.0'
    }
    ```

3. Uygulamanıza aşağıdaki kodları ekleyin ve aşağıdaki değiştirmeleri yapıtınız:

    * **INSERT-AUTHORITY-HERE'ı** başvurunuzu yaptığınız kiracının adı ile değiştirin. Biçimi olmalıdır. https://login.microsoftonline.com/contoso.onmicrosoft.com
    * **INSERT-RESOURCE-ID-HERE'ı** mobil uygulama arka ucunuz için istemci kimliğiyle değiştirin. Portaldaki **Azure Etkin Dizin Ayarları** altında **Gelişmiş** sekmesinden istemci kimliğini edinebilirsiniz.
    * **INSERT-CLIENT-ID-HERE'ı** yerel istemci uygulamasından kopyaladığınız istemci kimliğiyle değiştirin.
    * HTTPS şemasını kullanarak **INSERT-REDIRECT-URI-HERE'ı** sitenizin */.auth/login/done* endpoint'iyle değiştirin. Bu değer *https://contoso.azurewebsites.net/.auth/login/done*.

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <a name="adjust-the-client-server-communication"></a><a name="filters"></a>İstemci-Sunucu İletişimini Ayarlama

İstemci bağlantısı normalde Android SDK ile birlikte verilen temel HTTP kitaplığını kullanarak temel bir HTTP bağlantısıdır.  Bunu değiştirmek istemenizin birkaç nedeni vardır:

* Zaman zamanlarını ayarlamak için alternatif bir HTTP kitaplığı kullanmak isteyebilirsiniz.
* İlerleme çubuğu sağlamak istiyorsun.
* API yönetimi işlevselliğini desteklemek için özel bir üstbilgi eklemek isteyebilirsiniz.
* Yeniden kimlik doğrulaması uygulayabilmek için başarısız bir yanıtı engellemek istersiniz.
* Arka uç isteklerini bir analiz hizmetine günlüğe kaydetmek istersiniz.

### <a name="using-an-alternate-http-library"></a>Alternatif bir HTTP Kitaplığı kullanma

İstemci `.setAndroidHttpClientFactory()` başvurunuzu oluşturduktan hemen sonra yöntemi arayın.  Örneğin, bağlantı zaman dilimini 60 saniyeye ayarlamak için (varsayılan 10 saniye yerine):

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClient();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a>İlerleme Filtresi Uygulama

Bir `ServiceFilter`.  Örneğin, önceden oluşturulmuş bir ilerleme çubuğu aşağıdaki güncelleştirmeleri:

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

Bu filtreyi istemciye aşağıdaki gibi ekleyebilirsiniz:

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a>İstek Üstbilgilerini Özelleştir

Aşağıdakileri `ServiceFilter` kullanın ve filtreyi aşağıdaki gibi `ProgressFilter`takın:

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <a name="configure-automatic-serialization"></a><a name="conversions"></a>Otomatik Serileştirmeyi Yapılandır

[Gson][3] API'sini kullanarak her sütun için geçerli olan bir dönüşüm stratejisi belirtebilirsiniz. Android istemci kitaplığı, veriler Azure Uygulama Hizmeti'ne gönderilmeden önce Java nesnelerini JSON verilerine seri hale getirmek için arka planda [gson][3] kullanır.  Aşağıdaki kod stratejiyi ayarlamak için **setFieldNamingStrategy()** yöntemini kullanır. Bu örnek, her alan adı için ilk karakteri ("m") ve sonraki karakteri küçük harfle siler. Örneğin, "mId"i "id"e dönüştürür.  Çoğu alanda ek açıklama gereksinimini azaltmak için `SerializedName()` bir dönüşüm stratejisi uygulayın.

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStrategy)
);
```

**MobileServiceClient**kullanarak bir mobil istemci başvurusu oluşturmadan önce bu kodun yürütülmesi gerekir.

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: https://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-android-get-started-users.md
[1]: https://static.javadoc.io/com.google.code.gson/gson/2.8.5/com/google/gson/JsonObject.html
[2]: https://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: https://www.javadoc.io/doc/com.google.code.gson/gson/2.8.5
[4]: https://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: https://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: https://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: https://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: https://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: https://www.odata.org/documentation/odata-version-3-0/
[20]: https://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: ../app-service/configure-authentication-provider-aad.md
[Future]: https://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: https://developer.android.com/reference/android/os/AsyncTask.html
