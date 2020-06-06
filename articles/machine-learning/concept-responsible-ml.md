---
title: Sorumlu Machine Learning (ML)
titleSuffix: Azure Machine Learning
description: Sorumlu ML 'nin ne olduğunu ve nasıl kullanılacağını öğrenin Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 05/08/2020
ms.openlocfilehash: acf329092d082b9e9cece451e90e96fc0e24fa5c
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447841"
---
# <a name="responsible-machine-learning-ml"></a>Sorumlu Machine Learning (ML)

Bu makalede, sorumlu ML 'nin ne olduğunu ve Azure Machine Learning ile uygulamaya nasıl koyabileceğinizi öğreneceksiniz.

AI sistemlerinin geliştirilmesi ve kullanımı boyunca güvenin çekirdek üzerinde olması gerekir. Platformda, süreçte ve modellerde güvenin. Microsoft 'ta, sorumlu ML aşağıdaki değerleri ve ilkeleri kapsar:

- Makine öğrenimi modellerini anlama
  - Model davranışını yorumlama ve açıklama
  - Model unfairness değerlendir ve azaltma
- Kişileri ve verilerini koruyun
  - Değişiklik gizliliğiyle veri pozlamasını önleyin  
- Uçtan uca makine öğrenimi işlemini denetleme
  - Machine Learning yaşam döngüsünü veri sayfaları ile belgeleme

:::image type="content" source="media/concept-responsible-ml/responsible-ml-pillars.png" alt-text="Sorumlu ML pilde":::

Yapay zeka ve özerk sistemler topluluğu dokusuna daha fazlasını tümleştirmekle, bu teknolojilerin istenmeden sonuçlý sonuçlarını tahmin etmek ve azaltmak için önceden önlem olarak bir çaba olması önemlidir.

## <a name="interpret-and-explain-model-behavior"></a>Model davranışını yorumlama ve açıklama

Sistem geliştiricileri, düzenleyiciler, kullanıcılar ve iş karar mekanizmalarının, sistemlerin neden belirli kararlar vermesini anlayabilmesi için zor olan veya kara kutu sistemleri sorunlu olabilir. Bazı AI sistemleri diğerlerinden daha explainable ve bazı durumlarda, daha yüksek doğruluk ve daha fazla explainable olan bir sistem arasında bir zorunluluğunu getirir.

Intertable AI sistemleri oluşturmak için, Microsoft tarafından oluşturulan açık kaynaklı bir paket olan [ınterbir](https://github.com/interpretml/interpret)Package kullanın. Inter, [otomatik makine öğrenimi modelleri](how-to-machine-learning-interpretability-automl.md)de dahil olmak üzere [makine öğrenimi modellerinizi yorumlamak ve açıklamak](how-to-machine-learning-interpretability-aml.md)için, [Azure Machine Learning içinde yorumbir şekilde kullanılabilir](how-to-machine-learning-interpretability.md) .

## <a name="assess-and-mitigate-model-unfairness"></a>Model unfairness değerlendir ve azaltma

AI sistemleri, topluluğu 'in günlük karar verme konusunda daha fazla sonuç aldığından, bu sistemlerin herkese açık sonuçlar sağlamak için iyi bir öneme sahip olma.

AI sistemlerindeki Unfairness, aşağıdaki istenmeyen sonuçlara neden olabilir:

- Vergi fırsatları, kaynaklar veya kişilerden gelen bilgiler.
- Sapmaları ve stereotipleri yeniden zorluyor.

Eşitliği 'in birçok yönü yakalanamaz veya ölçümler tarafından gösterilemez. AI sistemlerinin tasarımında ve geliştirilmesinde eşitliği iyileştirebilecek araçlar ve uygulamalar vardır.

AI sistemlerinde unfairness azaltmada iki temel adım değerlendirme ve risk azaltma. AI sistemlerinin olası unfairness 'i değerlendirebileceği ve azaltabilen açık kaynaklı bir paket olan [FairLearn](https://github.com/fairlearn/fairlearn)önerilir. Eşitliği ve FairLearn paketi hakkında daha fazla bilgi edinmek için bkz. [ml 'de eşitliği makalesi](./concept-fairness-ml.md).

## <a name="prevent-data-exposure-with-differential-privacy"></a>Değişiklik gizliliğiyle veri pozlamasını önleyin

Analiz için veriler kullanıldığında, verilerin kullanımı boyunca özel ve gizli olarak kalması önemlidir. Değişiklik gizliliği, kişilerin verilerinin güvende ve özel olmasını sağlamaya yardımcı olan bir sistem ve uygulamalar kümesidir.

Geleneksel senaryolarda, ham veriler dosya ve veritabanlarında depolanır. Kullanıcılar verileri çözümlerse genellikle ham verileri kullanır. Bu sorun, bir kişinin gizliliğine infringe olabileceğinden bir kaygıdır. Değişiklik gizliliği, kullanıcıların herhangi bir veri noktasını tanımlayamaması için verilere "gürültü" veya rasgelelik ekleyerek bu sorunla başa geçmeye çalışır.

Farklı şekilde özel sistemlerin uygulanması zordur. [Whitenoıse](https://github.com/opendifferentialprivacy/whitenoise-core) , küresel farklı özel sistemler oluşturmak için farklı bileşenler içeren açık kaynaklı bir projem. Değişiklik gizliliği ve Whitenoıse projesi hakkında daha fazla bilgi edinmek için, [değişiklik gizliliği ve çalışırken veri gizliliğini koruma](./concept-differential-privacy.md) makalesini inceleyin.

## <a name="document-the-machine-learning-lifecycle-with-datasheets"></a>Machine Learning yaşam döngüsünü veri sayfaları ile belgeleme

Machine Learning işleminde doğru bilgilerin belgelenerek, her aşamada sorumlu kararlar alma anahtarıdır. Veri sayfaları, makine öğrenimi yaşam döngüsünün bir parçası olarak kullanılan ve oluşturulan makine öğrenimi varlıklarını belgelemek için bir yoldur.

Modeller "kara kutular" olarak düşünüldük ve genellikle bunlar hakkında çok az bilgi vardır. Machine Learning sistemleri daha fazla bilgi edintiğinden ve karar vermek için kullanıldığından, veri sayfalarını kullanmak, daha sorumlu makine öğrenimi sistemleri geliştirmeye yönelik bir adımdır.

Bir veri sayfasının parçası olarak belgelemek isteyebileceğiniz bazı model bilgileri:

- Amaçlanan kullanım
- Model mimarisi
- Kullanılan eğitim verileri
- Kullanılan değerlendirme verileri
- Eğitim modeli performans ölçümleri
- Eşitliği bilgileri.

[Modeller için veri sayfalarını](https://github.com/microsoft/MLOps/blob/master/pytorch_with_datasheet/model_with_datasheet.ipynb)uygulamak üzere Azure Machine Learning SDK 'nın nasıl kullanılacağını öğrenmek için aşağıdaki örneğe bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [Şifrelenmiş bir ınırtıon Web hizmeti dağıtmak](how-to-homomorphic-encryption-seal.md)için homomorphic şifrelemesi kullanın.
- Machine Learning sistem belgeleri [hakkında](https://www.partnershiponai.org/about-ml/) daha fazla bilgi için bkz..