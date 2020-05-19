---
title: 'Hızlı başlangıç: özel model oluşturma, dağıtma ve kullanma-özel çevirici'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, özel çeviriciyi kullanarak bir çeviri sistemi oluşturmaya yönelik adım adım işleme devam edersiniz.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 12/09/2019
ms.author: swmachan
ms.topic: quickstart
ms.openlocfilehash: c86a387e66cce914f9d0b92793893b0cba08e7bc
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587111"
---
# <a name="quickstart-build-deploy-and-use-a-custom-model-for-translation"></a>Hızlı başlangıç: çeviri için özel model oluşturma, dağıtma ve kullanma

Bu makalede, özel çevirmenle bir çeviri sistemi oluşturmak için adım adım yönergeler sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

1. [Özel çevirmen](https://portal.customtranslator.azure.ai) portalını kullanmak için, oturum açmak için bir [MICROSOFT HESABı](https://signup.live.com) veya [Azure AD hesabına](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure üzerinde barındırılan kuruluş hesabı) ihtiyacınız olacaktır.

2. Azure portal aracılığıyla çeviriciye bir abonelik. Özel çevirmende çalışma alanınız ile ilişkilendirmek için Translator abonelik anahtarına ihtiyacınız olacak. Bkz. [çevirmene kaydolma](https://docs.microsoft.com/azure/cognitive-services/translator/translator-text-how-to-signup).

3. Yukarıdakilerden her ikisine de sahip olduğunuzda, çalışma alanları, projeler, dosyaları karşıya yükleme ve model oluşturma/dağıtma modelleri oluşturmak için [özel çevirmen](https://portal.customtranslator.azure.ai) portalında oturum açın.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

İlk kez Kullanıcı kullanıyorsanız, hizmet koşulları 'nı kabul etmeniz istenir, bir çalışma alanı oluşturup çalışma alanınızı Translator aboneliğiyle ilişkilendirebilirsiniz.

![](media/quickstart/terms-of-service.png)
Çalışma alanı oluşturma çalışma alanı oluştur çalışma alanı oluştur ![ çalışma alanı oluştur ](media/quickstart/create-workspace-1.png)
 ![ ](media/quickstart/create-workspace-2.png)
 ![ ](media/quickstart/create-workspace-3.png)
 ![ ](media/quickstart/create-workspace-4.png)
 ![ çalışma ](media/quickstart/create-workspace-5.png)
 ![ alanı oluştur çalışma alanı oluştur](media/quickstart/create-workspace-6.png)

Özel çevirmen portalının sonraki ziyaretlerde, çalışma alanınızı yönetebileceğiniz ayarlar sayfasına gidin, daha fazla çalışma alanı oluşturun, çevirmen abonelik anahtarınızı çalışma alanlarınız ile ilişkilendirin, ikincil sahipler ekleyin ve bir abonelik anahtarını değiştirin.

## <a name="create-a-project"></a>Proje oluşturma

Özel çevirmen Portalı giriş sayfasında, yeni proje ' ye tıklayın. İletişim kutusunda istenen proje adını, Dil çiftini ve kategorisini, diğer ilgili alanları da girebilirsiniz. Ardından, projenizi kaydedin. Daha fazla ayrıntı için [proje oluştur](how-to-create-project.md)' u ziyaret edin.

![Proje oluşturma](media/quickstart/ct-how-to-create-project.png)


## <a name="upload-documents"></a>Belgeleri karşıya yükleme

Sonra, [eğitim](training-and-model.md#training-document-type-for-custom-translator), [ayarlama](training-and-model.md#tuning-document-type-for-custom-translator) ve [Test](training-and-model.md#testing-dataset-for-custom-translator) belge kümelerini karşıya yükleyin. Hem [paralel](what-are-parallel-documents.md) hem de Birleşik belgeler yükleyebilirsiniz. Ayrıca [Sözlük](what-is-dictionary.md)yükleyebilirsiniz.

Belgeler sekmesinden ya da belirli bir projenin sayfasından belge yükleyebilirsiniz.

![Belgeleri karşıya yükleme](media/quickstart/ct-how-to-upload.png)

Belgeler karşıya yüklenirken belge türünü (eğitim, ayarlama veya test) ve Dil çiftini seçin. Paralel belgeler karşıya yüklenirken Ayrıca bir belge adı belirtmeniz gerekir. Daha fazla ayrıntı için [belgeyi karşıya yükle](how-to-upload-document.md)sayfasını ziyaret edin.

## <a name="create-a-model"></a>Bir model oluşturma

Tüm gerekli belgeleriniz karşıya yüklendiğinde, bir sonraki adım modelinize inşa edilir.

Oluşturduğunuz projeyi seçin. Bu projeyle bir dil çiftini paylaşan tüm karşıya yüklediğiniz belgeleri görürsünüz. Modelinize dahil etmek istediğiniz belgeleri seçin. [Eğitim](training-and-model.md#training-document-type-for-custom-translator), [ayarlama](training-and-model.md#tuning-document-type-for-custom-translator)ve [Test](training-and-model.md#testing-dataset-for-custom-translator) verilerini seçebilir ya da yalnızca eğitim verilerini seçebilir ve özel çeviricisinin modelinizin ayarlama ve test kümelerini otomatik olarak oluşturmasını sağlayabilirsiniz.

![Bir model oluşturma](media/quickstart/ct-how-to-train.png)

İstediğiniz belgeleri seçmeyi bitirdiğinizde model Oluştur düğmesine tıklayarak modelinizi oluşturun ve eğitimi başlatın. Eğitim durumunuzu ve eğitilen tüm modellerin ayrıntılarını modeller sekmesinde görebilirsiniz.

Daha fazla ayrıntı için [model oluşturma](how-to-train-model.md)sayfasını ziyaret edin.

## <a name="analyze-your-model"></a>Modelinizi çözümleyin

Eğitimler başarıyla tamamlandıktan sonra, sonuçları inceleyin. BLEU puanı, çevirinizin kalitesini gösteren bir ölçümdür. Ayrıca, özel modelinize yapılan çevirileri, "test" sekmesine gidip "sistem sonuçları" ' na tıklayarak test kümesinde sunulan çevirilerle el ile karşılaştırabilirsiniz. Bu çevirilerin birkaçının el ile araştırmanız, sisteminiz tarafından üretilen çeviri kalitesi hakkında size iyi bir fikir verecektir. Daha fazla ayrıntı için [sistem test sonuçları](how-to-view-system-test-results.md)ziyaret edin.

## <a name="deploy-a-trained-model"></a>Eğitilen model dağıtma

Eğitilen modelinizi dağıtmaya hazırsanız, "Dağıt" düğmesine tıklayın. Her proje için dağıtılan bir modeliniz olabilir ve dağıtım durumunu durum sütununda görüntüleyebilirsiniz. Daha ayrıntılı bilgi için [model dağıtımı](how-to-view-system-test-results.md#deploy-a-model) ' nı ziyaret edin

![Eğitilen model dağıtma](media/quickstart/ct-how-to-deploy.png)

## <a name="use-a-deployed-model"></a>Dağıtılan bir model kullan

Dağıtılmış modellere, [CategoryID 'yi belirterek Microsoft Translator metin API v3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)aracılığıyla erişilebilir. Çevirmen hakkında daha fazla bilgi için [API başvuru](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) Web sayfasında bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel çevirmen çalışma alanında gezinmeyi ve projelerinizi yönetmeyi](workspace-and-project.md)öğrenin.
