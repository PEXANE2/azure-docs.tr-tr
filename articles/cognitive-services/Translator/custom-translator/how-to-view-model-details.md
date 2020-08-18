---
title: Model ayrıntılarını görüntüleme-özel çevirici
titleSuffix: Azure Cognitive Services
description: Herhangi bir proje altındaki modeller sekmesi model adı, model durumu, BLEU puanı, eğitim, ayarlama ve test tümcesi sayısı gibi her bir modelin ayrıntılarını gösterir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 52e305f9d50ab0f383810778be681ac41be97c72
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88509815"
---
# <a name="view-model-details"></a>Model ayrıntılarını görüntüleme

Proje altındaki modeller sekmesi, bu projedeki tüm modelleri gösterir. Bu proje için eğitilen tüm modeller bu sekmede listelenmiştir.

Projedeki her model için bu ayrıntılar görüntülenir.

1. Model adı: belirli bir modelin model adını gösterir.

2. Durum: belirli bir modelin durumunu gösterir. Yeni eğitiminizi kabul edilene kadar gönderildi durumuna sahip olur. Hizmet belgelerinizin içeriğini değerlendirirken durum veri işleme olarak değişir. Belgelerinizin değerlendirmesi tamamlandığında durum çalışır olarak değişir ve sizin için otomatik olarak oluşturulan ayarlama ve test kümeleri de dahil olmak üzere eğitimin parçası olan Tümcelerin sayısını görebilirsiniz. Modellerin durumunu açıklayan model durumunun listesi aşağıda verilmiştir.

    - Gönderildi: Arka ucun bu modelin belgelerini işleolduğunu belirtir.

    - Sıraya alındı: eğitimin bu model için MT sistemine sıraya konmakta olduğunu belirtir.

    - Çalışıyor: eğitimin bu model için MT sisteminde çalıştığını belirtir.

    - Başarılı: Bu eğitimin MT sisteminde başarılı olduğunu ve bir modelin kullanılabildiğini belirtir. Bu durumda, bu model için bir BLEU puanı görüntülenir.

    - Dağıtıldı: başarılı eğitilen modelin dağıtım için MT sistemine gönderildiğini belirtir.

    - Dağıtımı geri al: dağıtılan modelin dağıtım kaldırmadığını belirtir.

    - Dağıtılmamış: bir modelin dağıtım işleminin başarıyla tamamlandığını belirtir.

    - Eğitim başarısız oldu: eğitimin başarısız olduğunu belirtir. Bir eğitim hatası oluşursa, eğitim işini yeniden deneyin. Hata devam ederse bizimle iletişim kurun. Başarısız modeli silmeyin.

    - DataProcessingFailed: modele ait bir veya daha fazla belge için veri işlemenin başarısız olduğunu belirtir.

    - DeploymentFailed: model dağıtımının başarısız olduğunu belirtir.

    - MigratedDraft: hub 'dan özel çevirmene geçişten sonra modelin taslak durumunda olduğunu belirtir.

3. BLEU puanı: çeviri sisteminizin kalitesini belirten, modelin BLEU (Iki dilli değerlendirme düşük Incelemesi) sayısını gösterir. Bu puan, bu eğitimin sonucu olan çeviri sistemi tarafından yapılan çevirilerin, test veri kümesindeki başvuru cümleleriyle ne kadar yakın olduğunu söyler. Eğitim başarıyla tamamlandıysanız BLEU puanı görüntülenir. Eğitim tamamlanmamış/başarısız olursa, herhangi bir BLEK puanı görmezsiniz.

4. Eğitim tümce sayısı: eğitim kümesi olarak kullanılan Tümcelerin toplam sayısını gösterir.

5. Cümle sayısını ayarlama: ayarlama kümesi olarak kullanılan Tümcelerin toplam sayısını gösterir.

6.  Eğitim tümce sayısı: test kümesi olarak kullanılan Tümcelerin toplam sayısını gösterir.

7.  Mono tümce sayısı: Mono kümesi olarak kullanılan Tümcelerin toplam sayısını gösterir.

8.  Dağıtım eylemi düğmesi: başarılı bir eğitilen model Için dağıtılmamışsa "Dağıt" düğmesi görüntülenir. Bir model dağıtılmışsa, "dağıtım geri al" düğmesi gösterilir.

9. Sil: modeli silmek istiyorsanız bu düğmeyi kullanabilirsiniz. Bir modelin silinmesi, bu modeli oluşturmak için kullanılan belgelerden hiçbirini silmez.

    ![Model ayrıntılarını görüntüleme](media/how-to/how-to-view-model-details.png)

>[!Note]
>Aynı sistemler için ardışık işlemleri karşılaştırmak için ayarlama kümesi ve test kümesi sabitinden haberdar olmanız önemlidir.

## <a name="view-model-training-details"></a>Model eğitimi ayrıntılarını görüntüleme

Eğitim tamamlandığında, Ayrıntılar sayfasından eğitim hakkındaki ayrıntıları gözden geçirebilirsiniz. Bir proje seçin, modeller sekmesini bulun ve seçin ve bir model seçin.

Model sayfası iki sekmeye sahiptir: eğitim ayrıntıları ve test.

1.  **Eğitim ayrıntıları:** Bu sekme, eğitiminde kullanılan belge (ler) i listesini gösterir:

    -  Belge adı: Bu alan belgenin adını gösterir

    -  Belge türü: Bu alan, bu belgenin paralel/mono olduğunu gösterir.

    -  Kaynak dildeki tümce sayısı: Bu alan, kaynak dilin bir parçası olan Tümcelerin sayısını gösterir.

    -  Hedef dilde tümce sayısı: Bu alan, hedef dilin bir parçası olan Tümcelerin sayısını gösterir.

    -  Hizalanmış cümleler: Bu alan, hizalama işlemi sırasında özel çevirmen tarafından hizalanan cümle sayısını gösterir.

    -  Kullanılan cümleler: Bu alan, bu eğitim sırasında özel çevirmen tarafından kullanılan Tümcelerin sayısını gösterir.

    ![Model eğitimi ayrıntıları](media/how-to/how-to-model-training-details.png)

2.  **Test:** Bu sekmede başarılı bir eğitimin test ayrıntıları gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Test sonuçlarını](how-to-view-system-test-results.md) gözden geçirin ve eğitim sonuçlarını çözümleyin.
