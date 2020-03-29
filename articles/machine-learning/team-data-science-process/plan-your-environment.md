---
title: Senaryoları belirleyin ve analiz sürecini planlayın - Ekip Veri Bilimi Süreci | Azure Makine Öğrenimi
description: Senaryoları belirleyin ve bir dizi önemli soruyu göz önünde bulundurarak gelişmiş analitik veri işleme planını planlayın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76710318"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Senaryoları tanımlama ve gelişmiş analiz verileri işlemeyi planlama

Bir veri setinde gelişmiş analitik işleme gerçekleştirebilen bir ortam oluşturmanız için hangi kaynaklar gereklidir? Bu makalede, senaryonuzla ilgili görevleri ve kaynakları belirlemenize yardımcı olabilecek bir dizi soru önermektedir.

Tahmine dayalı analitik için üst düzey adımların sırası hakkında bilgi edinmek [için, Ekip Veri Bilimi Süreci (TDSP) nedir bölümüne](overview.md)bakın. Her adım, belirli senaryonuzla ilgili görevler için belirli kaynaklar gerektirir.

Senaryonuzu tanımlamak için aşağıdaki alanlardaki temel soruları yanıtlayın:

* veri lojistiği
* veri özellikleri
* dataset kalitesi
* tercih edilen araçlar ve diller

## <a name="logistic-questions-data-locations-and-movement"></a>Lojistik sorular: veri konumları ve hareket

Lojistik sorular aşağıdaki öğeleri kapsamaktadır:

* veri kaynağı konumu
* Azure'da hedef hedef
* ilgili zamanlama, tutar ve kaynaklar da dahil olmak üzere verilerin taşınması için gereklilikler

Analiz işlemi sırasında verileri birkaç kez taşımanız gerekebilir. Yaygın bir senaryo, yerel verileri Azure'da bir tür depolama alanına, ardından da Machine Learning Studio'ya taşımaktır.

### <a name="what-is-your-data-source"></a>Veri kaynağınız nedir?

Verileriniz yerel mi yoksa bulutta mı? Olası konumlar şunlardır:

* kamuya açık bir HTTP adresi
* yerel veya ağ dosyası konumu
* bir SQL Server veritabanı
* bir Azure Depolama kapsayıcısı

### <a name="what-is-the-azure-destination"></a>Azure hedefi nedir?

İşleme veya modelleme için verilerinizin nerede olması gerekiyor? 

