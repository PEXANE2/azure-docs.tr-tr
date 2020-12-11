---
title: 'Öğretici: otomatik ML kullanarak model eğitme'
description: Apache Spark ve otomatik ML kullanarak Azure SYNAPSE 'de kod olmadan makine öğrenimi modelini eğitme hakkında öğretici.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 4967d5305b4b438f3baa6fca078d7b3169612590
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97093409"
---
# <a name="tutorial-train-a-machine-learning-model-code-free-in-azure-synapse-with-apache-spark-and-automated-ml"></a>Öğretici: Apache Spark ve otomatikleştirilmiş ML ile Azure 'da Machine Learning modeli kodu ücretsiz olarak eğitme

[Azure Machine Learning ' de OTOMATIK ml](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml)kullanarak eğitediğiniz yeni makine öğrenimi modelleriyle verileri Spark tablolarında kolayca zenginleştirme hakkında bilgi edinin.  SYNAPSE ' deki bir Kullanıcı, bir kod ücretsiz deneyimde makine öğrenimi modelleri oluşturmak için eğitim veri kümesi olarak kullanmak üzere Azure SYNAPSE çalışma alanında bir Spark tablosu seçebilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> - Azure Machine Learning 'de otomatik ML kullanan Azure SYNAPSE Studio 'da bir kod ücretsiz deneyim kullanarak makine öğrenimi modellerini eğitme. Eğeceğiniz modelin türü, çözmeye çalıştığınız soruna bağlıdır.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Varsayılan depolama alanı olarak yapılandırılmış bir ADLS 2. depolama hesabıyla [SYNAPSE Analytics çalışma alanı](../get-started-create-workspace.md) . Birlikte çalıştığınız ADLS 2. FileSystem 'ın **Depolama Blobu veri katılımcısı** olması gerekir.
- Azure SYNAPSE Analytics çalışma alanınızdaki Spark Havuzu. Ayrıntılar için bkz. [Azure 'Da Spark havuzu oluşturma SYNAPSE](../quickstart-create-sql-pool-studio.md).
- Azure SYNAPSE Analytics çalışma alanınızda bağlı hizmeti Azure Machine Learning. Ayrıntılar için bkz. [Azure SYNAPSE 'de Azure Machine Learning bağlantılı hizmet oluşturma](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com/) oturum açma

## <a name="create-a-spark-table-for-training-dataset"></a>Eğitim veri kümesi için Spark tablosu oluşturma

Bu öğretici için Spark tablosuna ihtiyacınız olacaktır. Aşağıdaki Not defteri bir Spark tablosu oluşturacak.

1. [Create-Spark-Table-NYCTaxi-Data. ipynb](https://go.microsoft.com/fwlink/?linkid=2149229) Not defterini indirin

1. Not defterini Azure SYNAPSE Studio 'ya aktarın.
![Not Defteri al](media/tutorial-automl-wizard/tutorial-automl-wizard-00a.png)

1. Kullanmak istediğiniz Spark havuzunu seçin ve öğesine tıklayın `Run all` . Bu Not defterini çalıştırmak, açık veri kümesinden yeni bir York vergileni verisi alacak ve varsayılan Spark veritabanınıza kaydedilecek.
![Tümünü Çalıştır](media/tutorial-automl-wizard/tutorial-automl-wizard-00b.png)

1. Not defteri çalıştırıldıktan sonra, varsayılan Spark veritabanı altında yeni bir Spark tablosu oluşturulur. Veri merkezine gidin ve ile adlı tabloyu bulun `nyc_taxi` .
![Spark tablosu](media/tutorial-automl-wizard/tutorial-automl-wizard-00c.png)

## <a name="launch-automated-ml-wizard-to-train-a-model"></a>Bir modeli eğitme için otomatik ML Sihirbazı 'nı başlatın

Önceki adımda oluşturulan Spark tablosuna sağ tıklayın. Sihirbazı açmak için "Machine Learning-> Enrich New model" seçeneğini belirleyin.
![Otomatik ML 'yi Başlatma Sihirbazı](media/tutorial-automl-wizard/tutorial-automl-wizard-00d.png)

Bir yapılandırma paneli görünür ve Azure Machine Learning bir otomatik ML denemesi oluşturmak için yapılandırma ayrıntıları sağlamanız istenir. Bu çalıştırma birden çok modeli eğitecektir ve başarılı bir çalıştırmanın en iyi modeli Azure Machine Learning modeli kayıt defterine kaydedilir:

![Run adım yapılandırma](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00a.png)

- **Azure Machine Learning çalışma alanı**: otomatik ml denemesi çalıştırmasının oluşturulması için bir Azure Machine Learning çalışma alanı gereklidir. Ayrıca, [bağlantılı bir hizmet](quickstart-integrate-azure-machine-learning.md)kullanarak Azure SYNAPSE çalışma alanınızı Azure Machine Learning çalışma alanıyla bağlamanız gerekir. Ön koşul = koşullar ' a sahip olduktan sonra, bu otomatik ML çalıştırması için kullanmak istediğiniz Azure Machine Learning çalışma alanını belirtebilirsiniz.

- **Deneme adı**: deneme adını belirtin. Otomatikleştirilmiş bir ML çalıştırması gönderdiğinizde, bir deneme adı sağlarsınız. Çalıştırma bilgileri, Azure Machine Learning çalışma alanında bu deneme altında depolanır. Bu deneyim, varsayılan olarak yeni bir deneme oluşturacak ve önerilen bir ad oluşturuyor, ancak aynı zamanda var olan bir deneyime de bir ad verebilirsiniz.

- **En iyi model**: otomatik ml çalıştırağından en iyi modelin adını belirtin. En iyi modele bu ad verilir ve bu çalışma sonrasında Azure Machine Learning modeli kayıt defterine otomatik olarak kaydedilir. Otomatik ML çalıştırması birçok makine öğrenimi modeli oluşturacaktır. Sonraki adımda seçtiğiniz birincil ölçüme bağlı olarak, bu modeller karşılaştırılabilir ve en iyi model seçilebilir.

- **Hedef sütun**: modelin tahmin etmek için eğitilme işlemi budur. Tahmin etmek istediğiniz sütunu seçin.

- **Spark havuzu**: otomatik ml denemesi çalıştırması için kullanmak istediğiniz Spark Havuzu. Hesaplamalar belirttiğiniz havuzda yürütülür.

- **Spark yapılandırma ayrıntıları**: Spark havuzuna ek olarak, oturum yapılandırma ayrıntılarını sağlama seçeneğiniz de vardır.

Bu öğreticide, hedef sütun olarak sayısal sütunu seçtik `fareAmount` .

"Devam" a tıklayın.

## <a name="choose-task-type"></a>Görev türünü seçin

Yanıtlamaya çalıştığınız soruya göre deneme için makine öğrenimi model türünü seçin. `fareAmount`Hedef sütun olarak seçtiğimiz ve sayısal bir değer olduğundan *gerileme*' ı seçeceğiz.

Ek ayarları yapılandırma için "devam" ı tıklatın.

![Görev türü seçimi](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00b.png)

## <a name="additional-configurations"></a>Ek yapılandırmalar

*Sınıflandırma* veya *regresyon* türü ' nü seçerseniz, ek konfigürasyonlar şunlardır:

- **Birincil ölçüm**: modelin ne kadar iyi yaptığını ölçmek için kullanılan ölçüm. Bu, otomatik ML çalıştırmasında oluşturulan farklı modelleri karşılaştırmak için kullanılacak ölçüdür ve hangi modelin en iyi şekilde gerçekleştirileceğini tespit eder.

- **Eğitim işi süresi (saat)**: modelleri çalıştırma ve eğitme denemesi için en uzun süre (saat cinsinden). 1 ' den küçük değerler de sağlayabileceğinizi unutmayın. Örneğin, `0.5`.

- **Maksimum eşzamanlı yineleme**: paralel olarak yürütülecek en fazla yineleme sayısını temsil eder.

- **Onnx model uyumluluğu**: etkinleştirilirse, otomatik ml tarafından eğitilen modeller onnx biçimine dönüştürülür. Bu, özellikle Azure SYNAPSE SQL havuzlarında Puanlama için model kullanmak istiyorsanız geçerlidir.

Bu ayarların hepsi, özelleştirebileceğiniz bir varsayılan değere sahiptir.
![ek yapılandırma](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00c.png)

> "Zaman serisi tahmini" seçeneğini belirlerseniz, daha fazla yapılandırma gerektiğini unutmayın. Tahmin Ayrıca ONNX model uyumluluğunu desteklemez.

Tüm gerekli yapılandırma yapıldıktan sonra otomatik ML çalıştırması ' nı başlatabilirsiniz.

Azure Azure SYNAPSE 'de otomatik bir ML çalıştırması başlatmak için iki yol vardır. Kod ücretsiz bir deneyim için doğrudan **çalıştırma oluşturmayı** seçebilirsiniz. Kodu tercih ediyorsanız, çalıştırma ve Not defterini çalıştırma kodunu görmenizi sağlayan **Not defterinde aç**' ı seçebilirsiniz.

### <a name="create-run-directly"></a>Doğrudan çalıştırma oluştur

Otomatik ML 'yi doğrudan çalıştırmak için "çalıştırmaya başla" ya tıklayın. Otomatik ML çalıştırmasının başladığını gösteren bir bildirim olacaktır.

Otomatik ML çalıştırması başarıyla başlatıldıktan sonra, başka bir başarılı bildirim görürsünüz. Ayrıca, çalışma gönderimini durumunu denetlemek için bildirim düğmesine de tıklayabilirsiniz.
Başarılı bildirimdeki bağlantıya tıklayarak Azure Machine Learning.
![Başarılı bildirim](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00d.png)

### <a name="create-run-with-notebook"></a>Not defteriyle çalışma oluştur

Not defteri oluşturmak için *Not defterinde aç '* ı seçin. Not defterini yürütmek için *Tümünü Çalıştır* ' a tıklayın.
Bu Ayrıca, otomatik ML çalıştırmaya ek ayarlar ekleme olanağı sunar.

![Not defterini aç](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00e.png)

Not defterinden çalıştırma başarıyla gönderildikten sonra, Not defteri çıkışında Azure Machine Learning çalışma alanında çalışan denemenin bağlantısı olacaktır. Azure Machine Learning ' de otomatik ML çalıştırmanızı izlemek için bağlantıya tıklayabilirsiniz.
![Tümünü çalıştıran Not defteri ](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00f.png) )

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure SYNAPSE ADANMıŞ SQL havuzlarında makine öğrenimi modeli Puanlama](tutorial-sql-pool-model-scoring-wizard.md).
- [Hızlı başlangıç: Azure 'da yeni Azure Machine Learning bağlantılı hizmet oluşturma SYNAPSE](quickstart-integrate-azure-machine-learning.md)
- [Azure SYNAPSE Analytics 'teki Machine Learning özellikleri](what-is-machine-learning.md)
