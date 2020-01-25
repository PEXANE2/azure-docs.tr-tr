---
title: Senaryoları belirleyip analiz işlemini planlayın-Team Data Science Işlemi | Azure Machine Learning
description: Senaryolar ve Gelişmiş analiz verileri işlemeyi planlama birtakım önemli sorular dikkate alarak belirleyin.
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
ms.openlocfilehash: b0b811a2b7ed432b7fc5015886b28337ca33424e
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76710318"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Senaryoları tanımlama ve gelişmiş analiz verileri işlemeyi planlama

Bir veri kümesinde gelişmiş analiz işleme gerçekleştirebilen bir ortam oluşturmak için hangi kaynaklar gereklidir? Bu makalede, senaryolarınızla ilgili görevleri ve kaynakları belirlemenize yardımcı olabilecek bir dizi soru önerisinde bulunulur.

Tahmine dayalı analizler için üst düzey adımların sırası hakkında bilgi edinmek için bkz. [Team Data Science süreci nedir (TDSP)](overview.md). Her adım, belirli senaryolarınızla ilgili görevler için belirli kaynaklar gerektirir.

Senaryonuzu tanımlamak için aşağıdaki alanlardaki önemli soruları yanıtlayın:

* veri lojistik
* veri özellikleri
* veri kümesi kalitesi
* tercih edilen araçlar ve diller

## <a name="logistic-questions-data-locations-and-movement"></a>Lojistik sorular: veri konumları ve taşıma

Lojistik soruları aşağıdaki öğeleri kapsar:

* veri kaynağı konumu
* Azure 'da hedef hedef
* zamanlama, miktar ve dahil edilen kaynaklar dahil olmak üzere verileri taşıma gereksinimleri

Analiz işlemi sırasında verileri birkaç kez taşımanız gerekebilir. Azure'da ve Machine Learning Studio'ya yerel veri depolama bazı forma taşıma yaygın bir senaryodur.

### <a name="what-is-your-data-source"></a>Veri kaynağınız nedir?

Verileriniz yerel mi yoksa bulutta mı? Olası konumlar şunlardır:

* genel kullanıma açık bir HTTP adresi
* Yerel veya ağ dosya konumu
* bir SQL Server veritabanı
* Azure depolama kapsayıcısı

### <a name="what-is-the-azure-destination"></a>Azure hedefi nedir?

Verilerinizin işlenmek veya modellemesi için nerede olması gerekir? 

