---
title: Azure Stream Analytics Edge işleri için .NET Standard işlevleri geliştirme (Önizleme)
description: CodeBehind, mevcut bir paketi veya yerel bir projeyi kullanarak Azure Stream Analytics işleri için .NET Kullanıcı tanımlı işlevleri geliştirmeyi öğrenin.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 287ee2d84ea3d5c6f1568edb1636191f509681e0
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901626"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-edge-jobs-preview"></a>Azure Stream Analytics Edge işleri için .NET Standard Kullanıcı tanımlı işlevler geliştirme (Önizleme)

Azure Stream Analytics, olay verilerinin akışları üzerinde dönüşümler ve hesaplamalar gerçekleştirmek için SQL benzeri bir sorgu dili sunar. Birçok yerleşik işlev vardır ancak bazı karmaşık senaryolar ek esneklik gerektirir. Kullanıcı tanımlı işlevler (UDF) .NET Standard ile, Stream Analytics sorgu dilini genişletmek için herhangi bir .net standart dilinde (C#, F#, vb.) yazılmış kendi işlevlerinizi çağırabilirsiniz. UDF 'ler, karmaşık matematik hesaplamaları gerçekleştirmenizi, ML.NET kullanarak özel ML modellerini içeri aktarmanızı ve eksik veriler için özel imputation mantığı kullanmanıza imkan tanır. Stream Analytics Edge işleri için UDF özelliği şu anda önizleme sürümündedir ve üretim iş yüklerinde kullanılmamalıdır.

## <a name="overview"></a>Genel Bakış
Azure Stream Analytics için Visual Studio Araçları, UDF 'Leri yazmanızı, işlerinizi yerel olarak test yapmayı ve Stream Analytics işinizi Azure 'da yayımlamayı kolaylaştırır. Azure 'da yayımlandıktan sonra, IoT Hub kullanarak işinizi IoT cihazlarına dağıtabilirsiniz.

UDF 'Leri uygulamak için üç yol vardır:

* Bir ASA projesindeki CodeBehind dosyaları
* Yerel projeden UDF
* Azure Storage hesabından mevcut bir paket

## <a name="package-path"></a>Paket yolu

Herhangi bir UDF paketinin biçimi `/UserCustomCode/CLR/*`yoludur. Dinamik bağlantı kitaplıkları (dll 'Ler) ve kaynaklar `/UserCustomCode/CLR/*` klasörü altına kopyalanır ve bu, Kullanıcı dll 'Lerinin sistem ve Azure Stream Analytics dll 'lerden yalıtılmasına yardımcı olur. Bu paket yolu, bunları kullanmak için kullanılan yöntemden bağımsız olarak tüm işlevler için kullanılır.

## <a name="supported-types-and-mapping"></a>Desteklenen türler ve eşleme

|**UDF türü (C#)**  |**Azure Stream Analytics türü**  |
|---------|---------|
|Kalacağını  |  bigint   |
|double  |  double   |
|string  |  nvarchar (max)   |
|Hem  |  Hem   |
|sýný  |  IRecord   |
|object  |  IRecord   |
|Dizi\<nesnesi >  |  IArray   |
|sözlük < dize, nesne >  |  IRecord   |

## <a name="codebehind"></a>CodeBehind
Kullanıcı tanımlı işlevleri **Script. ASQL** codebehind içinde yazabilirsiniz. Visual Studio Araçları otomatik olarak CodeBehind dosyasını derleme dosyasına derler. Derlemeler bir zip dosyası olarak paketlenir ve işinizi Azure 'a gönderdiğinizde depolama hesabınıza yüklenir. [ C# Stream Analytics Edge işleri](stream-analytics-edge-csharp-udf.md) öğreticisini izleyerek codebehind C# kullanarak bir UDF yazma hakkında bilgi edinebilirsiniz. 

## <a name="local-project"></a>Yerel proje
Kullanıcı tanımlı işlevler, daha sonra bir Azure Stream Analytics sorgusunda başvurulan bir derlemede yazılabilir. Bu, .NET Standard dilinin, yordamsal mantık veya özyineleme gibi ifade dilinin ötesinde tam gücünü gerektiren karmaşık işlevler için önerilen seçenektir. Yerel bir projeden gelen UDF 'ler, birkaç Azure Stream Analytics sorgu arasında işlev mantığını paylaşmanız gerektiğinde de kullanılabilir. Yerel projenize UDF 'ler eklemek, işlevlerinizi Visual Studio 'dan yerel olarak hata ayıklama ve test etme olanağı sunar.

Yerel bir projeye başvurmak için:

1. Çözümünüzde yeni bir sınıf kitaplığı oluşturun.
2. Kodunuzu sınıfınıza yazın. Sınıfların *ortak* olarak tanımlanması gerektiğini ve nesnelerin *statik ortak*olarak tanımlanması gerektiğini unutmayın. 
3. Projenizi derleyin. Araçlar bin klasöründeki tüm yapıtları bir ZIP dosyasına paketler ve ZIP dosyasını depolama hesabına yükler. Dış başvurular için NuGet paketi yerine derleme başvurusunu kullanın.
4. Azure Stream Analytics projenizdeki yeni sınıfa başvurun.
5. Azure Stream Analytics projenize yeni bir işlev ekleyin.
6. Derleme yolunu iş yapılandırma dosyasında yapılandırın `JobConfig.json`. Derleme yolunu **Yerel proje başvurusu veya codebehind**olarak ayarlayın.
7. Hem işlev projesini hem de Azure Stream Analytics projeyi yeniden derleyin.  

### <a name="example"></a>Örnek

Bu örnekte **udftest** bir C# sınıf kitaplığı projem ve **Asaedgeudfdemo** , **Udftest**'e başvuracak Azure Stream Analytics Edge projem.

![Visual Studio 'da IoT Edge Azure Stream Analytics projesi](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. C# Projenizi oluşturun, bu, Azure Stream Analytics sorgusundan UDF 'nize C# bir başvuru eklemenize olanak tanır.
    
   ![Visual Studio 'da Azure Stream Analytics IoT Edge projesi oluşturma](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. Başvuruyu ASA Edge projesinde C# projeye ekleyin. Başvurular düğümüne sağ tıklayın ve başvuru Ekle ' yi seçin.

   ![Visual Studio 'da bir C# projeye başvuru ekleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. Listeden C# proje adını seçin. 
    
   ![Başvuru listesinden C# projenizin adını seçin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. **Çözüm Gezgini** **Başvurular** altında listelenen **udftest** ' i görmeniz gerekir.

   ![Çözüm Gezgini 'nde Kullanıcı tanımlı işlev başvurusunu görüntüleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. **İşlevler** klasörüne sağ tıklayın ve **Yeni öğe**' yi seçin.

   ![Azure Stream Analytics Edge çözümünde IŞLEVLERE yeni öğe ekleme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Azure Stream Analytics projenize C# bir **squarefunction. JSON** işlevi ekleyin.

   ![Visual Studio 'da Stream Analytics Edge öğelerinden CSharp işlevi seçme](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. Yapılandırma iletişim kutusunu açmak için **Çözüm Gezgini** işlevine çift tıklayın.

   ![Visual Studio 'da C Sharp işlev yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. C# İşlev YAPıLANDıRMASıNDA, **asa proje başvurusundan yükle** ' yi ve açılan listeden ilgili derleme, sınıf ve yöntem adlarını seçin. Stream Analytics Edge sorgusundaki yöntemlere, türlere ve işlevlere başvurmak için sınıfların *ortak* olarak tanımlanması ve nesnelerin *statik ortak*olarak tanımlanması gerekir.

   ![Stream Analytics C Sharp işlev yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Mevcut paketler

.NET Standard UDF 'Leri dilediğiniz IDE 'de yazabilir ve Azure Stream Analytics sorgınızdan çağırabilirsiniz. İlk olarak kodunuzu derleyin ve tüm dll 'Leri paketleyin. Paketin biçimi `/UserCustomCode/CLR/*`yoludur. Sonra, Azure Depolama hesabınızdaki kapsayıcının köküne `UserCustomCode.zip` yükleyin.

Azure depolama hesabınıza derleme ZIP paketleri yüklendikten sonra, Azure Stream Analytics sorgulardaki işlevleri kullanabilirsiniz. Tüm yapmanız gereken Stream Analytics Edge iş yapılandırmasındaki depolama bilgilerini içerir. Visual Studio Araçları paketinizi indirmediği için işlevi bu seçenekle yerel olarak test edebilirsiniz. Paket yolu doğrudan hizmete ayrıştırılır. 

İş yapılandırma dosyasında derleme yolunu yapılandırmak için `JobConfig.json`:

**Kullanıcı Tanımlı Kod Yapılandırması** bölümünü genişletin ve yapılandırmaya aşağıdaki önerilen değerleri ekleyin:

 |**Ayar**  |**Önerilen değer**  |
 |---------|---------|
 |Derleme Kaynağı  | Buluttan mevcut derleme paketleri    |
 |Kaynak  |  Geçerli hesaptaki verileri seçin   |
 |Abonelik  |  Aboneliğinizi seçin.   |
 |Depolama Hesabı  |  Depolama hesabınızı seçin.   |
 |Kapsayıcı  |  Depolama hesabınızda oluşturduğunuz kapsayıcıyı seçin.   |

![Visual Studio’da Azure Stream Analytics Edge işi yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-job-config.png)

## <a name="limitations"></a>Sınırlamalar
UDF önizlemesi Şu anda aşağıdaki sınırlamalara sahiptir:

* .NET Standard diller yalnızca IoT Edge üzerinde Azure Stream Analytics için kullanılabilir. Bulut işleri için JavaScript Kullanıcı tanımlı işlevler yazabilirsiniz. Daha fazla bilgi edinmek için [Azure Stream Analytics JAVASCRIPT UDF](stream-analytics-javascript-user-defined-functions.md) öğreticisini ziyaret edin.

* .NET Standard UDF 'ler yalnızca Visual Studio 'da yazılabilir ve Azure 'da yayımlanabilir. .NET Standard UDF 'Leri salt okuma sürümleri Azure portal **işlevler** altında görüntülenebilir. .NET Standard işlevlerinin yazılması Azure portal desteklenmez.

* Azure portal sorgu Düzenleyicisi portalda .NET Standard UDF kullanırken bir hata gösterir. 

* Özel kod Azure Stream Analytics altyapısı ile bağlam paylaştığından, özel kod Azure Stream Analytics kodla çakışan bir ad alanı/DLL_name olan herhangi bir şeye başvuramaz. Örneğin, *Newtonsoft JSON*öğesine başvurulamıyor.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Stream Analytics Edge C# işi için Kullanıcı tanımlı bir işlev yazma (Önizleme)](stream-analytics-edge-csharp-udf.md)
* [Öğretici: JavaScript Kullanıcı tanımlı işlevleri Azure Stream Analytics](stream-analytics-javascript-user-defined-functions.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio 'Yu kullanma](stream-analytics-vs-tools.md)
