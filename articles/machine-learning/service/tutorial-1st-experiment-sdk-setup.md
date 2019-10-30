---
title: 'Öğretici: ilk ML denemenize oluşturma: Kurulum'
titleSuffix: Azure Machine Learning
description: Bu öğreticide, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlayacaksınız.  1\. Bölüm 'de, denemeleri ve ML modellerini yönetebileceğiniz bir çalışma alanı oluşturursunuz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 09/25/2019
ms.openlocfilehash: fc26b224a2af6ab4f1f6bf5551381d4739831351
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053864"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Öğretici: Python SDK ile ilk ML denemenizi oluşturmaya başlama

Bu öğreticide, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlamak için uçtan uca adımları tamamlarız. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin bir parçasıdır**ve Python ortamı kurulumu ve yapılandırmasını ve denemeleri ve makine öğrenimi modellerinizi yönetmek için bir çalışma alanı oluşturmayı içerir. Birden çok makine öğrenimi modellerini eğitmek ve hem Azure portal hem de SDK kullanarak model yönetimi işlemini tanıtmak için bu [**Iki bölümden oluşan iki**](tutorial-1st-experiment-sdk-train.md) derleme.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sonraki öğreticide kullanmak üzere bir [Azure Machine Learning çalışma alanı](concept-workspace.md) oluşturun.
> * Öğreticiler Not defterini çalışma alanındaki klasörünüze kopyalayın.
> * Azure Machine Learning Python SDK yüklü ve önceden yapılandırılmış bir bulut tabanlı Jupyter Not defteri VM 'si oluşturun.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, bulutta makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullandığınız temel bir kaynaktır. Azure aboneliğiniz ve kaynak grubunuz, hizmette kolayca tüketilen bir nesne ile aynı olur. 

Azure kaynaklarınızı yönetmek için Web tabanlı bir konsol olan Azure portal bir çalışma alanı oluşturursunuz. 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**bir yere göz atın. Denemenizin doğru yerde oluşturulmasını sağlamak için bunlara ihtiyacınız olacaktır. 


## <a name="azure"></a>Not defteri klasörünü Kopyala

Bu örnek, bir yüklemeden ücretsiz ve önceden yapılandırılmış bir deneyim için çalışma alanınızdaki bulut Not defteri sunucusunu kullanır. Ortamınız, paketler ve bağımlılıklarınız üzerinde denetim sahibi olmayı tercih ediyorsanız [kendi ortamınızı](how-to-configure-environment.md#local) kullanın.

Aşağıdaki deneme kurulumunu tamamlayıp çalışma alanı giriş sayfasında (Önizleme), tüm beceri seviyelerinin veri bilimi senaryolarına yönelik veri bilimi senaryoları gerçekleştirmek için Machine Learning araçları 'nı içeren birleştirilmiş bir arabirim olan

1. [Çalışma alanı giriş sayfasında](https://ml.azure.com/)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. Sol taraftaki **not defterlerini ve dosyaları** seçin.

1. **Örnekler** klasörünü açın.

1. **Öğreticiler** klasörünün sağ tarafındaki **"..."** öğesini seçin ve ardından **Kopyala**' yı seçin.

    ![Klasörü kopyala](media/tutorial-1st-experiment-sdk-setup/clone-tutorials.png)

1. Çalışma alanına erişen her kullanıcı için bir klasör görüntülenir.  **Eğitim** klasörünü kopyalamak için klasörünüzü seçin.

## <a name="a-nameopenselect-a-vm-to-run-the-notebook"></a>Not defterini çalıştırmak için bir VM seçin <a name="open">

1. **Kullanıcı dosyaları** altında klasörünüzü açın ve kopyalanan **öğreticiler** klasörünü açın.

    ![Öğreticiler klasörünü aç](media/tutorial-1st-experiment-sdk-setup/expand-user-folder.png)

    > [!IMPORTANT]
    > Not defterlerini **örnekler** klasöründe görüntüleyebilirsiniz, ancak buradan Not defteri çalıştıramazsınız.  Bir not defteri çalıştırmak için, **Kullanıcı dosyaları** bölümünde Not defterinin kopyalanmış sürümünü çalıştırdığınızdan emin olun.
    
1. **Öğreticiler** klasörünüzdeki **öğretici-1-deneme-SDK-eğitme. ipynb** dosyasını seçin.

1. Üstteki çubukta, Not defterini çalıştırmak için kullanılacak bir not defteri sanal makinesi seçin. Bu VM 'Ler Azure Machine Learning çalıştırmanız gereken her şey ile önceden yapılandırılmıştır. Çalışma alanınızın herhangi bir kullanıcısı tarafından oluşturulan bir VM 'yi seçebilirsiniz. 

1. VM bulunamazsa, sanal makineyi oluşturmak için **+ yenı VM** ' yi seçin.

    ![VM oluşturma](media/tutorial-1st-experiment-sdk-setup/no-vm.png)

    1. Bir VM oluşturduğunuzda, bir ad sağlayın.  Ad, 2 ila 16 karakter arasında olmalıdır. Geçerli karakterler harfler, rakamlar ve-karakterdir ve ayrıca Azure aboneliğinizde benzersiz olmalıdır.

    1. Ardından **Oluştur**’u seçin. VM 'nizi ayarlamak yaklaşık 5 dakika sürebilir.

1. VM kullanılabilir olduktan sonra, üst araç çubuğunda görüntülenir.  Artık Not defterini, araç çubuğunda **Tümünü Çalıştır** ' ı kullanarak veya Not defterinin kod hücrelerinde **SHIFT + enter** ' u kullanarak çalıştırabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu görevleri tamamladınız:

* Azure Machine Learning çalışma alanı oluşturuldu.
* Çalışma alanınızda bir bulut Not defteri sunucusu oluşturulup yapılandırıldı.

Öğreticinin **ikinci bölümünde** , makine öğrenimi modelini eğitmek için `tutorial-1st-experiment-sdk-train.ipynb` kodu çalıştırırsınız. 

> [!div class="nextstepaction"]
> [Öğretici: ilk modelinizi eğitme](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Bu öğreticinin veya diğer öğreticilerin 2. bölümünü planlamıyorsanız, maliyeti azaltmak için kullanmadığınız durumlarda [bulut Not defteri sunucu VM 'sini durdurmanız](tutorial-1st-experiment-sdk-train.md#clean-up-resources) gerekir.


