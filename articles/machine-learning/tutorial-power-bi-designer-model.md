---
title: 'Öğretici: tahmine dayalı model oluşturmak için sürükleyip bırakın (Bölüm 1/2)'
titleSuffix: Azure Machine Learning
description: Tasarımcıyı kullanarak makine öğrenimi tahmine dayalı model oluşturma ve dağıtma hakkında bilgi edinin. Daha sonra, Microsoft Power BI sonuçları tahmin etmek için bunu kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.custom: designer
ms.openlocfilehash: 995979c7fe100637aa8e241489805fb09d6723f7
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814797"
---
# <a name="tutorial-power-bi-integration---drag-and-drop-to-create-the-predictive-model-part-1-of-2"></a>Öğretici: Power BI tümleştirme-tahmine dayalı model oluşturmak için sürükleyip bırakın (Bölüm 1/2)

Bu öğreticinin 1. bölümünde, tahmine dayalı bir makine öğrenimi modelini Azure Machine Learning tasarımcısını kullanarak eğitmeniz ve dağıtmanız gerekir. Tasarımcı, düşük kodlu bir sürükle ve bırak kullanıcı arabirimidir. 2. bölümde, Microsoft Power BI sonuçları tahmin etmek için modeli kullanacaksınız.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure Machine Learning işlem örneği oluşturun.
> * Azure Machine Learning çıkarım kümesi oluşturun.
> * Bir veri kümesi oluşturun.
> * Regresyon modeli eğitme.
> * Modeli gerçek zamanlı bir Puanlama uç noktasına dağıtın.


Power BI ' de kullanacağınız modeli oluşturmanın ve dağıtmanın üç yolu vardır.  Bu makalede "seçenek B: Tasarımcıyı kullanarak modelleri eğitme ve dağıtma" konuları ele alınmaktadır.  Bu seçenek, tasarımcı arabirimini kullanan bir düşük kodlu yazma deneyimidir.  

Ancak bunun yerine diğer seçeneklerden birini de kullanabilirsiniz:

* [Seçenek A: Jupyter not defterlerini kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-custom-model.md). Bu kod ilk yazma deneyimi Azure Machine Learning Studio barındırılan Jupyıter not defterlerini kullanır.
* [Seçenek C: otomatik makine öğrenimini kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-automated-model.md). Bu kod içermeyen yazma deneyimi, veri hazırlama ve model eğitimini tamamen otomatikleştirir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://aka.ms/AMLFree)kullanabilirsiniz. 
- Azure Machine Learning çalışma alanı. Zaten bir çalışma alanınız yoksa, bkz. [Azure Machine Learning çalışma alanları oluşturma ve yönetme](./how-to-manage-workspace.md#create-a-workspace).
- Machine Learning iş akışlarının giriş bilgisi.


## <a name="create-compute-to-train-and-score"></a>Eğitim ve skor için işlem oluşturma

Bu bölümde bir *işlem örneği* oluşturursunuz. İşlem örnekleri makine öğrenimi modellerini eğitmek için kullanılır. Ayrıca, dağıtılan modeli gerçek zamanlı Puanlama için barındırmak üzere bir *çıkarım kümesi* oluşturursunuz.

[Azure Machine Learning Studio](https://ml.azure.com)oturum açın. Soldaki menüde **işlem** ' i ve sonra **Yeni**' yi seçin:

:::image type="content" source="media/tutorial-power-bi/create-new-compute.png" alt-text="İşlem örneğinin nasıl oluşturulacağını gösteren ekran görüntüsü.":::

**İşlem örneği oluştur** sayfasında, bir VM boyutu seçin. Bu öğretici için bir **Standard_D11_v2** VM seçin. Sonra **İleri**’yi seçin. 

**Ayarlar** sayfasında, işlem örneğinizi adlandırın. Ardından **Oluştur**’u seçin. 

>[!TIP]
> Ayrıca, not defterlerini oluşturmak ve çalıştırmak için işlem örneğini kullanabilirsiniz.

İşlem örneği **durumunuz** şimdi **oluşturuluyor**. Makinenin sağlanması yaklaşık 4 dakika sürer. 

Beklerken, **işlem** sayfasında, **çıkarım kümeleri** sekmesini seçin. Ardından **Yeni**' yi seçin:

:::image type="content" source="media/tutorial-power-bi/create-cluster.png" alt-text="Çıkarım kümesi oluşturmayı gösteren ekran görüntüsü.":::

**Çıkarım kümesi oluştur** sayfasında bir bölge ve VM boyutu seçin. Bu öğretici için bir **Standard_D11_v2** VM seçin. Sonra **İleri**’yi seçin. 

**Ayarları Yapılandır** sayfasında:

1. Geçerli bir işlem adı belirtin.
1. Küme amacı olarak **geliştirme-test** ' i seçin. Bu seçenek dağıtılan modeli barındırmak için tek bir düğüm oluşturur.
1. **Oluştur**’u seçin.

Çıkarım kümenizin **durumu** şimdi **oluşturuluyor**. Tek düğümlü kümenizin dağıtılması 4 dakika sürer.

## <a name="create-a-dataset"></a>Veri kümesi oluşturma

Bu öğreticide [diabetes veri kümesini](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html)kullanırsınız. Bu veri kümesi, [Azure açık veri kümelerinde](https://azure.microsoft.com/services/open-datasets/)kullanılabilir.

Veri kümesini oluşturmak için sol taraftaki menüden **veri kümeleri**' ni seçin. Ardından **veri kümesi oluştur**' u seçin. Aşağıdaki seçenekleri görürsünüz:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Yeni bir veri kümesi oluşturmayı gösteren ekran görüntüsü.":::

**Açık veri kümeleri arasından** seçim yapın. **Açık veri kümelerinden veri kümesi oluştur** sayfasında:

1. *Diabetes* bulmak için arama çubuğunu kullanın.
1. **Örnek: Diabetes** seçin.
1. **İleri**’yi seçin.
1. Veri kümesi *diagklarınızı* adlandırın.
1. **Oluştur**’u seçin.

Verileri araştırmak için veri kümesini seçin ve ardından **keşfet**' i seçin:

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="Bir veri kümesinin nasıl araştıralınacağını gösteren ekran görüntüsü.":::

Veride yaş, Sex, gövde yığın dizini, ortalama kan basıncı ve altı kan serum ölçümleri gibi 10 temel giriş değişkeni vardır. Ayrıca, **Y** adlı bir hedef değişkeni de vardır. Bu hedef değişken, taban çizgisinden sonraki bir yıl ilerleme durumu için bir nicel ölçüdür.

## <a name="create-a-machine-learning-model-by-using-the-designer"></a>Tasarımcıyı kullanarak makine öğrenimi modeli oluşturma

İşlem ve veri kümelerini oluşturduktan sonra, makine öğrenimi modelini oluşturmak için tasarımcıyı kullanabilirsiniz. Azure Machine Learning Studio, **Tasarımcı** ' yı ve ardından **Yeni işlem hattı**' nı seçin:

:::image type="content" source="media/tutorial-power-bi/create-designer.png" alt-text="Yeni bir işlem hattının nasıl oluşturulacağını gösteren ekran görüntüsü.":::

Boş bir *tuval* ve bir **Ayarlar** menüsü görürsünüz:

:::image type="content" source="media/tutorial-power-bi/select-compute.png" alt-text="İşlem hedefinin nasıl seçileceğini gösteren ekran görüntüsü.":::

**Ayarlar** menüsünde, **işlem hedefini seç**' i seçin. Daha önce oluşturduğunuz işlem örneğini seçin ve ardından **Kaydet**' i seçin. **Taslak adını** *diabetes modeli* gibi daha kolay bir şekilde değiştirin. Son olarak, bir açıklama girin.

Varlıklar listesinde, **veri kümeleri** ' ni genişletin ve **diabetes** veri kümesini bulun. Bu bileşeni tuvale sürükleyin:

:::image type="content" source="media/tutorial-power-bi/drag-component.png" alt-text="Bir bileşenin tuvale nasıl sürükleyileceğini gösteren ekran görüntüsü.":::

Sonra, aşağıdaki bileşenleri tuvale sürükleyin:

1. **Doğrusal regresyon** ( **Machine Learning algoritmalarda** bulunur)
1. **Modeli eğitme** ( **model eğitiminde** bulunur)

Tuvalinizde, bileşenlerin üst ve altındaki çemberler olduğunu fark edin. Bu daireler bağlantı noktalardır.

:::image type="content" source="media/tutorial-power-bi/connections.png" alt-text="Bağlı olmayan bileşenlerin bağlantı noktalarını gösteren ekran görüntüsü.":::
 
Şimdi bileşenleri birbirine *bağlayın* . **Diabetes** veri kümesinin altındaki bağlantı noktasını seçin. Bunu **eğitme modeli** bileşeninin sağ üst tarafındaki bağlantı noktasına sürükleyin. **Doğrusal regresyon** bileşeninin altındaki bağlantı noktasını seçin. Bunu **eğitme modeli** bileşeninin sol üst tarafındaki bağlantı noktasına sürükleyin.

Tahmin edilecek etiket (hedef) değişkeni olarak kullanılacak veri kümesi sütununu seçin. **Modeli eğitme** bileşenini seçin ve ardından **sütunu Düzenle**' yi seçin. 

İletişim kutusunda, **sütun adı Y girin**' i seçin  >  :

:::image type="content" source="media/tutorial-power-bi/label-columns.png" alt-text="Etiket sütununun nasıl seçileceğini gösteren ekran görüntüsü.":::

**Kaydet**’i seçin. Machine Learning *iş akışınız* şu şekilde görünmelidir:

:::image type="content" source="media/tutorial-power-bi/connected-diagram.png" alt-text="Bağlı bileşenleri gösteren ekran görüntüsü.":::

**Gönder**’i seçin. **Deneme** bölümünde **Yeni oluştur**' u seçin. Denemeyi adlandırın ve ardından **Gönder**' i seçin.

>[!NOTE]
> Denemenizin ilk çalışması 5 dakika sürer. Tasarımcı gecikme süresini azaltmak için çalıştırılan bileşenleri önbelleğe aldığından sonraki çalıştırmalar çok daha hızlıdır.

Deneme tamamlandığında şu görünümü görürsünüz:

:::image type="content" source="media/tutorial-power-bi/completed-run.png" alt-text="Tamamlanan bir çalışmayı gösteren ekran görüntüsü.":::

Deneme günlüklerini denetlemek için **modeli eğitme** ' yi seçin ve ardından **çıktılar + Günlükler**' i seçin.

## <a name="deploy-the-model"></a>Modeli dağıtma

Modeli dağıtmak için tuvalin üst kısmında, **çıkarım ardışık düzeni oluştur**  >  **gerçek zamanlı çıkarım işlem hattı**' nı seçin:

:::image type="content" source="media/tutorial-power-bi/pipeline.png" alt-text="Gerçek zamanlı bir çıkarım işlem hattının nerede seçileceğini gösteren ekran görüntüsü.":::
 
İşlem hattı, yalnızca modeli Puanlama için gereken bileşenlere göre daha fazla. Verileri puanlandırdığınızda, hedef değişken değerlerini bilemezsiniz. Böylece, veri kümesinden **Y** 'yi kaldırabilirsiniz. 

**Y**'yi kaldırmak için, **tuvalde DataSet bileşenine bir SELECT sütunları** ekleyin. Bileşene tel veri kümesi girişi olacak şekilde bileşen bağlayın. Sonuçlar **puan modeli** bileşeninin çıktılardır:

:::image type="content" source="media/tutorial-power-bi/remove-column.png" alt-text="Bir sütunun nasıl kaldırılacağını gösteren ekran görüntüsü.":::

Tuvalde, **veri kümesi bileşeninde sütunları seç** ' i seçin ve ardından **Sütunları Düzenle**' yi seçin. 

**Sütunları seçin** iletişim kutusunda **ada göre**' yi seçin. Tüm giriş değişkenlerinin seçili olmasına karşın hedefin seçili *olmadığından* emin olun:

:::image type="content" source="media/tutorial-power-bi/removal-settings.png" alt-text="Sütun ayarlarının nasıl kaldırılacağını gösteren ekran görüntüsü.":::

**Kaydet**’i seçin. 

Son olarak **model puan** bileşenini seçin ve **çıkışa puan sütunları ekle** onay kutusunun temizlenmiş olduğundan emin olun. Gecikmeyi azaltmak için, tahminler, girişler olmadan geri gönderilir.

:::image type="content" source="media/tutorial-power-bi/score-settings.png" alt-text="Puan modeli bileşeninin ayarlarını gösteren ekran görüntüsü.":::

Tuvalin üst kısmında **Gönder**' i seçin.

Çıkarım ardışık düzenini başarıyla çalıştırdıktan sonra, modeli çıkarım kümenize dağıtabilirsiniz. **Dağıt**'ı seçin. 

Gerçek zamanlı **uç nokta ayarla** iletişim kutusunda **yeni gerçek zamanlı uç nokta dağıt**' ı seçin. Uç nokta *My-diabetes-model* olarak adlandırın. Daha önce oluşturduğunuz çıkarımı seçin ve ardından **Dağıt**' ı seçin:

:::image type="content" source="media/tutorial-power-bi/endpoint-settings.png" alt-text="Gerçek zamanlı uç nokta ayarlarını gösteren ekran görüntüsü.":::
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, tasarımcı modelinin nasıl eğiteini ve dağıtılacağını gördünüz. Sonraki bölümde, Power BI bu modeli kullanmayı (puan) öğreneceksiniz.

> [!div class="nextstepaction"]
> [Öğretici: Power BI model kullanma](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
