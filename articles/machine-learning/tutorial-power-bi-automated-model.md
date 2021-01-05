---
title: "Öğretici: otomatik ML 'yi kullanarak tahmine dayalı model oluşturma (Bölüm 1/2)"
titleSuffix: Azure Machine Learning
description: Microsoft Power BI sonuçları tahmin etmek için en iyi modeli kullanabilmeniz için otomatik makine öğrenimi modelleri derleyip dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: 6dc99d58f15653e9d3f991622de3bb3388690459
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814814"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-by-using-automated-machine-learning-part-1-of-2"></a>Öğretici: Power BI tümleştirme-otomatik makine öğrenimi kullanarak tahmine dayalı model oluşturma (Bölüm 1/2)

Bu öğreticinin 1. bölümünde, tahmine dayalı bir makine öğrenimi modeli eğitin ve dağıtırsınız. Azure Machine Learning Studio içinde otomatik makine öğrenimi (ML) kullanıyorsunuz.  2. bölümde, Microsoft Power BI sonuçları tahmin etmek için en iyi performanslı modeli kullanacaksınız.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure Machine Learning işlem kümesi oluşturun.
> * Bir veri kümesi oluşturun.
> * Otomatik makine öğrenimi çalıştırması oluşturun.
> * En iyi modeli gerçek zamanlı bir Puanlama uç noktasına dağıtın.


Power BI ' de kullanacağınız modeli oluşturmanın ve dağıtmanın üç yolu vardır.  Bu makalede, "seçenek C: Studio 'da otomatik makine öğrenimi kullanarak modelleri eğitme ve dağıtma" seçeneği ele alınmaktadır.  Bu seçenek, kod içermeyen bir yazma deneyimidir. Veri hazırlama ve model eğitimini tamamen otomatikleştirir. 

Ancak bunun yerine diğer seçeneklerden birini de kullanabilirsiniz:

* [Seçenek A: Jupyter not defterlerini kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-custom-model.md). Bu kod ilk yazma deneyimi Azure Machine Learning Studio barındırılan Jupyıter not defterlerini kullanır.
* [Seçenek B: Azure Machine Learning tasarımcısını kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-designer-model.md). Bu düşük kod yazma deneyimi bir sürükle ve bırak Kullanıcı arabirimi kullanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://aka.ms/AMLFree)kullanabilirsiniz. 
- Azure Machine Learning çalışma alanı. Zaten bir çalışma alanınız yoksa, bkz. [Azure Machine Learning çalışma alanları oluşturma ve yönetme](./how-to-manage-workspace.md#create-a-workspace).

## <a name="create-a-compute-cluster"></a>İşlem kümesi oluşturma

Otomatik makine öğrenimi, "en iyi" algoritmayı ve parametreleri bulmak için birçok makine öğrenimi modeli sağlar. Azure Machine Learning, model eğitiminin bir işlem kümesi üzerinde çalışmasını paralelleştirme.

Başlamak için, [Azure Machine Learning Studio](https://ml.azure.com)' de, sol taraftaki menüde **işlem**' i seçin. **İşlem kümeleri** sekmesini açın. Ardından **Yeni**' yi seçin:

:::image type="content" source="media/tutorial-power-bi/create-compute-cluster.png" alt-text="Hesaplama kümesi oluşturmayı gösteren ekran görüntüsü.":::

**İşlem kümesi oluştur** sayfasında:

1. VM boyutunu seçin. Bu öğretici için bir **Standard_D11_v2** makine iyidir.
1. **İleri**’yi seçin.
1. Geçerli bir işlem adı belirtin.
1. **En az düğüm sayısını** şurada tut `0` .
1. **En fazla düğüm sayısını** ile değiştirin `4` .
1. **Oluştur**’u seçin.

Kümenizin **oluşturulmasına** yönelik durumu değişir.

>[!NOTE]
> Yeni kümede 0 düğüm bulunur, bu nedenle hiçbir işlem maliyeti tahakkuk ettirilmiştir. Yalnızca otomatik makine öğrenimi işi çalıştırıldığında maliyetleriniz vardır. Küme, boşta kalma süresi 120 saniye sonra otomatik olarak 0 olarak ölçeklendirilir.


## <a name="create-a-dataset"></a>Veri kümesi oluşturma

Bu öğreticide [diabetes veri kümesini](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html)kullanırsınız. Bu veri kümesi, [Azure açık veri kümelerinde](https://azure.microsoft.com/services/open-datasets/)kullanılabilir.

Veri kümesini oluşturmak için sol taraftaki menüden **veri kümeleri**' ni seçin. Ardından **veri kümesi oluştur**' u seçin. Aşağıdaki seçenekleri görürsünüz:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Yeni bir veri kümesi oluşturmayı gösteren ekran görüntüsü.":::

**Açık veri kümeleri arasından** seçim yapın. Ardından, **veri kümesinden veri kümesi oluştur** sayfasından şunu yapın:

1. *Diabetes* bulmak için arama çubuğunu kullanın.
1. **Örnek: Diabetes** seçin.
1. **İleri**’yi seçin.
1. Veri kümesi *diagklarınızı* adlandırın.
1. **Oluştur**’u seçin.

Verileri araştırmak için veri kümesini seçin ve ardından **keşfet**' i seçin:

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="Bir veri kümesinin nasıl araştıralınacağını gösteren ekran görüntüsü.":::

Veride yaş, Sex, gövde yığın dizini, ortalama kan basıncı ve altı kan serum ölçümleri gibi 10 temel giriş değişkeni vardır. Ayrıca, **Y** adlı bir hedef değişkeni de vardır. Bu hedef değişken, taban çizgisinden sonraki bir yıl ilerleme durumu için bir nicel ölçüdür.

## <a name="create-an-automated-machine-learning-run"></a>Otomatik makine öğrenimi çalıştırması oluşturma

[Azure Machine Learning Studio](https://ml.azure.com)' de, sol taraftaki menüde **Otomatik ml**' yi seçin. Ardından **Yeni OTOMATIK ml çalıştırması**' nı seçin:

:::image type="content" source="media/tutorial-power-bi/create-new-run.png" alt-text="Yeni bir otomatik makine öğrenimi çalıştırmasının nasıl oluşturulacağını gösteren ekran görüntüsü.":::

Ardından, daha önce oluşturduğunuz **diabetes** veri kümesini seçin. Ardından **İleri**' yi seçin:

:::image type="content" source="media/tutorial-power-bi/select-dataset.png" alt-text="Veri kümesinin nasıl seçileceğini gösteren ekran görüntüsü.":::
 
**Çalıştırma Yapılandır** sayfasında:

1. **Deneme adı** altında **Yeni oluştur**' u seçin.
1. Denemeyi adlandırın.
1. **Hedef sütun** alanında **Y**' yi seçin.
1. **İşlem kümesi Seç** alanında, daha önce oluşturduğunuz bilgi işlem kümesini seçin. 

Tamamlanan formunuz şöyle görünmelidir:

:::image type="content" source="media/tutorial-power-bi/configure-automated.png" alt-text="Otomatik makine öğrenimini yapılandırmayı gösteren ekran görüntüsü.":::

Son olarak, bir makine öğrenimi görevi seçin. Bu durumda, görev **gerileme**:

:::image type="content" source="media/tutorial-power-bi/configure-task.png" alt-text="Bir görevin nasıl yapılandırılacağını gösteren ekran görüntüsü.":::

**Son**'u seçin.

> [!IMPORTANT]
> Otomatik makine öğrenimi, 100 modellerinin eğitiminin tamamlanmasıyla yaklaşık 30 dakika sürer.

## <a name="deploy-the-best-model"></a>En iyi modeli dağıtma

Otomatik makine öğrenimi tamamlandığında **modeller** sekmesini seçerek çalıştık tüm makine öğrenimi modellerini görebilirsiniz. Modeller performansa göre sıralanır; en iyi performanslı model ilk olarak gösterilir. En iyi modeli seçtikten sonra **Dağıt** düğmesi etkinleştirilir:

:::image type="content" source="media/tutorial-power-bi/list-models.png" alt-text="Modellerin listesini gösteren ekran görüntüsü.":::

Bir **model dağıt** penceresi açmak için **Dağıt** ' ı seçin:

1. Model hizmeti *diabetes modellerinizi* adlandırın.
1. **Azure Container Service** seçin.
1. **Dağıt**'ı seçin.

Modelin başarıyla dağıtıldığını belirten bir ileti görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, otomatik makine öğrenimi kullanarak makine öğrenimi modelinin nasıl eğilmesi ve dağıtılması gerektiğini gördünüz. Sonraki öğreticide bu modeli Power BI kullanmayı (puan) öğreneceksiniz.

> [!div class="nextstepaction"]
> [Öğretici: Power BI model kullanma](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
