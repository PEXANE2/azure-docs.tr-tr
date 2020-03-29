---
title: Ekip Veri Bilimi Sürecinin Veri Toplama ve Anlayışı
description: Veri bilimi projelerinizin veri toplama ve anlama aşaması için hedefler, görevler ve teslim edilebilir ler
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720512"
---
# <a name="data-acquisition-and-understanding-stage-of-the-team-data-science-process"></a>Ekip Veri Bilimi Sürecinin Veri Toplama ve Anlama Aşaması

Bu makalede, Ekip Veri Bilimi Süreci'nin (TDSP) veri toplama ve anlama aşamasıyla ilişkili hedefler, görevler ve teslim edilebilirler sıralanmıştır. Bu işlem, veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü, projelerin genellikle yineleyici olarak yürüttüğü başlıca aşamaları özetler:

   1. **Kurumsal yaklaşım**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsünün görsel bir gösterimi aşağıdavelvettir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefler
* Hedef değişkenlerle ilişkisi anlaşılan temiz, yüksek kaliteli bir veri kümesi üretin. Modelolmaya hazır olmak için veri kümesini uygun analiz ortamında bulun.
* Verileri düzenli olarak yenileyen ve puanlayan veri ardışık yapısının çözüm mimarisini geliştirin.

## <a name="how-to-do-it"></a>Nasıl yapılacağını
Bu aşamada ele üç ana görev vardır:

   * Verileri hedef analitik ortama **doğru yutun.**
   * Veri kalitesinin soruyu yanıtlamak için yeterli olup olmadığını belirlemek için **verileri keşfedin.** 
   * Yeni veya düzenli olarak yenilenen verileri elde etmek için **bir veri ardışık düzenemi ayarlayın.**

### <a name="ingest-the-data"></a>Verileri yutma
Verileri kaynak konumlardan eğitim ve öngörüler gibi analitik işlemleri çalıştırdığınız hedef konumlara taşımak için işlemi ayarlayın. Çeşitli Azure veri hizmetleriyle verilerin nasıl taşınırak ile ilgili teknik ayrıntılar ve seçenekler [için, verileri analitik için depolama ortamlarına](ingest-data.md)yükleyin'e bakın. 

### <a name="explore-the-data"></a>Verileri keşfetme
Modellerinizi eğitmeden önce, verilerin sağlam bir şekilde anlaşılması nı geliştirmeniz gerekir. Gerçek dünya veri kümeleri genellikle gürültülüdür, eksik değerlerdir veya bir dizi başka tutarsızlıkları da vardır. Verilerinizin kalitesini denetlemek ve verileri modellemeye hazır olmadan önce işlemek için gereken bilgileri sağlamak için veri özetleme ve görselleştirmeyi kullanabilirsiniz. Bu işlem genellikle yinelemeli.

TDSP, verileri görselleştirmeye ve veri özet raporları hazırlamaya yardımcı olmak için [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils)adı verilen otomatik bir yardımcı program sağlar. İlk olarak iDEAR ile başlayıp, ilk veri anlayışını kodlama olmadan etkileşimli olarak geliştirmeye yardımcı olmak için keşfetmenizi öneririz. Daha sonra veri arama ve görselleştirme için özel kod yazabilirsiniz. Verilerin temizlenmesiyle ilgili rehberlik için gelişmiş [makine öğrenimi için veri hazırlamak için Görevler'e](prepare-data.md)bakın.  

Temizlenen verilerin kalitesinden memnun olduktan sonra, bir sonraki adım, verilerin doğasında olan desenleri daha iyi anlamaktır. Bu veri çözümlemesi, hedefiniz için uygun bir tahmin modeli seçmenize ve geliştirmenize yardımcı olur. Verilerin hedefe ne kadar iyi bağlanabildiğini kanıtolarak arayın. Ardından, sonraki modelleme adımlarıyla ilerlemek için yeterli veri olup olmadığını belirleyin. Yine, bu işlem genellikle yinelemeli. Başlangıçta önceki aşamada tanımlanan veri kümesini artırmak için daha doğru veya daha alakalı verilere sahip yeni veri kaynakları bulmanız gerekebilir. 

### <a name="set-up-a-data-pipeline"></a>Veri ardışık bir iş tonu ayarlama
Verilerin ilk alımı ve temizlenmesine ek olarak, genellikle devam eden bir öğrenme sürecinin bir parçası olarak yeni veriler elde etmek veya verileri düzenli olarak yenilemek için bir işlem ayarlamanız gerekir. Puanlama bir veri ardışık alanı veya iş akışı ile tamamlanabilir. Verileri [şirket içi BIR SQL Server örneğinden Azure Veri Fabrikası makalesiyle Azure SQL Veritabanına taşıma,](move-sql-azure-adf.md) Azure Veri [Fabrikası](https://azure.microsoft.com/services/data-factory/)ile bir ardışık yapı nın nasıl ayarlanacak larına ilişkin bir örnek verir. 

Bu aşamada, veri ardışık bir çözüm mimarisi geliştirmek. Veri bilimi projesinin bir sonraki aşamasına paralel olarak boru hattını geliştirebilirsiniz. İş gereksinimlerinize ve bu çözümün entegre edildiği mevcut sistemlerinizin kısıtlamalarına bağlı olarak, boru hattı aşağıdaki seçeneklerden biri olabilir: 

   * Toplu iş tabanlı
   * Akış veya gerçek zamanlı 
   * Bir melez 

## <a name="artifacts"></a>Yapıtlar
Bu aşamada teslim edilebilir ler şunlardır:

   * [Veri kalitesi raporu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/DataSummaryReport.md): Bu rapor veri özetlerini, her öznitelik ve hedef arasındaki ilişkileri, değişken sıralamasını ve daha fazlasını içerir. TDSP'nin bir parçası olarak sağlanan [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) aracı, bu raporu CSV dosyası veya ilişkisel tablo gibi herhangi bir tabloveri kümesinde hızlı bir şekilde oluşturabilir. 
   * **Çözüm mimarisi**: Çözüm mimarisi, bir model yaptıktan sonra yeni veriler üzerinde puanlama veya öngörüler çalıştırmak için kullandığınız veri ardışık sisteminizin diyagramı veya açıklaması olabilir. Ayrıca, yeni verilere dayanarak modelinizi yeniden eğitmek için boru hattını da içerir. TDSP dizin yapısı şablonu kullandığınızda belgeyi [Proje](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) dizininde depolayın.
   * **Checkpoint kararı**: Tam özellikli mühendislik ve model oluşturmaya başlamadan önce, projeyi yeniden değerlendirerek beklenen değerin devam etmek için yeterli olup olmadığını belirleyebilirsiniz. Örneğin, devam etmeye hazır olabilirsiniz, daha fazla veri toplamanız veya soruyu yanıtlamak için veriler bulunmadığından projeyi terk etmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

TDSP'nin yaşam döngüsündeki her adıma bağlantılar aşağıda vereb

   1. [Kurumsal yaklaşım](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren tam izlenme ler sağlarız. [Örnek walkthroughs](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları içeren senaryoların bir listesini sağlar. İzler, bulut, şirket içi araçlar ve hizmetlerin akıllı bir uygulama oluşturmak için iş akışı veya ardışık ardışık yollarla nasıl birleştirilebildiğini gösterir. 

Azure Machine Learning Studio kullanan TDSP'lerde adımların nasıl yürütüldüne ilgili örnekler [için](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle-data)bkz.
