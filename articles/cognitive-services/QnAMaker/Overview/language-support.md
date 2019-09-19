---
title: Dil desteği - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Bir Bilgi Bankası 'nın dili, kaynaklardan gelen soruların ve yanıtların otomatik olarak ayıklanabileceği Soru-Cevap Oluşturma ve sonuçların uygunluğunu, Kullanıcı sorgularına yanıt olarak Soru-Cevap Oluşturma etkiler. Kültür, doğal Bilgi Bankası'nda soru-cevap Oluşturucu tarafından desteklenen dillerin listesi. Aynı Bilgi Bankası dillerde karıştırmayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/21/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: bec09e5cd053774902083c6dd042c1ed4b293d20
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71066672"
---
# <a name="language-support-for-qna-maker"></a>Soru-Cevap Oluşturma için dil desteği

Bilgi Bankası dili otomatik ayıklama soruların soru-cevap Oluşturucu'nın özelliğini etkiler ve gelen yanıtları [kaynakları](../Concepts/data-sources-supported.md), soru-cevap Oluşturucu sağlayan kullanıcı sorgularına yanıt sonuçlarının ilgi yanı sıra.

## <a name="auto-extraction"></a>Otomatik ayıklama
Soru-cevap Oluşturucu, herhangi bir dil sayfanın soru/yanıt ayıklama destekler, ancak soru-cevap Oluşturucu soruları tanımlamak için anahtar sözcükler kullandığından ayıklama verimliliğini aşağıdaki diller için çok daha yüksektir.

|Desteklenen diller| Yerel Ayar|
|-----|----|
|Türkçe|tr-*|
|Fransızca|FR-*|
|İtalyanca|BT-*|
|Almanca|de-*|
|İspanyolca|ES-*|

## <a name="primary-language-detection"></a>Birincil dil algılama

Algılama için kullanılan birincil dil, ilk belge veya URL ilk bilgi tabanına eklendiğinde, Soru-Cevap Oluşturma kaynak için ve bu kaynakta oluşturulan tüm bilgi temellerine ayarlanır. Dil değiştirilemez. 

Kullanıcı birden çok dili desteklemeyi planlıyorsa, her dil için yeni bir Soru-Cevap Oluşturma kaynağına sahip olmaları gerekir. [Dil tabanlı soru-cevap oluşturma Bilgi Bankası oluşturma](../how-to/language-knowledge-base.md)hakkında bilgi edinin.  

Birincil dili aşağıdaki adımlarla doğrulayın:

1. [Azure Portal](https://portal.azure.com) oturum açın.  
1. Soru-Cevap Oluşturma kaynağınızın bir parçası olarak oluşturulan Azure Search kaynağını bulup seçin. Azure Search kaynak adı Soru-Cevap Oluşturma kaynakla aynı adla başlayacak ve tür **arama hizmetine**sahip olacaktır. Lütfen tek bir Azure Search kaynağına bağlanabilen tek bir Soru-Cevap Oluşturma kaynağı olduğunu aklınızda bulundurun.
1. Arama kaynağının **genel bakış** sayfasından **dizinler**' i seçin. 
1. **Testkb** dizinini seçin.
1. **Alanlar** sekmesini seçin. 
1. **Sorular** ve **Yanıt** alanları için **çözümleyici** sütununu görüntüleyin. 


## <a name="query-matching-and-relevance"></a>Sorguyla eşleşen ve ilgi düzeyi
Soru-cevap Oluşturucu bağlıdır [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) sonuçları sağlamak için Azure Search'te. Özel yeniden sıralama özellikleri kullanılabilir tr - için * daha iyi uygunluğu sağlayan diller.

Azure arama özellikleri, desteklenen diller için nominal açık olduğunda, soru-cevap Oluşturucu, Azure arama sonuçlarının üstünde yer alan bir ek derecelendiricisini sahiptir. Bu derecelendiricisini modelinde, henüz diğer diller için kullanılamayan, en-* ' de bazı özel anlam ve sözcük tabanlı özellikleri kullanırız. Soru-Cevap Oluşturma Ranker 'ın iç çalışmalarımız kapsamında olduklarından, bu özellikleri kullanmıyoruz. 

Soru-Cevap Oluşturma oluşturma sırasında [Bilgi Bankası 'nın dilini otomatik olarak algılar](#primary-language-detection) ve çözümleyici 'yi buna göre ayarlar. Aşağıdaki dillerde bilgi bankalarından oluşturabilirsiniz. 

|Desteklenen diller|
|-----|
|Arapça|
|Ermenice|
Bengali|
|Bask dili|
|Bulgarca|
|Katalanca|
|Chinese_Simplified|
|Chinese_Traditional|
|Hırvatça|
|Çekçe|
|Danca|
|Felemenkçe|
|Türkçe|
|Estonca|
|Fince|
|Fransızca|
|Galiçya dili|
|Almanca|
|Yunanca|
|Gucerat dili|
|İbranice|
|Hintçe|
|Macarca|
|İzlanda dili|
|Endonezya dili|
|İrlanda dili|
|İtalyanca|
|Japonca|
|Kannada dili|
|Korece|
|Letonca|
|Litvanca|
|Malayalam dili|
|Malay dili|
|Norveççe|
|Lehçe|
|Portekizce|
|Pencap dili|
|Rumence|
|Rusça|
|Serbian_Cyrillic|
|Serbian_Latin|
|Slovakça|
|Slovence|
|İspanyolca|
|İsveççe|
|Tamil dili|
|Telugu dili|
|Tay Dili|
|Türkçe|
|Ukrayna dili|
|Urduca|
|Vietnam dili|
