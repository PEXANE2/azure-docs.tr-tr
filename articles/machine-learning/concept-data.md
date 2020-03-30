---
title: Azure Makine Öğreniminde Veriler
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'in verilerinize nasıl güvenli bir şekilde bağladığını ve bu verileri makine öğrenimi görevleri için nasıl kullandığını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 03/20/2020
ms.openlocfilehash: 982c9c9eadec4403c8116430e1e25092de99f1d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128483"
---
# <a name="data-access-in-azure-machine-learning"></a>Azure Machine Learning'de veri erişimi

Azure Machine Learning, buluttaki verilerinize bağlanmanızı kolaylaştırır.  Temel depolama hizmeti üzerinde bir soyutlama katmanı sağlar, böylece depolama türüne özgü kod yazmak zorunda kalmadan verilerinize güvenli bir şekilde erişebilir ve verilerinizi ile çalışır. Azure Machine Learning ayrıca aşağıdaki veri özelliklerini de sağlar:

*    Veri soyundan sürüm ve izleme
*    Veri etiketleme 
*    Veri değişikliklerini izleme
*    Pandalar ve Kıvılcım DataFrame'lerle birlikte çalışabilirlik

## <a name="data-workflow"></a>Veri iş akışı

Bulut tabanlı depolama çözümünüzdeki verileri kullanmaya hazır olduğunuzda, aşağıdaki veri teslim iş akışını öneririz. Bu iş akışı, Azure'daki bulut tabanlı bir depolama hizmetinde bir [Azure depolama hesabınız](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) ve verilerinizin olduğunu varsayar. 

