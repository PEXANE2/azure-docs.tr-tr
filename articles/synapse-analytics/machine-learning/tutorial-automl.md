---
title: 'Öğretici: oto ml kullanarak Machine Learning modeli eğitimi'
description: Azure SYNAPSE 'de bir makine öğrenimi modelini oto ml kullanarak eğitme öğreticisi.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 8dd99b60a548e3c392bbe468ddde484081e6eb8b
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468667"
---
# <a name="tutorial-code-free-machine-learning-model-training-in-azure-synapse-with-automl-preview"></a>Öğretici: Azure SYNAPSE 'de oto ml ile kod ücretsiz makine öğrenimi modeli eğitimi (Önizleme)

[Azure Machine Learning ' de oto ml](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml)kullanarak eğitediğiniz yeni makine öğrenimi modelleriyle verileri Spark tablolarında kolayca zenginleştirme hakkında bilgi edinin.  SYNAPSE ' deki bir Kullanıcı, bir kod ücretsiz deneyimde makine öğrenimi modelleri oluşturmak için eğitim veri kümesi olarak kullanmak üzere Azure SYNAPSE çalışma alanında bir Spark tablosu seçebilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> - Azure ML 'de otomatik ML kullanan Azure SYNAPSE Studio 'da bir kod ücretsiz deneyimi kullanarak makine öğrenimi modellerini eğitme. Eğeceğiniz modelin türü, çözmeye çalıştığınız soruna bağlıdır.

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

## <a name="launch-automl-wizard-to-train-a-model"></a>Bir modeli eğitme için oto ml Sihirbazı 'nı başlatın

Önceki adımda oluşturulan Spark tablosuna sağ tıklayın. Sihirbazı açmak için "Machine Learning-> Enrich New model" seçeneğini belirleyin.
![Oto ml Sihirbazını Başlat](media/tutorial-automl-wizard/tutorial-automl-wizard-00d.png)

Bir yapılandırma paneli görünür ve Azure Machine Learning ' de bir oto ml denemesi oluşturmak için yapılandırma ayrıntıları sağlamanız istenir. Bu çalıştırma birden çok modeli eğitecektir ve başarılı bir çalıştırmanın en iyi modeli Azure ML modeli kayıt defterine kaydedilecek:

![Run adım yapılandırma](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00a.png)

- **Azure ML çalışma alanı**: oto ml deneme çalıştırmasının oluşturulması Için BIR Azure ML çalışma alanı gereklidir. Ayrıca, [bağlantılı bir hizmet](quickstart-integrate-azure-machine-learning.md)kullanarak Azure SYNAPSE çalışma ALANıNıZı Azure ML çalışma alanıyla bağlamanız gerekir. Tüm öncesi = gereksinimleri aldıktan sonra, bu otomatik ml çalıştırması için kullanmak istediğiniz Azure ML çalışma alanını belirtebilirsiniz.

- **Deneme adı**: deneme adını belirtin. Bir oto ml çalıştırması gönderdiğinizde, bir deneme adı sağlarsınız. Çalıştırma bilgileri, Azure ML çalışma alanındaki bu deneme altında depolanır. Bu deneyim, varsayılan olarak yeni bir deneme oluşturacak ve önerilen bir ad oluşturuyor, ancak aynı zamanda var olan bir deneyime de bir ad verebilirsiniz.

- **En iyi model**: oto ml çalıştırmasının en iyi modelinin adını belirtin. En iyi modele bu ad verilir ve bu çalıştırma sonrasında Azure ML modeli kayıt defterine otomatik olarak kaydedilir. Bir oto ml çalıştırması birçok makine öğrenme modeli oluşturacaktır. Sonraki adımda seçtiğiniz birincil ölçüme bağlı olarak, bu modeller karşılaştırılabilir ve en iyi model seçilebilir.

- **Hedef sütun**: modelin tahmin etmek için eğitilme işlemi budur. Tahmin etmek istediğiniz sütunu seçin.

- **Spark havuzu**: oto ml denemesi çalıştırması için kullanmak istediğiniz Spark Havuzu. Hesaplamalar belirttiğiniz havuzda yürütülür.

- **Spark yapılandırma ayrıntıları**: Spark havuzuna ek olarak, oturum yapılandırma ayrıntılarını sağlama seçeneğiniz de vardır.

Bu öğreticide, hedef sütun olarak sayısal sütunu seçtik `fareAmount` .

"Devam" a tıklayın.

## <a name="choose-task-type"></a>Görev türünü seçin

Yanıtlamaya çalıştığınız soruya göre deneme için makine öğrenimi model türünü seçin. `fareAmount`Hedef sütun olarak seçtiğimiz ve sayısal bir değer olduğundan *gerileme*' ı seçeceğiz.

Ek ayarları yapılandırma için "devam" ı tıklatın.

![Görev türü seçimi](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00b.png)

## <a name="additional-configurations"></a>Ek yapılandırmalar

*Sınıflandırma* veya *regresyon* türü ' nü seçerseniz, ek konfigürasyonlar şunlardır:

- **Birincil ölçüm**: modelin ne kadar iyi yaptığını ölçmek için kullanılan ölçüm. Bu, oto ml çalıştırmasında oluşturulan farklı modelleri karşılaştırmak için kullanılacak ölçüdür ve hangi modelin en iyi şekilde gerçekleştirileceğini tespit eder.

- **Eğitim işi süresi (saat)**: modelleri çalıştırma ve eğitme denemesi için en uzun süre (saat cinsinden). 1 ' den küçük değerler de sağlayabileceğinizi unutmayın. Örneğin, `0.5`.

- **Maksimum eşzamanlı yineleme**: paralel olarak yürütülecek en fazla yineleme sayısını temsil eder.

- **Onnx model uyumluluğu**: etkinleştirilirse, oto ml tarafından eğitilen modeller onnx biçimine dönüştürülür. Bu, özellikle Azure SYNAPSE SQL havuzlarında Puanlama için model kullanmak istiyorsanız geçerlidir.

Bu ayarların hepsi, özelleştirebileceğiniz bir varsayılan değere sahiptir.
![ek yapılandırma](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00c.png)

> "Zaman serisi tahmini" seçeneğini belirlediğinizde, daha fazla yapılandırma gerektiğini unutmayın. Tahmin Ayrıca ONNX model uyumluluğunu desteklemez.

Tüm gerekli yapılandırma yapıldıktan sonra, oto ml çalıştırmayı başlatabilirsiniz.

Azure Azure 'da bir oto ml çalıştırması başlatmak için iki yol vardır. Kod ücretsiz bir deneyim için doğrudan **çalıştırma oluşturmayı** seçebilirsiniz. Kodu tercih ediyorsanız, çalıştırma ve Not defterini çalıştırma kodunu görmenizi sağlayan **Not defterinde aç**' ı seçebilirsiniz.

### <a name="create-run-directly"></a>Doğrudan çalıştırma oluştur

Doğrudan Çalıştır 'ı başlatmak için "çalıştırmaya başla" ya tıklayın. Otomatik ml çalıştırmasının başladığını gösteren bir bildirim olacaktır.

Oto çalıştırma başarıyla başlatıldıktan sonra, başka bir başarılı bildirim görürsünüz. Ayrıca, çalışma gönderimini durumunu denetlemek için bildirim düğmesine de tıklayabilirsiniz.
Başarılı bildirimde bulunan bağlantıya tıklayarak Azure ML.
![Başarılı bildirim](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00d.png)

### <a name="create-run-with-notebook"></a>Not defteriyle çalışma oluştur

Not defteri oluşturmak için *Not defterinde aç '* ı seçin. Not defterini yürütmek için *Tümünü Çalıştır* ' a tıklayın.
Bu Ayrıca, diğer ayarları, sizin de oto ml çalıştırmaya ekleme olanağı sunar.

![Not defterini aç](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00e.png)

Not defterinden çalıştırma başarıyla gönderildikten sonra, Not defteri çıkışında Azure ML çalışma alanında çalıştırılan denemenin bir bağlantısı olacaktır. Azure ML 'de, oto ml çalıştırmanızı izlemek için bağlantıya tıklayabilirsiniz.
![Tümünü çalıştıran Not defteri ](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00f.png) )

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure SYNAPSE ADANMıŞ SQL havuzlarında makine öğrenimi modeli Puanlama](tutorial-sql-pool-model-scoring-wizard.md).
- [Hızlı başlangıç: Azure 'da yeni Azure Machine Learning bağlantılı hizmet oluşturma SYNAPSE](quickstart-integrate-azure-machine-learning.md)
- [Azure SYNAPSE Analytics 'teki Machine Learning özellikleri](what-is-machine-learning.md)