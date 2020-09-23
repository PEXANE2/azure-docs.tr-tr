---
title: 'Öğretici: Jupyter Notebook (Python) ile çalışmaya başlama'
titleSuffix: Azure Machine Learning
description: Jupyter Notebook öğreticileri için kurulum.  Azure Machine Learning çalışma alanı oluşturun, Jupyıter not defterlerini çalışma alanına kopyalayın ve not defterlerini çalıştırdığınız bir işlem örneği oluşturun.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sdgilley
ms.author: sgilley
ms.date: 02/10/2020
ms.custom: devx-track-python
ms.openlocfilehash: de52013628f5d02bedcf72a99e0fad25cabe5d8f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90896876"
---
# <a name="tutorial-get-started-with-azure-machine-learning-in-jupyter-notebooks"></a>Öğretici: Jupyter not defterlerinde Azure Machine Learning kullanmaya başlayın

Bu öğreticide, [yönetilen bulut tabanlı bir iş istasyonunda (işlem örneği)](concept-compute-instance.md)Jupyıter not defterlerini kullanarak Azure Machine Learning kullanmaya başlama adımlarını tamamlayabilirsiniz. Bu öğretici, diğer tüm Jupyter Notebook öğreticileri için bir precurör.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Diğer Jupyter Notebook öğreticilerde kullanmak üzere bir [Azure Machine Learning çalışma alanı](concept-workspace.md) oluşturun.
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

Azure Machine Learning, bir yüklemesi ücretsiz ve önceden yapılandırılmış bir deneyim için çalışma alanınızda bir bulut Not defteri sunucusu içerir. Ortamınız, paketler ve bağımlılıklarınız üzerinde denetim sahibi olmayı tercih ediyorsanız [kendi ortamınızı](tutorial-1st-experiment-sdk-setup-local.md) kullanın.

 Bu videoyla birlikte izleyin veya eğitim Not defterini kopyalayıp çalışma alanınızdan çalıştırmak için aşağıdaki ayrıntılı adımları kullanın.

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
    
1. **Öğreticiler/Image-Classification-mnist-Data** klasörünüzdeki **öğretici-1-deneme-SDK-eğitme. ipynb** dosyasını seçin.

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

    1.  Kullanılabilir seçimlerden sanal makine boyutunu seçin. Öğreticiler için, varsayılan VM iyi bir seçimdir.

    1. Ardından **Oluştur**’u seçin. VM 'nizi ayarlamak yaklaşık 5 dakika sürebilir.

1. VM kullanılabilir olduktan sonra, üst araç çubuğunda görüntülenir.  Artık Not defterini, araç çubuğunda **Tümünü Çalıştır** ' ı kullanarak veya Not defterinin kod hücrelerinde **SHIFT + enter** ' u kullanarak çalıştırabilirsiniz.

Özel pencere öğeleri varsa veya Jupyıter/Jupyıterlab kullanmayı tercih ediyorsanız, en sağdaki **jupi** açılan öğesini seçin ve ardından **jupi** veya **jupi veya jupi**'i seçin. Yeni tarayıcı penceresi açılır.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir geliştirme ortamı ayarlamış olduğunuza göre, bir modeli Jupyter Notebook eğitme devam edin:

> [!div class="nextstepaction"]
> [Öğretici: veri ve scikit ile görüntü sınıflandırma modellerini eğitme-öğrenme](tutorial-train-models-with-aml.md)

<a name="stop-compute-instance"></a> Şu anda başka öğreticiler de planlanmazsanız, maliyeti azaltmak için kullanmadığınız durumlarda bulut Not defteri sunucu VM 'sini durdurmanız gerekir:

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]
