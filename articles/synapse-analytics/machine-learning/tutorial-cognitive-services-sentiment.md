---
title: 'Öğretici: bilişsel hizmetler ile yaklaşım Analizi'
description: SYNAPSE ' de yaklaşım analizine yönelik bilişsel hizmetler 'in nasıl kullanılacağına ilişkin öğretici
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 6a4833cf0d73939e01fd3e3e7263c6cba3c0a28a
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98222199"
---
# <a name="tutorial-sentiment-analysis-with-cognitive-services-preview"></a>Öğretici: bilişsel hizmetler ile yaklaşım Analizi (Önizleme)

Bu öğreticide, bilişsel [Hizmetler](../../cognitive-services/index.yml)Ile Azure SYNAPSE 'da verilerinizi kolayca zenginleştirme hakkında bilgi edineceksiniz. Yaklaşım analizini gerçekleştirmek için [metin analizi](../../cognitive-services/text-analytics/index.yml) yeteneklerini kullanacağız. Azure SYNAPSE ' deki bir Kullanıcı, yalnızca duygu ile zenginleştirmek üzere metin sütunu içeren bir tablo seçebilir. Bu yaklaşım pozitif, negatif, karışık veya nötr olabilir ve bir olasılık da döndürülür.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> - Yaklaşım analizi için metin sütununu içeren Spark tablo veri kümesini alma adımları.
> - Metin Analizi bilişsel hizmetler kullanarak verileri zenginleştirmek için Azure SYNAPSE 'de sihirbaz deneyimi kullanın.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- Varsayılan depolama alanı olarak yapılandırılmış bir ADLS 2. depolama hesabıyla [Azure SYNAPSE Analytics çalışma alanı](../get-started-create-workspace.md) . Birlikte çalıştığınız ADLS 2. FileSystem 'ın **Depolama Blobu veri katılımcısı** olması gerekir.
- Azure SYNAPSE Analytics çalışma alanınızdaki Spark Havuzu. Ayrıntılar için bkz. [Azure 'Da Spark havuzu oluşturma SYNAPSE](../quickstart-create-sql-pool-studio.md).
- Bu öğreticiyi kullanabilmeniz için, bu öğreticide açıklanan ön yapılandırma adımlarını da gerçekleştirmeniz gerekir. [Azure SYNAPSE 'de bilişsel hizmetler 'ı yapılandırın](tutorial-configure-cognitive-services-synapse.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com/) oturum açma

## <a name="create-a-spark-table"></a>Spark tablosu oluşturma

Bu öğretici için Spark tablosuna ihtiyacınız olacaktır.

1. Metin analizi için bir veri kümesi içeren aşağıdaki CSV dosyasını indirin: [FabrikamComments.csv](https://github.com/Kaiqb/KaiqbRepo0731190208/blob/master/CognitiveServices/TextAnalytics/FabrikamComments.csv)

1. Dosyayı Azure SYNAPSE ADLSGen2 Storage hesabınıza yükleyin.
![Verileri karşıya yükleme](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00a.png)

1. Dosya üzerinde sağ tıklayıp **Yeni Not defteri-> Spark tablosu oluştur**' u seçerek. csv dosyasından bir Spark tablosu oluşturun.
![Spark tablosu oluşturma](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00b.png)

1. Tabloyu kod hücresine adlandırın ve Not defterini Spark havuzunda çalıştırın. "Header = true" olarak ayarlamayı unutmayın.
![Not defterini çalıştır](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00c.png)

```python
%%pyspark
df = spark.read.load('abfss://default@azuresynapsesa.dfs.core.windows.net/data/FabrikamComments.csv', format='csv'
## If header exists uncomment line below
, header=True
)
df.write.mode("overwrite").saveAsTable("default.YourTableName")
```

## <a name="launch-cognitive-services-wizard"></a>Bilişsel hizmetler Sihirbazı 'nı Başlat

1. Önceki adımda oluşturulan Spark tablosuna sağ tıklayın. Sihirbazı açmak için "Machine Learning->" var olan modelle zenginleştirme "seçeneğini belirleyin.
![Puanlama Başlatma Sihirbazı](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00d.png)

2. Bir yapılandırma paneli görünür ve bilişsel hizmetler modeli seçmeniz istenir. Metin analizi-Yaklaşım Analizi seçin.

![Puanlama Başlatma Sihirbazı-Adım](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00e.png)

## <a name="provide-authentication-details"></a>Kimlik doğrulama ayrıntılarını sağlayın

Bilişsel hizmetler 'in kimliğini doğrulamak için, Key Vault kullanmak üzere gizli dizi başvurusunda bulunmak gerekir. Aşağıdaki girişler, bu adımdan önce tamamlanması gereken [Önkoşul adımlarına](tutorial-configure-cognitive-services-synapse.md) bağlıdır.

- **Azure aboneliği**: anahtar kasanızın ait olduğu Azure aboneliğini seçin.
- Bilişsel **Hizmetler hesabı**: Bu, bağlandığınız metin analizi kaynağıdır.
- **Bağlı hizmet Azure Key Vault**: önkoşul adımlarının bir parçası olarak, metin analizi kaynağınız için bağlı bir hizmet oluşturdunuz. Lütfen buradan seçin.
- **Gizli dizi adı**: Bu, anahtar kasasındaki gizli anahtar, bilişsel hizmetler kaynağınızın kimlik doğrulaması için kullanılacak anahtarı içeren addır.

![Azure Key Vault ayrıntılarını sağlayın](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00f.png)

## <a name="configure-sentiment-analysis"></a>Yaklaşım Analizi Yapılandır

Sonra, yaklaşım analizini yapılandırmanız gerekir. Lütfen aşağıdaki ayrıntıları seçin:
- **Dil**: üzerinde yaklaşım analizi gerçekleştirmek istediğiniz metnin dilini seçin. **En**' ı seçin.
- **Metin sütunu**: Bu, veri kümenizdeki, yaklaşımı tespit etmek üzere çözümlemek istediğiniz metin sütunudur. Tablo sütunu **açıklaması** seçin.

İşiniz bittiğinde, **Not defterini aç**' a tıklayın. Bu işlem, Azure bilişsel hizmetler ile yaklaşım analizini gerçekleştiren PySpark kodu ile sizin için bir not defteri oluşturur.

![Yaklaşım Analizi Yapılandır](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00g.png)

## <a name="open-notebook-and-run"></a>Not defteri 'ni açın ve çalıştırın

Az önce açtığınız Not defteri, bilişsel hizmetlere bağlanmak için [mmlspark kitaplığını](https://github.com/Azure/mmlspark) kullanıyor.

Verdiğiniz Azure Key Vault ayrıntıları, bu deneyimden verilerinize güvenli bir şekilde başvurmanızı sağlar.

Artık tüm hücreleri **çalıştırarak** verilerinizi Sentiments ile zenginleştirebilirsiniz. Yaklaşımları pozitif/negatif/nötr/karışık olarak döndürülecek ve yaklaşım başına olasılıklara de sahip olacak. Bilişsel [Hizmetler](../../cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md)hakkında daha fazla bilgi edinin-yaklaşım analizi.

![Yaklaşım Analizi Çalıştır](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00h.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Öğretici: Azure bilişsel hizmetler ile anomali algılama](tutorial-cognitive-services-sentiment.md)
- [Öğretici: Azure SYNAPSE adanmış SQL havuzlarında makine öğrenimi modeli Puanlama](tutorial-sql-pool-model-scoring-wizard.md)
- [Azure Azure SYNAPSE Analytics 'teki Machine Learning özellikleri](what-is-machine-learning.md)