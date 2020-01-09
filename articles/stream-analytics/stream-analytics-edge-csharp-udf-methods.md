---
title: Azure Stream Analytics işleri için .NET Standard işlevleri geliştirme (Önizleme)
description: Stream Analytics işleri için c# Kullanıcı tanımlı işlevleri yazmayı öğrenin.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/28/2019
ms.custom: seodec18
ms.openlocfilehash: f07c02df1b8e0032c9e1b4ef9a24c345fee20a40
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426308"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-jobs-preview"></a>Azure Stream Analytics işleri için .NET Standard Kullanıcı tanımlı işlevler geliştirme (Önizleme)

Azure Stream Analytics, olay veri akışları üzerinde dönüşümler ve hesaplamalar gerçekleştirmek için bir SQL benzeri sorgu dili sağlar. Birçok yerleşik işlevleri vardır, ancak bazı karmaşık senaryolar daha fazla esneklik gerektirir. .NET Standard kullanıcı tanımlı işlevlerle (UDF), herhangi bir .NET standard dilde yazılmış kendi işlevlerinizi çağırma (C#, F#, vs.) için Stream Analytics sorgu dili genişletir. UDF karmaşık matematik hesaplamaları gerçekleştirme, özel ML modelleri ML.NET kullanarak içeri aktarmak izin ve özel imputation mantığının eksik verileri için kullanın. Stream Analytics işleri için UDF özelliği şu anda önizlemededir ve üretim iş yükleri içinde kullanılmamalıdır.

Bulut işlerinin .NET Kullanıcı tanımlı işlevi, ' de kullanılabilir:
* Orta Batı ABD
* Kuzey Avrupa
* Doğu ABD
* Batı ABD
* Doğu ABD 2
* Batı Avrupa

Bu özelliği başka bir bölgede kullanmaya ilgileniyorsanız, [erişim isteyebilirsiniz](https://aka.ms/ccodereqregion).

## <a name="overview"></a>Genel Bakış
Visual Studio Araçları Azure Stream Analytics, UDF'ler, yazmanızı kolaylaştırmak için işlerinizi yerel olarak test etme (çevrimdışıyken bile) ve Stream Analytics işinizi Azure'da yayımlayın. Azure'a yayımlandıktan sonra işinizi IOT hub'ı kullanarak IOT cihazlarına dağıtabilirsiniz.

UDF uygulamak için üç yolu vardır:

* CodeBehind bir ASA projesindeki dosyalar
* Yerel bir projeden UDF
* Azure depolama hesabınız var olan bir paketten

## <a name="package-path"></a>Paket yolu

Herhangi bir UDF paket biçimi yoluna sahip `/UserCustomCode/CLR/*`. Dinamik bağlantı kitaplıklarını (DLL'ler) ve kaynakları altında kopyalanır `/UserCustomCode/CLR/*` yardımcı olan, kullanıcı DLL'leri sisteminden ve Azure Stream Analytics DLL'leri yalıtmak klasörü. Bu paket yolu bunları kullanmak istemiyorsunuz kullanılan yöntem ne olursa olsun tüm işlevler için kullanılır.

## <a name="supported-types-and-mapping"></a>Desteklenen türler ve eşleme

|**UDF türü (C#)**  |**Azure Stream Analytics yazın**  |
|---------|---------|
|uzun  |  bigint   |
|double  |  double   |
|string  |  nvarchar(max)   |
|Tarih/saat  |  Tarih/saat   |
|Yapı  |  Irecord   |
|object  |  Irecord   |
|Dizi\<nesnesi >  |  IArray   |
|Sözlük < string, object >  |  Irecord   |

## <a name="codebehind"></a>CodeBehind
Kullanıcı tanımlı işlevler yazabilirsiniz **Script.asql** CodeBehind. Visual Studio Araçları, otomatik olarak dosyası bir derleme dosyasına derlenir. Derlemeleri zip dosyası olarak paketlenir ve Azure iş gönderdiğinizde, depolama hesabına yüklediniz. Bir C# CodeBehind izleyerek kullanarak UDF yazılacak öğrenebilirsiniz [Stream Analytics Edge işleri için C# UDF](stream-analytics-edge-csharp-udf.md) öğretici. 

## <a name="local-project"></a>Yerel proje
Kullanıcı tanımlı işlevleri, daha sonra bir Azure Stream Analytics sorguda başvurulan bir derlemede yazılabilir. .NET Standard dilinin ifade dili, yordam mantığı ya da özyineleme gibi dışında tüm gücünden gerektiren karmaşık işlevler için önerilen seçenek budur. İşlevi mantığı birden çok Azure Stream Analytics sorguları arasında paylaşmak istediğinizde UDF'ler yerel bir projeden de kullanılabilir. UDF yerel projenize ekleme işlevlerinizi Visual Studio'dan yerel olarak test ve hata ayıklama olanağı sağlar.

Yerel bir proje başvurusu için:

1. Çözümünüzde bir yeni sınıf kitaplığı oluşturun.
2. Kod Sınıfınız içinde yazın. Sınıflar olarak tanımlanması gerekir unutmayın *genel* ve nesneleri olarak tanımlanması gerektiğini *statik genel*. 
3. Projenizi derleyin. Araçları tüm yapılar bin klasörü bir zip dosyasına paketleyin ve zip dosyasını depolama hesabına yükleyin. Dış başvurular için bütünleştirilmiş kod başvurusu yerine NuGet paketini kullanın.
4. Azure Stream Analytics projenizde yeni sınıf başvurusu.
5. Yeni bir işlev, Azure Stream Analytics projenize ekleyin.
6. Proje yapılandırma dosyasına bütünleştirilmiş kod yolu yapılandırma `JobConfig.json`. Derleme yolunu ayarlamak **yerel proje başvurusu ya da CodeBehind**.
7. İşlev projesi hem Azure Stream Analytics projeyi yeniden derleyin.  

### <a name="example"></a>Örnek

Bu örnekte **udftest** bir C# sınıf kitaplığı projem ve **asaudfdemo** , **udftest**'e başvuracaktır Azure Stream Analytics projem.

![Visual Studio'da Azure Stream Analytics IOT Edge projesi](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. Azure Stream Analytics sorgusu, C# UDF için başvuru eklemenize olanak tanıyan, C# projesini oluşturun.
    
   ![Visual Studio'da bir Azure Stream Analytics IOT Edge projesi oluşturmak](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. Başvuruyu, ASA projesindeki C# projeye ekleyin. Başvurular düğümüne sağ tıklayın ve Başvuru Ekle öğesini seçin.

   ![Visual Studio'da C# projesine bir başvuru ekleyin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. C# proje adı listeden seçin. 
    
   ![C# projenizin adına başvuru listeden seçin.](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. Görmelisiniz **UDFTest** altında listelenen **başvuruları** içinde **Çözüm Gezgini**.

   ![Görünümü Kullanıcı işlev başvurusu Çözüm Gezgini içinde tanımlanır.](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. Sağ tıklayın **işlevleri** klasörü seçin **yeni öğe**.

   ![Azure Stream Analytics Edge çözümde işlevleri için Yeni Öğe Ekle](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Bir C# işlev eklemek **SquareFunction.json** Azure Stream Analytics projenize.

   ![Visual Studio için Stream Analytics Edge öğelerinde CSharp işlevi seçin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. İşlev çift **Çözüm Gezgini** yapılandırma iletişim kutusunu açın.

   ![Visual Studio'da C sharp işlevi yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. C# işlevi yapılandırması, seçin **ASA proje başvurusu yükün** ve aşağı açılan listeden ilgili derleme, sınıf ve yöntem adları. Stream Analytics sorgusundaki yöntemlere, türlere ve işlevlere başvurmak için sınıfların *ortak* olarak tanımlanması ve nesnelerin *statik ortak*olarak tanımlanması gerekir.

   ![Stream Analytics C sharp işlevi yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Var olan paketler

Tercih ettiğiniz herhangi bir IDE'de .NET standart UDF'ler yazabilir ve bunları Azure Stream Analytics sorgunuz çağırır. Önce kod ve DLL'leri paket derleyin. Paket biçimi yoluna sahip `/UserCustomCode/CLR/*`. Ardından, karşıya yükleme `UserCustomCode.zip` Azure depolama hesabınızdaki kapsayıcı köküne.

Azure depolama hesabınıza derleme ZIP paketlerine karşıya yüklendikten sonra Azure Stream Analytics sorguları işlevleri kullanabilirsiniz. Tüm yapmanız gereken Stream Analytics iş yapılandırmasındaki depolama bilgilerini içerir. Visual Studio Araçları, paketi indirmez çünkü işlevi yerel olarak bu seçenek ile test edilemez. Paket yolu doğrudan hizmete ayrıştırılır. 

Proje yapılandırma dosyasına bütünleştirilmiş kod yolu yapılandırmak için `JobConfig.json`:

**Kullanıcı Tanımlı Kod Yapılandırması** bölümünü genişletin ve yapılandırmaya aşağıdaki önerilen değerleri ekleyin:

   |**Ayar**|**Önerilen Değer**|
   |-------|---------------|
   |Genel depolama ayarları kaynağı|Geçerli hesaptaki veri kaynağını seçin|
   |Genel depolama ayarları aboneliği| aboneliğinizi < >|
   |Genel depolama ayarları depolama hesabı| depolama hesabınızı < >|
   |Özel kod depolama ayarları kaynağı|Geçerli hesaptaki veri kaynağını seçin|
   |Özel kod depolama ayarları depolama hesabı|depolama hesabınızı < >|
   |Özel kod depolama ayarları kapsayıcısı|depolama kapsayıcınızı < >|
   |Özel kod derleme kaynağı|Buluttan mevcut derleme paketleri|
   |Özel kod derleme kaynağı|UserCustomCode. zip|

## <a name="limitations"></a>Sınırlamalar
UDF önizlemesi şu anda aşağıdaki sınırlamalara sahiptir:

* .NET standard UDF'ler yalnızca Visual Studio'da yazılan ve Azure'da yayımladınız. .NET Standard UDF'ler salt okunur sürümlerini altında görüntülenebilir **işlevleri** Azure portalında. .NET Standard işlevlerini yazma Azure Portalı'nda desteklenmiyor.

* Azure portal sorgu Düzenleyicisi, .NET standart UDF portalında kullanırken bir hata gösterir. 

* Özel kod Azure Stream Analytics altyapısıyla bağlam paylaştığından, özel kod Azure Stream Analytics kod ile çakışan bir ad alanı/dll_name sahip herhangi bir şey başvuramaz. Örneğin, başvuramaz *Newtonsoft Json*.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Stream Analytics işi C# için Kullanıcı tanımlı bir işlev yazma (Önizleme)](stream-analytics-edge-csharp-udf.md)
* [Öğretici: Azure Stream Analytics JavaScript kullanıcı tanımlı işlevler](stream-analytics-javascript-user-defined-functions.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio](stream-analytics-vs-tools.md)
