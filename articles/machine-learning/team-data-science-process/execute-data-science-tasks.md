---
title: Veri bilimi görevlerini yerine getirme - Ekip Veri Bilimi Süreci
description: Bir veri bilimcinin bir veri bilimi projesini izlenebilir, sürüm kontrollü ve işbirlikçi bir şekilde nasıl yürütebileceği.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: e69a03cd142fdbcc5864ee38a4843e1c2e44a124
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477162"
---
# <a name="execute-data-science-tasks-exploration-modeling-and-deployment"></a>Veri bilimi görevlerini yürütmek: arama, modelleme ve dağıtım

Tipik veri bilimi görevleri arasında veri arama, modelleme ve dağıtım yer almaktadır. Bu makalede, etkileşimli veri arama, veri analizi, raporlama ve model oluşturma gibi çeşitli yaygın veri bilimi görevlerini tamamlamak için **Etkileşimli Veri Arama, Analiz ve Raporlama (IDEAR)** ve **Otomatik Modelleme ve Raporlama (AMAR)** yardımcı programlarının nasıl kullanılacağı gösterilmektedir. Bir modeli üretim ortamına dağıtma seçenekleri şunları içerebilir:

- [Azure Makine Öğrenimi](../index.yml)
- [ML hizmetleri ile SQL-Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-services)
- [Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)


## <a name="1--exploration"></a>1. <a name='DataQualityReportUtility-1'></a> Keşif 

Bir veri bilimcisi çeşitli şekillerde keşif ve raporlama yapabilir: Python için kullanılabilir kitaplıklar ve paketler (örneğin matplotlib) veya R (örneğin ggplot veya kafes) ile birlikte. Veri bilim adamları belirli senaryolar için veri arama ihtiyaçlarına uyacak şekilde bu tür kod özelleştirebilirsiniz. Yapılandırılmış verilerle başa çıkma gereksinimleri, metin veya resim gibi yapılandırılmamış veriler için farklıdır. 

Azure Machine Learning gibi ürünler, özellik oluşturma da dahil olmak üzere veri çekişmesi ve arama için [gelişmiş veri hazırlama](../how-to-create-register-datasets.md) da sağlar. Kullanıcı, gereksinimlerini en iyi şekilde paketleyen araçlara, kitaplıklara ve paketlere karar vermelidir. 

Bu aşamanın sonunda teslim edilebilir bir veri arama raporudur. Rapor, modelleme için kullanılacak verilerin oldukça kapsamlı bir görünümünü ve verilerin modelleme adımına devam etmek için uygun olup olmadığının değerlendirilmesi olmalıdır. Yarı otomatik arama, modelleme ve raporlama için aşağıdaki bölümlerde tartışılan Ekip Veri Bilimi Süreci (TDSP) programları da standartlaştırılmış veri arama ve modelleme raporları sağlar. 

### <a name="interactive-data-exploration-analysis-and-reporting-using-the-idear-utility"></a>IDEAR yardımcı programını kullanarak etkileşimli veri arama, analiz ve raporlama

Bu R işaretleme tabanlı veya Python not defteri tabanlı yardımcı program, veri kümelerini değerlendirmek ve keşfetmek için esnek ve etkileşimli bir araç sağlar. Kullanıcılar en az kodlama ile veri kümesinden hızlı bir şekilde raporlar oluşturabilir. Kullanıcılar, etkileşimli araçtaki arama sonuçlarını istemcilere teslim edilebilen veya sonraki modelleme adımına dahil edilecek değişkenlere karar vermek için kullanılan nihai bir rapora aktarmak için düğmeleri tıklatabilir.

Şu anda, araç yalnızca bellekteki veri çerçeveleri üzerinde çalışır. Araştırılacak veri kümesinin parametrelerini belirtmek için bir YAML dosyası gereklidir. Daha fazla bilgi için [TDSP Data Science Utilities'te IDEAR'a](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils)bakın.


## <a name="2--modeling"></a>2. <a name='ModelingUtility-2'></a> Modelleme

Çeşitli dillerde eğitim modelleri için çok sayıda araç seti ve paket bulunmaktadır. Veri bilimciler, doğruluk ve gecikme ile ilgili performans hususları ilgili iş kullanımı durumları ve üretim senaryoları için tatmin olduğu sürece, hangilerini rahat çalardı kullanmakta özgür olmalıdır.

Sonraki bölümde yarı otomatik modelleme için R tabanlı bir TDSP yardımcı programı nasıl kullanılacağı nı gösterir. Bu AMAR yardımcı programı, daha iyi performans gösteren bir model sağlamak için ayarlanılması gereken parametrelerin yanı sıra taban çizgisi modellerini hızlı bir şekilde oluşturmak için kullanılabilir.
Aşağıdaki model yönetimi bölümü, birden çok modeli kaydetmek ve yönetmek için nasıl bir sisteme sahip olunur gösterir.


### <a name="model-training-modeling-and-reporting-using-the-amar-utility"></a>Model eğitimi: AMAR yardımcı programını kullanarak modelleme ve raporlama

[Otomatik Modelleme ve Raporlama (AMAR) Yardımcı Programı,](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling) hiper-parametre süpürme ile model oluşturma gerçekleştirmek ve bu modellerin doğruluğunu karşılaştırmak için özelleştirilebilir, yarı otomatik bir araç sağlar. 

