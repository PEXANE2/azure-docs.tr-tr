---
title: Azure Machine Learning veriler
titleSuffix: Azure Machine Learning
description: Azure Machine Learning, verilerinize nasıl etkileşime gireceğini ve Machine Learning denemeleri 'te nasıl kullanıldığını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 12/09/2019
ms.openlocfilehash: 4ea0f7092862ef910e9f44e81cc4f5f40849bc70
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74822961"
---
# <a name="data-access-in-azure-machine-learning"></a>Azure Machine Learning veri erişimi

Bu makalede, Machine Learning görevleriniz için Azure Machine Learning veri yönetimi ve tümleştirme çözümleri hakkında bilgi edineceksiniz. Bu makalede, zaten bir [Azure depolama hesabı](https://docs.microsoft.comazure/storage/common/storage-quickstart-create-account?tabs=azure-portal) ve [Azure depolama hizmeti](https://docs.microsoft.com/azure/storage/common/storage-introduction)oluşturmuş olduğunuz varsayılmaktadır.

Depolama ortamınızdaki verileri kullanmaya hazırsanız, şunları yapmanızı öneririz

1. Azure Machine Learning veri deposu oluşturun.
2. Bu veri deposundan bir Azure Machine Learning veri kümesi oluşturun. 
3. Machine Learning denemesinizdeki bu veri kümesini kullanın 
    1. Model eğitimi için deneme işlem hedefine bağlama

        **VEYA** 

    1. Doğrudan otomatik makine öğrenimi (otomatik ML) deneme çalıştırmaları, makine öğrenimi ardışık düzenleri ve [Azure Machine Learning Tasarımcısı](concept-designer.md)gibi Azure Machine Learning çözümlerinde bu uygulamayı kullanma.
4. Model giriş ve çıkış veri kümeleriniz için veri kümesini algılamak üzere veri kümesi izleyicileri oluşturun. 
5. Veri kayması algılanırsa, veri kümenizi güncelleştirin ve modelinizi uygun şekilde yeniden eğitin.

Aşağıdaki diyagramda, bu önerilen veri erişimi iş akışının görsel bir gösterimi sunulmaktadır.

![Veri kavramı-diyagram](media/concept-data/data-concept-diagram.svg)

## <a name="access-data-in-storage"></a>Depolama alanındaki verilere erişme

Depolama hesabınızdaki verilerinize erişmek için, Azure Machine Learning veri depoları ve veri kümeleri sunar. Veri depoları, depolama hizmetiniz üzerinde bir soyutlama katmanı sağlar. Bu, bağlantı bilgileri veri deposunda tutulduğundan ve betiklerin açığa çıkmadığından, depolama verilerinize yönelik güvenlik ve erişim kolaylığına yardımcı olur. Veri kümeleri, makine öğrenimi denemeniz için kullanmak istediğiniz belirli dosya veya temel depolamadaki dosyaları işaret ediyor. Aynı şekilde, veri depoları ve veri kümeleri, makine öğrenimi görevleriniz için güvenli, ölçeklenebilir ve tekrarlanabilir bir veri teslim iş akışı sağlar.

### <a name="datastores"></a>Veri depoları

Azure Machine Learning veri deposu, Azure depolama hizmetlerinizin bir depolama soyutlamasıdır. Azure depolama hesabınıza kolayca bağlanmak ve temel Azure depolama hizmetinizdeki verilere erişmek için [bir veri deposu kaydedin ve oluşturun](how-to-access-data.md) .

Veri depoları olarak kaydedilenebilir desteklenen Azure depolama hizmetleri:
+ Azure Blob kapsayıcısı
+ Azure Dosya Paylaşımı
+ Azure Data Lake
+ Azure Data Lake Gen2
+ Azure SQL Veritabanı
+ PostgreSQL için Azure Veritabanı
+ Databricks Dosya Sistemi
+ MySQL için Azure Veritabanı

### <a name="datasets"></a>Veri kümeleri

Veri mağazalarınızın verileriyle etkileşim kurmak ve verilerinizi Machine Learning görevleri için tüketilebilir bir nesneye paketlemek üzere [bir Azure Machine Learning veri kümesi oluşturun](how-to-create-register-datasets.md) . Veri alma karmaşıklıkları olmadan farklı denemeleri genelinde paylaşmak ve yeniden kullanmak için veri kümesini çalışma alanınıza kaydedin.

Veri kümeleri yerel dosyalardan, genel URL 'lerden, [Azure açık veri](#open)kümelerinden veya veri mağazalarınızın belirli dosyalarından oluşturulabilir. Bellek Pandas dataframe 'ten bir veri kümesi oluşturmak için, verileri CSV gibi yerel bir dosyaya yazın ve veri kümenizi bu dosyadan oluşturun. Veri kümeleri verilerinizin kopyası değildir, ancak depolama hizmetinizdeki verileri işaret eden başvurulardır, ek depolama maliyeti tahakkuk etmemektedir. 

Aşağıdaki diyagramda, bir Azure Storage hizmetiniz yoksa doğrudan yerel dosyalardan, genel URL 'lerden veya Azure açık veri kümesinden bir veri kümesi oluşturabilirsiniz. Bunu yaptığınızda, veri kümeniz, denemenizin [Azure Machine Learning çalışma alanıyla](concept-workspace.md)otomatik olarak oluşturulan varsayılan veri deposuna bağlanır.

![Veri kavramı-diyagram](media/concept-data/dataset-workflow.svg)

Ek veri kümesi özellikleri aşağıdaki belgelerde bulunabilir:

+ [Sürüm ve izleme](how-to-version-track-datasets.md) veri kümesi kökenini.
+ Veri kümesini Izleme hakkında yardım almak için veri [kümenizi izleyin](how-to-monitor-datasets.md) .
+  İki veri kümesi türü hakkındaki belgeler için aşağıdakilere bakın:
    + [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) , belirtilen dosya veya dosya listesini ayrıştırarak verileri tablolu biçimde temsil eder. Böylece, verileri daha fazla düzenleme ve temizleme için bir Pandas veya Spark veri çerçevesine sunmanızı sağlar. İçinden Tabulardataset 'i oluşturabileceğiniz dosyaların tamamı listesi için [Tabulardatasetfactory sınıfına](https://aka.ms/tabulardataset-api-reference)bakın.

    + [Filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) , veri mağazalarınızın veya genel URL 'lerdeki tek veya birden çok dosyaya başvurur. Bu yöntemde, seçtiğiniz dosyaları dosya veri kümesi nesnesi olarak işlem Hedefinizle indirebilir veya bağlayabilirsiniz.

## <a name="work-with-your-data"></a>Verileriniz ile çalışma

Veri kümeleri ile Azure Machine Learning özellikleriyle sorunsuz tümleştirme aracılığıyla bir dizi makine öğrenimi görevi gerçekleştirebilirsiniz. 

+ [Makine öğrenimi modellerini eğitme](how-to-train-with-datasets.md).
+ Veri kümelerini tüketme 
     + [otomatikleştirilen ML denemeleri](how-to-create-portal-experiments.md)
     + [Tasarımcı](tutorial-designer-automobile-price-train-score.md#import-data) 
+ [Makine öğrenimi ardışık düzenleri](how-to-create-your-first-pipeline.md)'nde Batch çıkarımı ile Puanlama için veri kümelerine erişin.
+ [Veri etiketleme projesi](#label)oluşturun.
+ [Veri kayması](#drift) algılaması için bir veri kümesi İzleyicisi ayarlayın.

<a name="open"></a>

## <a name="azure-open-datasets"></a>Azure Açık Veri Kümeleri

[Azure açık veri](how-to-create-register-datasets.md#create-datasets-with-azure-open-datasets) kümeleri, daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz, seçkin ortak veri kümeleridir. Açık veri kümeleri bulutta Microsoft Azure ve Azure Machine Learning tümleşiktir. Ayrıca, veri kümelerine API 'Ler aracılığıyla erişebilir ve bunları Power BI ve Azure Data Factory gibi diğer ürünlerde kullanabilirsiniz.

Azure açık veri kümeleri, makine öğrenimi modellerini ve zenginleştirme çözümlerini eğitmenize yardımcı olan hava durumu, Census, tatiller, genel güvenlik ve konum için genel etki alanı verilerini içerir. Ayrıca, Azure açık veri kümelerinde ortak veri kümelerinizi de paylaşabilirsiniz.

<a name="label"></a>

## <a name="data-labeling"></a>Veri etiketleme

Büyük miktarlarda verilerin etiketlenmesi, genellikle makine öğrenimi projelerinde bir zahmetli 'e sahiptir. Görüntü sınıflandırması veya nesne algılama gibi bir bilgisayar vizyonu bileşeni olan kişiler, genellikle binlerce görüntü ve karşılık gelen Etiketler gerektirir.

Azure Machine Learning, etiketleme projelerini oluşturmak, yönetmek ve izlemek için size merkezi bir konum sağlar. Verilerin etiketlenmesi, verileri, etiketleri ve takım üyelerini koordine etmenize yardımcı olmak için etiketleme görevlerini daha verimli bir şekilde yönetmenizi sağlar. Şu anda desteklenen görevler, çok etiketli veya çok sınıflı ve sınırlanmış kutular kullanılarak nesne tanımlaması olan görüntü sınıflandırmasıdır.

+ Bir [veri etiketleme projesi](how-to-create-labeling-projects.md)oluşturun ve Machine Learning denemeleri 'te kullanmak üzere bir veri kümesi çıkışı yapın.

<a name="drift"></a>

## <a name="data-drift"></a>Veri kayması

Machine Learning bağlamında, veri dolu modeli, performans düşüşünü modellemeyi sağlayan model girişi verilerinde değişiklik gösterir. Model doğruluğunun zaman içindeki en önemli nedenlerinden biridir, böylece izleme verileri, model performans sorunlarını algılamaya yardımcı olur.
Veri kümesindeki yeni verilerde verileri algılama ve bu verileri uyarma hakkında daha fazla bilgi edinmek için veri [kümesi Izleyicisi oluşturma](how-to-monitor-datasets.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar 

+ Azure Machine Learning Studio 'da veya Python SDK ile veri kümesi oluşturma, [Bu adımları kullanın.](how-to-create-register-datasets.md)
+ [Örnek Not defterlerimizle](https://aka.ms/dataset-tutorial)veri kümesi eğitimi örneklerini deneyin.
+ Veri kayması örnekleri için, bu [veri kayması öğreticisine](https://aka.ms/datadrift-notebook)bakın.