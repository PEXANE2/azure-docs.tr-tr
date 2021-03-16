---
title: Azure Machine Learning stüdyo nedir?
description: Studio, Azure Machine Learning çalışma alanları için bir Web portalıdır. Studio, kapsamlı bir veri bilimi platformu için kod içermez ve kod ilk deneyimlerini birleştirir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: peterclu
ms.author: peterlu
ms.date: 08/24/2020
adobe-target: true
ms.openlocfilehash: 48c4b2a73628ab2105e23054d747e28acc105d01
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103563201"
---
# <a name="what-is-azure-machine-learning-studio"></a>Azure Machine Learning stüdyo nedir?

Bu makalede, [Azure Machine Learning](overview-what-is-azure-ml.md)veri bilimi geliştiricileri için Web portalı olan Azure Machine Learning Studio hakkında bilgi edineceksiniz. Studio, kapsamlı bir veri bilimi platformu için kod içermez ve kod ilk deneyimlerini birleştirir.

Bu makalede şunları öğreneceksiniz:
>[!div class="checklist"]
> - Studio 'da [makine öğrenimi projelerini yazma](#author-machine-learning-projects) .
> - Studio 'da [varlıkları ve kaynakları yönetme](#manage-assets-and-resources) .
> - [Azure Machine Learning Studio ile ml Studio arasındaki farklar (klasik)](#ml-studio-classic-vs-azure-machine-learning-studio).

İşletim sisteminizle uyumlu en güncel tarayıcıyı kullanmanızı öneririz. Aşağıdaki tarayıcılar desteklenir:
  * Microsoft Edge (yeni Microsoft Edge, en son sürüm. Microsoft Edge eski değil)
  * Safari (en so sürüm, yalnızca Mac)
  * Chrome (en son sürüm)
  * Firefox (en son sürüm)

## <a name="author-machine-learning-projects"></a>Machine Learning projelerini yaz

Studio, tür projesine ve kullanıcı deneyiminin düzeyine bağlı olarak birden çok yazma deneyimi sunar.

+ **Not Defterleri**

  Doğrudan Studio 'da tümleştirilmiş olan yönetilen [Jupyter Notebook sunucularında](how-to-run-jupyter-notebooks.md) kendi kodunuzu yazın ve çalıştırın. 

:::image type="content" source="media/overview-what-is-azure-ml-studio/notebooks.gif" alt-text="Ekran görüntüsü: kodu bir not defterine yazma ve çalıştırma":::

+ **Azure Machine Learning tasarımcısı**

  Tasarımcı kullanarak makine öğrenimi modellerini herhangi bir kod yazmadan eğitme ve dağıtma. ML işlem hatları oluşturmak için veri kümelerini ve modülleri sürükleyip bırakın. [Tasarımcı öğreticisini](tutorial-designer-automobile-price-train-score.md)deneyin.

    ![Azure Machine Learning tasarımcı örneği](media/concept-designer/designer-drag-and-drop.gif)

+ **Otomatik makine öğrenme Kullanıcı arabirimi**

  Kullanımı kolay bir arabirimle [OTOMATIK ml denemeleri](tutorial-first-experiment-automated-ml.md) oluşturma hakkında bilgi edinin. 

  [![Azure Machine Learning Studio gezinti bölmesi](./media/overview-what-is-azure-ml/azure-machine-learning-automated-ml-ui.jpg)](./media/overview-what-is-azure-ml/azure-machine-learning-automated-ml-ui.jpg)

+ **Veri etiketleme**

    Veri etiketleme projelerini etkin bir şekilde koordine etmek için [Azure Machine Learning veri etiketleme](how-to-create-labeling-projects.md) kullanın.

## <a name="manage-assets-and-resources"></a>Varlıkları ve kaynakları yönetme

Makine öğrenimi varlıklarınızı doğrudan tarayıcınızda yönetin. Varlıklar, sorunsuz bir deneyim için SDK ile Studio arasındaki aynı çalışma alanında paylaşılır. Yönetmek için Studio 'yu kullanın:

- Modeller
- Veri kümeleri
- Veri depoları
- İşlem kaynakları
- Notebooks
- Denemeler
- Günlükleri Çalıştır
- Pipelines 
- Ardışık düzen uç noktaları

Deneyimli bir geliştirici olsanız bile, Studio çalışma alanı kaynaklarını nasıl yönetebileceğinizi kolaylaştırabilir.

## <a name="ml-studio-classic-vs-azure-machine-learning-studio"></a>ML Studio (klasik) vs Azure Machine Learning Studio

2015 ' de yayınlanan **ml Studio (klasik)** ilk sürükle ve bırak Machine Learning kurucumız vardı. 

**Ml Studio (klasik)** yalnızca bir görsel deneyim sunan bir tek başına hizmettir. Studio (klasik) Azure Machine Learning ile birlikte çalışmıyor.

**Azure Machine Learning** , tamamen bir veri bilimi platformu sunan ayrı ve modernlenmiş bir hizmettir. Hem kod hem de düşük kod deneyimlerini destekler.

**Azure Machine Learning Studio** , proje yazma ve varlık yönetimi için düşük kod ve kod içermeyen seçenekler *içeren Azure Machine Learning bir* Web portalıdır. 

Yeni kullanıcıların, en son veri bilimi araçları aralığı için ML Studio (klasik) yerine **Azure Machine Learning** seçmesini öneririz. Mevcut bir ML Studio (klasik) Kullanıcı kullanıyorsanız, [Azure Machine Learning geçiş](classic/migrate-overview.md)yapmayı düşünün.

Azure Machine Learning geçiş avantajlarından bazıları şunlardır:

- Büyük ölçekli eğitim için ölçeklenebilir işlem kümeleri.
- Kurumsal Güvenlik ve idare.
- Popüler açık kaynak araçları ile birlikte çalışabilir.
- Uçtan uca MLOps.

### <a name="feature-comparison"></a>Özellik karşılaştırması

[!INCLUDE [aml-compare-classic](../../includes/machine-learning-compare-classic-aml.md)]

## <a name="troubleshooting"></a>Sorun giderme

* **Studio 'Da eksik kullanıcı arabirimi öğeleri** Azure rol tabanlı erişim denetimi, Azure Machine Learning gerçekleştirebileceğiniz eylemleri kısıtlamak için kullanılabilir. Bu kısıtlamalar, Kullanıcı arabirimi öğelerinin Azure Machine Learning Studio 'da görünmesini engelleyebilir. Örneğin, bir işlem örneği oluşturmayan bir rol atanırsa, işlem örneği oluşturma seçeneği Studio 'da görünmez. Daha fazla bilgi için bkz. [kullanıcıları ve rolleri yönetme](how-to-assign-roles.md).

## <a name="next-steps"></a>Sonraki adımlar

[Studio 'yu](https://ml.azure.com)ziyaret edin veya bu öğreticilerle farklı yazma seçeneklerini keşfedebilirsiniz:  

- + [Kendi geliştirme ortamınızı kullanmaya başlayın](tutorial-1st-experiment-sdk-setup-local.md)
  + [& modellerini eğitmek için bir işlem örneğinde Jupyıter not defterlerini kullanma](tutorial-1st-experiment-sdk-setup.md)
  + [& dağıtım modellerini eğitme için otomatik makine öğrenimi kullanma](tutorial-first-experiment-automated-ml.md)  
  + [& modellerini eğitmek için tasarımcıyı kullanma](tutorial-designer-automobile-price-train-score.md)
  + [Güvenli bir sanal ağda Studio 'yu kullanma](how-to-enable-studio-virtual-network.md)