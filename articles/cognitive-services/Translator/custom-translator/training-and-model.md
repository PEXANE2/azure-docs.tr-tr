---
title: Eğitim ve model nedir? - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Model, belirli bir dil çifti için çeviri sağlayan sistemdir. Başarılı bir eğitimin sonucu bir modeldir. Bir modeli eğitirken, birbirini dışlayan üç veri kümesinin veri kümesini ayarlaması, veri kümesini ayarlaması ve veri kümesini sınaması gerekir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: swmachan
ms.openlocfilehash: 71f1e3f460fa58b999af0a60c8cffa90c8ac8cd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219461"
---
# <a name="what-are-trainings-and-models"></a>Eğitimler ve modeller nelerdir?

Model, belirli bir dil çifti için çeviri sağlayan sistemdir.
Başarılı bir eğitimin sonucu bir modeldir. Bir modeli eğitirken, birbirini dışlayan üç belge türü gereklidir: eğitim, atoalma ve test. Sözlük belge türü de sağlanabilir. Lütfen Cümle [hizalama](https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/sentence-alignment#suggested-minimum-number-of-sentences)bakın.

Yalnızca bir eğitim sırasında eğitim verileri sağlanırsa, Özel Çevirmen otomatik olarak ayar ve test verilerini bir araya getirecektir. Eğitim belgelerinizdeki cümlelerin rasgele bir alt kümesini kullanır ve bu cümleleri eğitim verilerinin kendisinden hariç tutar.

## <a name="training-document-type-for-custom-translator"></a>Özel Çevirmen için eğitim belge türü

Eğitim setinde yer alan belgeler, modelinizi oluşturmak için Özel Çevirmen tarafından kullanılır. Eğitim yürütme sırasında, bu belgelerde bulunan cümleler hizalanır (veya eşlenir). Eğitim belgelerinizi oluştururken özgürlüklerinizi alabilirsiniz. Teğetsel bir alaka olduğuna inandığınız belgeleri tek bir modele ekleyebilirsiniz. Yine [bleu (İki dilli Değerlendirme Yedeği) puanı](what-is-bleu-score.md)etkisini görmek için başka onları dışlamak. Ayar kümesini ve test setini sabit tutsanız, eğitim setinin bileşimini denemekten çekinmeyin. Bu yaklaşım, çeviri sisteminizin kalitesini değiştirmenin etkili bir yoludur.

Bir proje içinde birden çok eğitim çalıştırabilir ve tüm eğitim çalışmalarında [BLEU puanlarını](what-is-bleu-score.md) karşılaştırabilirsiniz. Karşılaştırma için birden çok eğitim çalıştırırken, her seferinde aynı tuning/ test verilerinin belirtildiğinden emin olun. Ayrıca sonuçları ["Test"](how-to-view-system-test-results.md) sekmesinde el ile de incelediğinden emin olun.

## <a name="tuning-document-type-for-custom-translator"></a>Özel Çevirmen için belge türünü tonlama

Bu kümede yer alan paralel belgeler, çeviri sistemini en iyi sonuçlar için ayarlamak için Custom Translator tarafından kullanılır.

Ayarlama verileri, çeviri sisteminin tüm parametrelerini ve ağırlıklarını en uygun değerlere ayarlamak için eğitim sırasında kullanılır. Ayar verilerinizi dikkatle seçin: ayar verileri gelecekte çevirmeyi planladığınız belgelerin içeriğini temsil etmelidir. Tuning verileri üretilen çevirilerin kalitesi üzerinde büyük bir etkiye sahiptir. Tuning, çeviri sisteminin aparat verilerinde sağladığınız örneklere en yakın çevirileri sağlamasını sağlar. Atoklama verilerinizde 2500'den fazla cümleye ihtiyacınız yoktur. En iyi çeviri kalitesi için, en temsili cümle seçimini seçerek ayar kümesini el ile seçmeniz önerilir.

Ayarlama kümenizi oluştururken, çevirmeyi beklediğiniz gelecekteki cümlelerin anlamlı ve temsili uzunlukta olan cümleleri seçin. Ayrıca, gelecekteki çevirilerinizde beklediğiniz yaklaşık dağıtımda çevirmeyi istediğiniz sözcük ve tümcecikleri içeren cümleler de seçmelisiniz. Uygulamada, 7 ile 10 kelimelik bir cümle uzunluğu en iyi sonuçları verecektir, çünkü bu cümleler çekimgöstermek ve aşırı karmaşık olmadan önemli bir ifade uzunluğu sağlamak için yeterli bağlam içerir.

Ayarlama kümesinde kullanılacak cümle türünün iyi bir açıklaması düzyazıdır: gerçek akıcı cümleler. Değil tablo hücreleri, şiir, şeylerin listeleri, sadece noktalama, ya da bir cümlede sayılar - normal dil.

Alame verilerinizi el ile seçerseniz, bu veriler, eğitim ve test verilerinizle aynı cümlelerden hiçbirine sahip olmamalıdır. Ayar verileri çevirilerin kalitesi üzerinde önemli bir etkiye sahiptir - cümleleri dikkatle seçin.

Alabildiğiniz verilerinizi ne seçeceğinizi bilmiyorsanız, eğitim verilerini seçin ve Özel Çevirmen'in sizin için alet verilerini seçmesine izin verin. Özel Çevirmen'in ayarlama verilerini otomatik olarak seçmesine izin verdiyseniz, iki dilli eğitim belgelerinizden rastgele bir cümle alt kümesini kullanır ve bu cümleleri eğitim materyalinin kendisinden hariç tutar.

## <a name="testing-dataset-for-custom-translator"></a>Özel Çevirmen için veri kümesini test etme

Test kümesinde yer alan paralel belgeler, BLEU (İki Dilli Değerlendirme Yedeği) puanını hesaplamak için kullanılır. Bu puan, çeviri sisteminizin kalitesini gösterir. Bu puan aslında bu eğitimden kaynaklanan çeviri sistemi tarafından yapılan çevirilerin test veri kümesindeki referans cümlelerle ne kadar yakından eşleştiğinizi gösterir.

BLEU puanı, otomatik çeviri ve referans çevirisi arasındaki deltanın bir ölçüsüdür. Değeri 0 ile 100 arasında değişmektedir. 0 puan, çeviride başvurunun tek bir sözcüğünün bile görünmediğini gösterir. 100 puan, otomatik çevirinin başvuruyla tam olarak eşleştiğini gösterir: aynı sözcük aynı konumdadır. Aldığınız puan, test verilerinin tüm cümleleri için BLEU puan ortalamasıdır.

Test verileri, hedef dil cümlelerinin kaynak hedef çiftinde karşılık gelen kaynak dil cümlelerinin en çok arzu edilen çevirileri olduğu paralel belgeler içermelidir. Alame verilerini oluşturmak için kullandığınız ölçütleri kullanmak isteyebilirsiniz. Ancak, test verilerinin çeviri sisteminin kalitesi üzerinde hiçbir etkisi yoktur. Bu sizin için BLEU puanı oluşturmak için sadece kullanılır.

Test verileri olarak 2.500'den fazla cümleye ihtiyacınız yoktur. Sistemin test kümesini otomatik olarak seçmesine izin verdiyseniz, iki dilli eğitim belgelerinizden rastgele bir cümle alt kümesi ni kullanır ve bu cümleleri eğitim materyalinin kendisinden hariç tutar.

Test kümesinin özel çevirilerini görüntüleyebilir ve bir model içindeki test sekmesine yönlendirerek bunları test setinizde sağlanan çevirilerle karşılaştırabilirsiniz.