* Azure Blob Depolama
* SQL Azure veritabanları
* Azure VM’lerde SQL Server
* HDInsight (Azure'da Hadoop) veya Hive tabloları
* Azure Machine Learning
* Monte edilebilir Azure sanal sabit diskler

### <a name="how-are-you-going-to-move-the-data"></a>Verileri nasıl taşıyacaksın?

Çeşitli depolama ve işleme ortamlarına veri yutmak veya yüklemek için prosedürler ve kaynaklar için bkz:

* [Analiz için depolama ortamlarına veri yükleme](ingest-data.md)
* [Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio'ya (klasik) aktarın](../studio/import-data.md)

### <a name="does-the-data-need-to-be-moved-on-a-regular-schedule-or-modified-during-migration"></a>Verilerin düzenli bir zamanlamaya taşınması mı gerekiyor yoksa geçiş sırasında mı değiştirilsin?

Verilerin sürekli olarak aktarılması gerektiğinde Azure Veri Fabrikası'nı (ADF) kullanmayı düşünün. ADF şu lar için yararlı olabilir:

* hem şirket içi hem de bulut kaynaklarını içeren karma bir senaryo
* verilerin geçirildiği sırada iş mantığıyla işletildiği, değiştirildiği veya değiştirildiği bir senaryo

Daha fazla bilgi için [bkz.](move-sql-azure-adf.md)

### <a name="how-much-of-the-data-is-to-be-moved-to-azure"></a>Verilerin ne kadarı Azure'a taşınacak?

Büyük veri kümeleri belirli ortamların depolama kapasitesini aşabilir. Örneğin, bir sonraki bölümde Machine Learning Studio (klasik) için boyut sınırları tartışmasına bakın. Bu gibi durumlarda, çözümleme sırasında verilerin bir örneğini kullanabilirsiniz. Çeşitli Azure ortamlarında bir veri kümesini nasıl alttan örnekleyene ilişkin ayrıntılar [için, Ekip Veri Bilimi Sürecindeörnek verilere](sample-data.md)bakın.

## <a name="data-characteristics-questions-type-format-and-size"></a>Veri özellikleri soruları: tür, biçim ve boyut

Bu sorular depolama ve işleme ortamlarınızı planlamanın anahtarıdır. Bunlar, veri türünüz için uygun senaryoyu seçmenize ve kısıtlamaları anlamanıza yardımcı olur.

### <a name="what-are-the-data-types"></a>Veri türleri nelerdir?

* Sayısal
* Kategorik
* Dizeler
* İkili

### <a name="how-is-your-data-formatted"></a>Verileriniz nasıl biçimlendirilir?

* Virgülle ayrılmış (CSV) veya sekme ayrılmış (TSV) düz dosyalar
* Sıkıştırılmış veya sıkıştırılmamış
* Masmavi lekeler
* Hadoop Kovan tabloları
* SQL Server tabloları

### <a name="how-large-is-your-data"></a>Verileriniz ne kadar büyük?

* Küçük: 2 GB'dan az
* Orta: 2 GB'dan büyük ve 10 GB'dan az
* Büyük: 10 GB'dan büyük

Azure Machine Learning Studio (klasik) ortamını ele alalım:

* Azure Machine Learning Studio tarafından desteklenen veri biçimleri nin ve türlerinin listesi için [bkz.](../studio/import-data.md#supported-data-formats-and-data-types)
* Analiz sürecinde kullanılan diğer Azure hizmetlerinin sınırlamaları hakkında bilgi için [Azure Abonelik ve Hizmet Sınırları, Kotalar ve Kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md)bölümüne bakın.

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Veri kalitesi soruları: keşif ve ön işleme

### <a name="what-do-you-know-about-your-data"></a>Verileriniz hakkında ne biliyorsunuz?

Verilerinizle ilgili temel özellikleri anlayın:

* Hangi desenleri veya eğilimleri sergiler
* Ne aykırı var
* Kaç değer eksik

Bu adım size yardımcı olmak için önemlidir:

* Ön işlemenin ne kadar gerekli olduğunu belirleme
* En uygun özellikleri veya analiz türünü öneren hipotezleri formüle edin
* Ek veri toplama planlarını formüle edin

Veri denetimi için yararlı teknikler açıklayıcı istatistik hesaplama ve görselleştirme çizimleri içerir. Çeşitli Azure ortamlarında bir veri kümesini nasıl keşfedebilirsiniz hakkında ayrıntılı bilgi için, [Ekip Veri Bilimi Sürecindeki Verileri Keşfedin bölümüne](explore-data.md)bakın.

### <a name="does-the-data-require-preprocessing-or-cleaning"></a>Veriler ön işleme veya temizleme gerektirir mi?

Makine öğrenimi için veri kümesini etkili bir şekilde kullanabilmeniz için verilerinizi önceden işlemeniz ve temizlemeniz gerekebilir. Ham veriler genellikle gürültülü ve güvenilmezdir. Eksik değerler olabilir. Bu tür verileri modelleme kullanabilirsiniz yanıltıcı sonuçlar doğurabilir. Açıklama için, [gelişmiş makine öğrenimi için veri hazırlamak için Görevler'e](prepare-data.md)bakın.

## <a name="tools-and-languages-questions"></a>Araçlar ve diller soruları

Diller, geliştirme ortamları ve araçlar için birçok seçenek vardır. İhtiyaçlarınızın ve tercihlerinizin farkında olun.

### <a name="what-languages-do-you-prefer-to-use-for-analysis"></a>Analiz için hangi dilleri kullanmayı tercih edersiniz?

* R
* Python
* SQL

### <a name="what-tools-should-you-use-for-data-analysis"></a>Veri analizi için hangi araçları kullanmalısınız?

* [Microsoft Azure Powershell](/powershell/azure/overview) - Azure kaynaklarınızı komut dosyası dilinde yönetmek için kullanılan bir komut dosyası dili
* [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
* [Devrim Analizi](https://www.microsoft.com/sql-server/machinelearningserver)
* [RStudio](https://www.rstudio.com)
* [Visual Studio için Python Araçları](https://aka.ms/ptvsdocs)
* [Anaconda](https://www.continuum.io/why-anaconda)
* [Jupyter not defterleri](https://jupyter.org/)
* [Microsoft Power BI](https://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Gelişmiş analitik senaryonuzu belirleyin

Önceki bölümdeki soruları yanıtladıktan sonra, hangi senaryonun durumunuza en uygun olduğunu belirlemeye hazırsınız. Örnek senaryolar Azure [Machine Learning'de gelişmiş analitik senaryolarında](plan-sample-scenarios.md)özetlenmiştir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Team Data Science Process (TDSP) nedir?](overview.md)
