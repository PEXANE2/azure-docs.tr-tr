---
title: "Öğretici: otomatik ML 'yi kullanarak tahmine dayalı model oluşturma (Bölüm 1/2)"
titleSuffix: Azure Machine Learning
description: Microsoft Power BI sonuçları tahmin etmek için en iyi modeli kullanabilmeniz için otomatik ML modelleri oluşturmayı ve dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: 897f493edf6ccdebb25c201e8e4f9babfb0754c5
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370351"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-using-automated-machine-learning-part-1-of-2"></a>Öğretici: Power BI tümleştirme-otomatik makine öğrenimini kullanarak tahmine dayalı model oluşturma (Bölüm 1/2)

Bu öğreticinin ilk bölümünde, Azure Machine Learning Studio 'da otomatik makine öğrenimini kullanarak tahmine dayalı bir makine öğrenimi modeli eğitin ve dağıtırsınız.  2. Bölüm 'de, Microsoft Power BI sonuçları tahmin etmek için en iyi gerçekleştiriliyor modelini kullanacaksınız.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure Machine Learning işlem kümesi oluşturma
> * Veri kümesi oluşturma
> * Otomatik ML çalıştırması oluşturma
> * En iyi modeli gerçek zamanlı bir Puanlama uç noktasına dağıtın


Power BI ' de kullanacağınız modeli oluşturmanın ve dağıtmanın üç farklı yolu vardır.  Bu makale, Studio 'daki otomatikleştirilmiş ML 'yi kullanarak modelleri eğitme ve dağıtma seçeneğini içerir.  Bu seçenek, veri hazırlama ve model eğitimini tamamen otomatikleştiren kod içermeyen bir yazma deneyimi gösterir. 

Bunun yerine şunları kullanabilirsiniz:

