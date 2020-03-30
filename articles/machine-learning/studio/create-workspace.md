---
title: Çalışma alanı oluşturma
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'yu (klasik) kullanmak için bir Machine Learning Studio (klasik) çalışma alanına sahip olmanız gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 12/07/2017
ms.openlocfilehash: 1a391a7a061d1382b5e07b45625c44fc0f5dec54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204469"
---
# <a name="create-and-share-an-azure-machine-learning-studio-classic-workspace"></a>Azure Machine Learning Studio (klasik) çalışma alanı oluşturma ve paylaşma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Machine Learning Studio'yu (klasik) kullanmak için bir Machine Learning Studio (klasik) çalışma alanına sahip olmanız gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.

## <a name="create-a-studio-classic-workspace"></a>Studio (klasik) çalışma alanı oluşturma

1. [Azure portalında](https://portal.azure.com/) oturum açın

    > [!NOTE]
    > Oturum açmak ve bir Studio (klasik) çalışma alanı oluşturmak için Azure abonelik yöneticisi olmanız gerekir. 
    >
    > 

2. **+Yeni'yi** tıklatın

3. Arama kutusunda, **Machine Learning Studio (klasik) Çalışma Alanı'nı** yazın ve eşleşen öğeyi seçin. Ardından, sayfanın altındaki **Oluştur'u** tıklatın' seçeneğini belirleyin.

4. Çalışma alanı bilgilerinizi girin:

   - *Çalışma alanı adı* 260 karaktere kadar olabilir ve bir boşlukta bitmeyebilir. Ad bu karakterleri içeremez:`< > * % & : \ ? + /`
   - Seçtiğiniz *web hizmeti planı* (veya oluşturduğunuz) seçtiğiniz ilişkili fiyatlandırma *katmanı* ile birlikte, bu çalışma alanından web hizmetlerini dağıtıyorsanız kullanılır.

     ![Yeni bir Studio (klasik) çalışma alanı oluşturma](./media/create-workspace/create-new-workspace.png)

5. **Oluştur'u**tıklatın.

> [!NOTE]
> Machine Learning Studio (klasik), iş akışını yürütürken ara verileri kaydetmek için sağladığınız bir Azure depolama hesabına güvenir. Çalışma alanı oluşturulduktan sonra, depolama hesabı silinirse veya erişim anahtarları değiştirilirse, çalışma alanı çalışmayı durdurur ve bu çalışma alanındaki tüm denemeler başarısız olur.
Depolama hesabını yanlışlıkla silerseniz, silinen depolama hesabıyla aynı bölgede aynı ada sahip depolama hesabını yeniden oluşturun ve erişim anahtarını yeniden senkronize edin. Depolama hesabının erişim anahtarlarını değiştirdiyseniz, Azure portalını kullanarak çalışma alanındaki erişim anahtarlarını yeniden eşitleyin.

Çalışma alanı dağıtıldıktan sonra Machine Learning Studio'da (klasik) açabilirsiniz.

1. Machine Learning Studio'ya (klasik) göz [https://studio.azureml.net/](https://studio.azureml.net/)atın.

2. Sağ üst köşeden çalışma alanınızı seçin.

    ![Çalışma alanını seçme](./media/create-workspace/open-workspace.png)

3. **Denemelerimi**tıklatın.

    ![Açık deneyler](./media/create-workspace/my-experiments.png)

Studio (klasik) çalışma alanınızı yönetme hakkında daha fazla bilgi için [bkz.](manage-workspace.md)
Çalışma alanınızı oluştururken bir sorunla karşılaşırsanız, [Sorun Giderme kılavuzuna bakın: Machine Learning Studio (klasik) çalışma alanı oluşturun ve bağlanın.](troubleshooting-creating-ml-workspace.md)


## <a name="share-an-azure-machine-learning-studio-classic-workspace"></a>Azure Machine Learning Studio (klasik) çalışma alanını paylaşma
Machine Learning Studio (klasik) çalışma alanı oluşturulduktan sonra, kullanıcıları çalışma alanınıza ve tüm deneylerine, veri kümelerine, not defterlerine vb. erişimi paylaşmaları için iş alanınıza davet edebilirsiniz. Kullanıcıları iki rolden birine ekleyebilirsiniz:

* **Kullanıcı** - Çalışma alanı kullanıcısı çalışma alanında denemeler, veri kümeleri vb. oluşturabilir, açabilir, değiştirebilir ve silebilir.
* **Sahibi** - Bir kullanıcının yapabileceklerine ek olarak, bir kullanıcı çalışma alanındaki kullanıcıları davet edebilir ve kaldırabilir.

> [!NOTE]
> Çalışma alanını oluşturan yönetici hesabı çalışma alanı sahibi olarak otomatik olarak çalışma alanına eklenir. Ancak, bu abonelikteki diğer yöneticilere veya kullanıcılara çalışma alanına otomatik olarak erişim izni verilmez - bunları açıkça davet etmeniz gerekir.
> 
> 

### <a name="to-share-a-studio-classic-workspace"></a>Studio (klasik) çalışma alanını paylaşmak için

1. Machine Learning Studio 'da (klasik) oturum açın[https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. Sol panelde **AYARLAR'ı** tıklatın

3. **KULLANIMCIlar** sekmesini tıklatın

4. Sayfanın altındaki **DAHA FAZLA KULLANICI DAVET** ET'E tıklayın

    ![Stüdyo ayarları](./media/create-workspace/settings.png)

5. Bir veya daha fazla e-posta adresi girin. Kullanıcıların geçerli bir Microsoft hesabına veya kuruluş hesabına (Azure Active Directory'den) ihtiyacı vardır.

6. Kullanıcıları Sahibi veya Kullanıcı olarak eklemek isteyip istemediğinizi seçin.

7. **Tamam** onay işareti düğmesini tıklatın.

Eklediğiniz her kullanıcı, paylaşılan çalışma alanında nasıl oturum açAcağınıza ilişkin talimatları içeren bir e-posta alır.

> [!NOTE]
> Kullanıcıların bu çalışma alanında web hizmetlerini dağıtabilmeleri veya yönetebilmeleri için Azure aboneliğine katkıda bulunan veya yönetici olmaları gerekir. 



