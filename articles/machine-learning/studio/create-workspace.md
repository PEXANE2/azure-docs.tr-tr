---
title: Çalışma alanı oluşturma
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) kullanmak için bir Machine Learning Studio (klasik) çalışma alanınız olması gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.custom: seodec18
ms.date: 12/07/2017
ms.openlocfilehash: 419fbd987c1deb95a7c903d85105958443d06b01
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153613"
---
# <a name="create-and-share-an-azure-machine-learning-studio-classic-workspace"></a>Azure Machine Learning Studio (klasik) çalışma alanı oluşturma ve paylaşma

Azure Machine Learning Studio (klasik) kullanmak için bir Machine Learning Studio (klasik) çalışma alanınız olması gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.

## <a name="create-a-studio-classic-workspace"></a>Studio (klasik) çalışma alanı oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açma

    > [!NOTE]
    > Oturum açmak ve bir Studio (klasik) çalışma alanı oluşturmak için bir Azure abonelik yöneticisi olmanız gerekir. 
    >
    > 

2. **+ Yeni** seçeneğine tıklayın

3. Arama kutusuna **Machine Learning Studio (klasik) çalışma alanı** yazın ve eşleşen öğeyi seçin. Ardından sayfanın alt kısmındaki **Oluştur** ' a tıklayın ' ı seçin.

4. Çalışma alanı bilgilerinizi girin:

   - *Çalışma alanı adı* en fazla 260 karakter uzunluğunda olabilir ve bir boşluk ile sonlanmaz. Ad şu karakterleri içeremez: `< > * % & : \ ? + /`
   - Seçtiğiniz ilişkili *fiyatlandırma katmanıyla* birlikte seçtiğiniz (veya oluşturduğunuz) *Web hizmeti planı* , Web hizmetlerini bu çalışma alanından dağıtırsanız kullanılır.

     ![Yeni bir Studio (klasik) çalışma alanı oluştur](./media/create-workspace/create-new-workspace.png)

5. **Oluştur**'a tıklayın.

> [!NOTE]
> Machine Learning Studio (klasik), iş akışını yürütürken ara verileri kaydetmek için sağladığınız bir Azure Depolama hesabını kullanır. Çalışma alanı oluşturulduktan sonra, depolama hesabı silinirse veya erişim anahtarları değiştirilirse çalışma alanı çalışmayı durdurur ve bu çalışma alanındaki tüm denemeleri başarısız olur.
Depolama hesabını yanlışlıkla silerseniz, silinen depolama hesabıyla aynı bölgede aynı ada sahip depolama hesabını yeniden oluşturun ve erişim tuşunu yeniden eşitleyin. Depolama hesabının erişim anahtarlarını değiştirdiyseniz, Azure portalını kullanarak çalışma alanındaki erişim anahtarlarını yeniden eşitleyin.

Çalışma alanı dağıtıldıktan sonra, Machine Learning Studio (klasik) içinde açabilirsiniz.

1. [https://studio.azureml.net/](https://studio.azureml.net/)Machine Learning Studio (klasik) konumuna gidin.

2. Üst sağ taraftaki köşede çalışma alanınızı seçin.

    ![Çalışma alanını seçme](./media/create-workspace/open-workspace.png)

3. **Denemeleri**tıklayın.

    ![Açık denemeleri](./media/create-workspace/my-experiments.png)

Studio (klasik) çalışma alanınızı yönetme hakkında daha fazla bilgi için bkz. [Azure Machine Learning Studio (klasik) çalışma alanını yönetme](manage-workspace.md).
Çalışma alanınızı oluştururken bir sorunla karşılaşırsanız bkz. [sorun giderme kılavuzu: oluşturma ve Machine Learning Studio (klasik) çalışma alanı ile bağlanma](troubleshooting-creating-ml-workspace.md).


## <a name="share-an-azure-machine-learning-studio-classic-workspace"></a>Azure Machine Learning Studio (klasik) çalışma alanını paylaşma
Machine Learning Studio (klasik) çalışma alanı oluşturulduktan sonra, çalışma alanınıza ve tüm denemeleri, veri kümelerine, Notebook, vb. erişimi paylaşmak için kullanıcıları çalışma alanınıza davet edebilirsiniz. Kullanıcıları, iki rolden birine ekleyebilirsiniz:

* **Kullanıcı** -bir çalışma alanı kullanıcısı, çalışma alanında denemeleri, veri kümeleri, vb. oluşturabilir, açabilir, değiştirebilir ve silebilir.
* **Sahip** -bir sahip, kullanıcının ne yapabileceklerini ek olarak çalışma alanındaki kullanıcıları davet edebilir ve kaldırabilir.

> [!NOTE]
> Çalışma alanını oluşturan yönetici hesabı, çalışma alanı sahibi olarak çalışma alanına otomatik olarak eklenir. Ancak, diğer Yöneticiler veya kullanıcılar bu abonelikte çalışma alanına erişim otomatik olarak verilmez - açıkça davet etmek gerekir.
> 
> 

### <a name="to-share-a-studio-classic-workspace"></a>Bir Studio (klasik) çalışma alanını paylaşmak için

1. [https://studio.azureml.net/Home](https://studio.azureml.net/Home) Machine Learning Studio (klasik) oturum açın

2. Sol bölmede, **Ayarlar** ' a tıklayın.

3. **Kullanıcılar** sekmesine tıklayın

4. Sayfanın alt kısmındaki **daha fazla kullanıcı davet et** ' e tıklayın

    ![Studio ayarları](./media/create-workspace/settings.png)

5. Bir veya daha fazla e-posta adreslerini girin. Kullanıcıların geçerli bir Microsoft hesabı veya bir kuruluş hesabı (Azure Active Directory'den) gerekir.

6. Sahibi veya kullanıcı kullanıcılar eklemek isteyip istemediğinizi seçin.

7. **Tamam** onay işareti düğmesine tıklayın.

Eklediğiniz her bir kullanıcı, paylaşılan çalışma alanına oturum açma yönergelerini içeren bir e-posta alırsınız.

> [!NOTE]
> Kullanıcıların ve bu çalışma alanında web Hizmetleri dağıtımı yapmak için katkıda bulunan veya Azure aboneliğinde yönetici olmaları gerekir. 