1. Bağlantı bilgilerini Azure depolamaalanınızda depolamak için bir [Azure Machine Learning veri deposu](#datastores) oluşturun.

2. Bu veri deposundan, temel depolama alanınızdaki belirli bir dosyayı(lar) işaret etmek için bir [Azure Machine Learning veri kümesi](#datasets) oluşturun. 

3. Makine öğrenimi denemenizde bu veri kümesini kullanmak için
    1. Model eğitimi için denemenizin işlem hedefine monte edin.

        **Veya** 

    1. Otomatik makine öğrenimi (otomatik ML) deneme çalışır, makine öğrenimi boru hatları veya [Azure Machine Learning tasarımcısı](concept-designer.md)gibi Azure Machine Learning çözümlerinde doğrudan tüketin.

4. Veri kayması için algılamak için model çıktı veri seti için [veri kümesi monitörleri](#data-drift) oluşturun. 

5. Veri kayması algılanırsa, giriş veri kümenizi güncelleştirin ve modelinizi buna göre yeniden eğitin.

Aşağıdaki diyagram, bu önerilen iş akışının görsel bir gösterisağlar.

![Veri kavramı-diyagramı](./media/concept-data/data-concept-diagram.svg)

## <a name="datastores"></a>Veri Depoları

Azure Machine Learning veri depoları bağlantı bilgilerini Azure depolama alanınıza güvenli bir şekilde saklar, böylece komut dosyalarınızda kodlamanız gerekmez. Depolama hesabınıza kolayca bağlanmak ve temel Azure depolama hizmetinizdeki verilere erişmek için bir veri deposu kaydedin ve [oluşturun.](how-to-access-data.md) 

Azure'da veri deposu olarak kaydedilebilen desteklenen bulut tabanlı depolama hizmetleri:

+ Azure Blob Konteyner
+ Azure Dosya Paylaşımı
+ Azure Data Lake
+ Azure Veri Gölü Gen2
+ Azure SQL Veritabanı
+ PostgreSQL için Azure Veritabanı
+ Databricks Dosya Sistemi
+ MySQL için Azure Veritabanı

## <a name="datasets"></a>Veri kümeleri

Azure Machine Learning veri kümeleri, depolama hizmetinizdeki verilere işaret eden referanslardır. Bunlar verilerinizin kopyaları olmadığından, ek depolama maliyeti tahakkuk etmez. Depolamadaki verilerinizle etkileşimde kalmak için, verilerinizi makine öğrenimi görevleri için sarf edilebilir bir nesneye dönüştürmek için [bir veri kümesi oluşturun.](how-to-create-register-datasets.md) Veri alma karmaşıklıkları olmadan farklı denemeler arasında paylaşmak ve yeniden kullanmak için veri kümesini çalışma alanınıza kaydedin.

Veri kümeleri, veri depoları aracılığıyla yerel dosyalardan, genel url'lerden, [Azure Açık Veri kümelerinden](https://azure.microsoft.com/services/open-datasets/)veya Azure depolama hizmetlerinden oluşturulabilir. Bellekpandas veri çerçevesinden bir veri kümesi oluşturmak için, verileri parke gibi yerel bir dosyaya yazın ve bu dosyadan veri kümenizi oluşturun.  

2 tür veri kümesini destekliyoruz: 
+ [TabularDataset,](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) sağlanan dosyayı veya dosya listesini ayrıştarak verileri tabular biçiminde temsil eder. Daha fazla manipülasyon ve temizlik için Pandalara veya Spark DataFrame'e Bir TabularDataset yükleyebilirsiniz. Veri biçimlerinin tam listesi için TabularDatasets oluşturabilir, [TabularDatasetFactory sınıfına](https://aka.ms/tabulardataset-api-reference)bakın.

+ [FileDataset,](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) veri depolarınızda veya herkese açık URL'lerinizde tek veya birden çok dosyaya başvurur. DosyaDatasets tarafından başvurulan dosyaları bilgi işlem hedefinize [indirebilir veya monte](how-to-train-with-datasets.md#option-2--mount-files-to-a-remote-compute-target) edebilirsiniz.

Ek veri kümeleri özellikleri aşağıdaki belgelerde bulunabilir:

+ [Sürüm ve izleme](how-to-version-track-datasets.md) veri kümesi soyu.
+ Veri kayması algılamasına yardımcı olmak için [veri setinizi izleyin.](how-to-monitor-datasets.md)    

## <a name="work-with-your-data"></a>Verilerinizle çalışın

Veri kümeleri ile Azure Machine Learning özellikleriyle sorunsuz entegrasyon yoluyla bir dizi makine öğrenimi görevini gerçekleştirebilirsiniz. 

+ Bir [veri etiketleme projesi](#label)oluşturun.
+ Tren makine öğrenme modelleri:
     + [otomatik ML deneyleri](how-to-use-automated-ml-for-ml-models.md)
     + [tasarımcı](tutorial-designer-automobile-price-train-score.md#import-data)
     + [Dizüstü](how-to-train-with-datasets.md)
     + [Azure Machine Learning boru hatları](how-to-create-your-first-pipeline.md)
+ [Makine öğrenimi boru hatlarında](how-to-create-your-first-pipeline.md) [toplu çıkarımla](how-to-use-parallel-run-step.md) puanlama için veri kümelerine erişin.
+ [Veri kayması](#drift) algılamaiçin bir veri kümesi monitörü ayarlayın.

<a name="label"></a>

## <a name="data-labeling"></a>Veri etiketleme

Büyük miktarda verinin etiketlanması genellikle makine öğrenimi projelerinde baş ağrısı olmuştur. Görüntü sınıflandırması veya nesne algılama gibi bir bilgisayar görme bileşenine sahip olanlar genellikle binlerce görüntü ve ilgili etiketler gerektirir.

Azure Machine Learning, etiketleme projeleri oluşturmanız, yönetmeniz ve izlemeniz için size merkezi bir konum sağlar. Projeleri etiketlemek, verileri, etiketleri ve ekip üyelerini koordine ederek etiketleme görevlerini daha verimli bir şekilde yönetmenize yardımcı olur. Şu anda desteklenen görevler, çok etiketli veya çok katmanlı görüntü sınıflandırması ve sınırlı kutular kullanılarak nesne tanımlamadır.

Bir [veri etiketleme projesi](how-to-create-labeling-projects.md)oluşturun ve makine öğrenimi denemelerinde kullanılmak üzere bir veri kümesi çıktı.

<a name="drift"></a>

## <a name="data-drift"></a>Veri kayması

Makine öğrenimi bağlamında, veri kayması model performansının düşmesine yol açan model giriş verilerindeki değişimdir. Bu model doğruluğu zaman içinde bozulur üst nedenlerinden biridir, böylece veri sürüklenme izleme model performans sorunları algılamaya yardımcı olur.

Bir veri kümesindeki yeni verilerdeki verileri nasıl algılayıp uyaraceksiniz hakkında daha fazla bilgi edinmek için [veri kümesi monitörü oluştur](how-to-monitor-datasets.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar 

+ Bu adımları kullanarak Azure Machine Learning stüdyosunda veya Python SDK ile bir veri kümesi [oluşturun.](how-to-create-register-datasets.md)
+ Örnek defterlerimizle veri seti eğitim [örneklerini](https://aka.ms/dataset-tutorial)deneyin.
+ Veri drift örnekleri için, bu [veri drift öğretici](https://aka.ms/datadrift-notebook)bakın.
