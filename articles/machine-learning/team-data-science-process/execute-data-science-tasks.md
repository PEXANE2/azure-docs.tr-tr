---
title: Veri bilimi görevleri - Team Data Science Process yürütme
description: Bir veri Bilimcisi bir veri bilimi proje nasıl izlenebilir, yürütebilir sürüm denetimli ve işbirliğine dayalı yolu.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 984b03288b8dae644fc04a2cd78fb03a2e027f62
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722212"
---
# <a name="execute-data-science-tasks-exploration-modeling-and-deployment"></a>Veri bilimi görevleri yürütme: keşfi, modelleme ve dağıtım

Veri keşfi, modelleme ve dağıtım tipik veri bilimi görevleri içerir. Bu makalede etkileşimli veri araştırma, **analiz ve raporlama (ıDEAR)** ve **Otomatik modelleme ve raporlama (Amar)** yardımcı programlarının nasıl kullanılacağı, etkileşimli veri araştırması, veri analizi, raporlama ve model oluşturma gibi çeşitli yaygın veri bilimi görevlerinin nasıl tamamlanacağı gösterilmektedir. Bir üretim ortamına model dağıtmaya yönelik seçenekler şunlar olabilir:

- [Azure Machine Learning](../index.yml)
- [ML Hizmetleri ile SQL-Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-services)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)


## 1. <a name='DataQualityReportUtility-1'></a> araştırma 

Bir veri Bilimcisi keşfi ve çeşitli yollarla raporlama gerçekleştirebilirsiniz: kitaplıkları ve paketleri kullanılabilir Python (örneğin matplotlib) kullanarak veya R (ggplot veya örneğin kafes). Veri bilimcileri bu tür kod belirli senaryolar için veri İnceleme gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Yapılandırılmış verileri ilgilenmekten sorumlu gereksinimlerini farklıdır, metin veya resimler gibi yapılandırılmamış veriler için. 

Azure Machine Learning gibi ürünler Ayrıca, özellik oluşturma da dahil olmak üzere veri denetimi ve araştırmasına yönelik [Gelişmiş veri hazırlığı](../how-to-create-register-datasets.md) sağlar. Kullanıcı Araçlar, kitaplıklar ve suite en iyi paketleri ihtiyaçlarını karar vermeniz gerekir. 

Bu aşamanın sonunda teslim edilebilir bir veri araştırma rapor eder. Rapor, veri modelleme için kullanılmak üzere oldukça kapsamlı bir görünüm ve veri modelleme adıma ilerlemek uygun olup'in bir değerlendirme sağlamanız gerekir. Yarı otomatik keşfi için aşağıdaki bölümlerde açıklanan Team Data Science işlem (TDSP) yardımcı programlar, modelleme ve raporlama da standartlaştırılmış veri keşfi ve modelleme raporlar sağlar. 

### <a name="interactive-data-exploration-analysis-and-reporting-using-the-idear-utility"></a>Etkileşimli veri keşfi, analiz ve raporlama IDEAR yardımcı programını kullanma

Bu markdown tabanlı R veya Python not defteri tabanlı bir hizmet değerlendirmek ve veri kümeleri keşfetmek için esnek ve etkileşimli bir araç sağlar. Kullanıcılar, hızlı bir şekilde minimal bir kodlama ile veri kümesinde raporlar oluşturabilirsiniz. Kullanıcıların etkileşimli aracında araştırma sonuçları istemciye teslim veya modelleme sonraki adımda eklenecek hangi değişkenler hakkında kararlar almak için kullanılan bir son rapor vermek için düğmeler tıklayabilirsiniz.

Şu anda aracı yalnızca veri çerçeveleri bellekte üzerinde çalışır. Bir YAML dosyası incelenecek veri kümesi parametrelerini belirtmek için gereklidir. Daha fazla bilgi için bkz. [TDSP Data Science yardımcı programlarında ıdear](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


## 2. <a name='ModelingUtility-2'></a> modelleme

Çok sayıda araç setleri ve çeşitli dillerde eğitim modellerinde paketleri vardır. Veri bilimcileri, doğruluk ve gecikme süresi ile ilgili performans değerlendirmeleri için ilgili iş memnun olduğu sürece, rahat oldukları tıklayacağınız olanları durumları ve üretim senaryolarında kullanmak ücretsiz gelecektir.

Sonraki bölümde, yarı otomatik modelleme için R tabanlı TDSP yardımcı kullanmayı gösterir. Bu AMAR yardımcı programı, taban çizgisinin modelleri hızla yanı sıra daha iyi gerçekleştirilmesi sağlamak üzere ayarlanması için gereken parametreler model oluşturmak için kullanılabilir.
Aşağıdaki model Yönetimi bölümünde, kaydetme ve birden çok modeli yönetmek için bir sistem olan işlemi gösterilmektedir.


### <a name="model-training-modeling-and-reporting-using-the-amar-utility"></a>Eğitim modeli: modelleme ve AMAR yardımcı programını kullanarak raporlama

[Otomatik modelleme ve raporlama (AMAR) yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling) , model oluşturma işlemini hiper parametre ile gerçekleştirmek ve bu modellerin doğruluğunu karşılaştırmak için özelleştirilebilir, yarı otomatik bir araç sağlar. 

Modeli oluşturma, farklı bölümler arasında kolay gezinebilmenizi içindekiler tablosu ile müstakil HTML çıktı oluşturmak için çalıştırılabilir bir R Markdown dosyası programıdır. Markdown dosyası (birbirine bağlı) çalıştırıldığında üç algoritmalar çalıştırılır: glmnet kullanarak regularized regresyon paketini, randomForest paketini kullanarak ve xgboost paketini kullanarak ağaçları artırma rastgele orman). Bu algoritmalar her eğitilen bir modelin üretir. Bu modellerin kesinlik ardından karşılaştırılır ve göreli özellik önem çizimleri raporlanır. Şu anda iki yardımcı programı vardır: biri için bir ikili sınıflandırma görevi ve regresyon görev için biridir. Bunlar arasındaki farklar olduğu şekilde denetim parametrelerini ve doğruluk ölçümler için bu öğrenme görevleri belirtilir. 

