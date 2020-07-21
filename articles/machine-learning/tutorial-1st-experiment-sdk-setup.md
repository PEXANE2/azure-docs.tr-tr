---
title: 'Öğretici: Jupyter not defterlerinde deneme (Python)'
titleSuffix: Machine Learning - Azure
description: Bu öğreticide, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlayacaksınız.  1. Bölüm 'de, denemeleri ve ML modellerini yönetebileceğiniz bir çalışma alanı oluşturursunuz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sdgilley
ms.author: sgilley
ms.date: 02/10/2020
ms.custom: tracking-python
ms.openlocfilehash: 8ba6141bddd60c2bdc2fdf17845e8f470dd6ef55
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86520536"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Öğretici: Python SDK ile ilk ML denemenizi oluşturmaya başlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğreticide, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlamak için uçtan uca adımları tamamlarız. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin bir parçasıdır**ve Python ortamı kurulumu ve yapılandırmasını ve denemeleri ve makine öğrenimi modellerinizi yönetmek için bir çalışma alanı oluşturmayı içerir. Birden çok makine öğrenimi modeli eğitmek ve hem Azure Machine Learning Studio hem de SDK kullanarak model yönetimi işlemini tanıtmak için bu [**Iki bölümden oluşan iki**](tutorial-1st-experiment-sdk-train.md) derleme.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sonraki öğreticide kullanmak üzere bir [Azure Machine Learning çalışma alanı](concept-workspace.md) oluşturun.
> * Öğreticiler Not defterini çalışma alanındaki klasörünüze kopyalayın.
> * Azure Machine Learning Python SDK yüklü ve önceden yapılandırılmış bir bulut tabanlı işlem örneği oluşturun.


Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, bulutta makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullandığınız temel bir kaynaktır. Azure aboneliğiniz ve kaynak grubunuz, hizmette kolayca tüketilen bir nesne ile aynı olur. 

Azure kaynaklarınızı yönetmek için Web tabanlı bir konsol olan Azure portal bir çalışma alanı oluşturursunuz. 

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**bir yere göz atın. Denemenizin doğru yerde oluşturulmasını sağlamak için bunlara ihtiyacınız olacaktır. 

## <a name="run-notebook-in-your-workspace"></a><a name="azure"></a>Çalışma alanınızda Not defteri çalıştırma

Bu öğretici, bir yüklemeden ücretsiz ve önceden yapılandırılmış bir deneyim için çalışma alanınızdaki bulut Not defteri sunucusunu kullanır. Ortamınız, paketler ve bağımlılıklarınız üzerinde denetim sahibi olmayı tercih ediyorsanız [kendi ortamınızı](how-to-configure-environment.md#local) kullanın.

 Bu videoyla birlikte izleyin veya öğreticiyi çalışma alanınızdan çalıştırmak ve çalıştırmak için aşağıdaki ayrıntılı adımları kullanın. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4mTUr]

### <a name="clone-a-notebook-folder"></a>Not defteri klasörünü Kopyala

Tüm beceri seviyeleri için veri bilimi senaryoları gerçekleştirmek üzere Machine Learning araçları 'nı içeren birleştirilmiş bir arabirim olan Azure Machine Learning Studio 'da aşağıdaki deneme sürümü kurulumunu tamamlayıp adımları gerçekleştirin.

