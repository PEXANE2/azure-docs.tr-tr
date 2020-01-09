---
title: Çalışma alanı oluşturma
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) kullanmak için bir Machine Learning Studio (klasik) çalışma alanınız olması gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 12/07/2017
ms.openlocfilehash: 91ba4d1f7d32071cce0de1de528abf02982ce7be
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75427626"
---
# <a name="create-and-share-an-azure-machine-learning-studio-classic-workspace"></a>Azure Machine Learning Studio (klasik) çalışma alanı oluşturma ve paylaşma

Azure Machine Learning Studio (klasik) kullanmak için bir Machine Learning Studio (klasik) çalışma alanınız olması gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.

## <a name="create-a-studio-classic-workspace"></a>Studio (klasik) çalışma alanı oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açma

    > [!NOTE]
    > Oturum açmak ve bir Studio (klasik) çalışma alanı oluşturmak için bir Azure abonelik yöneticisi olmanız gerekir. 
    >
    > 

2. Tıklayın **+ yeni**

3. Arama kutusuna **Machine Learning Studio (klasik) çalışma alanı** yazın ve eşleşen öğeyi seçin. Seç'a tıklayarak **Oluştur** sayfanın alt kısmındaki.

4. Çalışma alanı bilgilerinizi girin:

   - *Çalışma alanı adı* bitiş boşluk olmayan en fazla 260 karakter olabilir. Ad şu karakterleri içeremez: `< > * % & : \ ? + /`
   - *Web hizmeti planı* siz seçin (veya oluşturma), ilişkili birlikte *fiyatlandırma katmanı* seçin, web hizmetleri bu çalışma alanından dağıtırsanız kullanılır.

     ![Yeni bir Studio (klasik) çalışma alanı oluştur](./media/create-workspace/create-new-workspace.png)

5. **Oluştur**'a tıklayın.

> [!NOTE]
> Machine Learning Studio (klasik), iş akışını yürütürken ara verileri kaydetmek için sağladığınız bir Azure Depolama hesabını kullanır. Çalışma alanı oluşturulduktan sonra, depolama hesabı silinirse veya erişim anahtarları değiştirilirse çalışma alanı çalışmayı durdurur ve bu çalışma alanındaki tüm denemeleri başarısız olur.
Depolama hesabını yanlışlıkla silerseniz, silinen depolama hesabıyla aynı bölgede aynı ada sahip depolama hesabını yeniden oluşturun ve erişim tuşunu yeniden eşitleyin. Depolama hesabının erişim anahtarlarını değiştirdiyseniz, Azure portalını kullanarak çalışma alanındaki erişim anahtarlarını yeniden eşitleyin.

Çalışma alanı dağıtıldıktan sonra, Machine Learning Studio (klasik) içinde açabilirsiniz.

1. [https://studio.azureml.net/](https://studio.azureml.net/)Machine Learning Studio (klasik) konumuna gidin.

2. Üst sağ taraftaki köşede çalışma alanınızı seçin.

    ![Çalışma alanını seçme](./media/create-workspace/open-workspace.png)

3. Tıklayın **denemelerim**.

    ![Açık denemeleri](./media/create-workspace/my-experiments.png)

Studio (klasik) çalışma alanınızı yönetme hakkında daha fazla bilgi için bkz. [Azure Machine Learning Studio (klasik) çalışma alanını yönetme](manage-workspace.md).
Çalışma alanınızı oluştururken bir sorunla karşılaşırsanız bkz. [sorun giderme kılavuzu: oluşturma ve Machine Learning Studio (klasik) çalışma alanı ile bağlanma](troubleshooting-creating-ml-workspace.md).


## <a name="share-an-azure-machine-learning-studio-classic-workspace"></a>Azure Machine Learning Studio (klasik) çalışma alanını paylaşma
Machine Learning Studio (klasik) çalışma alanı oluşturulduktan sonra, çalışma alanınıza ve tüm denemeleri, veri kümelerine, Notebook, vb. erişimi paylaşmak için kullanıcıları çalışma alanınıza davet edebilirsiniz. Kullanıcıları, iki rolden birine ekleyebilirsiniz:

* **Kullanıcı** -çalışma alanına kullanıcı oluşturma, açma, değiştirebilir ve silebilirsiniz denemeleri, veri kümeleri, çalışma alanındaki vb.
* **Sahibi** - bir sahip davet edebilir ve hangi kullanıcının ek olarak, çalışma alanınızdaki kullanıcıları kaldırma gerçekleştirebilirsiniz.

> [!NOTE]
> Çalışma alanını oluşturan yönetici hesabı, çalışma alanı sahibi olarak çalışma alanına otomatik olarak eklenir. Ancak, diğer Yöneticiler veya kullanıcılar bu abonelikte çalışma alanına erişim otomatik olarak verilmez - açıkça davet etmek gerekir.
> 
> 

### <a name="to-share-a-studio-classic-workspace"></a>Bir Studio (klasik) çalışma alanını paylaşmak için

1. [https://studio.azureml.net/Home](https://studio.azureml.net/Home) Machine Learning Studio (klasik) oturum açın

2. Sol bölmede bulunan tıklayın **ayarları**

3. Tıklayın **kullanıcılar** sekmesi

4. Tıklayın **daha fazla kullanıcı davet** sayfanın alt kısmındaki

    ![Studio ayarları](./media/create-workspace/settings.png)

5. Bir veya daha fazla e-posta adreslerini girin. Kullanıcıların geçerli bir Microsoft hesabı veya bir kuruluş hesabı (Azure Active Directory'den) gerekir.

6. Sahibi veya kullanıcı kullanıcılar eklemek isteyip istemediğinizi seçin.

7. Tıklayın **Tamam** onay işareti düğmesine.

Eklediğiniz her bir kullanıcı, paylaşılan çalışma alanına oturum açma yönergelerini içeren bir e-posta alırsınız.

> [!NOTE]
> Kullanıcıların ve bu çalışma alanında web Hizmetleri dağıtımı yapmak için katkıda bulunan veya Azure aboneliğinde yönetici olmaları gerekir. 



