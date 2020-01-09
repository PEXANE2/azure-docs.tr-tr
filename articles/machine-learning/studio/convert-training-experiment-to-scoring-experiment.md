---
title: Modeli dağıtım için hazırla
titleSuffix: ML Studio (classic) - Azure
description: Machine Learning Studio (klasik) eğitim denemenizi tahmine dayalı bir deneyle dönüştürerek, eğitilen modelinizi Web hizmeti olarak dağıtıma hazırlama.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.date: 03/28/2017
ms.openlocfilehash: e397cfaa9ce521ebe3c2f46ef6cc015bff3112f7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75454819"
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio (klasik) dağıtım için modelinizi hazırlama

Azure Machine Learning Studio (klasik), tahmine dayalı bir analiz modeli geliştirmek için ihtiyacınız olan araçları sağlar ve Azure Web hizmeti olarak dağıtarak onu çalıştırabilirsiniz.

Bunu yapmak için Studio 'Yu (klasik) kullanarak, modelinizi eğitebileceğiniz, puanlayıp düzenlediğiniz *eğitim deney* adı verilir. Memnun olduğunuzda, modelinizi eğitim denemenizi dönüştürerek dağıtmaya hazırlanma bir *Tahmine dayalı denemeye* puanı kullanıcı verileri için yapılandırılır.

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
Denemenizi çalıştırdıktan sonra (tıklayın **ÇALIŞTIRMA** deneme tuvalinin altındaki), tıklayın **Web hizmetinin ayarı** düğmesine (seçin **Tahmine dayalı Web hizmeti** seçeneği). **Web hizmetinin ayarı** sizin için eğitim denemesini öngörücü bir denemeye dönüştürme üç adımı gerçekleştirir:

1. Eğitilen modelinizde kaydeder **eğitilen modelleri** (deneme tuvalinin sol için) modül paletinin bölümü. Ardından makine öğrenimi algoritmasını değiştirir ve model modüllerini kaydedilen eğitilen modelle [eğitme][train-model] .
2. Denemenizi analiz eder ve yalnızca eğitim için açıkça kullanılmış ve artık gerekmeyen modülleri kaldırır.
3. Bunu ekler _Web hizmeti giriş_ ve _çıkış_ varsayılan konumda (Bu modülleri kabul edin ve dönüş kullanıcı verileri), denemenize modülleri.

Örneğin, aşağıdaki denemenin örnek görselleştirmenizdeki verilerin kullanarak iki sınıflı artırmalı karar ağacı modeli eğitir:

![Eğitim denemesini](./media/convert-training-experiment-to-scoring-experiment/figure1.png)

Bu deneyde modülleri temelde dört farklı işlevleri gerçekleştirir:

![Modül işlevleri](./media/convert-training-experiment-to-scoring-experiment/figure2.png)

Bu eğitim denemesini öngörücü bir denemeye dönüştürme yaptığınızda, bazı bu modüllerin artık gerekli olmayan veya artık farklı bir amaç sağladıkları:

* **Veri** -örnek veri kümesinde veri Puanlama sırasında kullanılmaz - web hizmeti kullanıcı verileri puanlanması sağlayacak. Ancak, veri türleri gibi bu veri kümesi meta verileri, eğitilen model tarafından kullanılır. Bu nedenle bu meta veriler sağlayabilmesi veri kümesi Tahmine dayalı denemeye tutmanız gerekir.

