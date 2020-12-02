---
title: 'Öğretici: bilişsel hizmetler ile anomali algılama'
description: SYNAPSE 'de anomali algılama için bilişsel hizmetlerden yararlanma öğreticisi
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 4052d6a0773aa27e0a378ee04975c7946f1ffbfe
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468655"
---
# <a name="tutorial-anomaly-detection-with-cognitive-services-preview"></a>Öğretici: bilişsel hizmetler ile anomali algılama (Önizleme)

Bu öğreticide, bilişsel [Hizmetler](https://go.microsoft.com/fwlink/?linkid=2147492)Ile Azure SYNAPSE 'da verilerinizi kolayca zenginleştirme hakkında bilgi edineceksiniz. Anomali algılama işlemini gerçekleştirmek için [anomali algılayıcısının](https://go.microsoft.com/fwlink/?linkid=2147493) kullanılması gerekir. Azure SYNAPSE ' deki bir Kullanıcı, anomali algılama için zenginleştirmek üzere bir tablo seçebilir.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> - Zaman serisi verilerini içeren Spark tablo veri kümesini alma adımları.
> - Anomali algılayıcı bilişsel hizmetini kullanarak verileri zenginleştirmek için Azure SYNAPSE 'de sihirbaz deneyimi kullanın.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Varsayılan depolama alanı olarak yapılandırılmış bir ADLS 2. depolama hesabıyla [Azure SYNAPSE Analytics çalışma alanı](../get-started-create-workspace.md) . Birlikte çalıştığınız ADLS 2. FileSystem 'ın **Depolama Blobu veri katılımcısı** olması gerekir.
- Azure SYNAPSE Analytics çalışma alanınızdaki Spark Havuzu. Ayrıntılar için bkz. [Azure 'Da Spark havuzu oluşturma SYNAPSE](../quickstart-create-sql-pool-studio.md).
- Bu öğreticiyi kullanabilmeniz için, bu öğreticide açıklanan ön yapılandırma adımlarını da gerçekleştirmeniz gerekir. [Azure SYNAPSE 'de bilişsel hizmetler 'ı yapılandırın](tutorial-configure-cognitive-services-synapse.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com/) oturum açma

## <a name="create-a-spark-table"></a>Spark tablosu oluşturma

Bu öğretici için Spark tablosuna ihtiyacınız olacaktır.

1. Spark tablosu oluşturmak için kodu içeren aşağıdaki Not defteri dosyasını indirin: [prepare_anomaly_detector_data. ipynb](https://go.microsoft.com/fwlink/?linkid=2149577)

1. Dosyayı Azure SYNAPSE çalışma alanınıza yükleyin.
![Not defterini karşıya yükle](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00a.png)

1. Not defteri dosyasını açın ve tüm hücreleri **çalıştırmayı** seçin.
![Spark tablosu oluşturma](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00b.png)

1. **Anomaly_detector_testing_data** adlı Spark tablosu artık varsayılan Spark veritabanında görünmelidir.

## <a name="launch-cognitive-services-wizard"></a>Bilişsel hizmetler Sihirbazı 'nı Başlat

1. Önceki adımda oluşturulan Spark tablosuna sağ tıklayın. Sihirbazı açmak için "Machine Learning->" var olan modelle zenginleştirme "seçeneğini belirleyin.
![Puanlama Başlatma Sihirbazı](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00g.png)

2. Bir yapılandırma paneli görünür ve bilişsel hizmetler modeli seçmeniz istenir. Anomali algılayıcısı ' nı seçin.

![Puanlama Başlatma Sihirbazı-Adım](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00c.png)

## <a name="provide-authentication-details"></a>Kimlik doğrulama ayrıntılarını sağlayın

Bilişsel hizmetler 'in kimliğini doğrulamak için, Key Vault kullanmak üzere gizli dizi başvurusunda bulunmak gerekir. Aşağıdaki girişler, bu adımdan önce tamamlanması gereken [Önkoşul adımlarına](tutorial-configure-cognitive-services-synapse.md) bağlıdır.

- **Azure aboneliği**: anahtar kasanızın ait olduğu Azure aboneliğini seçin.
- Bilişsel **Hizmetler hesabı**: Bu, bağlandığınız metin analizi kaynağıdır.
- **Bağlı hizmet Azure Key Vault**: önkoşul adımlarının bir parçası olarak, metin analizi kaynağınız için bağlı bir hizmet oluşturdunuz. Lütfen buradan seçin.
- **Gizli dizi adı**: Bu, anahtar kasasındaki gizli anahtar, bilişsel hizmetler kaynağınızın kimlik doğrulaması için kullanılacak anahtarı içeren addır.

![Azure Key Vault ayrıntılarını sağlayın](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00d.png)

## <a name="configure-anomaly-detection"></a>Anomali algılamayı yapılandırma

Sonra anomali algılamayı yapılandırmanız gerekir. Lütfen aşağıdaki ayrıntıları sağlayın:

- Ayrıntı düzeyi: verilerinizin örneklendiği hız. Örneğin, veriniz her dakika için bir değer içeriyorsa, ayrıntı düzeyi dakikalarınızın süresi geçde olur. **Aylık** Seç 

- Zaman damgası: serinin saatini temsil eden sütun. Sütun **zaman damgasını** seçin

- Timeseries değeri: zaman damgası sütunu tarafından belirtilen zamanda serinin değerini temsil eden sütun. Sütun **değeri** Seç

- Gruplandırma: seriyi gruplandıran sütun. Diğer bir deyişle, bu sütunda aynı değere sahip tüm satırlar bir zaman serisi oluşturmalıdır. Sütun **grubu** seçin

İşiniz bittiğinde, **Not defterini aç**' ı seçin. Bu işlem, Azure bilişsel hizmetler 'i kullanarak anomali algılama gerçekleştiren PySpark kodu ile sizin için bir not defteri oluşturur.

![Anomali algılayıcısı 'nı yapılandırma](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00e.png)

## <a name="open-notebook-and-run"></a>Not defteri 'ni açın ve çalıştırın

Az önce açtığınız Not defteri, bilişsel hizmetlere bağlanmak için [mmlspark kitaplığını](https://github.com/Azure/mmlspark) kullanıyor.

Verdiğiniz Azure Key Vault ayrıntıları, bu deneyimden verilerinize güvenli bir şekilde başvurmanızı sağlar.

Şimdi anomali algılama işlemini gerçekleştirmek için **tüm hücreleri çalıştırabilirsiniz** . Bilişsel [Hizmetler-anomali algılayıcısı](https://go.microsoft.com/fwlink/?linkid=2147493)hakkında daha fazla bilgi edinin.

![Anomali algılama çalıştırma](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00f.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure bilişsel hizmetler ile yaklaşım Analizi](tutorial-cognitive-services-sentiment.md)
- [Öğretici: Azure SYNAPSE adanmış SQL havuzlarında makine öğrenimi modeli Puanlama](tutorial-sql-pool-model-scoring-wizard.md)
- [Azure SYNAPSE Analytics 'teki Machine Learning özellikleri](what-is-machine-learning.md)