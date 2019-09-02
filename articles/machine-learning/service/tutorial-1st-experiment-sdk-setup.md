---
title: 'Öğretici: İlk ML denemenizi oluşturun: Kurulum'
titleSuffix: Azure Machine Learning service
description: Bu öğretici serisinde, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlamak için uçtan uca adımları tamamlarız.  Birinci bölüm, bir bulut Not defteri sunucu ortamının oluşturulmasını ve denemeleri ve makine öğrenimi modellerinizi yönetmek için bir çalışma alanı oluşturmayı kapsamaktadır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 08/28/2019
ms.openlocfilehash: df74d2d07937634e2b656746c0a9fc9cd86b8c93
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70182607"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment"></a>Öğretici: İlk ML denemenizi oluşturmaya başlayın

Bu öğreticide, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlamak için uçtan uca adımları tamamlarız. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin bir parçasıdır**ve Python ortamı kurulumu ve yapılandırmasını ve denemeleri ve makine öğrenimi modellerinizi yönetmek için bir çalışma alanı oluşturmayı içerir. Birden çok makine öğrenimi modellerini eğitmek ve hem Azure portal hem de SDK kullanarak model yönetimi işlemini tanıtmak için bu [**Iki bölümden oluşan iki**](tutorial-1st-experiment-sdk-train.md) derleme.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sonraki öğreticide kullanmak üzere bir Machine Learning çalışma alanı oluşturun.
> * Azure Machine Learning Python SDK yüklü ve önceden yapılandırılmış bir bulut tabanlı Jupyter Not defteri VM 'si oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için tek önkoşul bir Azure aboneliğidir. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Çalışma alanı, bulutta makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullandığınız temel bir kaynaktır. Azure aboneliğiniz ve kaynak grubunuz SDK 'daki kolayca tüketilen bir nesneye sahiptir. Zaten bir Azure Machine Learning Service çalışma alanınız varsa, [sonraki bölüme](#azure)atlayın. Aksi takdirde, şimdi bir tane oluşturun.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="azure"></a>Bulut Not defteri sunucusu oluşturma

Bu örnek, bir yüklemeden ücretsiz ve önceden yapılandırılmış bir deneyim için çalışma alanınızdaki bulut Not defteri sunucusunu kullanır. Ortamınız, paketler ve bağımlılıklarınız üzerinde denetim sahibi olmayı tercih ediyorsanız [kendi ortamınızı](how-to-configure-environment.md#local) kullanın.

Jupyıter not defterlerini kullanmaya başlamak için çalışma alanınızdan bir bulut kaynağı oluşturursunuz. Bu kaynak, Azure Machine Learning hizmetini çalıştırmak için ihtiyacınız olan her şey ile önceden yapılandırılmış bulut tabanlı bir Linux sanal makinedir.

1. Çalışma alanınızı [Azure Portal](https://portal.azure.com/)açın.  Portalda çalışma alanınızı bulma konusunda emin değilseniz, bkz. [çalışma alanınızı bulma](how-to-manage-workspace.md#view).

1. Azure portal çalışma alanı sayfanızda, sol taraftaki **Not defteri VM** 'lerini seçin.

1. Bir not defteri VM 'si oluşturmak için **+ Yeni** ' yi seçin.

     ![Yeni VM 'yi seçin](./media/tutorial-1st-experiment-sdk-setup/add-workstation.png)

1. VM 'niz için bir ad sağlayın. 
   + Not defteri sanal makinenizin adı 2 ila 16 karakter arasında olmalıdır. Geçerli karakterler harfler, rakamlar ve-karakterdir.  
   + Ad ayrıca Azure aboneliğinizde benzersiz olmalıdır.

1. Ardından **Oluştur**’u seçin. SANAL makinenizin ayarlanması biraz zaman alabilir.

1. Durum **çalışıyor**olarak değişene kadar bekleyin.

### <a name="launch-jupyter-web-interface"></a>Jupyıter Web arabirimini Başlat

VM 'niz çalıştıktan sonra, Jupyter Web arabirimini başlatmak için **Not defteri VM 'leri** bölümünü kullanın.

1. VM 'niz için **URI** sütununda **Jupyıter** ' ı seçin.

    ![Jupyter Not defteri sunucusunu başlatma](./media/tutorial-1st-experiment-sdk-setup/start-server.png)

   Bağlantı, Not defteri sunucunuzu başlatır ve yeni bir tarayıcı sekmesinde Jupyter Not defteri Web sayfasını açar.  Bu bağlantı yalnızca VM 'yi oluşturan kişi için çalışır. Çalışma alanının her bir kullanıcısının kendi sanal makinesini oluşturması gerekir.

1. Jupyter Not defteri Web sayfasında, Kullanıcı adınızı içeren üstteki KlasörAdı öğesini seçin.  

   Bu klasör, Not defteri VM 'sinin kendisi yerine çalışma alanı [depolama hesabında](concept-workspace.md#resources) bulunur.  Not defteri VM 'sini silerseniz, işinizi yine de devam edeceksiniz.  Daha sonra yeni bir not defteri VM 'si oluşturduğunuzda, bu dosya aynı klasöre yüklenir. Çalışma alanınızı başkalarıyla paylaşıyorsanız, klasörünüzü görürler ve onu görürsünüz.

1. `samples-*` Alt dizini açın ve ardından Jupyter Not defterini açın`tutorials/tutorial-1st-experiment-sdk-train.ipynb`

   > [!Warning]
   > > Aynı adı `tutorial-1st-experiment-sdk-train.ipynb` **değil** `.yml` , dosyayı açtığınızdan emin olun. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu görevleri tamamladınız:

* Azure Machine Learning hizmet çalışma alanı oluşturuldu.
* Çalışma alanınızda bir bulut Not defteri sunucusu oluşturulup yapılandırıldı.

Öğreticinin **ikinci bölümünde** , bir makine öğrenimi modelini eğitebilmeniz `tutorial-1st-experiment-sdk-train.ipynb` için kodu çalıştırırsınız. 

> [!div class="nextstepaction"]
> [Öğretici: İlk modelinizi eğitme](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Bu öğreticinin veya diğer öğreticilerin 2. bölümünü planlamıyorsanız, maliyeti azaltmak için kullanmadığınız durumlarda [bulut Not defteri sunucu VM 'sini durdurmanız](tutorial-1st-experiment-sdk-train.md#clean-up-resources) gerekir.