* [Seçenek A: not defterlerini kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-custom-model.md) -Azure Machine Learning Studio 'Da barındırılan Jupyter not defterlerini kullanan kod ilk yazma deneyimi.
* Seçenek B: tasarımcı kullanarak (bir sürükle ve bırak Kullanıcı arabirimi) düşük kodlu bir yazma deneyimi [kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-designer-model.md).

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure aboneliği ([ücretsiz deneme sürümü mevcuttur](https://aka.ms/AMLFree)). 
- Azure Machine Learning çalışma alanı. Zaten bir çalışma alanınız yoksa [Azure Machine Learning çalışma alanı oluşturma](./how-to-manage-workspace.md#create-a-workspace)' yı izleyin.

## <a name="create-compute-cluster"></a>İşlem kümesi oluştur

Otomatik ML, "en iyi" algoritmayı ve parametreleri bulmak için farklı makine öğrenimi modellerini otomatik olarak ister. Azure Machine Learning, model eğitiminin bir işlem kümesi üzerinden yürütülmesini paralelleştirme.

[Azure Machine Learning Studio](https://ml.azure.com), sol taraftaki menüden, ardından **Işlem kümeleri** sekmesinden **işlem** ' i seçin. **Yeni**' yi seçin:

:::image type="content" source="media/tutorial-power-bi/create-compute-cluster.png" alt-text="İşlem kümesi oluşturmayı gösteren ekran görüntüsü":::

**İşlem kümesi oluştur** ekranında:

1. Bir VM boyutu seçin (Bu öğreticinin bir `Standard_D11_v2` makineye ince olması için).
1. **İleri**’yi seçin
1. Geçerli bir işlem adı sağlayın
1. **En az düğüm sayısını** 0 ' da tut
1. **En fazla düğüm sayısını** 4 olarak değiştirin
1. **Oluştur**’u seçin

Kümenizin durumunun **oluşturma** olarak değiştiğini görebilirsiniz.

>[!NOTE]
> Küme oluşturulduğunda, hiçbir işlem maliyeti tahakkuk etmediği anlamına gelen 0 düğüme sahip olur. Yalnızca otomatikleştirilmiş ML işi çalıştırıldığında ücretlendirilirsiniz. Küme, boşta kalma süresi 120 saniye sonra otomatik olarak 0 olarak ölçeklenir.


## <a name="create-dataset"></a>Veri kümesi oluşturma

Bu öğreticide, [Azure açık veri kümelerinde](https://azure.microsoft.com/services/open-datasets/)kullanılabilir hale getirilen [diabetes veri kümesini](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html)kullanırsınız.

Veri kümesini oluşturmak **için veri kümesi sol menüsünü** ve ardından **veri kümesi oluştur** ' u seçin. aşağıdaki seçenekleri görürsünüz:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Yeni bir veri kümesi oluşturmayı gösteren ekran görüntüsü":::

**Açık veri kümeleri ' nden** seçim yapın ve sonra **açık veri kümelerinde veri kümesi oluştur** ekranında:

1. Arama çubuğunu kullanarak *diabtes* arama
1. **Örnek seçin: Diabetes**
1. **İleri**’yi seçin
1. Veri kümeniz için bir ad sağlayın- *diabetes*
1. **Oluştur**’u seçin

Veri kümesini ve ardından **keşfet**' i seçerek verileri keşfedebilirsiniz:

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="Veri kümesinin nasıl araştıralınacağını gösteren ekran görüntüsü":::

Veriler 10 temel giriş değişkenine sahiptir (yaş, Sex, gövde yığın dizini, ortalama kan basıncı ve altı kan serum ölçümleri gibi) ve **Y** adlı bir hedef değişken (taban çizgisinden sonra bir yılda bir yıl ilerleme durumu ölçüsü).

## <a name="create-automated-ml-run"></a>Otomatik ML çalıştırması oluştur

[Azure Machine Learning Studio](https://ml.azure.com) sol taraftaki menüden **Otomatik ml** ' yi ve ardından **Yeni otomatik ml Çalıştır**' ı seçin:

:::image type="content" source="media/tutorial-power-bi/create-new-run.png" alt-text="Yeni bir otomatik ML çalıştırması oluşturmayı gösteren ekran görüntüsü":::

Ardından, daha önce oluşturduğunuz **diabetes** veri kümesini seçin ve **İleri**' yi seçin:

:::image type="content" source="media/tutorial-power-bi/select-dataset.png" alt-text="Veri kümesinin nasıl seçileceğini gösteren ekran görüntüsü":::
 
**Çalıştırmayı Yapılandır** ekranında:

1. Deneme adı altında **Yeni oluştur** **' u** seçin.
1. Bir deneme adı girin
1. Hedef sütun alanında **Y** ' yi seçin.
1. **Hesaplama kümesi seçin** alanında, daha önce oluşturduğunuz işlem kümesini seçin. 

Tamamlanan formunuz şuna benzemelidir:

:::image type="content" source="media/tutorial-power-bi/configure-automated.png" alt-text="Otomatik ML 'nin nasıl yapılandırılacağını gösteren ekran görüntüsü":::

Son olarak, gerçekleştirilecek makine öğrenimi görevini seçmeniz gerekir, bu da **gerileme**:

:::image type="content" source="media/tutorial-power-bi/configure-task.png" alt-text="Görevin nasıl yapılandırılacağını gösteren ekran görüntüsü":::

**Son**'u seçin.

> [!IMPORTANT]
> Otomatik ML 'nin 100 farklı modellere eğitim tamamlaması için 30 dakika sürer.

## <a name="deploy-the-best-model"></a>En iyi modeli dağıtma

Otomatik ML çalıştırması tamamlandıktan sonra **modeller** sekmesini seçerek çalıştık tüm farklı makine öğrenimi modellerinin listesini görebilirsiniz. Modeller performans sırasına göre sıralanır. en iyi şekilde uygulanan model ilk olarak gösterilir. En iyi modeli seçtiğinizde **Dağıt** düğmesi etkinleştirilir:

:::image type="content" source="media/tutorial-power-bi/list-models.png" alt-text="Modellerin listesini gösteren ekran görüntüsü":::

**Dağıt**' ı seçtiğinizde **model dağıt** ekranı bulunur:

1. Model hizmetiniz için bir ad sağlayın- **diabetes-model** kullanın
1. **Azure Container Service** seçin
1. **Dağıt** 'ı seçin

Modelin başarıyla dağıtıldığını belirten bir ileti görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, otomatik ML kullanarak makine öğrenimi modelinin nasıl eğilmesi ve dağıtılması gerektiğini gördünüz. Sonraki öğreticide bu modelin Power BI nasıl kullanılacağına (puan) gösterilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Power BI modeli kullanın](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
