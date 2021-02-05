---
title: Yanıt yayma algılaması kullanılarak kesin yanıt verme-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma yönetilmekte olan kesin yanıt özelliğini anlayın.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 11/09/2020
ms.openlocfilehash: 5dde3da693d87d537fd2177a6f12b55297b5776e
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99582205"
---
# <a name="precise-answering"></a>Hassas yanıtlama

Soru-Cevap Oluşturma yönetilen (Önizleme) ' de sunulan kesin yanıt özelliği, herhangi bir Kullanıcı sorgusu için Bilgi Bankası 'nda bulunan en iyi aday yanıtı ile ilgili kesin kısa cevap elde etmenizi sağlar. Bu özellik, çalışma zamanında, Kullanıcı sorgusunun amacını anlayan ve yanıt yolunca bir olgu olarak kısa bir yanıt sunan tam kısa yanıtı algılayan derin bir öğrenme modeli kullanır. 

Bu özellik, senaryonuza özgü işlevselliği test edebilmeniz için test bölmesinde varsayılan olarak üzerinde bulunur. Bu özellik hem içerik geliştiricileri hem de son kullanıcılar için son derece faydalıdır. Artık, içerik geliştiricilerinin bilgi tabanında bulunan her olgu için belirli QnA çiftlerini el ile seçmelerine gerek yoktur ve son kullanıcının, kullanıcının sorgusuna yanıt veren gerçek olguyu bulmak için hizmetten döndürülen tüm yanıt yollamasına bakmasına gerek yoktur. 

## <a name="precise-answering-on-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında kesin yanıt verme

Soru-Cevap Oluşturma portalında, test bölmesini açtığınızda, en üstte **kısa yanıtı görüntüleme** seçeneği görüntülenir. Bu seçenek, varsayılan olarak seçilecek. Test bölmesine bir sorgu girdiğinizde, yanıt yolnda kısa bir yanıt varsa Yanıt ile birlikte kısa bir yanıt görürsünüz. 
 
![Yönetilen etkin test bölmesi](../QnAMaker/media/conversational-context/test-pane-with-managed.png)

Yalnızca test bölmesinde yanıt pasanımı görmek istiyorsanız **kısa yanıtı görüntüle** seçeneğinin seçimini kaldırabilirsiniz. 

Hizmet Ayrıca, sınama bölmesindeki sorgunun hemen **altında bulunan denetle seçeneğini belirleyerek** kontrol ettiğiniz bir **Yanıt yayma puanı** olarak kesin yanıtın güvenilirlik Puanını geri döndürür.

![Yönetilen yanıt yayma puanı](../QnAMaker/media/conversational-context/managed-answer-span-score.png)

## <a name="publishing-a-qna-maker-bot"></a>Soru-Cevap Oluşturma bot yayımlama

Bir bot yayımladığınızda, uygulamanızda varsayılan olarak kesin yanıt özelliği, yanıt bestecisiyle birlikte kısa bir cevap görürsünüz. Yanıtta kesin yanıtın (AnswerSpan olarak adlandırılır) nasıl kullanılacağını görmek için [Yanıt oluştur](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerv5.0-preview.1/knowledgebase/generateanswer#answerspan) için API başvurusuna bakın. Kullanıcı, bu şablonu botu App Service aracılığıyla güncelleştirerek diğer deneyimleri seçme esnekliğine sahiptir. 

## <a name="language-support"></a>Dil desteği

Şu anda kesin yanıt özelliği yalnızca Ingilizce için desteklenir.
