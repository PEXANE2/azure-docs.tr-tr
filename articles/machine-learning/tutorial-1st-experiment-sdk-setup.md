---
title: 'Öğretici: İlk ML denemenizi oluşturun'
titleSuffix: Azure Machine Learning
description: Bu eğitimde, Jupyter dizüstü bilgisayarlarda çalışan Azure Machine Learning Python SDK ile çalışmaya başlayacaksınız.  Bölüm 1'de, denemeleri ve ML modellerini yöneteceğiniz bir çalışma alanı oluşturursunuz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 02/10/2020
ms.openlocfilehash: a6f977c0cdca670b40ccdc01db64a493962e3dda
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239892"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Öğretici: Python SDK ile ilk ML denemeoluşturmaya başlayın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu eğitimde, Jupyter dizüstü bilgisayarlarda çalışan Azure Machine Learning Python SDK ile başlamak için uça doğru adımları tamamlarsınız. Bu öğretici **iki bölümlük öğretici serisinin bir parçasıdır**ve Python ortamı kurulumu ve yapılandırmasının yanı sıra deneylerinizi ve makine öğrenimi modellerinizi yönetmek için bir çalışma alanı oluşturur. [**İkinci bölüm,**](tutorial-1st-experiment-sdk-train.md) birden çok makine öğrenme modeli eğitmek ve hem Azure Machine Learning stüdyosunu hem de SDK'yı kullanarak model yönetimi sürecini tanıtmak için bunun üzerine inşa edilmiştir.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Bir sonraki öğreticide kullanmak üzere bir [Azure Machine Learning Çalışma Alanı](concept-workspace.md) oluşturun.
> * Çalışma alanında öğreticiler not defterini klasörünüze kopyala.
> * Azure Machine Learning Python SDK yüklü ve önceden yapılandırılmış bulut tabanlı bir bilgi işlem örneği oluşturun.


Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, makine öğrenimi modellerini denemek, eğitmek ve dağıtmak için bulutta kullandığınız temel bir kaynaktır. Azure aboneliğinizi ve kaynak grubunuzu hizmette kolayca tüketilen bir nesneye bağlar. 

Azure kaynaklarınızı yönetmek için web tabanlı bir konsol olan Azure portalı üzerinden bir çalışma alanı oluşturursunuz. 

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**not alın. Denemenizi doğru yerde oluşturduğunuzdan emin olmak için bunlara ihtiyacınız olacak. 

## <a name="run-notebook-in-your-workspace"></a><a name="azure"></a>Çalışma alanınızda not defterini çalıştırma

Bu öğretici, yüklemegerektirmeyen ve önceden yapılandırılmış bir deneyim için çalışma alanınızdaki bulut dizüstü bilgisayar sunucusunu kullanır. [Ortamınız, paketleriniz](how-to-configure-environment.md#local) ve bağımlılıklarınız üzerinde denetim sahibi olmayı tercih ediyorsanız kendi ortamınızı kullanın.

Bu videoyu takip edin veya çalışma alanınızdaki öğreticiyi klonlamak ve çalıştırmak için aşağıdaki ayrıntılı adımları kullanın. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4mTUr]



### <a name="clone-a-notebook-folder"></a>Not defteri klasörünü klonlama

Tüm beceri düzeylerindeki veri bilimi uygulayıcıları için veri bilimi senaryoları gerçekleştirmek için makine öğrenimi araçlarını içeren birleştirilmiş arabirim olan Azure Machine Learning stüdyosunda aşağıdaki deneme kurulum ve çalıştırma adımlarını tamamlarsınız.

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com/)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. Soldaki **Not Defterleri'ni** seçin.

1. **Örnekler** klasörünü açın.

1. **Python** klasörünü açın.

1. Üzerinde sürüm numarası olan klasörü açın.  Bu sayı Python SDK için geçerli sürümü temsil eder.

1. **Öğreticiler** klasörünün sağındaki **"..."** seçeneğini belirleyin ve ardından **Klon'u**seçin.

    ![Klon klasörü](./media/tutorial-1st-experiment-sdk-setup/clone-tutorials.png)

1. Çalışma alanına erişen her kullanıcıyı gösteren klasörlerin listesi görüntülenir.  Orada **öğreticiler** klasörü klonlamak için klasörünüzü seçin.

### <a name="a-nameopenopen-the-cloned-notebook"></a><a name="open">Klonlanmış not defterini açma

1. **Kullanıcı Dosyaları** altında klasörünüzü açın ve sonra klonlanmış **öğreticiler** klasörünü açın.

    ![Öğreticiler klasörünü aç](./media/tutorial-1st-experiment-sdk-setup/expand-user-folder.png)

    > [!IMPORTANT]
    > **Örnekler** klasöründe not defterlerini görüntüleyebilirsiniz, ancak not defterini buradan çalıştıramazsınız.  Not defterini çalıştırmak **için, Kullanıcı Dosyaları** bölümündeki not defterinin klonlanmış sürümünü açtığından emin olun.
    
1. **Öğreticiler/create-first-ml-experiment** klasöründe **tutorial-1st-experiment-sdk-train.ipynb** dosyasını seçin.

1. Üst çubukta, not defterini çalıştırmak için kullanılacak bir işlem örneği seçin. Bu VM'ler Azure [Machine Learning'i çalıştırmak için ihtiyacınız olan her şeyle](concept-compute-instance.md#contents)önceden yapılandırılmıştır. Çalışma alanınızın herhangi bir kullanıcısı tarafından oluşturulan bir VM seçebilirsiniz. 

1. VM yoksa, işlem örneği VM'yi oluşturmak için **+ Ekle'yi** seçin. 

    1. Bir VM oluşturduğunuzda, bir ad sağlayın.  Ad 2 ila 16 karakter arasında olmalıdır. Geçerli karakterler harfler, basamaklar ve - karakterdir ve Azure aboneliğinizde de benzersiz olmalıdır.

    1.  Kullanılabilir seçeneklerden Sanal Makine boyutunu seçin.

    1. Ardından **Oluştur**’u seçin. VM'nizi ayarlamak yaklaşık 5 dakika sürebilir.

1. VM kullanılabilir olduğunda üst araç çubuğunda görüntülenir.  Artık not defterini araç çubuğunda **Çalıştır'ı** kullanarak veya not defterinin kod hücrelerinde **Shift+Enter'u** kullanarak çalıştırabilirsiniz.

Eğer özel widget'lar varsa veya Jupyter / JupyterLab kullanmayı tercih en sağda **Jupyter** damla seçin, sonra **Jupyter** veya **JupyterLab**seçin. Yeni tarayıcı penceresi açılacak.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu görevleri tamamladınız:

* Azure Machine Learning çalışma alanı oluşturdu.
* Çalışma alanınızda bir bulut dizüstü bilgisayar sunucusu oluşturuldu ve yapılandırıldı.

Öğretici bölüm **iki** bir makine öğrenme `tutorial-1st-experiment-sdk-train.ipynb` modeli eğitmek için kodu çalıştırın. 

> [!div class="nextstepaction"]
> [Öğretici: İlk modelinizi eğitin](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Bu öğreticinin 2. [stop the cloud notebook server VM](tutorial-1st-experiment-sdk-train.md#clean-up-resources)


