---
title: 'Öğretici: otomatik makine öğrenimi kullanarak model eğitme'
description: Azure SYNAPSE Analytics 'te kod olmadan makine öğrenimi modelini eğitme hakkında öğretici.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: e219531a88787f19197a2e8c2a80040497c6dc1e
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97901428"
---
# <a name="tutorial-train-a-machine-learning-model-without-code"></a>Öğretici: kod olmadan makine öğrenimi modelini eğitme

Spark tablolarında verilerinizi, [otomatik makine öğrenimini](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml)kullanarak eğitediğiniz yeni makine öğrenimi modelleriyle zenginleştirebilirsiniz. Azure SYNAPSE Analytics 'te, makine öğrenimi modelleri oluşturmak için eğitim veri kümesi olarak kullanmak üzere çalışma alanında Spark tablosu seçebilir ve bunu bir kod ücretsiz deneyimde yapabilirsiniz.

Bu öğreticide, Azure SYNAPSE Analytics Studio 'da bir kod ücretsiz deneyim kullanarak makine öğrenimi modellerini eğitme hakkında bilgi edineceksiniz. Deneyimi el ile kodlamak yerine Azure Machine Learning otomatik makine öğrenimini kullanırsınız. Eğeceğiniz modelin türü, çözmeye çalıştığınız soruna bağlıdır.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- [Azure SYNAPSE Analytics çalışma alanı](../get-started-create-workspace.md). Aşağıdaki depolama hesabının varsayılan depolama alanı olarak yapılandırılmış olduğundan emin olun: Azure Data Lake Storage 2.. Birlikte çalıştığınız Data Lake Storage 2. dosya sistemi için, **Depolama Blobu veri katılımcısı** olduğunuzdan emin olun.
- Azure SYNAPSE Analytics çalışma alanınızda bir Apache Spark Havuzu. Ayrıntılar için bkz. [hızlı başlangıç: Azure SYNAPSE Analytics Studio kullanarak adanmış BIR SQL havuzu oluşturma](../quickstart-create-sql-pool-studio.md).
- Azure SYNAPSE Analytics çalışma alanınızda Azure Machine Learning bağlı bir hizmet. Ayrıntılar için bkz. [hızlı başlangıç: Azure SYNAPSE Analytics 'te yeni Azure Machine Learning bağlantılı hizmet oluşturma](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-spark-table-for-training-dataset"></a>Eğitim veri kümesi için Spark tablosu oluşturma

Bu öğretici için Spark tablosuna ihtiyacınız vardır. Aşağıdaki Not defteri bir tane oluşturur.

1. [Create-Spark-Table-NYCTaxi-Data. ipynb](https://go.microsoft.com/fwlink/?linkid=2149229)Not defterini indirin.

1. Not defterini Azure SYNAPSE Analytics Studio 'ya aktarın.
![Içeri aktarma seçeneği vurgulanmış şekilde Azure SYNAPSE Analytics ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-00a.png)

1. Kullanmak istediğiniz Spark havuzunu seçin ve **Tümünü Çalıştır**' ı seçin. Bu, açık veri kümesinden yeni bir York vergileni verisi alır ve bunu varsayılan Spark veritabanınıza kaydeder.
![Tüm ve Spark veritabanını çalıştır vurgulanmış şekilde Azure SYNAPSE Analytics 'in ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-00b.png)

1. Not defteri çalıştırıldıktan sonra, varsayılan Spark veritabanı altında yeni bir Spark tablosu görürsünüz. **Verilerden** **nyc_taxi** adlı tabloyu bulun.
![Yeni tablo vurgulanmış şekilde Azure SYNAPSE Analytics verileri sekmesinin ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-00c.png)

## <a name="launch-automated-machine-learning-wizard"></a>Otomatik makine öğrenimi Sihirbazını Başlat

Aşağıdaki adımları uygulayın:

1. Önceki adımda oluşturduğunuz Spark tablosuna sağ tıklayın. Sihirbazı açmak için   >  **Yeni modelle Machine Learning zenginleştirme**' yi seçin.
![Spark tablosunun, Machine Learning ve yeni model vurgulanmış şekilde zenginleştirme ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-00d.png)

1. Daha sonra, Azure Machine Learning içinde otomatik makine öğrenimi denemesi oluşturmak için yapılandırma ayrıntılarınızı sağlayabilirsiniz. Bu çalıştırma birden çok modeli ve başarılı bir çalıştırmanın en iyi modeli Azure Machine Learning modeli kayıt defterine kaydedilir.

   ![Yeni model yapılandırma belirtimleriyle zenginleştirme ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00a.png)

    - **Azure Machine Learning çalışma alanı**: otomatik makine öğrenimi çalıştırması oluşturmak için bir Azure Machine Learning çalışma alanı gerekir. Ayrıca, [bağlantılı bir hizmet](quickstart-integrate-azure-machine-learning.md)kullanarak Azure SYNAPSE Analytics çalışma alanınızı Azure Machine Learning çalışma alanıyla bağlamanız gerekir. Tüm önkoşulları yerine getirdiğinizde, bu otomatik çalıştırma için kullanmak istediğiniz Azure Machine Learning çalışma alanını belirtebilirsiniz.

    - **Deneme adı**: deneme adını belirtin. Bir otomatik makine öğrenimi çalıştırması gönderdiğinizde, bir deneme adı sağlarsınız. Çalıştırma bilgileri, Azure Machine Learning çalışma alanında bu deneme altında depolanır. Bu deneyim, varsayılan olarak yeni bir deneme oluşturur ve önerilen bir ad oluşturur, ancak aynı zamanda var olan bir deneyime ait bir ad da sağlayabilirsiniz.

    - **En iyi model**: otomatik çalıştırmada en iyi modelin adını belirtin. En iyi modele bu ad verilir ve bu çalışma sonrasında Azure Machine Learning modeli kayıt defterine otomatik olarak kaydedilir. Otomatik makine öğrenimi çalıştırması birçok makine öğrenimi modeli oluşturur. Sonraki adımda seçtiğiniz birincil ölçüme bağlı olarak, bu modeller karşılaştırılabilir ve en iyi model seçilebilir.

    - **Hedef sütun**: modelin tahmin etmek için eğitilme işlemi budur. Tahmin etmek istediğiniz sütunu seçin. (Bu öğreticide, hedef sütun olarak sayısal sütunu seçtik `fareAmount` .)

    - **Spark havuzu**: otomatikleştirilmiş deneme çalıştırması için kullanmak istediğiniz Spark Havuzu. Hesaplamalar belirttiğiniz havuzda çalıştırılır.

    - **Spark yapılandırma ayrıntıları**: Spark havuzuna ek olarak, oturum yapılandırma ayrıntılarını sağlama seçeneğiniz de vardır.

1. **Devam**’ı seçin.

## <a name="choose-task-type"></a>Görev türünü seçin

Denemeye çalıştığınız soruya bağlı olarak, deneme için makine öğrenimi model türünü seçin. `fareAmount`Hedef sütun olduğundan ve sayısal bir değer olduğundan, buradan **regresyon** ' i seçin. Daha sonra **Devam** seçeneğini belirleyin.

![Yeni modelle zenginleştirme ve gerileme vurgulanacak ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00b.png)

## <a name="additional-configurations"></a>Ek yapılandırmalar

Önceki bölümde model türü olarak **regresyon** veya **Sınıflandırma** ' yı seçerseniz, aşağıdaki yapılandırma kullanılabilir:

- **Birincil ölçüm**: modelin ne kadar iyi yaptığını ölçmek için kullanılan ölçüm. Bu, otomatik çalıştırmada oluşturulan farklı modelleri karşılaştırmak için kullanılan ölçüdür ve hangi modelin en iyi şekilde gerçekleştirileceğini tespit edin.

- **Eğitim işi süresi (saat)**: modelleri çalıştırma ve eğitme denemesi için en uzun süre (saat cinsinden). 1 ' den küçük değerler de sağlayabileceğinizi unutmayın (örneğin `0.5` ).

- **Maksimum eşzamanlı yineleme**: paralel olarak çalıştırılan en fazla yineleme sayısını temsil eder.

- **Onnx model uyumluluğu**: Bu seçeneği etkinleştirirseniz otomatik makine öğrenimi tarafından eğitilen modeller onnx biçimine dönüştürülür. Bu, özellikle Azure SYNAPSE Analytics SQL havuzlarında Puanlama için model kullanmak istiyorsanız geçerlidir.

Bu ayarların hepsi, özelleştirebileceğiniz bir varsayılan değere sahiptir.
![Yeni model ek yapılandırmalarına sahip zenginleştirme ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00c.png)

Tüm gerekli yapılandırma yapıldıktan sonra otomatik çalıştırmayı başlatabilirsiniz. Kod olmadan, çalıştırmayı doğrudan başlatan **çalıştırma oluştur** seçeneğini belirleyebilirsiniz. Alternatif olarak, kodu tercih ediyorsanız **Not defterinde aç**' ı seçebilirsiniz. Bu seçenek, çalıştırmayı oluşturan ve Not defterini çalıştıran kodu görmenizi sağlar.

>[!NOTE]
>Önceki bölümde model türü olarak **zaman serisi tahmini** seçeneğini belirlerseniz ek konfigürasyonlar yapmanız gerekir. Tahmin Ayrıca ONNX model uyumluluğunu desteklemez.

### <a name="create-run-directly"></a>Doğrudan çalıştırma oluştur

Otomatik makine öğrenimini doğrudan çalıştırmak için **çalıştırmayı Başlat**' ı seçin. Çalıştırmanın başladığını belirten bir bildirim görürsünüz. Sonra başarıyı belirten başka bir bildirim görürsünüz. Ayrıca, bildirimde bulunan bağlantıyı seçerek Azure Machine Learning durumu kontrol edebilirsiniz.
![Başarılı bildirimin ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00d.png)

### <a name="create-run-with-notebook"></a>Not defteriyle çalışma oluştur

Bir not defteri oluşturmak için **Not defterinde aç**' ı seçin. Ardından **Tümünü Çalıştır**' ı seçin. Bu ayrıca otomatik makine öğrenimi çalıştırmaya ek ayarlar ekleme olanağı sunar.

![Tümünü Çalıştır vurgulanmış şekilde Not defterinin ekran görüntüsü.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00e.png)

Çalıştırmayı başarılı bir şekilde gönderdikten sonra, Not defteri çıkışında Azure Machine Learning çalışma alanında çalışan denemenize bir bağlantı görürsünüz. Azure Machine Learning otomatik çalıştırmayı izlemek için bağlantıyı seçin.
![Bağlantı vurgulanmış şekilde Azure SYNAPSE Analytics ekran görüntüsü. ](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00f.png) )

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: adanmış SQL havuzları için makine öğrenme modeli Puanlama Sihirbazı (Önizleme)](tutorial-sql-pool-model-scoring-wizard.md)
- [Hızlı başlangıç: Azure SYNAPSE Analytics 'te yeni Azure Machine Learning bağlantılı hizmet oluşturma](quickstart-integrate-azure-machine-learning.md)
- [Azure SYNAPSE Analytics 'te makine öğrenimi özellikleri](what-is-machine-learning.md)