Model oluşturma yardımcı programı, farklı bölümleri arasında kolay gezinme için içeriğibir tablo ile kendi kendine yeten HTML çıkışı üretmek için çalıştırılabilir bir R Markdown dosyasıdır. Markdown dosyası çalıştırıldığında (örme): glmnet paketini kullanarak düzenli leştirilmiş gerileme, randomForest paketini kullanarak rastgele orman ve xgboost paketini kullanarak ağaçları artırma olmak üzere üç algoritma çalıştırılır. Bu algoritmaların her biri eğitimli bir model üretir. Bu modellerin doğruluğu daha sonra karşılaştırılır ve göreli özellik önemi çizimleri bildirilir. Şu anda, iki yardımcı program vardır: biri ikili sınıflandırma görevi, diğeri ise bir regresyon görevi içindir. Aralarındaki temel farklar, bu öğrenme görevleri için kontrol parametreleri ve doğruluk ölçümleri belirtilmiş olmasıdır. 

Bir YAML dosyası belirtmek için kullanılır:

- veri girişi (bir SQL kaynağı veya Bir R-Data dosyası) 
- verilerin hangi kısmı eğitim için kullanılır ve test için hangi kısmı
- hangi algoritmaların çalıştırılması 
- model optimizasyonu için kontrol parametrelerinin seçimi:
    - çapraz doğrulama 
    - Önyükleme
    - çapraz doğrulama kıvrımları
- her algoritma için hiper-parametre kümeleri. 

Algoritma sayısı, optimizasyon için kıvrım sayısı, hiper-parametreler ve süpürülecek hiper parametre kümelerinin sayısı da modelleri hızlı bir şekilde çalıştırmak için Yaml dosyasında değiştirilebilir. Örneğin, daha az sayıda CV kıvrımı, daha düşük sayıda parametre kümesi ile çalıştırılabilirler. Garanti edilirse, daha fazla sayıda CV kıvrımı veya daha fazla sayıda parametre kümesi yle daha kapsamlı bir şekilde çalıştırılabilirler.

Daha fazla bilgi için [TDSP Data Science Utilities'te Otomatik Modelleme ve Raporlama Yardımcı Programı'na](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling)bakın.

### <a name="model-management"></a>Model yönetimi
Birden çok model üretildikten sonra, genellikle modelleri kaydetmek ve yönetmek için bir sisteme sahip olmanız gerekir. Genellikle komut dosyaları veya API'ler ve bir arka uç veritabanı veya sürüm sistemi bir arada gerekir. Bu yönetim görevleri için göz önünde bulundurabileceğiniz birkaç seçenek şunlardır:

1. [Azure Machine Learning - model yönetimi hizmeti](../index.yml)
2. [MIT'den ModelDB](http://modeldb.csail.mit.edu:3000/projects) 
3. [Model yönetim sistemi olarak SQL-server](https://blogs.technet.microsoft.com/dataplatforminsider/2016/10/17/sql-server-as-a-machine-learning-model-management-system/)
4. [Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

## <a name="3--deployment"></a>3. <a name='Deployment-3'></a> Dağıtım

Üretim dağıtımı, bir modelin bir işletmede etkin bir rol oynamasını sağlar. Dağıtılmış bir modelden öngörüler iş kararları için kullanılabilir.

### <a name="production-platforms"></a>Üretim platformları
Modelleri üretime sokacak çeşitli yaklaşımlar ve platformlar vardır. Aşağıda birkaç seçenek verilmiştir:


- [Azure Machine Learning'de model dağıtımı](../how-to-deploy-and-where.md)
- [SQL sunucusunda bir modelin dağıtımı](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

> [!NOTE]
> Dağıtımdan önce, model puanlamanın gecikmesinin üretimde kullanılacak kadar düşük olduğundan emin olmak gerekir.
>
>

**Belirli senaryolar**için işlemdeki tüm adımları gösteren gözden geçirme lerde başka örnekler de mevcuttur. Bunlar, [Örnek walkthroughs](walkthroughs.md) makalesinde küçük resim açıklamaları ile listelenir ve bunlarla bağlantılıdır. Bunlar, akıllı bir uygulama oluşturmak için bulut, şirket içi araçlar ve hizmetleri iş akışı veya ardışık ardışık yollarla nasıl birleştirebileceklerini gösterir.

> [!NOTE]
> Azure Machine Learning Studio'su kullanarak dağıtım için [bkz.](../studio/deploy-a-machine-learning-web-service.md)
>
>

### <a name="ab-testing"></a>A/B testi
Birden çok model üretimdeyken, modellerin performansını karşılaştırmak için [A/B testi](https://en.wikipedia.org/wiki/A/B_testing) gerçekleştirmek yararlı olabilir. 

 
## <a name="next-steps"></a>Sonraki adımlar

[Veri bilimi projelerinin ilerlemesini izlemek,](track-progress.md) bir veri bilimcinin bir veri bilimi projesinin ilerlemesini nasıl izleyebilirsiniz gösterir.

[Model işlemi ve CI/CD,](ci-cd-flask.md) CI/CD'nin geliştirilen modeller ile nasıl gerçekleştirilebileceğini gösterir.


