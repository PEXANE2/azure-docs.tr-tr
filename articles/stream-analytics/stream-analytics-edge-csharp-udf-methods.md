---
title: Azure Akış Analizi işleri için .NET Standart işlevleri geliştirme (Önizleme)
description: Stream Analytics işleri için c# kullanıcı tanımlı işlevleri nasıl yazacaklarınızı öğrenin.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/28/2019
ms.custom: seodec18
ms.openlocfilehash: f07c02df1b8e0032c9e1b4ef9a24c345fee20a40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426308"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-jobs-preview"></a>Azure Akış Analizi işleri için .NET Standart kullanıcı tanımlı işlevler geliştirme (Önizleme)

Azure Akış Analizi, olay veri akışları üzerinden dönüşümler ve hesaplamalar gerçekleştirmek için SQL benzeri bir sorgu dili sunar. Birçok yerleşik işlev vardır, ancak bazı karmaşık senaryolar ek esneklik gerektirir. .NET Standart kullanıcı tanımlı işlevlerle (UDF) Akış Analizi sorgu dilini genişletmek için herhangi bir .NET standart dilde (C#, F#, vb.) yazılmış kendi işlevlerinizi çağırabilirsiniz. UDF'ler karmaşık matematik hesaplamaları gerçekleştirmenize, ML.NET kullanarak özel ML modellerini içe aktarmanıza ve eksik veriler için özel imputasyon mantığı kullanmanıza olanak sağlar. Stream Analytics işleri için UDF özelliği şu anda önizlemededir ve üretim iş yüklerinde kullanılmamalıdır.

.NET bulut işleri için kullanıcı tanımlı işlev:
* Orta Batı ABD
* Kuzey Avrupa
* Doğu ABD
* Batı ABD
* Doğu ABD 2
* Batı Avrupa

Bu özelliği başka bir bölgede kullanmak istiyorsanız, [erişim isteğinde](https://aka.ms/ccodereqregion)bulunabilirsiniz.

## <a name="overview"></a>Genel Bakış
Azure Akış Analizi için Visual Studio araçları, UDF yazmanızı, işinizi yerel olarak (hatta çevrimdışı) test etmenizi ve Stream Analytics işinizi Azure'da yayınlamanızı kolaylaştırır. Azure'da yayımlandıktan sonra, IoT Hub'ını kullanarak işinizi IoT aygıtlarına dağıtabilirsiniz.

UDF'leri uygulamanın üç yolu vardır:

* Bir ASA projesinde CodeBehind dosyaları
* Yerel bir projeden UDF
* Azure depolama hesabından varolan bir paket

## <a name="package-path"></a>Paket yolu

Herhangi bir UDF paketinin `/UserCustomCode/CLR/*`biçimi yolu vardır. Dinamik Bağlantı Kitaplıkları (DL'ler) ve `/UserCustomCode/CLR/*` kaynaklar klasörün altında kopyalanır ve bu da kullanıcı DL'lerini sistemden ve Azure Akış Analizi DL'lerinden yalıtmanıza yardımcı olur. Bu paket yolu, bunları kullanmak için kullanılan yöntemden bağımsız olarak tüm işlevler için kullanılır.

## <a name="supported-types-and-mapping"></a>Desteklenen türleri ve haritalama

|**UDF tipi (C#)**  |**Azure Akışı Analizi türü**  |
|---------|---------|
|long  |  bigint   |
|double  |  double   |
|string  |  nvarchar(maks)   |
|tarih saat  |  tarih saat   |
|struct   |  IRecord   |
|object  |  IRecord   |
|Dizi\<nesnesi>  |  IArray   |
|sözlük<dize, nesne>  |  IRecord   |

## <a name="codebehind"></a>CodeBehind
Kullanıcı tanımlı işlevleri **Script.asql** CodeBehind'a yazabilirsiniz. Visual Studio araçları CodeBehind dosyasını otomatik olarak bir derleme dosyasına derler. Derlemeler zip dosyası olarak paketlenir ve işinizi Azure'a gönderdiğinizde depolama hesabınıza yüklenir. Stream Analytics Edge işleri öğreticisi [için C# UDF'yi](stream-analytics-edge-csharp-udf.md) izleyerek CodeBehind kullanarak C# UDF yazmayı öğrenebilirsiniz. 

## <a name="local-project"></a>Yerel proje
Kullanıcı tanımlı işlevler, daha sonra bir Azure Akışı Analizi sorgusunda başvurulan bir derlemeye yazılabilir. Bu, bir .NET Standart dilinin ifade dilinin ötesinde yordam mantığı veya özyineleme gibi tam güç gerektiren karmaşık işlevler için önerilen seçenektir. İşlev mantığını birkaç Azure Akışı Analizi sorgusunda paylaşmanız gerektiğinde yerel bir projeden gelen UDF'ler de kullanılabilir. Yerel projenize UDF'ler eklemek, Visual Studio'dan işlevlerinizi ayıklama ve test etme olanağı sağlar.

Yerel bir projeye başvurmak için:

1. Çözümünüzde yeni bir sınıf kitaplığı oluşturun.
2. Kodu sınıfınıza yazın. Sınıfların *ortak,* nesnelerin *statik ortak*olarak tanımlanması gerektiğini unutmayın. 
3. Projenizi yapılandırın. Araçlar, çöp kutusu klasöründeki tüm yapıları bir zip dosyasına paketleyecek ve zip dosyasını depolama hesabına yükler. Dış başvurular için NuGet paketi yerine montaj başvurusu kullanın.
4. Azure Akış Analizi projenizde yeni sınıfa başvurun.
5. Azure Akış Analizi projenizde yeni bir işlev ekleyin.
6. İş yapılandırma dosyasındaki montaj yolunu `JobConfig.json`yapılandırın, . Montaj Yolunu **Yerel Proje Başvurusu veya CodeBehind'a**ayarlayın.
7. Hem işlev projesini hem de Azure Akışı Analizi projesini yeniden oluştur.  

### <a name="example"></a>Örnek

Bu **örnekte, UDFTest** bir C# sınıfı kitaplık projesidir ve **ASAUDFDemo,** **UDFTest'e**başvurulacak olan Azure Akışı Analytics projesidir.

![Visual Studio'da Azure Stream Analytics IoT Edge projesi](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. Azure Akış Analizi sorgusundan C# UDF'nize referans eklemenize olanak sağlayacak C# projenizi oluşturun.
    
   ![Visual Studio'da Azure Akışı Analytics IoT Edge projesi oluşturun](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. ASA projesindeki C# projesine başvuruyu ekleyin. Başvurular düğümüne sağ tıklayın ve Başvuru Ekle'yi seçin.

   ![Visual Studio'daki C# projesine başvuru ekleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. Listeden C# proje adını seçin. 
    
   ![Başvuru listesinden C# proje adınızı seçin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. **Çözüm Gezgini'nde** **Başvurular** altında listelenen **UDFTest'i** görmeniz gerekir.

   ![Çözüm gezgininde kullanıcı tanımlı işlev referansını görüntüleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. **Fonksiyonlar** klasörüne sağ tıklayın ve **Yeni Öğe'yi**seçin.

   ![Azure Akış Analizi Edge çözümünde Fonksiyonlara yeni öğe ekleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Azure Akış Analizi projenize C# fonksiyonu **SquareFunction.json** ekleyin.

   ![Visual Studio'daki Stream Analytics Edge öğelerinden CSharp işlevini seçin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. Yapılandırma iletişim kutusunu açmak için **Solution Explorer'daki** işlevi çift tıklatın.

   ![Visual Studio'da C keskin fonksiyon yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. C# işlevi yapılandırmasında, **ASA Project Reference'dan Yükle'yi** ve açılan listeden ilgili derleme, sınıf ve yöntem adlarını seçin. Akış Analizi sorgusundaki yöntemlere, türlere ve işlevlere başvurmak için sınıflar *ortak* olarak tanımlanmalı ve nesneler *statik ortak*olarak tanımlanmalıdır.

   ![Stream Analytics C keskin fonksiyon yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Mevcut paketler

.NET Standart UDF'leri seçtiğiniz herhangi bir IDE'de yazar ve Azure Akış Analizi sorgunuzdan çağırabilirsiniz. Önce kodunuzu derve tüm DL'leri paketle. Paketin biçimi yolu `/UserCustomCode/CLR/*`vardır. Ardından, `UserCustomCode.zip` Azure depolama hesabınızdaki kapsayıcının köküne yükleyin.

Azure depolama hesabınıza montaj zip paketleri yüklendikten sonra, Azure Akış Analizi sorgularında işlevleri kullanabilirsiniz. Tek yapmanız gereken, Akış Analizi iş yapılandırmasına depolama bilgilerini eklemektir. Visual Studio araçları paketinizi indirmeyeceği için işlevi bu seçenekle yerel olarak test edemezsiniz. Paket yolu doğrudan hizmete ayrıştırılır. 

İş yapılandırma dosyasındaki montaj yolunu yapılandırmak için: `JobConfig.json`

**Kullanıcı Tanımlı Kod Yapılandırması** bölümünü genişletin ve yapılandırmaya aşağıdaki önerilen değerleri ekleyin:

   |**Ayar**|**Önerilen Değer**|
   |-------|---------------|
   |Genel Depolama Ayarları Kaynak|Geçerli hesaptaki veri kaynağını seçin|
   |Genel Depolama Ayarları Aboneliği| Aboneliğinizi < >|
   |Genel Depolama Ayarları Depolama Hesabı| depolama hesabınızı < >|
   |Özel Kod Depolama Ayarları Kaynak|Geçerli hesaptaki veri kaynağını seçin|
   |Özel Kod Depolama Ayarları Depolama Hesabı|depolama hesabınızı < >|
   |Özel Kod Depolama Ayarları Kapsayıcısı|Depolama kapsayıcınızı < >|
   |Özel Kod Derleme Kaynağı|Buluttan mevcut montaj paketleri|
   |Özel Kod Derleme Kaynağı|KullanıcıCustomCode.zip|

## <a name="limitations"></a>Sınırlamalar
UDF önizlemeşu anda aşağıdaki sınırlamalar vardır:

* .NET Standart UDF'leri yalnızca Visual Studio'da yazılabilir ve Azure'da yayınlanabilir. .NET Standart UDF'lerinin salt okunur sürümleri Azure portalındaki **Işlevler** altında görüntülenebilir. .NET Standart işlevlerinin yazlanması Azure portalında desteklenmez.

* Azure portal sorgu düzenleyicisi, portalda .NET Standart UDF kullanırken bir hata gösterir. 

* Özel kod bağlamı Azure Akış Analizi altyapısıyla paylaştığından, özel kod Azure Akış Analizi koduyla çakışan ad alanı/dll_name olan hiçbir şeye başvuru yapamaz. Örneğin, *Newtonsoft Json'a*başvuru yapamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Akışı Analizi işi için C# kullanıcı tanımlı bir işlev yazın (Önizleme)](stream-analytics-edge-csharp-udf.md)
* [Öğretici: Azure Stream Analytics JavaScript kullanıcı tanımlı işlevleri](stream-analytics-javascript-user-defined-functions.md)
* [Azure Akış Analizi işlerini görüntülemek için Visual Studio'yı kullanın](stream-analytics-vs-tools.md)
