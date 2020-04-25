---
title: Bulutta güvenli veri erişimi
titleSuffix: Azure Machine Learning
description: Azure Machine Learning verilerinize güvenli bir şekilde bağlanmayı ve ML görevleri için veri kümelerini ve veri depolarını kullanmayı öğrenin. Veri depoları, Azure Data Lake Gen 1 & 2, SQL DB, Databricks,... Azure Blobundan veri depolayabilirler.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 04/24/2020
ms.openlocfilehash: 614cc866529cd4ead8a6ea798526d59aff13d4d0
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82144481"
---
# <a name="secure-data-access-in-azure-machine-learning"></a>Azure Machine Learning 'da güvenli veri erişimi

Azure Machine Learning buluttaki verilerinize bağlanmanızı kolaylaştırır.  Bu, temel alınan depolama hizmeti üzerinde bir Özet katman sağlar, böylece depolama türüne özel kod yazmak zorunda kalmadan verilerinize güvenli bir şekilde erişebilir ve bunlarla çalışabilirsiniz. Azure Machine Learning Ayrıca aşağıdaki veri yeteneklerini de sağlar:

*    Veri kökenini sürümü oluşturma ve izleme
*    Veri etiketleme 
*    Veri değişikliklerini izleme
*    Pandas ve Spark veri çerçeveleri ile birlikte çalışabilirlik

## <a name="data-workflow"></a>Veri iş akışı

Bulut tabanlı depolama çözümünüzdeki verileri kullanmaya hazırsanız, aşağıdaki veri teslimi iş akışını öneririz. Bu iş akışı, Azure 'daki bulut tabanlı bir depolama hizmetinde bir [Azure depolama hesabınız](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) ve verileriniz olduğunu varsayar. 

