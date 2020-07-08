---
title: Azure Stream Analytics işleri için .NET Standard işlevleri geliştirme (Önizleme)
description: Stream Analytics işleri için c# Kullanıcı tanımlı işlevleri yazmayı öğrenin.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/28/2019
ms.custom: seodec18
ms.openlocfilehash: 4f9d117ccc763744411bfe24163ed955532e8e56
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85921864"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-jobs-preview"></a>Azure Stream Analytics işleri için .NET Standard Kullanıcı tanımlı işlevler geliştirme (Önizleme)

Azure Stream Analytics, olay verilerinin akışları üzerinde dönüşümler ve hesaplamalar gerçekleştirmek için SQL benzeri bir sorgu dili sunar. Birçok yerleşik işlev vardır ancak bazı karmaşık senaryolar ek esneklik gerektirir. Kullanıcı tanımlı işlevler (UDF) .NET Standard sayesinde, Stream Analytics sorgu dilini genişletmek için herhangi bir .NET standart dilinde (C#, F #, vb.) yazılmış kendi işlevlerinizi çağırabilirsiniz. UDF 'ler, karmaşık matematik hesaplamaları gerçekleştirmenizi, ML.NET kullanarak özel ML modellerini içeri aktarmanızı ve eksik veriler için özel imputation mantığı kullanmanıza imkan tanır. Stream Analytics işleri için UDF özelliği şu anda önizlemededir ve üretim iş yükleri içinde kullanılmamalıdır.

Bulut işlerinin .NET Kullanıcı tanımlı işlevi, ' de kullanılabilir:
* Orta Batı ABD
* Kuzey Avrupa
* Doğu ABD
* Batı ABD
* Doğu ABD 2
* Batı Avrupa

Bu özelliği başka bir bölgede kullanmaya ilgileniyorsanız, [erişim isteyebilirsiniz](https://aka.ms/ccodereqregion).

## <a name="overview"></a>Genel Bakış
Azure Stream Analytics için Visual Studio Araçları, UDF 'Leri yazmanızı, işlerinizi yerel olarak test yapmayı ve Stream Analytics işinizi Azure 'da yayımlamayı kolaylaştırır. Azure 'da yayımlandıktan sonra, IoT Hub kullanarak işinizi IoT cihazlarına dağıtabilirsiniz.

UDF 'Leri uygulamak için üç yol vardır:

* Bir ASA projesindeki CodeBehind dosyaları
* Yerel projeden UDF
* Azure Storage hesabından mevcut bir paket

## <a name="package-path"></a>Paket yolu

Herhangi bir UDF paketinin biçimi yolu vardır `/UserCustomCode/CLR/*` . Dinamik bağlantı kitaplıkları (dll 'Ler) ve kaynakları klasörü altına kopyalanır ve `/UserCustomCode/CLR/*` Bu, Kullanıcı dll 'lerinin sistem ve Azure Stream Analytics dll 'lerden yalıtılmasına yardımcı olur. Bu paket yolu, bunları kullanmak için kullanılan yöntemden bağımsız olarak tüm işlevler için kullanılır.

## <a name="supported-types-and-mapping"></a>Desteklenen türler ve eşleme
C# dilinde kullanılacak Azure Stream Analytics değerler için, bir ortamdan diğerine sıralanması gerekir. Bir UDF 'nin tüm giriş parametreleri için sıralama gerçekleşir. Her Azure Stream Analytics türü, aşağıdaki tabloda gösterilen C# dilinde karşılık gelen bir türe sahiptir:

|**Azure Stream Analytics türü** |**C# türü** |
|---------|---------|
|bigint | long |
|float | double |
|nvarchar (max) | string |
|datetime | DateTime |
|Kayıt | Sözlük\<string, object> |
|Dizi | Object [] |

Aynı durum, verilerin C# ' den Azure Stream Analytics, UDF 'nin çıkış değerinde meydana gelen olarak sıralanması gerektiğinde de geçerlidir. Aşağıdaki tabloda hangi türlerin desteklendiğini gösterilmektedir:

|**C# türü**  |**Azure Stream Analytics türü**  |
|---------|---------|
|long  |  bigint   |
|double  |  float   |
|string  |  nvarchar (max)   |
|DateTime  |  tarih saat   |
|struct  |  Kayıt   |
|nesne  |  Kayıt   |
|Object []  |  Dizi   |
|Sözlük\<string, object>  |  Kayıt   |

## <a name="codebehind"></a>CodeBehind
Kullanıcı tanımlı işlevleri **Script. ASQL** codebehind içinde yazabilirsiniz. Visual Studio Araçları otomatik olarak CodeBehind dosyasını derleme dosyasına derler. Derlemeler bir zip dosyası olarak paketlenir ve işinizi Azure 'a gönderdiğinizde depolama hesabınıza yüklenir. [Stream Analytics Edge işleri öğreticisi Için C# UDF](stream-analytics-edge-csharp-udf.md) 'Yi izleyerek codebehind kullanarak c# UDF yazma hakkında bilgi edinebilirsiniz. 

## <a name="local-project"></a>Yerel proje
Kullanıcı tanımlı işlevler, daha sonra bir Azure Stream Analytics sorgusunda başvurulan bir derlemede yazılabilir. Bu, .NET Standard dilinin, yordamsal mantık veya özyineleme gibi ifade dilinin ötesinde tam gücünü gerektiren karmaşık işlevler için önerilen seçenektir. Yerel bir projeden gelen UDF 'ler, birkaç Azure Stream Analytics sorgu arasında işlev mantığını paylaşmanız gerektiğinde de kullanılabilir. Yerel projenize UDF 'ler eklemek, işlevlerinizi Visual Studio 'dan yerel olarak hata ayıklama ve test etme olanağı sunar.

Yerel bir projeye başvurmak için:

1. Çözümünüzde yeni bir sınıf kitaplığı oluşturun.
2. Kodunuzu sınıfınıza yazın. Sınıfların *ortak* olarak tanımlanması gerektiğini ve nesnelerin *statik ortak*olarak tanımlanması gerektiğini unutmayın. 
3. Projenizi yapılandırın. Araçlar bin klasöründeki tüm yapıtları bir ZIP dosyasına paketler ve ZIP dosyasını depolama hesabına yükler. Dış başvurular için NuGet paketi yerine derleme başvurusunu kullanın.
4. Azure Stream Analytics projenizdeki yeni sınıfa başvurun.
5. Azure Stream Analytics projenize yeni bir işlev ekleyin.
6. Derleme yolunu iş yapılandırma dosyasında yapılandırın `JobConfig.json` . Derleme yolunu **Yerel proje başvurusu veya codebehind**olarak ayarlayın.
7. Hem işlev projesini hem de Azure Stream Analytics projeyi yeniden derleyin.  

### <a name="example"></a>Örnek

Bu örnekte, **udftest** bir C# sınıf kitaplığı projem ve **Asaudfdemo** , **udftest**'e başvuracaktır Azure Stream Analytics projem.

![Visual Studio 'da IoT Edge Azure Stream Analytics projesi](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. C# projenizi oluşturun, bu, Azure Stream Analytics sorgusundan C# UDF 'nize bir başvuru eklemenize olanak sağlar.
    
   ![Visual Studio 'da Azure Stream Analytics IoT Edge projesi oluşturma](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. Başvuruyu ASA projesindeki C# projesine ekleyin. Başvurular düğümüne sağ tıklayın ve başvuru Ekle ' yi seçin.

   ![Visual Studio 'da C# projesine başvuru ekleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. Listeden C# proje adını seçin. 
    
   ![Başvuru listesinden C# proje adınızı seçin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. **Çözüm Gezgini** **Başvurular** altında listelenen **udftest** ' i görmeniz gerekir.

   ![Çözüm Gezgini 'nde Kullanıcı tanımlı işlev başvurusunu görüntüleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. **İşlevler** klasörüne sağ tıklayın ve **Yeni öğe**' yi seçin.

   ![Azure Stream Analytics Edge çözümünde IŞLEVLERE yeni öğe ekleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Azure Stream Analytics projenize bir C# işlevi **SquareFunction.js** ekleyin.

   ![Visual Studio 'da Stream Analytics Edge öğelerinden CSharp işlevi seçme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. Yapılandırma iletişim kutusunu açmak için **Çözüm Gezgini** işlevine çift tıklayın.

   ![Visual Studio 'da C Sharp işlev yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. C# işlev yapılandırmasında, **asa proje başvurusundan yükle** ' yi ve açılan listeden ilgili derleme, sınıf ve yöntem adlarını seçin. Stream Analytics sorgusundaki yöntemlere, türlere ve işlevlere başvurmak için sınıfların *ortak* olarak tanımlanması ve nesnelerin *statik ortak*olarak tanımlanması gerekir.

   ![Stream Analytics C Sharp işlev yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Mevcut paketler

.NET Standard UDF 'Leri dilediğiniz IDE 'de yazabilir ve Azure Stream Analytics sorgınızdan çağırabilirsiniz. İlk olarak kodunuzu derleyin ve tüm dll 'Leri paketleyin. Paketin biçimi yolu vardır `/UserCustomCode/CLR/*` . Ardından, `UserCustomCode.zip` Azure Depolama hesabınızdaki kapsayıcının köküne yükleyin.

Azure depolama hesabınıza derleme ZIP paketleri yüklendikten sonra, Azure Stream Analytics sorgulardaki işlevleri kullanabilirsiniz. Tüm yapmanız gereken Stream Analytics iş yapılandırmasındaki depolama bilgilerini içerir. Visual Studio Araçları paketinizi indirmediği için işlevi bu seçenekle yerel olarak test edebilirsiniz. Paket yolu doğrudan hizmete ayrıştırılır. 

İş yapılandırma dosyasında derleme yolunu yapılandırmak için `JobConfig.json` :

**Kullanıcı Tanımlı Kod Yapılandırması** bölümünü genişletin ve yapılandırmaya aşağıdaki önerilen değerleri ekleyin:

   |**Ayar**|**Önerilen değer**|
   |-------|---------------|
   |Genel depolama ayarları kaynağı|Geçerli hesaptaki veri kaynağını seçin|
   |Genel depolama ayarları aboneliği| Aboneliğinizi < >|
   |Genel depolama ayarları depolama hesabı| Depolama hesabınızı < >|
   |Özel kod depolama ayarları kaynağı|Geçerli hesaptaki veri kaynağını seçin|
   |Özel kod depolama ayarları depolama hesabı|Depolama hesabınızı < >|
   |Özel kod depolama ayarları kapsayıcısı|Depolama kapsayıcınızı < >|
   |Özel kod derleme kaynağı|Buluttan mevcut derleme paketleri|
   |Özel kod derleme kaynağı|UserCustomCode.zip|

## <a name="user-logging"></a>Kullanıcı günlüğü
Günlüğe kaydetme mekanizması, bir iş çalışırken özel bilgileri yakalamanızı sağlar. Hata ayıklamak veya gerçek zamanlı olarak özel kodun doğruluğunu değerlendirmek için günlük verilerini kullanabilirsiniz.

`StreamingContext`Sınıfı, işlevini kullanarak tanılama bilgilerini yayımlamanıza olanak sağlar `StreamingDiagnostics.WriteError` . Aşağıdaki kodda Azure Stream Analytics tarafından sunulan arabirim gösterilmektedir.

```csharp
public abstract class StreamingContext
{
    public abstract StreamingDiagnostics Diagnostics { get; }
}

public abstract class StreamingDiagnostics
{
    public abstract void WriteError(string briefMessage, string detailedMessage);
}
```

`StreamingContext`, UDF yöntemine bir giriş parametresi olarak geçirilir ve özel günlük bilgilerini yayımlamak için UDF içinde kullanılabilir. Aşağıdaki örnekte, `MyUdfMethod` sorgu tarafından belirtilen bir **veri** girişi ve **context** `StreamingContext` çalışma zamanı altyapısı tarafından sağlanmış olan olarak bir bağlam girişi tanımlanmaktadır. 

```csharp
public static long MyUdfMethod(long data, StreamingContext context)
{
    // write log
    context.Diagnostics.WriteError("User Log", "This is a log message");
    
    return data;
}
```

`StreamingContext`DEĞERIN SQL sorgusu tarafından geçirilmesi gerekmez. Azure Stream Analytics bir giriş parametresi varsa otomatik olarak bir bağlam nesnesi sağlar. `MyUdfMethod`Aşağıdaki sorguda gösterildiği gibi, öğesinin kullanımı değişmez:

```sql
SELECT udf.MyUdfMethod(input.value) as udfValue FROM input
```

Günlük iletilerine [tanılama günlükleri](data-errors.md)aracılığıyla erişebilirsiniz.

## <a name="limitations"></a>Sınırlamalar
UDF önizlemesi Şu anda aşağıdaki sınırlamalara sahiptir:

* .NET Standard UDF 'ler yalnızca Visual Studio 'da yazılabilir ve Azure 'da yayımlanabilir. .NET Standard UDF 'Leri salt okuma sürümleri Azure portal **işlevler** altında görüntülenebilir. .NET Standard işlevlerinin yazılması Azure portal desteklenmez.

* Azure portal sorgu Düzenleyicisi portalda .NET Standard UDF kullanırken bir hata gösterir. 

* Özel kod Azure Stream Analytics altyapısı ile bağlam paylaştığından, özel kod Azure Stream Analytics kodla çakışan bir ad alanı/dll_name olan herhangi bir şeye başvuramaz. Örneğin, *Newtonsoft JSON*öğesine başvurulamıyor.

* Projeye dahil olan destekleyici dosyalar, işi buluta yayımladığınızda kullanılan Kullanıcı özel kod ZIP dosyasına kopyalanır. Alt klasörlerdeki tüm dosyalar, daraltılmış durumda, bulutta bulunan Kullanıcı özel kod klasörünün köküne doğrudan kopyalanır. ZIP açıldığında "düzleştirilmiş" olur.

* Kullanıcı özel kodu boş klasörleri desteklemez. Projedeki destekleyici dosyalara boş klasörler eklemeyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Stream Analytics işi için C# Kullanıcı tanımlı bir işlev yazma (Önizleme)](stream-analytics-edge-csharp-udf.md)
* [Öğretici: Azure Stream Analytics JavaScript kullanıcı tanımlı işlevleri](stream-analytics-javascript-user-defined-functions.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio 'Yu kullanma](stream-analytics-vs-tools.md)
