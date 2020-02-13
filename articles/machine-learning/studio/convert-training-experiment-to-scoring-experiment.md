---
title: Modeli dağıtım için hazırla
titleSuffix: ML Studio (classic) - Azure
description: Machine Learning Studio (klasik) eğitim denemenizi tahmine dayalı bir deneyle dönüştürerek, eğitilen modelinizi Web hizmeti olarak dağıtıma hazırlama.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 03/28/2017
ms.openlocfilehash: fa9c5e5fa84a1ea6718bd82cd349f9248d283722
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168846"
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio (klasik) dağıtım için modelinizi hazırlama

Azure Machine Learning Studio (klasik), tahmine dayalı bir analiz modeli geliştirmek için ihtiyacınız olan araçları sağlar ve Azure Web hizmeti olarak dağıtarak onu çalıştırabilirsiniz.

Bunu yapmak için Studio 'Yu (klasik) kullanarak, modelinizi eğitebileceğiniz, puanlayıp düzenlediğiniz *eğitim deney* adı verilir. Memnun olduktan sonra, eğitim denemenizi, Kullanıcı verilerini Puanlama için yapılandırılmış bir tahmine *dayalı deneye* dönüştürerek modelinizi dağıtıma hazırlıyoruz.

Bu işleme örnek olarak [öğretici 1: kredi riskini tahmin](tutorial-part1-credit-risk.md)edin ' de bakabilirsiniz.

Bu makalede bir yakından eğitim denemesini öngörücü bir denemeye nasıl dönüştürüldüğünü ve bu Tahmine dayalı denemeye nasıl dağıtıldığını ayrıntılarını alır. Bu ayrıntılar anlayarak, dağıtılan modelinizin daha verimli hale getirmek için yapılandırma konusunda bilgi edinebilirsiniz.



## <a name="overview"></a>Genel Bakış 

Eğitim denemesini öngörücü bir denemeye dönüştürme işlemi üç adımdan oluşur:

1. Makine öğrenimi algoritması, eğitilen model modüllerle değiştirin.
2. Denemeyi Puanlama için gerekli olan modüller için kesim. Eğitim denemesini eğitim için gerekli olan ancak modeli eğitilir sonra gerekli olmayan modüller içerir.
3. Modelinizi web hizmeti kullanıcı verileri kabul nasıl ve hangi verilerin döndürülecek tanımlayın.

> [!TIP]
> Eğitim denemenizi eğitim ve puanlama kendi verilerinizi kullanarak modelinizi endişe başardım. Ancak uygulama dağıtıldıktan sonra kullanıcılar modelinize yeni veri göndermek ve tahmin sonuçlarını döndürür. Dağıtım için hazır almak için öngörücü bir denemeye eğitim denemenizi Dönüştür gibi bu nedenle, nasıl modelin başkaları tarafından kullanılacak göz önünde bulundurun.
> 
> 