1. [Azure Machine Learning Studio](https://ml.azure.com/)'da oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. Sol taraftaki **not defterlerini** seçin.

1. Üstteki **örnekler** sekmesini seçin.

1. **Python** klasörünü açın.

1. Klasörü üzerinde bir sürüm numarasıyla açın.  Bu sayı, Python SDK 'sının geçerli sürümünü temsil eder.

1. **Öğreticiler** klasörünün sağ tarafındaki **"..."** öğesini seçin ve ardından **Kopyala**' yı seçin.

    :::image type="content" source="media/tutorial-1st-experiment-sdk-setup/clone-tutorials.png" alt-text="Öğreticiler klasörünü Kopyala":::

1. Çalışma alanına erişen her kullanıcıyı gösteren bir klasör listesi görüntülenir.  **Öğreticiler** klasörünü kopyalamak için klasörünüzü seçin.

### <a name="open-the-cloned-notebook"></a><a name="open"></a>Kopyalanmış Not defterini açın

1. **Kullanıcı dosyaları** bölümüne yeni kapatılan **öğreticiler** klasörünü açın.

    > [!IMPORTANT]
    > Not defterlerini **örnekler** klasöründe görüntüleyebilirsiniz, ancak buradan Not defteri çalıştıramazsınız.  Bir not defteri çalıştırmak için, **Kullanıcı dosyaları** bölümünde Not defterinin kopyalanmış sürümünü çalıştırdığınızdan emin olun.
    
1. **Öğreticiler/oluşturma-ilk-ml-deneme** klasörünüzdeki **öğretici-1-deneme-SDK-eğitme. ipynb** dosyasını seçin.

    :::image type="content" source="media/tutorial-1st-experiment-sdk-setup/expand-user-folder.png" alt-text="Öğreticiler klasörünü aç":::


1. Üst kısımdaki çubukta, Not defterini çalıştırmak için kullanılacak bir işlem örneği seçin. Bu VM 'Ler [Azure Machine Learning çalıştırmanız gereken her şey](concept-compute-instance.md#contents)ile önceden yapılandırılmıştır. 

1. Hiçbir VM bulunamazsa, işlem örneği VM oluşturmak için **+ Ekle** ' yi seçin. 

    1. Bir VM oluşturduğunuzda, bu kuralları izleyin:  
        + Ad gereklidir ve boş bırakılamaz.
        + Ad alanı/işlem örneğinin Azure bölgesindeki tüm mevcut işlem örnekleri genelinde adının benzersiz olması gerekir (büyük/küçük harf duyarsız). Seçtiğiniz ad benzersiz değilse bir uyarı alırsınız.
        + Geçerli karakterler büyük ve küçük harfler, sayılar (0-9) ve tire karakteri (-).
        + Ad 3 ila 24 karakter uzunluğunda olmalıdır.
        + Ad bir harfle başlamalıdır (sayı veya tire karakteri değil).
        + Kesik çizgi karakteri kullanılırsa, tireden sonra en az bir harf gelmelidir. Örnek: test-, test-0, test-01 geçersiz, test-a0, test-0a, geçerli örneklerdir.

    1.  Kullanılabilir seçimlerden sanal makine boyutunu seçin.

    1. Ardından **Oluştur**’u seçin. VM 'nizi ayarlamak yaklaşık 5 dakika sürebilir.

1. VM kullanılabilir olduktan sonra, üst araç çubuğunda görüntülenir.  Artık Not defterini, araç çubuğunda **Tümünü Çalıştır** ' ı kullanarak veya Not defterinin kod hücrelerinde **SHIFT + enter** ' u kullanarak çalıştırabilirsiniz.

Özel pencere öğeleri varsa veya Jupyıter/Jupyıterlab kullanmayı tercih ediyorsanız, en sağdaki **jupi** açılan öğesini seçin ve ardından **jupi** veya **jupi veya jupi**'i seçin. Yeni tarayıcı penceresi açılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu görevleri tamamladınız:

* Azure Machine Learning çalışma alanı oluşturuldu.
* Çalışma alanınızda bir bulut Not defteri sunucusu oluşturulup yapılandırıldı.

Öğreticinin **ikinci bölümünde** , `tutorial-1st-experiment-sdk-train.ipynb` bir makine öğrenimi modelini eğitebilmeniz için kodu çalıştırırsınız. 

> [!div class="nextstepaction"]
> [Öğretici: ilk modelinizi eğitme](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Bu öğreticinin veya diğer öğreticilerin 2. bölümünü planlamıyorsanız, maliyeti azaltmak için kullanmadığınız durumlarda [bulut Not defteri sunucu VM 'sini durdurmanız](tutorial-1st-experiment-sdk-train.md#clean-up-resources) gerekir.


