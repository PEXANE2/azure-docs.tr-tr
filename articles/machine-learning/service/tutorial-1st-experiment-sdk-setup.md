---
title: 'Öğretici: Kurulum ortamı ve çalışma alanı.'
titleSuffix: Azure Machine Learning service
description: Bu öğreticide, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlamak için uçtan uca adımları tamamlarız.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 07/20/2019
ms.openlocfilehash: e17d9286453aeb5c7eac688555177c2c42faad21
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668633"
---
# <a name="tutorial-setup-environment-and-workspace"></a>Öğretici: Kurulum ortamı ve çalışma alanı

Bu öğreticide, Jupyıter not defterlerinde çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlamak için uçtan uca adımları tamamlarız. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin bir parçasıdır**ve Python ortamı kurulumu ve yapılandırmasını ve denemeleri ve makine öğrenimi modellerinizi yönetmek için bir çalışma alanı oluşturmayı içerir. Birden çok makine öğrenimi modellerini eğitmek ve hem Azure portal hem de SDK kullanarak model yönetimi işlemini tanıtmak için bu [**Iki bölümden oluşan iki**](tutorial-1st-experiment-sdk-train.md) derleme.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sonraki öğreticide kullanmak üzere bir Machine Learning çalışma alanı oluşturun.
> * Bir bulut Not defteri sunucu ortamı oluşturun.

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

     ![Yeni VM 'yi seçin](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. VM 'niz için bir ad sağlayın. Ardından **Oluştur**’u seçin.

    > [!NOTE]
    > Not defteri sanal makinenizin adı 2 ila 16 karakter arasında olmalıdır. Geçerli karakterler harfler, rakamlar ve-karakterdir.  Ad ayrıca Azure aboneliğinizde benzersiz olmalıdır.

    ![Yeni VM oluşturma](media/quickstart-run-cloud-notebook/create-new-workstation.png)

1. Durum **çalışıyor**olarak değişene kadar bekleyin.

### <a name="launch-jupyter-web-interface"></a>Jupyıter Web arabirimini Başlat

VM 'niz çalıştıktan sonra, Jupyter Web arabirimini başlatmak için **Not defteri VM 'leri** bölümünü kullanın.

1. VM 'niz için **URI** sütununda **Jupyıter** ' ı seçin.

    ![Jupyter Not defteri sunucusunu başlatma](./media/quickstart-run-cloud-notebook/start-server.png)

    Bağlantı, Not defteri sunucunuzu başlatır ve yeni bir tarayıcı sekmesinde Jupyter Not defteri Web sayfasını açar.  Bu bağlantı yalnızca VM 'yi oluşturan kişi için çalışır. Çalışma alanının her bir kullanıcısının kendi sanal makinesini oluşturması gerekir.

1. Jupyter Not defteri Web sayfasında, üst klasör adı sizin kullanıcı adıdır.  Bu klasörü seçin.

    > [!TIP]
    > Bu klasör, dizüstü makınenın kendisi yerine çalışma alanınızdaki [depolama kapsayıcısında](concept-workspace.md#resources) bulunur.  Not defteri sanal makinesini silebilir ve çalışmalarınızı devam edebilirsiniz.  Daha sonra yeni bir not defteri VM 'si oluşturduğunuzda, bu dosya aynı klasöre yüklenir. Çalışma alanınızı başkalarıyla paylaşıyorsanız, klasörünüzü görürler ve onu görürsünüz.

1. Alt dizini açın ve öğreticinin **ikinci kısmını** çalıştırmak için açın `tutorials/tutorial-1st-experiment-sdk-train.ipynb`. `samples-*`

## <a name="end"></a>Kaynakları Temizleme

Öğreticinin **2. bölümüne** devam ederseniz bu bölümü tamamlamayın.

### <a name="stop-the-notebook-vm"></a>Not defteri VM 'sini durdur

Bir bulut Not defteri sunucusu kullandıysanız, maliyeti azaltmak için kullanmadığınız sanal makineyi durdurun.

1. Çalışma alanınızda, **Not defteri VM 'leri**' ni seçin.

   ![VM sunucusunu durdur](./media/quickstart-run-cloud-notebook/stop-server.png)

1. Listeden VM’yi seçin.

1. **Durdur**' u seçin.

1. Sunucuyu yeniden kullanmaya hazırsanız **Başlat**' ı seçin.

### <a name="delete-everything"></a>Her şeyi sil

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu görevleri tamamladınız:

* Azure Machine Learning hizmet çalışma alanı oluşturuldu.
* Çalışma alanınızda bir bulut Not defteri sunucusu oluşturulup yapılandırıldı.

Basit bir makine öğrenimi modelini eğitmek için Bu öğreticinin **2. bölümünde** devam edin.

> [!div class="nextstepaction"]
> [Öğretici: İlk modelinizi eğitme](tutorial-1st-experiment-sdk-train.md)
