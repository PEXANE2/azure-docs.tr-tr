---
title: Sistem test sonuçlarını ve dağıtımı görüntüleyin - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Eğitiminiz başarılı olduğunda, eğitim sonuçlarınızı analiz etmek için sistem testlerini gözden geçirin. Eğitim sonuçlarından memnunsanız, eğitilmiş model için bir dağıtım isteği yerleştirin.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: f5e1a21f24fdd2458d4803d6ed1675455fa5037d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68595609"
---
# <a name="view-system-test-results"></a>Sistem test sonuçlarını görüntüleme

Eğitiminiz başarılı olduğunda, eğitim sonuçlarınızı analiz etmek için sistem testlerini gözden geçirin. Eğitim sonuçlarından memnunsanız, eğitilmiş model için bir dağıtım isteği yerleştirin.

## <a name="system-test-results-page"></a>Sistem test sonuçları sayfası

Bir proje seçin, ardından o projenin modeller sekmesini seçin, kullanmak istediğiniz modeli bulun ve son olarak test sekmesini seçin.

Test sekmesi size şunları gösterir:

1.  **Sistem Test Sonuçları:** Eğitimlerde test sürecinin sonucu. Test işlemi BLEU puanını üretir.

    **Cümle Sayısı:** Test kümesinde kaç paralel cümle kullanıldı.

     **BLEU Puanı:** BleU puanı eğitim tamamlandıktan sonra bir model için oluşturulur.

    **Durum:** Test işleminin tamamlanıp tamamlanmadığını veya sürücü olup oldu

    ![Sistem test sonuçları](media/how-to/how-to-system-test-results.png)

2.  Sistem test sonuçlarına tıklayın ve bu da sonuç ayrıntılarını test etmenizi sağlayacaktır. Bu sayfa, test veri kümesinin bir parçası olan cümlelerin makine çevirisigösterir.

3.  Test sonucu ayrıntıları sayfasındaki tabloda iki sütun vardır - bir çiftteki her dil için. Kaynak dil sütununda çevrilecek cümle gösterilmektedir. Hedef dil sütunu her satırda iki tümce içerir.

    **Hakem:** Bu cümle, test veri kümesinde verilen kaynak cümlenin başvuru çevirisidir.

    **MT:** Bu cümle, eğitim yapıldıktan sonra yapılan model tarafından yapılan kaynak cümlenin otomatik çevirisidir.

    ![Sistem test sonuçları karşılaştırmak](media/how-to/how-to-system-test-results-2.png)

## <a name="download-test"></a>Testi indir

Zip dosyasını indirmek için Çevirileri İndir bağlantısını tıklayın. Zip, test veri kümesindeki kaynak cümlelerin makine çevirilerini içerir.

![Testi indir](media/how-to/how-to-system-test-download.png)

İndirilen bu zip arşivi üç dosya içerir.

1.  **custom.mt.txt:** Bu dosya, kullanıcı verileri ile eğitilmiş model tarafından yapılan hedef dilde kaynak dil cümleleri makine çevirileri içerir.

2.  **ref.txt:** Bu dosya, hedef dilde kaynak dil cümlelerinin kullanıcı tarafından sağlanan çevirilerini içerir.

3.  **kaynak.txt:** Bu dosya kaynak dilde cümleler içerir.

    ![İndirilen sistem test sonuçları](media/how-to/how-to-download-system-test.png)

## <a name="deploy-a-model"></a>Model dağıtma

Dağıtım istemek için:

1.  Bir proje seçin, Modeller sekmesine gidin.

2. Başarılı bir şekilde eğitilmiş bir model için, dağıtılmazsa "Dağıt" düğmesini gösterir.

    ![Modeli dağıt](media/how-to/how-to-deploy-model.png)

3.  Dağıt'a tıklayın.
4.  Modelinizin dağıtılmasını istediğiniz bölge(ler) için **Dağıt'ı** seçin ve Kaydet'i tıklatın. Birden çok bölge için **Dağıtılmış'ı** seçebilirsiniz.

    ![Modeli dağıt](media/how-to/how-to-deploy-model-regions.png)

5.  Modelinizin durumunu "Durum" sütununda görüntüleyebilirsiniz.

>[!Note]
>Custom Translator, herhangi bir zamanda bir çalışma alanı içinde dağıtılan 10 modeli destekler.

## <a name="update-deployment-settings"></a>Dağıtım ayarlarını güncelleştirme

Dağıtım ayarlarını güncelleştirmek için:

1.  Bir proje seçin ve **Modeller** sekmesine gidin.

2. Başarıyla dağıtılan bir model için bir **Güncelleştirme** düğmesi gösterir.

    ![Modeli dağıt](media/how-to/how-to-update-undeploy-model.png)

3.  **Güncelleştir** seçeneğini belirleyin.
4.  Modelinizin dağıtılmasını veya dağıtılmalarını istediğiniz bölge(ler) için **Dağıtılmış** veya **Dağıtılmamış'ı** seçin ve ardından **Kaydet'i**tıklatın.

    ![Modeli dağıt](media/how-to/how-to-undeploy-model.png)

>[!Note]
>Tüm bölgeler için **Dağıtılmamış'ı** seçerseniz, model tüm bölgelerden dağıtılmamış ve dağıtılmamış duruma getirilmiştir. Artık kullanılamıyor.

## <a name="next-steps"></a>Sonraki adımlar

- Microsoft [Translator Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)üzerinden dağıtılan özel çeviri modelinizi kullanmaya başlayın.
- Çalışma alanınızı, abonelik anahtarınızı paylaşmak için [ayarları nasıl yöneteceğinizi](how-to-manage-settings.md) öğrenin.
- Çalışma alanınızı ve projenizi [Microsoft Translator Hub'dan](https://hub.microsofttranslator.com) [nasıl geçirteceklerinizi](how-to-migrate.md) öğrenin