1. Azure depolama verilerinize bağlantı bilgilerini depolamak için bir [Azure Machine Learning veri deposu](#datastores) oluşturun.

2. Bu veri deposundan, temel depolamadaki belirli bir dosyayı (dosyaları) göstermek için bir [Azure Machine Learning veri kümesi](#datasets) oluşturun. 

3. Machine Learning denemenize bu veri kümesini kullanmak için şunlardan birini yapabilirsiniz
    1. Model eğitimi için deneme işlem hedefine bağlayın.

        **VEYA** 

    1. Otomatik makine öğrenimi (otomatik ML) deneme çalıştırmaları, makine öğrenimi ardışık düzenleri veya [Azure Machine Learning Tasarımcısı](concept-designer.md)gibi Azure Machine Learning çözümlerinde doğrudan kullanın.

4. Model çıkış veri kümeniz için veri [kümesi izleyicileri](#data-drift) oluşturun. 

5. Veri kayması algılanırsa, giriş veri kümenizi güncelleştirip modelinize uygun şekilde yeniden eğitme.

Aşağıdaki diyagramda, önerilen bu iş akışının görsel bir gösterimi sunulmaktadır.

![Veri kavramı-diyagram](./media/concept-data/data-concept-diagram.svg)

## <a name="datastores"></a>Veri depoları

Azure Machine Learning veri depoları, bağlantı bilgilerini Azure depolama verilerinize güvenli bir şekilde saklayın, bu sayede betiklerinizde kod yazmanız gerekmez. Depolama hesabınıza kolayca bağlanmak ve temel Azure depolama hizmetindeki verilere erişmek için [bir veri deposu kaydedin ve oluşturun](how-to-access-data.md) . 

Azure 'da, veri depoları olarak kaydedilenebilir desteklenen bulut tabanlı depolama hizmetleri:

+ Azure Blob kapsayıcısı
+ Azure Dosya Paylaşımı
+ Azure Data Lake
+ Azure Data Lake Gen2
+ Azure SQL Veritabanı
+ PostgreSQL için Azure Veritabanı
+ Databricks Dosya Sistemi
+ MySQL için Azure Veritabanı

## <a name="datasets"></a>Veri kümeleri

Azure Machine Learning veri kümeleri, depolama hizmetinizdeki verileri işaret eden başvurulardır. Verilerinizin kopyaları olmadığından, ek depolama maliyeti tahakkuk etmemektedir. Depolama alanındaki verilerinizle etkileşim kurmak için, verilerinizi makine öğrenimi görevleri için tüketilebilir bir nesneye paketlemek üzere [bir veri kümesi oluşturun](how-to-create-register-datasets.md) . Veri alma karmaşıklıkları olmadan farklı denemeleri genelinde paylaşmak ve yeniden kullanmak için veri kümesini çalışma alanınıza kaydedin.

Veri kümeleri yerel dosyalardan, genel URL 'lerden, [Azure açık veri](https://azure.microsoft.com/services/open-datasets/)kümelerinden veya veri depoları aracılığıyla Azure Storage hizmetlerinden oluşturulabilir. Bellek Pandas dataframe 'ten bir veri kümesi oluşturmak için, verileri bir Parquet gibi yerel bir dosyaya yazın ve veri kümenizi bu dosyadan oluşturun.  

2 tür veri kümesini destekliyoruz: 
+ [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) , belirtilen dosya veya dosya listesini ayrıştırarak verileri tablolu biçimde temsil eder. Daha fazla düzenleme ve temizleme için TabularDataset ' i Pandas veya Spark veri çerçevesine yükleyebilirsiniz. ' Den Tabulardataset 'ler oluşturabileceğiniz veri biçimlerinin tamamen listesi için, [Tabulardatasetfactory sınıfına](https://aka.ms/tabulardataset-api-reference)bakın.

+ Bir [dosya veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) , veri mağazalarınızın veya genel URL 'nizin tek veya birden çok dosyasına başvurur. Dosya veri kümeleri tarafından başvurulan dosyaları, işlem hedeflerinize [indirebilir veya bağlayabilirsiniz](how-to-train-with-datasets.md#option-2--mount-files-to-a-remote-compute-target) .

Ek veri kümesi özellikleri aşağıdaki belgelerde bulunabilir:

+ [Sürüm ve izleme](how-to-version-track-datasets.md) veri kümesi kökenini.
+ Veri kümesini Izleme hakkında yardım almak için veri [kümenizi izleyin](how-to-monitor-datasets.md) .    

## <a name="work-with-your-data"></a>Verileriniz ile çalışma

Veri kümeleri ile Azure Machine Learning özellikleriyle sorunsuz tümleştirme aracılığıyla bir dizi makine öğrenimi görevi gerçekleştirebilirsiniz. 

+ [Veri etiketleme projesi](#label)oluşturun.
+ Makine öğrenimi modellerini eğitme:
     + [otomatikleştirilen ML denemeleri](how-to-use-automated-ml-for-ml-models.md)
     + [Tasarımcı](tutorial-designer-automobile-price-train-score.md#import-data)
     + [bilgisayarların](how-to-train-with-datasets.md)
     + [Azure Machine Learning işlem hatları](how-to-create-your-first-pipeline.md)
+ [Makine öğrenimi ardışık düzenleri](how-to-create-your-first-pipeline.md)'nde [Batch çıkarımı](how-to-use-parallel-run-step.md) ile Puanlama için veri kümelerine erişin.
+ [Veri kayması](#drift) algılaması için bir veri kümesi İzleyicisi ayarlayın.

<a name="label"></a>

## <a name="data-labeling"></a>Veri etiketleme

Büyük miktarlarda verilerin etiketlenmesi, genellikle makine öğrenimi projelerinde bir zahmetli 'e sahiptir. Görüntü sınıflandırması veya nesne algılama gibi bir bilgisayar vizyonu bileşeni olan kişiler, genellikle binlerce görüntü ve karşılık gelen Etiketler gerektirir.

Azure Machine Learning, etiketleme projelerini oluşturmak, yönetmek ve izlemek için size merkezi bir konum sağlar. Verilerin etiketlenmesi, verileri, etiketleri ve takım üyelerini koordine etmenize yardımcı olmak için etiketleme görevlerini daha verimli bir şekilde yönetmenizi sağlar. Şu anda desteklenen görevler, çok etiketli veya çok sınıflı ve sınırlanmış kutular kullanılarak nesne tanımlaması olan görüntü sınıflandırmasıdır.

Bir [veri etiketleme projesi](how-to-create-labeling-projects.md)oluşturun ve Machine Learning denemeleri 'te kullanmak üzere bir veri kümesi çıkışı yapın.

<a name="drift"></a>

## <a name="data-drift"></a>Veri kayması

Machine Learning bağlamında, veri dolu modeli, performans düşüşünü modellemeyi sağlayan model girişi verilerinde değişiklik gösterir. Model doğruluğunun zaman içindeki en önemli nedenlerinden biridir, böylece izleme verileri, model performans sorunlarını algılamaya yardımcı olur.

Veri kümesindeki yeni verilerde verileri algılama ve bu verileri uyarma hakkında daha fazla bilgi edinmek için veri [kümesi Izleyicisi oluşturma](how-to-monitor-datasets.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar 

+ [Bu adımları kullanarak](how-to-create-register-datasets.md) Azure Machine Learning Studio 'Da veya Python SDK ile bir veri kümesi oluşturun.
+ [Örnek Not defterlerimizle](https://aka.ms/dataset-tutorial)veri kümesi eğitimi örneklerini deneyin.
+ Veri kayması örnekleri için, bu [veri kayması öğreticisine](https://aka.ms/datadrift-notebook)bakın.
