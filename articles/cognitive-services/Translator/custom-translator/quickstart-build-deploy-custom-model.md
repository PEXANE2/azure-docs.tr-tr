---
title: 'Quickstart: Özel bir model oluşturun, dağıtın ve kullanın - Özel Çevirmen'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Özel Çevirmen'i kullanarak bir çeviri sistemi oluşturma işleminden adım adım geçersiniz.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 12/09/2019
ms.author: swmachan
ms.topic: quickstart
ms.openlocfilehash: a22ae3b4b11a4eac7da147cf3ad896bbad435448
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80370251"
---
# <a name="quickstart-build-deploy-and-use-a-custom-model-for-translation"></a>Hızlı başlatma: Çeviri için özel bir model oluşturun, dağıtın ve kullanın

Bu makalede, Özel Çevirmen ile bir çeviri sistemi oluşturmak için adım adım talimatlar sağlar.

## <a name="prerequisites"></a>Ön koşullar

1. [Özel Çevirmen](https://portal.customtranslator.azure.ai) Portalı'nı kullanmak için oturum açabilmek için bir [Microsoft hesabına](https://signup.live.com) veya [Azure REKLAM hesabına](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure'da barındırılan kuruluş hesabı) ihtiyacınız olacaktır.

2. Azure portalı üzerinden Çevirmen Metin API aboneliği. Özel Çevirmen'de çalışma alanınızla ilişkilendirmek için Çevirmen Metin API abonelik anahtarına ihtiyacınız olacaktır. [Çevirmen Metin API'sine nasıl kaydolunan](https://docs.microsoft.com/azure/cognitive-services/translator/translator-text-how-to-signup)bakın.

3. Yukarıdakilerin her ikisine de sahip olduğunuzda, çalışma alanları, projeler, dosya yükleme ve modeller oluşturma/dağıtma oluşturmak için [Özel Çevirmen](https://portal.customtranslator.azure.ai) portalında oturum açın.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

İlk kez kullanıcı ysanız, bir çalışma alanı oluşturmak ve çalışma alanınızı Microsoft Translator Text API aboneliğiyle ilişkilendirmek için Hizmet Koşullarını kabul etmeniz istenir.

![Çalışma alanı](media/quickstart/terms-of-service.png)
![oluşturma](media/quickstart/create-workspace-1.png)
![Çalışma alanı](media/quickstart/create-workspace-2.png)
![oluşturun](media/quickstart/create-workspace-3.png)
![Çalışma alanı](media/quickstart/create-workspace-4.png)
![oluşturun](media/quickstart/create-workspace-5.png)
![Çalışma Alanı Oluşturun Çalışma Alanı Oluşturun Çalışma Alanı Oluşturun](media/quickstart/create-workspace-6.png)

Özel Çevirmen portalına sonraki ziyaretlerde, çalışma alanınızı yönetebileceğiniz, daha fazla çalışma alanı oluşturabileceğiniz, Microsoft Translator Text API abonelik anahtarınızı çalışma alanlarınızın izleyebileceği, ortak larını ekleyebileceği ve abonelik anahtarı.

## <a name="create-a-project"></a>Proje oluşturma

Özel Çevirmen portalı açılış sayfasında Yeni Proje'yi tıklatın. İletişim kutusunda istediğiniz proje adını, dil çiftini ve kategorinizi ve diğer ilgili alanları girebilirsiniz. Sonra, projenizi kaydedin. Daha fazla bilgi için Project Oluştur'u ziyaret [edin.](how-to-create-project.md)

![Proje oluşturma](media/quickstart/ct-how-to-create-project.png)


## <a name="upload-documents"></a>Belgeleri karşıya yükleme

Ardından, yükleme [eğitimi,](training-and-model.md#training-document-type-for-custom-translator) [ayar](training-and-model.md#tuning-document-type-for-custom-translator) ve [test](training-and-model.md#testing-dataset-for-custom-translator) belge kümeleri. Hem [paralel](what-are-parallel-documents.md) hem de açılan belgeleri yükleyebilirsiniz. Ayrıca [sözlük](what-is-dictionary.md)yükleyebilirsiniz.

Belgeler sekmesinden veya belirli bir projenin sayfasından belgeleri yükleyebilirsiniz.

![Belgeleri karşıya yükleme](media/quickstart/ct-how-to-upload.png)

Belge yüklerken belge türünü (eğitim, atoncaya veya sınama) ve dil çiftini seçin. Paralel belgeler yüklerken, bir belge adı ayrıca belirtmeniz gerekir. Daha fazla bilgi [için, Yükleme belgesini ziyaret edin.](how-to-upload-document.md)

## <a name="create-a-model"></a>Bir model oluşturma

Gerekli tüm belgeleriniz yüklendiğinde bir sonraki adım modelinizi oluşturmaktır.

Oluşturduğunuz projeyi seçin. Yüklediğiniz ve bu projeyle bir dil çiftini paylaşan tüm belgeleri görürsünüz. Modelinize dahil olmasını istediğiniz belgeleri seçin. [Eğitim,](training-and-model.md#training-document-type-for-custom-translator) [ayar](training-and-model.md#tuning-document-type-for-custom-translator)ve [test](training-and-model.md#testing-dataset-for-custom-translator) verilerini seçebilir veya yalnızca eğitim verilerini seçebilir ve Özel Çevirmen'in modeliniz için otomatik olarak ayar ve test setleri oluşturmasına izin verebilirsiniz.

![Bir model oluşturma](media/quickstart/ct-how-to-train.png)

İstediğiniz belgeleri seçmeyi bitirdikten sonra, modelinizi oluşturmak ve eğitime başlamak için Model Oluştur düğmesini tıklatın. Eğitiminizin durumunu ve eğittiğiniz tüm modellerin ayrıntılarını Modeller sekmesinde görebilirsiniz.

Daha fazla bilgi için [Model Oluştur'u](how-to-train-model.md)ziyaret edin.

## <a name="analyze-your-model"></a>Modelinizi analiz edin

Eğitiminiz başarıyla tamamlandıktan sonra sonuçları inceleyin. BLEU puanı, çevirinizin kalitesini gösteren bir ölçümdür. Ayrıca, özel modelinizle yapılan çevirileri "Test" sekmesine gidip "Sistem Sonuçları"na tıklayarak test setinizde sağlanan çevirilerle el ile karşılaştırabilirsiniz. Bu çevirilerden birkaçını el ile incelemek, sisteminiz tarafından üretilen çevirikalitesi hakkında size iyi bir fikir verecektir. Daha fazla bilgi için Sistem Test Sonuçları'nı ziyaret [edin.](how-to-view-system-test-results.md)

## <a name="deploy-a-trained-model"></a>Eğitimli bir model dağıtma

Eğitimli modelinizi dağıtmaya hazır olduğunuzda "Dağıt" düğmesini tıklatın. Proje başına dağıtılan bir modele sahip olabilir ve dağıtımınızın durumunu Durum sütununda görüntüleyebilirsiniz. Daha fazla bilgi için [Model Dağıtım'ı](how-to-view-system-test-results.md#deploy-a-model) ziyaret edin

![Eğitimli bir model dağıtma](media/quickstart/ct-how-to-deploy.png)

## <a name="use-a-deployed-model"></a>Dağıtılan bir model kullanma

Dağıtılan modellere [CategoryID'yi belirterek](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)Microsoft Translator Text API V3 üzerinden erişilebilir. Çevirmen Metin API'si hakkında daha fazla bilgiyi [API Başvuru](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) web sayfasında bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Özel Çevirmen çalışma alanında nasıl gezinecek lerini ve projelerinizi nasıl [yönettünüzü](workspace-and-project.md)öğrenin.