* Azure Blob Depolama
* SQL Azure veritabanı
* Azure VM’lerde SQL Server
* HDInsight (Hadoop azure'da) ya da Hive tabloları
* Azure Machine Learning
* Takılamaz Azure sanal sabit diskleri

### <a name="how-are-you-going-to-move-the-data"></a>Verileri nasıl taşıyacağınız?

Çeşitli farklı depolama ve işleme ortamlarına veri almak veya bunları yüklemek için yordamlar ve kaynaklar için, bkz.:

* [Analiz için depolama ortamlarına veri yükleme](ingest-data.md)
* [Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio (klasik) içine aktarın](../studio/import-data.md)

### <a name="does-the-data-need-to-be-moved-on-a-regular-schedule-or-modified-during-migration"></a>Verilerin düzenli bir zamanlamaya göre taşınması veya geçiş sırasında değiştirilmesi gerekiyor mu?

Verilerin sürekli geçirilmesi gerektiğinde Azure Data Factory (ADF) kullanmayı düşünün. ADF şu şekilde yararlı olabilir:

* hem şirket içi hem de bulut kaynaklarını içeren karma bir senaryo
* geçiş sırasında verilerin işlenen, değiştirildiği veya iş mantığı tarafından değiştirildiği bir senaryo

Daha fazla bilgi için bkz. Şirket [ıçı SQL Server 'dan Azure Data Factory SQL Azure için verileri taşıma](move-sql-azure-adf.md).

### <a name="how-much-of-the-data-is-to-be-moved-to-azure"></a>Verilerin ne kadarını Azure 'a taşınacak?

Büyük veri kümeleri belirli ortamların depolama kapasitesini aşabilir. Bir örnek için, sonraki bölümde Machine Learning Studio (klasik) için boyut sınırları tartışmalarına bakın. Bu gibi durumlarda, analiz sırasında verilerin bir örneğini kullanabilirsiniz. Aşağı-örnek bir veri kümesi çeşitli Azure ortamlarında konusunda ayrıntılar için bkz [örnek veriler Team Data Science Process içinde](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Veri özellikleri soruları: türü, biçimi ve boyutu

Bu sorular, depolama ve işleme ortamlarınızı planlamaya yönelik bir anahtarlardır. Veri türü için uygun senaryoyu seçmenize ve tüm kısıtlamaları anlamanıza yardımcı olur.

### <a name="what-are-the-data-types"></a>Veri türleri nelerdir?

* Sayısal
* Kategorik
* Dizeler
* Binary

### <a name="how-is-your-data-formatted"></a>Verileriniz nasıl biçimlendirilir?

* Virgülle ayrılmış (CSV) veya (TSV) düz dosyaları sekmeyle ayrılmış
* Sıkıştırılmış veya sıkıştırılmamış
* Azure BLOB'ları
* Hadoop Hive tabloları
* SQL Server tabloları

### <a name="how-large-is-your-data"></a>Verileriniz ne kadar büyük?

* Küçük: en az 2 GB
* Orta: 2 GB ve boyutu 10 GB'tan büyük
* Büyük: 10 GB değerinden fazla

Örnek olarak Azure Machine Learning Studio (klasik) ortamı alın:

* Veri biçimleri ve Azure Machine Learning Studio tarafından desteklenen türleri listesi için bkz. [veri biçimlerini ve desteklenen veri türleri](../studio/import-data.md#supported-data-formats-and-data-types) bölümü.
* Analytics işleminde kullanılan diğer Azure Hizmetleri sınırlamaları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Veri Kalitesi soruları: inceleme ve ön işleme

### <a name="what-do-you-know-about-your-data"></a>Verileriniz hakkında ne bilmeniz gerekir?

Verileriniz hakkındaki temel özellikleri anlayın:

* Hangi desenleri veya eğilimleri sergiler
* Ne kadar aykırı Özellikler
* Kaç değer eksik

Bu adım size yardımcı olmak için önemlidir:

* Ne kadar ön işleme gerekli olduğunu belirleme
* En uygun özellikleri veya analiz türünü öneren hipotezleri formül oluştur
* Ek veri toplama için planları formül oluştur

Veri incelemesi için faydalı teknikler, açıklayıcı istatistik hesaplama ve görselleştirme çizimleri içerir. Çeşitli Azure ortamlarında bir veri kümesini araştırmak nasıl ayrıntıları için bkz: [Team Data Science Process verileri araştırın](explore-data.md).

### <a name="does-the-data-require-preprocessing-or-cleaning"></a>Veriler ön işleme veya temizleme gerektiriyor mu?

Makine öğrenimi için veri kümesini etkin bir şekilde kullanabilmeniz için önce verilerinizi önceden işleyin ve temizlemeniz gerekebilir. Ham veriler genellikle gürültülü ve güvenilmez olur. Eksik değerler olabilir. Modelleme için bu verileri kullanarak, yanıltıcı sonuçlara neden olabilir. Bir açıklama için bkz. [için Gelişmiş veri hazırlama görevleri makine öğrenimi](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Araçları ve dilleri ile ilgili sorular

Diller, geliştirme ortamları ve araçlar için birçok seçenek vardır. Gereksinimlerinize ve tercihlerinize dikkat edin.

### <a name="what-languages-do-you-prefer-to-use-for-analysis"></a>Analiz için hangi dilleri kullanacağınızı tercih ediyorsunuz?

* R
* Python
* SQL

### <a name="what-tools-should-you-use-for-data-analysis"></a>Veri analizi için hangi araçları kullanmanız gerekir?

* [Microsoft Azure PowerShell](/powershell/azure/overview) -Azure kaynaklarınızı bir betik dilinde yönetmek için kullanılan bir betik dili
* [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
* [Revolution Analytics](https://www.microsoft.com/sql-server/machinelearningserver)
* [RStudio](https://www.rstudio.com)
* [Visual Studio için Python Araçları](https://aka.ms/ptvsdocs)
* [Anaconda](https://www.continuum.io/why-anaconda)
* [Jupyter not defterleri](https://jupyter.org/)
* [Microsoft Power BI](https://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Gelişmiş analiz senaryonuzu tanımlama

Önceki bölümde verilen soruları yanıtladıktan sonra, hangi senaryonun büyük/küçük harflere en uygun olduğunu tespit etmeye hazırsınızdır. Örnek senaryolar özetlenen [Azure Machine learning'de Gelişmiş analiz senaryoları](plan-sample-scenarios.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Team Data Science Işlemi (TDSP) nedir?](overview.md)