## <a name="set-up-web-service-button"></a>Web hizmetini Ayarla düğmesi
Denemenizi çalıştırdıktan sonra (deneme tuvalinin alt kısmındaki **Çalıştır** ' a tıklayın), **Web hizmeti ayarla** düğmesine tıklayın (tahmine **dayalı Web hizmeti** seçeneğini belirleyin). **Web hizmeti** 'ni, eğitim denemenizi bir tahmine dayalı deneyle dönüştürmenin üç adımı sizin için gerçekleştirir:

1. Eğitilen modelinizi modül paleti (deneme tuvalinin sol tarafında) **eğitilen modeller** bölümüne kaydeder. Ardından makine öğrenimi algoritmasını değiştirir ve model modüllerini kaydedilen eğitilen modelle [eğitme][train-model] .
2. Denemenizi analiz eder ve yalnızca eğitim için açıkça kullanılmış ve artık gerekmeyen modülleri kaldırır.
3. _Web hizmeti giriş_ ve _Çıkış_ modüllerini denemenizin varsayılan konumlarına ekler (Bu modüller Kullanıcı verilerini kabul eder ve döndürür).

Örneğin, aşağıdaki denemenin örnek görselleştirmenizdeki verilerin kullanarak iki sınıflı artırmalı karar ağacı modeli eğitir:

![Eğitim denemesini](./media/convert-training-experiment-to-scoring-experiment/figure1.png)

Bu deneyde modülleri temelde dört farklı işlevleri gerçekleştirir:

![Modül işlevleri](./media/convert-training-experiment-to-scoring-experiment/figure2.png)

Bu eğitim denemesini öngörücü bir denemeye dönüştürme yaptığınızda, bazı bu modüllerin artık gerekli olmayan veya artık farklı bir amaç sağladıkları:

* **Veri** -Bu örnek veri kümesindeki veriler Puanlama sırasında kullanılmaz; Web hizmeti kullanıcısı puanlanmayacak verileri sağlar. Ancak, veri türleri gibi bu veri kümesi meta verileri, eğitilen model tarafından kullanılır. Bu nedenle bu meta veriler sağlayabilmesi veri kümesi Tahmine dayalı denemeye tutmanız gerekir.

* **Prep** -Puanlama için gönderilecek kullanıcı verilerine bağlı olarak, bu modüller gelen verileri işlemek için gerekli olmayabilir veya olmayabilir. **Web hizmeti ayarlama** düğmesine dokunmaz. bunları nasıl işlemek istediğinize karar vermeniz gerekir.
  
    Örneğin, bu örnekte örnek veri kümesinde eksik değerler olabilir, bu nedenle bunlarla başa çıkmak için bir [Temizleme eksik veri][clean-missing-data] modülü eklenmiştir. Ayrıca, örnek veri kümesi modeli eğitmek için gerekli olmayan sütunları içerir. Bu nedenle, veri akışından bu ek sütunları hariç tutmak için DataSet modülünde bir [Select sütunları][select-columns] eklenmiştir. Puanlama için Web hizmeti üzerinden gönderilecek verilerin eksik değerlere sahip olmadığını biliyorsanız, [eksik veri modülünü Temizleme][clean-missing-data] seçeneğini kaldırabilirsiniz. Ancak, [veri kümesi modülündeki sütunları seç][select-columns] , eğitilen modelin beklediği veri sütunlarını tanımlamaya yardımcı olduğundan, Bu modülün kalması gerekir.

* **Eğitme** -bu modüller modeli eğiteiçin kullanılır. **Web hizmeti ayarla**' ya tıkladığınızda, bu modüller, eğitilen modeli içeren tek bir modülle değiştirilmiştir. Bu yeni modül, modül paleti 'nin **eğitilen modeller** bölümüne kaydedilir.

* **Puan** -Bu örnekte, veri akışını test verilerine ve eğitim verilerine bölmek Için [bölünmüş veri][split] modülü kullanılır. Tahmine dayalı deneyde artık eğitim duymuyoruz, bu nedenle [bölünmüş veriler][split] kaldırılabilir. Benzer şekilde, ikinci [puan modeli][score-model] modülü ve [modeli değerlendir][evaluate-model] modülü, sonuçları test verileriyle karşılaştırmak için kullanılır; bu nedenle, bu modüller tahmine dayalı deneyde gerekli değildir. Ancak, kalan [puan modeli][score-model] modülü, Web hizmeti aracılığıyla bir puan sonucu döndürmek için gereklidir.

Aşağıda, **Web hizmeti ayarla**' yı tıkladıktan sonra örneğimiz şöyle görünür:

![Tahmine dayalı denemeye dönüştürüldü](./media/convert-training-experiment-to-scoring-experiment/figure3.png)

**Web hizmeti ayarlama** tarafından gerçekleştirilen iş, denemenizi bir Web hizmeti olarak dağıtılacak şekilde hazırlamak yeterli olabilir. Ancak, bazı ek işleri denemenizi için belirli yapmak isteyebilirsiniz.

### <a name="adjust-input-and-output-modules"></a>Giriş ve çıkış modülleri ayarlama
Eğitim denemenizi bir eğitim veri kümesi kullanılan ve daha sonra machine learning algoritmasını gerektiği ndaki bir forma veri almak için bazı işleme vermedi. Web hizmeti üzerinden almak istediğiniz verilerin bu işleme ihtiyacı yoksa, bu işlemi atlayabilirsiniz: **Web Hizmeti Giriş modülünün** çıkışını denemenizin farklı bir modülüne bağlama. Kullanıcı verileri, artık bu konumda modelinde ulaşırsınız.

Örneğin, varsayılan olarak, Web **hizmeti kurulumu** , yukarıdaki şekilde gösterildiği gibi, **Web hizmeti giriş** modülünü veri akışınızı en üst kısmına koyar. Ancak, **Web hizmeti girişini** veri işleme modüllerinden geçmiş olarak el ile konumlandırabiliriz:

![Web hizmeti girişini taşıma](./media/convert-training-experiment-to-scoring-experiment/figure4.png)

Tüm ön işleme olmadan web hizmeti aracılığıyla sağlanan giriş verilerini artık doğrudan Score Model modüle geçirin.

Benzer şekilde, varsayılan olarak, Web **hizmeti kurulumu** , Web hizmeti çıkış modülünü veri akışınız altına koyar. Bu örnekte, Web hizmeti, tüm giriş verisi vektörünü ve Puanlama sonuçlarını içeren, kullanıcıya [puan modeli][score-model] modülünün çıktısını döndürür.
Ancak, farklı bir şey döndürmeyi tercih ediyorsanız, **Web hizmeti çıkış** modülünden önce ek modüller ekleyebilirsiniz. 

Örneğin, tüm giriş verileri vektörünü değil yalnızca Puanlama sonuçlarını döndürmek için, Puanlama sonuçları hariç tüm sütunları hariç tutmak için DataSet modülüne bir [Select sütunları][select-columns] ekleyin. Daha sonra **Web hizmeti çıkış** modülünü [veri kümesi modülündeki sütunları seçme][select-columns] modülüne taşıyın. Denemeyi şöyle görünür:

![Web hizmeti çıkış taşıma](./media/convert-training-experiment-to-scoring-experiment/figure5.png)

### <a name="add-or-remove-additional-data-processing-modules"></a>Ek veri işleme modülleri Ekle Kaldır
Puanlama sırasında ihtiyaç bildiğiniz denemenizi daha fazla modülleri varsa bunlar kaldırılabilir. Örneğin, **Web hizmeti giriş** modülünü veri işleme modüllerinden sonra bir noktaya taşıdığımızda, [eksik veri][clean-missing-data] modülünü tahmine dayalı deneyden kaldırabiliriz.

Bizim Tahmine dayalı denemeye artık şöyle görünür:

![Ek modülü kaldırılıyor](./media/convert-training-experiment-to-scoring-experiment/figure6.png)


### <a name="add-optional-web-service-parameters"></a>İsteğe bağlı Web hizmeti parametrelerini Ekle
Bazı durumlarda, kullanıcının web hizmetinizin hizmet erişildiğinde modülleri davranışını değiştirmesine izin ver isteyebilirsiniz. *Web hizmeti parametreleri* bunu yapmanıza olanak sağlar.

Ortak bir örnek, bir [Içeri aktarma verileri][import-data] modülü, dağıtılmış Web hizmeti kullanıcısının Web hizmetine erişildiğinde farklı bir veri kaynağı belirtmesini sağlayacak şekilde ayarlıyor. Veya [dışarı aktarma veri][export-data] modülünü farklı bir hedef belirtime için yapılandırma.

Web hizmeti parametrelerini tanımlayın ve bunları bir veya daha fazla modül parametrelerini ile ilişkilendirin ve bunlar gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz. Web hizmeti kullanıcı hizmete erişme ve modül işlemleri uygun şekilde değiştirilir Bu parametreler için değerler sağlar.

Web hizmeti parametrelerinin ne olduğu ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti parametrelerini kullanma][webserviceparameters].

[webserviceparameters]: web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Tahmine dayalı denemeye bir web hizmeti olarak dağıtma
Tahmine dayalı denemeye yeterince hazırlandığından, bir Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar, modelinize veri gönderebilir ve modeli, Öngörüler döndürür.

Dağıtım işlemi hakkında daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti dağıtma][deploy]

[deploy]: deploy-a-machine-learning-web-service.md

<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