Bir YAML dosyası belirtmek için kullanılır:

- veri girişi (SQL kaynağı veya bir R-veri dosyası) 
- verilerin hangi kısmını, eğitim ve test etmek için hangi kısmını için kullanılır
- çalıştırmak için hangi algoritmaları 
- Denetim parametreleri modeli iyileştirme Seçimi:
    - Çapraz doğrulama 
    - önyükleme
    - Çapraz doğrulama, hatları
- Her bir algoritmanın için hiper parametreli ayarlar. 

Ayrıca algoritmaları, büyük Katlama sayısı için en iyi duruma getirme, hyper-parametreleriyle sayısı ve üzerinden süpürmek için hiper parametreli kümesi sayısı modelleri hızla çalıştırmak için Yaml dosyası içinde değiştirilebilir. Örneğin, daha az sayıda CV hatları, daha az sayıda parametre kümeleri ile çalıştırılabilir. Bunu verilmişse, bunlar ayrıca daha kapsamlı CV kat sayısı daha yüksek bir sayı veya çok sayıda parametre kümeleri ile çalıştırılabilir.

Daha fazla bilgi için bkz. [TDSP veri bilimi yardımcı programlarında otomatik modelleme ve raporlama yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).

### <a name="model-management"></a>Model yönetimi
Birden çok modeli oluşturduktan sonra genellikle kaydetmek ve yönetim modellerine yönelik bir sistem olması gerekir. Genelde betiklerini veya API'leri ve arka uç veritabanı veya sürüm oluşturma sistemi gerekir. Bu yönetim görevleri için göz önünde bulundurun birkaç seçenek vardır:

1. [Azure Machine Learning-model yönetimi hizmeti](../index.yml)
2. [MıT 'ten ModelDB](https://mitdbg.github.io/modeldb/) 
3. [Model yönetimi sistemi olarak SQL-Server](https://blogs.technet.microsoft.com/dataplatforminsider/2016/10/17/sql-server-as-a-machine-learning-model-management-system/)
4. [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

## 3. <a name='Deployment-3'></a> dağıtım

Üretim dağıtımı, etkin bir rol bir iş yürütmek bir model sağlar. Dağıtılan bir modelde tahminleri iş kararları için kullanılabilir.

### <a name="production-platforms"></a>Üretim platformları
Çeşitli yaklaşımlar ve modelleri üretime koymak için Platform vardır. Bazı seçenekler şunlardır:


- [Azure Machine Learning 'de model dağıtımı](../how-to-deploy-and-where.md)
- [SQL Server 'da bir modelin dağıtılması](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

> [!NOTE]
> Dağıtımdan önce bir üretim ortamında kullanmak için düşük Puanlama modeli, gecikme süresi sağlamak üzere vardır.
>
>

**Belirli senaryolar**için işlemdeki tüm adımları gösteren izlenecek yollarda daha fazla örnek mevcuttur. Bunlar, [örnek yönergeler](walkthroughs.md) makalesinde listelenmiş ve küçük resim açıklamalarıyla bağlantılandırılır. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir.

> [!NOTE]
> Azure Machine Learning Studio kullanarak dağıtım için bkz. [Azure Machine Learning Web hizmeti dağıtma](../studio/deploy-a-machine-learning-web-service.md).
>
>

### <a name="ab-testing"></a>A / B testi
Üretimde birden çok model olduğunda, modellerin performansını karşılaştırmak için [bir/B testi](https://en.wikipedia.org/wiki/A/B_testing) gerçekleştirmek yararlı olabilir. 

 
## <a name="next-steps"></a>Sonraki adımlar

Veri bilimi [projelerinin Ilerlemesini izlemek](track-progress.md) , bir veri bilimcinin bir veri bilimi projesinin ilerlemesini nasıl izleyebilme şeklini gösterir.

[Model işlemi ve CI/CD](ci-cd-flask.md) , geliştirilmiş modeller ile CI/CD 'nin nasıl gerçekleştirileceğini gösterir.


