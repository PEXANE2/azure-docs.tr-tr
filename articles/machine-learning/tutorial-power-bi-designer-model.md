---
title: 'Öğretici: tahmine dayalı model oluşturmak için sürükle ve bırak (Bölüm 1/2)'
titleSuffix: Azure Machine Learning
description: Tasarımcı ile makine öğrenimi tahmine dayalı bir model derleyip dağıtmayı öğrenin ve bu sayede Microsoft Power BI sonuçları tahmin etmek için kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: f0e1ffe60069a2379f8eddab1aae74db2b4eac50
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370297"
---
# <a name="tutorial--power-bi-integration---drag-and-drop-to-create-the-predictive-model-part-1-of-2"></a>Öğretici: tahmine dayalı model oluşturmak için Power BI tümleştirme-sürükleyip bırakma (Bölüm 1/2)

Bu öğreticinin ilk bölümünde, bir tahmine dayalı makine öğrenimi modelini Azure Machine Learning tasarımcısını kullanarak eğitmenizi ve dağıtırsınız; düşük kodlu bir sürükle ve bırak Kullanıcı arabirimi. 2. bölümde, Microsoft Power BI sonuçları tahmin etmek için modeli kullanacaksınız.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure Machine Learning işlem örneği oluşturma
> * Azure Machine Learning çıkarım kümesi oluşturma
> * Veri kümesi oluşturma
> * Regresyon modelini eğitme
> * Modeli gerçek zamanlı bir Puanlama uç noktasına dağıtma


Power BI ' de kullanacağınız modeli oluşturmanın ve dağıtmanın üç farklı yolu vardır.  Bu makale, tasarımcı kullanarak modelleri eğitme ve dağıtma seçeneğini içerir.  Bu seçenek, tasarımcı (bir sürükle ve bırak Kullanıcı arabirimi) kullanarak düşük kodlu bir yazma deneyimi gösterir.  

Bunun yerine şunları kullanabilirsiniz:

* [Seçenek A: not defterlerini kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-custom-model.md) -Azure Machine Learning Studio 'Da barındırılan Jupyter not defterlerini kullanan kod ilk yazma deneyimi.
* [Seçenek C: OTOMATIK ml kullanarak modelleri eğitme ve dağıtma](tutorial-power-bi-automated-model.md) -kod içermeyen bir yazma deneyimi, veri hazırlama ve model eğitimini tamamen otomatikleştirir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure aboneliği ([ücretsiz deneme sürümü mevcuttur](https://aka.ms/AMLFree)). 
- Azure Machine Learning çalışma alanı. Zaten bir çalışma alanınız yoksa [Azure Machine Learning çalışma alanı oluşturma](./how-to-manage-workspace.md#create-a-workspace)' yı izleyin.
- Machine Learning iş akışlarının giriş bilgisi.


## <a name="create-compute-for-training-and-scoring"></a>Eğitim ve Puanlama için işlem oluşturma

Bu bölümde, eğitim makine öğrenimi modelleri için kullanılan bir *işlem örneği* oluşturursunuz. Ayrıca, gerçek zamanlı Puanlama için dağıtılan modeli barındırmak üzere kullanılacak bir *çıkarım kümesi* oluşturursunuz.

[Azure Machine Learning Studio](https://ml.azure.com) 'da oturum açın ve sol taraftaki menüden **işlem** ' i seçin ve ardından **Yeni**' yi seçin:

:::image type="content" source="media/tutorial-power-bi/create-new-compute.png" alt-text="İşlem örneğinin nasıl oluşturulacağını gösteren ekran görüntüsü":::

Elde edilen **işlem örneği oluşturma** ekranında, bir VM boyutu seçin (Bu öğretici için, bir seçin ve `Standard_D11_v2` ardından **İleri**' ye tıklayın. Ayar sayfasında, işlem örneğiniz için, ardından **Oluştur**' u seçerek geçerli bir ad sağlayın. 

>[!TIP]
> İşlem örneği, not defterlerini oluşturmak ve yürütmek için de kullanılabilir.

Artık işlem örneği **durumunuzu** **görebilirsiniz; makinenin** sağlanması yaklaşık 4 dakika sürer. Beklerken, işlem sayfasında, **Yeni**' nin ardından gelen **çıkarım kümesi** sekmesini seçin:

:::image type="content" source="media/tutorial-power-bi/create-cluster.png" alt-text="Çıkarım kümesi oluşturmayı gösteren ekran görüntüsü":::

Sonuçta ortaya çıkan **çıkarım kümesi oluştur** sayfasında, bir bölge (Bu öğretici için bir seçin `Standard_D11_v2` ) ve ardından **İleri**' yi seçin. **Ayarları Yapılandır** sayfasında:

1. Geçerli bir işlem adı sağlayın
1. Küme amacı olarak **geliştirme-test** ' i seçin (dağıtılan modeli barındırmak için tek bir düğüm oluşturur)
1. **Oluştur**’u seçin

Artık, çıkarım kümenizin **durumunun** **oluşturuyor** olduğunu görebilirsiniz; tek düğümlü kümenizin dağıtılması 4 dakika sürer.

## <a name="create-a-dataset"></a>Veri kümesi oluşturma

Bu öğreticide, [Azure açık veri kümelerinde](https://azure.microsoft.com/services/open-datasets/)kullanılabilir hale getirilen [diabetes veri kümesini](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html)kullanırsınız.

Veri kümesini oluşturmak için sol taraftaki menüde **veri kümeleri**' ni ve ardından **veri kümesi oluştur** ' u seçin. aşağıdaki seçenekleri görürsünüz:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Yeni bir veri kümesi oluşturmayı gösteren ekran görüntüsü":::

**Açık veri kümeleri ' nden** seçim yapın ve sonra **açık veri kümelerinde veri kümesi oluştur** ekranında:

1. Arama çubuğunu kullanarak *diabtes* arama
1. **Örnek seçin: Diabetes**
1. **İleri**’yi seçin
1. Veri kümeniz için bir ad sağlayın- *diabetes*
1. **Oluştur**’u seçin

Veri kümesini ve ardından **keşfet**' i seçerek verileri keşfedebilirsiniz:

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="DataSet 'in araştırıp nasıl yapılacağını gösteren ekran görüntüsü":::

Veriler 10 temel giriş değişkenine sahiptir (yaş, Sex, gövde yığın dizini, ortalama kan basıncı ve altı kan serum ölçümleri gibi) ve **Y** adlı bir hedef değişken (taban çizgisinden sonra bir yılda bir yıl ilerleme durumu ölçüsü).

## <a name="create-a-machine-learning-model-using-designer"></a>Tasarımcı kullanarak Machine Learning modeli oluşturma

İşlem ve veri kümelerini oluşturduktan sonra tasarımcı kullanarak makine öğrenimi modelini oluşturmaya geçebilirsiniz. Azure Machine Learning Studio 'da **Tasarımcı** ' yı ve ardından **Yeni işlem hattı**' nı seçin:

:::image type="content" source="media/tutorial-power-bi/create-designer.png" alt-text="Yeni bir işlem hattının nasıl oluşturulacağını gösteren ekran görüntüsü":::

Bir **Ayarlar menüsünü** de görebileceğiniz boş bir *tuval* görürsünüz:

:::image type="content" source="media/tutorial-power-bi/select-compute.png" alt-text="İşlem hedefini seçme şeklini gösteren ekran görüntüsü":::

**Ayarlar menüsünde**, **işlem hedefi** ' ni seçin ve ardından daha önce oluşturduğunuz Işlem örneğini ve ardından **Kaydet**' i seçin. **Taslak adınızı** daha kolay bir şekilde yeniden adlandırın (örneğin *diabetes-model*) ve bir açıklama girin.

Ardından, listelenen varlıklar bölümünde **veri kümelerini** genişletin ve **diabetes** veri kümesini bulun-Bu modülün tuvale sürükleyip bırakın:

:::image type="content" source="media/tutorial-power-bi/drag-component.png" alt-text="Üzerinde bir bileşenin nasıl sürükleyeceğinizi gösteren ekran görüntüsü":::

Sonra, aşağıdaki bileşenleri tuvale sürükleyip bırakın:

1. Doğrusal regresyon ( **Machine Learning algoritmalarda** bulunur)
1. Modeli eğitme ( **model eğitiminde** bulunur)

Tuvaliniz benzer olmalıdır (bileşenlerin üst ve alt tarafında, bağlantı noktası adı verilen küçük daireler, aşağıda kırmızı renkle vurgulanır):

:::image type="content" source="media/tutorial-power-bi/connections.png" alt-text="Bağlı olmayan bileşenlerin nasıl yapıldığını gösteren ekran görüntüsü":::
 
Ardından *, bu bileşenlere* bir araya getirmeniz gerekir. **Diabetes** veri kümesinin altındaki bağlantı noktasını seçin ve **modeli eğitme modeli** bileşeninin en üstündeki sağ bağlantı noktasına sürükleyin. **Doğrusal regresyon** bileşeninin alt kısmındaki bağlantı noktasını seçin ve **tren modeli** bağlantı noktasının en üstündeki sol taraftaki bağlantı noktasına sürükleyin.

Tahmin edilecek etiket (hedef) değişkeni olarak kullanılacak veri kümesindeki sütunu seçin. **Modeli eğit** bileşenini ve ardından **sütunu Düzenle**' yi seçin. İletişim kutusunda-aşağı açılan listeden **sütun adını girin** ve ardından **Y** ' yi seçin:

:::image type="content" source="media/tutorial-power-bi/label-columns.png" alt-text="Ekran görüntüsü etiket sütununu Seç":::

**Kaydet**’i seçin. Machine Learning *iş akışınız* aşağıdaki gibi görünmelidir:

:::image type="content" source="media/tutorial-power-bi/connected-diagram.png" alt-text="Bağlı bileşenlerin nasıl yapıldığını gösteren ekran görüntüsü":::

**Gönder** ' i ve ardından deneme altında **Yeni oluştur** ' u seçin. Deneme için, ardından **Gönder** için bir ad girin.

>[!NOTE]
> Denemenizin ilk çalıştırmada tamamlanması 5 dakika sürer. Sonraki çalıştırmalar çok daha hızlı tasarımcı önbellekler, gecikme süresini azaltmak için bileşenleri zaten çalıştırıyor.

Deneme tamamlandığında şunları görürsünüz:

:::image type="content" source="media/tutorial-power-bi/completed-run.png" alt-text="Tamamlanan çalışmayı gösteren ekran görüntüsü":::

Deneme günlüğünü inceleyerek, **modeli eğitme** ve **çıktılar + Günlükler**' i seçebilirsiniz.

## <a name="deploy-the-model"></a>Modeli dağıtma

Modeli dağıtmak için, ardından **gerçek zamanlı çıkarım ardışık düzenine** sahip **çıkarım işlem hattı oluştur** (tuvalin en üstünde bulunur) ' ı seçin:

:::image type="content" source="media/tutorial-power-bi/pipeline.png" alt-text="Gerçek zamanlı çıkarım ardışık düzenini gösteren ekran görüntüsü":::
 
İşlem hattı, yalnızca model Puanlama yapmak için gereken bileşenlere göre daha fazla. Verileri puanlandırdığınızda, hedef değişken değerlerini bilemezsiniz. bu nedenle, veri kümesinden **Y** 'yi kaldırabiliriz. Öğesini kaldırmak için DataSet bileşeninde bir **Select sütunları** tuval öğesine ekleyin. Bileşene tel veri kümesinin girişi olması ve sonuçlar **puan modeli** bileşeninin çıktısıdır.

:::image type="content" source="media/tutorial-power-bi/remove-column.png" alt-text="Bir sütunun kaldırılmasını gösteren ekran görüntüsü":::

Tuvaldeki **veri kümesinde sütunları seç '** i ve ardından **Sütunları Düzenle**' yi seçin. Sütunları seçin iletişim kutusunda, **ad** ' ı seçin ve sonra tüm giriş değişkenlerinin seçili **olmasına rağmen hedefin** seçili olduğundan emin olun:

:::image type="content" source="media/tutorial-power-bi/removal-settings.png" alt-text="Sütun ayarlarını kaldırmayı gösteren ekran görüntüsü":::

**Kaydet**’i seçin. Son olarak **model puan** bileşeni ' ni seçin ve **Çıkış Ekle** onay kutusunun işaretli olduğundan emin olun (yalnızca tahmine meler, girişler *ve* tahminler yerine geri gönderilir, gecikme süresini azaltır):

:::image type="content" source="media/tutorial-power-bi/score-settings.png" alt-text="Puan modeli bileşen ayarlarını gösteren ekran görüntüsü":::

Tuvalin en üstünde **Gönder** ' i seçin.

Çıkarım işlem hattını başarıyla çalıştırdığınız zaman, modeli çıkarım kümenize dağıtabilirsiniz. **Dağıt**' ı seçin, bu, **gerçek zamanlı uç noktayı ayarla** iletişim kutusunu gösterecektir. **Yeni gerçek zamanlı uç nokta dağıt**' ı seçin, uç nokta **My-diabetes-model** olarak adlandırın, daha önce oluşturduğunuz çıkarımı seçin ve **Dağıt**' ı seçin:

:::image type="content" source="media/tutorial-power-bi/endpoint-settings.png" alt-text="Gerçek zamanlı uç nokta ayarlarını gösteren ekran görüntüsü":::
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, tasarımcı modelinin nasıl eğiteini ve dağıtılacağını gördünüz. Sonraki bölümde, Power BI bu modeli kullanmayı (puan) öğreneceksiniz.

> [!div class="nextstepaction"]
> [Öğretici: Power BI modeli kullanın](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
