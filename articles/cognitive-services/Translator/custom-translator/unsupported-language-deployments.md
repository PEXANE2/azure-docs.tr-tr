---
title: Desteklenmeyen dil dağıtımları - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Özel Çevirmen'de desteklenmeyen dil çiftleri nasıl dağıtılacanız gösterilmektedir.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/24/2019
ms.author: swmachan
ms.openlocfilehash: 3c5c74fc853b5a2425a58e1704aad43350cba212
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837444"
---
# <a name="unsupported-language-deployments"></a>Desteklenmeyen dil dağıtımları

<!--Custom Translator provides the highest-quality translations possible using the latest techniques in neural machine learning. While Microsoft intends to make neural training available in all languages, there are some limitations that prevent us from being able to offer neural machine translation in all language pairs.-->  

Microsoft Translator Hub'ın yakında emekliye ayrılmasıyla, Microsoft şu anda Hub üzerinden dağıtılan tüm modelleri dağıtmayı kaldıracaktır. Birçoğunuz Hub'da dağıtılan ve dil çiftleri Özel Çevirmen'te desteklenmeyen modellerivar.  Bu durumdaki kullanıcıların içeriklerini tercüme etmek için başvurmamalarını istemiyoruz.

Artık desteklenmeyen modellerinizi Özel Çevirmen aracılığıyla dağıtmanızı sağlayan bir işlemimiz var.  Bu işlem, en son V3 API'sini kullanarak içeriği çevirmeye devam etmenizi sağlar.  Bu modeller, siz dağıtmayı seçene veya dil çifti Özel Çevirmen'te kullanılabilir hale gelene kadar barındırılacaktır.  Bu makalede, desteklenmeyen dil çiftleri ile modelleri dağıtmak için işlem açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Modellerinizin dağıtıma aday olabilmesi için aşağıdaki ölçütleri karşılamaları gerekir:
* Modeli içeren proje, Geçiş Aracı kullanılarak Hub'dan Özel Çevirmen'e geçirilmiş olmalıdır.  Geçiş projeleri ve çalışma alanları için işlem [burada](how-to-migrate.md)bulunabilir.
* Geçiş olduğunda model dağıtılan durumda olmalıdır.  
* Modelin dil çifti, Özel Çevirmen'de desteklenmeyen bir dil çifti olmalıdır.  Bir dilin İngilizce'ye veya İngilizce'den desteklendiği, ancak çiftin İngilizce içermeyen dil çiftleri, desteklenmeyen dil dağıtımları için adaylardır.  Örneğin, Fransızca'dan Almanca'ya dil çifti için Hub modeli, Fransızca'dan İngilizce'ye ve İngilizce'den Almanca'ya desteklenmiş dil çifti olmasına rağmen desteklenmeyen bir dil çifti olarak kabul edilir.

## <a name="process"></a>İşleme
Dağıtım için aday olan modelleri Hub'dan kaydırdıktan sonra, çalışma alanınız için **Ayarlar** sayfasına giderek ve **desteklenmeyen Çevirmen Hub Eğitimleri** bölümünü göreceğiniz sayfanın sonuna kaydırarak bu modelleri bulabilirsiniz.  Bu bölüm yalnızca yukarıda belirtilen ön koşulları karşılayan projeleriniz varsa görüntülenir.

![Hub'dan nasıl geçirilir?](media/unsupported-language-deployments/unsupported-translator-hub-trainings.jpg)

**Desteklenmeyen Çevirmen Hub Eğitimleri** seçim sayfasında, **İstenmemiş eğitimler** sekmesi dağıtıma uygun modelleri içerir.  Dağıtmak istediğiniz modelleri seçin ve istek gönderin.   30 Nisan dağıtım tarihinden önce, dağıtım için istediğiniz kadar model seçebilirsiniz.
 
![Hub'dan nasıl geçirilir?](media/unsupported-language-deployments/unsupported-translator-hub-trainings-list.jpg)

Sunulduktan sonra, model artık **İstenemeyen eğitimler** sekmesinde kullanılamaz ve bunun yerine **İstenen eğitimler** sekmesinde görünür.  İstediğiniz eğitimleri istediğiniz zaman görüntüleyebilirsiniz.

![Hub'dan nasıl geçirilir?](media/unsupported-language-deployments/request-unsupported-trainings.jpg) 

## <a name="whats-next"></a>Sırada ne var?

Hub kullanımdan kaldırıldıktan ve tüm modeller dağıtılınca dağıtım için seçtiğiniz modeller kaydedilir.  Desteklenmeyen modellerin dağıtımı için istekgöndermek için 24 Mayıs'a kadar süreniz var.  Bu modelleri 15 Haziran'da, çevirmen V3 API üzerinden erişebilecekleri bir noktada dağıtacağız.  Buna ek olarak, 1 Temmuz'a kadar V2 API üzerinden satışa sunulacak.  

Hub'ın amortismanı ile ilgili önemli tarihler hakkında daha fazla bilgi için [burada](https://www.microsoft.com/translator/business/hub/)kontrol edin.
Dağıtıldıktan sonra, normal barındırma ücretleri geçerli olacaktır.  Ayrıntılar için [fiyatlandırmaya](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) bakın.  

Standart Custom Translator modellerinin aksine Hub modelleri yalnızca tek bir bölgede kullanılabilir, bu nedenle çok bölgeli barındırma ücretleri geçerli olmayacaktır.  Dağıtıldıktan sonra, geçirilen Özel Çevirmen projesi aracılığıyla Hub modelinizi istediğiniz zaman dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir model eğit.](how-to-train-model.md)
- Microsoft [Translator Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)üzerinden dağıtılan özel çeviri modelinizi kullanmaya başlayın.