* **Hazırlığı** - Puanlama için bu modülleri olabilir veya gelen veriyi işlemek gerekli olmayabilir gönderilecek kullanıcı verilere bağlı olarak. **Web hizmetinin ayarı** düğmesi bu touch değil - nasıl, bunları işlemek istediğinize karar vermeniz gerekir.
  
    Örneğin, bu örnekte örnek veri kümesinde eksik değerler olabilir, bu nedenle bunlarla başa çıkmak için bir [Temizleme eksik veri][clean-missing-data] modülü eklenmiştir. Ayrıca, örnek veri kümesi modeli eğitmek için gerekli olmayan sütunları içerir. Bu nedenle, veri akışından bu ek sütunları hariç tutmak için DataSet modülünde bir [Select sütunları][select-columns] eklenmiştir. Puanlama için Web hizmeti üzerinden gönderilecek verilerin eksik değerlere sahip olmadığını biliyorsanız, [eksik veri modülünü Temizleme][clean-missing-data] seçeneğini kaldırabilirsiniz. Ancak, [veri kümesi modülündeki sütunları seç][select-columns] , eğitilen modelin beklediği veri sütunlarını tanımlamaya yardımcı olduğundan, Bu modülün kalması gerekir.

* **Eğitim** -Bu modüller modeli eğitmek için kullanılır. Tıkladığınızda **Web hizmetinin ayarı**, bu modüller, eğitilen modeli içeren tek bir modül ile değiştirilir. Bu yeni modül kaydedilir **eğitilen modelleri** modül paletinin bölümü.

* **Puan** -Bu örnekte, veri akışını test verilerine ve eğitim verilerine bölmek Için [bölünmüş veri][split] modülü kullanılır. Tahmine dayalı deneyde artık eğitim duymuyoruz, bu nedenle [bölünmüş veriler][split] kaldırılabilir. Benzer şekilde, ikinci [puan modeli][score-model] modülü ve [modeli değerlendir][evaluate-model] modülü, sonuçları test verileriyle karşılaştırmak için kullanılır; bu nedenle, bu modüller tahmine dayalı deneyde gerekli değildir. Ancak, kalan [puan modeli][score-model] modülü, Web hizmeti aracılığıyla bir puan sonucu döndürmek için gereklidir.

İşte tıklandıktan sonra Örneğimizdeki nasıl göründüğünü **Web hizmetinin ayarı**:

![Tahmine dayalı denemeye dönüştürüldü](./media/convert-training-experiment-to-scoring-experiment/figure3.png)

İşleri halletmek **Web hizmetinin ayarı** deneyiminizi bir web hizmeti olarak dağıtılması hazırlamak yeterli olabilir. Ancak, bazı ek işleri denemenizi için belirli yapmak isteyebilirsiniz.

### <a name="adjust-input-and-output-modules"></a>Giriş ve çıkış modülleri ayarlama
Eğitim denemenizi bir eğitim veri kümesi kullanılan ve daha sonra machine learning algoritmasını gerektiği ndaki bir forma veri almak için bazı işleme vermedi. Web hizmeti aracılığıyla almaya beklediğiniz verileri bu işlem gerekli değildir, atlayabilirsiniz: çıkışını **Web hizmeti giriş Modülü** denemenizi içinde farklı bir modül için. Kullanıcı verileri, artık bu konumda modelinde ulaşırsınız.

Örneğin, varsayılan olarak **Web hizmetinin ayarı** koyar **Web hizmeti giriş** Yukarıdaki şekilde gösterildiği gibi veri akışı üst kısmındaki modülü. Ancak biz elle konumlandırabilirsiniz **Web hizmeti giriş** veri işleme modülleri geçmiş:

![Web hizmeti girişini taşıma](./media/convert-training-experiment-to-scoring-experiment/figure4.png)

Tüm ön işleme olmadan web hizmeti aracılığıyla sağlanan giriş verilerini artık doğrudan Score Model modüle geçirin.

Benzer şekilde, varsayılan olarak **Web hizmetinin ayarı** Web Hizmetleri çıkış modülü, veri akışı alt kısmındaki koyar. Bu örnekte, Web hizmeti, tüm giriş verisi vektörünü ve Puanlama sonuçlarını içeren, kullanıcıya [puan modeli][score-model] modülünün çıktısını döndürür.
Farklı bir döndürülecek tercih ederseniz, ancak daha sonra önce ek modüller ekleyebilirsiniz **Web hizmeti çıkış** modülü. 

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
