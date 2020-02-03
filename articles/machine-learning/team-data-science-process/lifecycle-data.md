---
title: Veri edinme ve anlama Team Data Science Process
description: Hedefleri, görevleri ve teslim edilebilirleri veri alımı ve veri bilimi projelerinizi anlama aşaması
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 3c299e9ec42d63812804b5ff7e50324a2de94200
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720512"
---
# <a name="data-acquisition-and-understanding-stage-of-the-team-data-science-process"></a>Veri edinme ve anlama aşamasına Team Data Science Process

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirleri veri edinme ve anlama aşama Team Data Science işlem (TDSP) ile ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İş anlama**
   2. **Veri alma ve anlama**
   3. **Oluşturmanın**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Hedef değişkenlere ilişkilerini anladım temiz, yüksek kaliteli veri kümesi üretir. Modeli hazır olacak şekilde veri kümesi uygun analytics ortamında bulun.
* Bir çözüm mimarisini yenilenir ve verileri düzenli olarak puanlar veri işlem hattı geliştirin.

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada yönelik üç ana görev vardır:

   * **Verileri** hedef analitik ortama alma.
   * Veri kalitesinin soruyu yanıtlamak için yeterli olup olmadığını öğrenmek için **verileri araştırın** . 
   * Yeni veya düzenli olarak yenilenen verileri öğrenmek için **bir veri işlem hattı ayarlayın** .

### <a name="ingest-the-data"></a>Veri alma
Verileri eğitim ve tahmin gibi analiz işlemlerini çalıştırdığınız hedef konumlara kaynak konumları taşıma işlemini ayarlayın. Verilerin çeşitli Azure veri hizmetleriyle nasıl taşınacağı hakkında teknik ayrıntılar ve seçenekler için bkz. [analiz için depolama ortamlarına veri yükleme](ingest-data.md). 

### <a name="explore-the-data"></a>Verileri keşfetme
Modellerinizi eğitmek önce verileri ses bir anlayış geliştirmek gerekir. Gerçek veri kümeleri genellikle gürültülü, değerleri eksik veya diğer tutarsızlıklar barındırmasını. Veri özetleme ve görselleştirme, veri kalitesini denetlemek ve modelleme için hazır hale gelmeden önce veriyi işlemek gereken bilgileri sağlamak için kullanabilirsiniz. Bu işlem genellikle yinelemelidir.

TDSP, verileri görselleştirmeye ve veri özeti raporlarının hazırlanmasına yardımcı olmak için [ıdear](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils)adlı otomatik bir yardımcı program sağlar. İlk veri kodlama ile etkileşimli olarak ilk veri anlama geliştirmenize yardımcı olmak için keşfetmek için IDEAR ile başlamanızı öneririz. Ardından veri keşfi ve görselleştirme için özel kod yazabilirsiniz. Verileri temizleme hakkında yönergeler için bkz. [Gelişmiş makine öğrenimi için veri hazırlama görevleri](prepare-data.md).  

Temizlenen veri kalitesi yaptıktan sonra sonraki verileri belirlidir desenleri daha iyi anlamak için adımdır. Bu veri analizi, hedef için uygun bir tahmine dayalı model seçmenize ve geliştirmenize yardımcı olur. Bulgu verileri ne kadar iyi bağlı hedef için bakın. Ardından sonraki modelleme adımlarla ileri taşımak için yeterli veri olup olmadığını belirler. Yeniden, bu işlem genellikle yinelemelidir. Başlangıçta önceki aşamada tanımlanan veri kümesi genişletmek için daha doğru ya da daha fazla ilgili verileri içeren yeni veri kaynaklarını bulmak gerekebilir. 

### <a name="set-up-a-data-pipeline"></a>Bir veri işlem hattı ayarlayın
İlk alımı ve veri temizleme ek olarak, genellikle yeni verilerinizi puanlamada veya verileri düzenli aralıklarla sürekli öğrenme sürecinin parçası olarak yenilemek için bir işlem ayarlamanız gerekir. Puanlama, bir veri işlem hattı veya iş akışı ile tamamlanabilir. [Şirket içi SQL Server örneğinden verileri Azure SQL veritabanı 'na Azure Data Factory makale Ile taşıma](move-sql-azure-adf.md) , [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)ile işlem hattının nasıl ayarlanacağı hakkında bir örnek sağlar. 

Bu aşamada, veri işlem hattı bir çözüm mimarisini geliştirin. İşlem hattı ile veri bilimi proje sonraki aşaması paralel geliştirme. İş gereksinimlerinize ve bu çözümün tümleştirildiği mevcut sistemlerinizin kısıtlamalarına bağlı olarak, işlem hattı aşağıdaki seçeneklerden biri olabilir: 

   * Toplu işleme dayalı
   * Akış ve gerçek zamanlı 
   * Karma 

## <a name="artifacts"></a>Yapıtlar
Aşağıda, bu aşamada teslim edilebilirleri verilmiştir:

   * [Veri kalitesi raporu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/DataSummaryReport.md): Bu rapor, veri özetlerini, her öznitelik ve hedef, değişken derecelendirmesi ve daha fazlasını içeren ilişkileri içerir. TDSP 'nin bir parçası olarak sunulan [ıdear](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) Aracı, bu raporu bir CSV dosyası veya ilişkisel tablo gibi tablosal veri kümesi üzerinde hızlı bir şekilde oluşturabilir. 
   * **Çözüm mimarisi**: çözüm mimarisi, bir model oluşturduktan sonra yeni verilerde Puanlama veya tahmin çalıştırmak için kullandığınız veri işlem hattının bir diyagramı veya açıklaması olabilir. Ayrıca, yeni verileri temel alan, modeli yeniden eğitme için işlem hattı içerir. TDSP dizin yapısı şablonunu kullandığınızda belgeyi [Proje](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) dizininde depolayın.
   * **Kontrol noktası kararı**: tam özellik Mühendisliği ve model oluşturmaya başlamadan önce, beklenen değerin, söz konusu değeri uygulamaya devam etmek için yeterli olup olmadığını tespit etmek üzere projeyi yeniden değerlendirmeye başlayabilirsiniz. Örneğin, devam etmek için daha fazla veri toplayabilir veya verileri soruyu yanıtlamak için mevcut olmadığından, projeyi iptal gerek hazır hale getirebilir.

## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alma ve anlama](lifecycle-data.md)
   3. [Oluşturmanın](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren tam izlenecek yollar sunuyoruz. [Örnek yönergeler](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları olan senaryoların bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 

Azure Machine Learning Studio kullanan TDSPs 'de adımların nasıl yürütüleceği hakkında örnekler için, bkz. [Azure Machine Learning Ile TDSP kullanma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle-data).
