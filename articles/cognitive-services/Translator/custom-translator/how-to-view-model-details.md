---
title: Model ayrıntılarını görüntüleyin - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Herhangi bir proje altında modeller sekmesi, model adı, model durumu, BLEU puanı, eğitim, ayar, cümle sayısını test etme gibi her modelin ayrıntılarını gösterir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 64f446c3b331c1aa6ddaae9081b7f61943f74ab2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68595573"
---
# <a name="view-model-details"></a>Model ayrıntılarını görüntüleme

Proje altındaki Modeller sekmesi, projedeki tüm modelleri gösterir. Bu proje için eğitilmiş tüm modeller bu sekmede listelenir.

Projedeki her model için bu ayrıntılar görüntülenir.

1.  Model Adı: Belirli bir modelin model adını gösterir.

2.  Durum: Belirli bir modelin durumunu gösterir. Yeni eğitiminiz kabul edilene kadar Submitted statüsüne sahip olacaktır. Hizmet belgelerinizin içeriğini değerlendirirken durum Veri işlemeye göre değişir. Belgelerinizin değerlendirilmesi tamamlandığında durum Running olarak değişecektir ve sizin için otomatik olarak oluşturulan ayar ve test kümeleri de dahil olmak üzere eğitimin bir parçası olan cümle sayısını görebilirsiniz. Aşağıda, modellerin durumunu açıklayan model durumunun bir listesi verilmiştir.

    -  Gönderilen: Arka uç, o modele ait belgeleri işletirdiğini belirtir.

    -  TrainingQueued: Eğitimin bu model için MT sistemine sıralandığını belirtir.

    -  Çalıştırma: Eğitimin bu model için MT sisteminde çalıştığını belirtir.

    -  Başarılı: Eğitimin MT sisteminde başarılı olduğunu ve bir modelin mevcut olduğunu belirtir. Bu durumda, bu model için bir BLEU puanı görüntülenir.

    -  Dağıtılan: Başarılı eğitilmiş modelin dağıtım için MT sistemine gönderildiğini belirtir.

    -  Dağıtma: Dağıtılan modelin dağıtılmayı amaçladığını belirtir.

    -  Dağıtılmamış: Bir modelin dağıtımını tamamlama işleminin başarıyla tamamlandığını belirtir.

    -  Eğitim Başarısız: Eğitimin başarısız olduğunu belirtir. Bir eğitim hatası oluşursa, eğitim işini yeniden deneyin. Hata devam ederse, bize ulaşın. Başarısız modeli silmeyin.

    - DataProcessingFailed: Modele ait bir veya daha fazla belge için veri işlemenin başarısız olduğunu belirtir.

    - DeploymentFailed: Model dağıtımının başarısız olduğunu belirtir.

    - MigratedDraft: Modelin Hub'dan Özel Çevirmen'e geçişten sonra taslak durumunda olduğunu belirtir.

4.  BLEU Puanı: Modelin BLEU (İki Dilli Değerlendirme Yedeği) puanını gösterir ve çeviri sisteminizin kalitesini gösterir. Bu puan, bu eğitimden kaynaklanan çeviri sistemi tarafından yapılan çevirilerin test veri kümesindeki başvuru cümlelerine ne kadar yakın olduğunu gösterir. Eğitim başarıyla tamamlanırsa BLEU puanı görüntülenir. Eğitim tamamlanmazsa/ başarısız olursa, bleu skoru görmezsiniz.

5.  Eğitim Cümlesi sayısı: Eğitim kümesi olarak kullanılan toplam cümle sayısını gösterir.

6.  Ayarlama Cümle sayısı: Ayarlama kümesi olarak kullanılan toplam cümle sayısını gösterir.

7.  Eğitim Cümlesi sayısı: Test kümesi olarak kullanılan toplam cümle sayısını gösterir.

8.  Mono Cümle sayısı: Mono küme olarak kullanılan toplam cümle sayısını gösterir.

9.  Eylem düğmesini dağıt: Başarılı bir şekilde eğitilmiş bir model için, dağıtılmazsa "Dağıt" düğmesini gösterir. Bir model dağıtılırsa, "Dağıtma" düğmesi gösterilir.

10. Sil: Modeli silmek istiyorsanız bu düğmeyi kullanabilirsiniz. Bir modeli silmek, bu modeli oluşturmak için kullanılan belgelerin hiçbirini silmez.

    ![Model ayrıntılarını görüntüleme](media/how-to/how-to-view-model-details.png)

>[!Note]
>Aynı sistemler için ardışık eğitimleri karşılaştırmak için, ayarlama kümesini ve test kümesini sabit tutmak önemlidir.

## <a name="view-model-training-details"></a>Model eğitim ayrıntılarını görüntüleme

Eğitiminiz tamamlandığında, eğitimle ilgili ayrıntıları ayrıntılar sayfasından inceleyebilirsiniz. Bir proje seçin, modeller sekmesini bulun ve seçin ve bir model seçin.

Model sayfasının iki sekmesi vardır: Eğitim ayrıntıları ve Test.

1.  **Eğitim Detayları:** Bu sekme, eğitimde kullanılan belge(ler) listesini gösterir:

    -  Belge Adı: Bu alan belgenin adını gösterir

    -  Belge Türü: Bu alan, bu belgenin paralel/mono olup olmadığını gösterir.

    -  Kaynak dilde cümle sayısı: Bu alan, kaynak dilin bir parçası olarak cümle sayısının olduğunu gösterir.

    -  Hedef dilde cümle sayısı: Bu alan, hedef dilin bir parçası olarak cümle sayısının olduğunu gösterir.

    -  Hizalanmış Cümleler: Bu alan, hizalama işlemi sırasında Özel Çevirmen tarafından hizalanmış cümle sayısının olduğunu gösterir.

    -  Kullanılan Cümleler: Bu alan, bu eğitim sırasında Custom Translator tarafından kullanılan cümle sayısını gösterir.

    ![Model eğitim detayları](media/how-to/how-to-model-training-details.png)

2.  **Test:** Bu sekme, başarılı bir eğitim için test ayrıntılarını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Test sonuçlarını](how-to-view-system-test-results.md) gözden geçirin ve eğitim sonuçlarını analiz edin.
