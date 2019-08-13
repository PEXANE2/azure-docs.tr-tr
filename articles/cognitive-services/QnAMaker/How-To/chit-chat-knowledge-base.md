---
title: Bir soru-cevap Oluşturucu Bilgi Bankası'na chit sohbet ekleme
titleSuffix: Azure Cognitive Services
description: Bir KB oluşturduğunuzda botunuz için kişisel chit sohbet ekleyerek daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici kolaylaştırır. Soru-cevap Oluşturucu üst chit-sohbet, önceden doldurulmuş bir dizi, KB kolayca eklemenize olanak sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: a8ebd0b36c6ee8bf6762a70af9f4a7c09f6b118d
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955149"
---
# <a name="add-chit-chat-to-a-knowledge-base"></a>Bilgi Bankası'na Chit sohbet Ekle

Botunuz için Sohbet chit ekleyerek daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici kolaylaştırır. Soru-cevap Oluşturucu chit sohbet özelliği üst chit-sohbet, önceden doldurulmuş bir dizi, Bilgi Bankası (KB) kolayca eklemenize olanak sağlar. Bu botunuzun ait kişilik için bir başlangıç noktası olabilir ve bu saat ve bunları sıfırdan yazmak maliyeti kaydedecek.  

Bu veri kümesinde, profesyonel, kolay ve WIT gibi birden çok kişinin sesinde, tek BT-sohbet için 100 senaryo vardır. Botunuzun ait ses en çok benzeyen bir kişi seçin. Kullanıcı sorgusu verildiğinde, soru-cevap Oluşturucu en yakın bilinen chit sohbet soru-cevap ile eşleştirmeye çalışır.  

Farklı kişiselleştirimler için bazı örnekler aşağıda verilmiştir. Tüm kişilik [veri kümelerini](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets) , kişiselleştirimler hakkındaki ayrıntılarla birlikte görebilirsiniz.

Kullanıcı sorgusu `When is your birthday?`için her kişiliğinizi biçimlendirilmiş bir yanıt vardır:

<!-- added quotes so acrolinx doesn't score these sentences -->
|Kişilik|Örnek|
|--|--|
|Profesyonel|Yaş gerçekten bana uygulanmaz.|
|Uygun|Gerçekten bir yaşı yok.|
|Wtty|Kullanım süresi ücretsizdir.|
|Giriş|Yaşı yok.|
|Heyecanlandırdı|Ben bir bot kullanıyorum, bu yüzden bir yaşı yok.|
||

> [!NOTE]
> Chit sohbet desteği yalnızca İngilizce dilinde şu anda kullanılabilir. 

## <a name="add-chit-chat-during-kb-creation"></a>KB oluşturma sırasında chit sohbet ekleme
Kaynak URL ve dosyaları ekledikten sonra Bilgi Bankası oluşturma sırasında chit sohbet ekleme seçeneği mevcuttur. Sohbet chit temeliniz olarak kullanmak istediğiniz kişilik seçin. Sohbet chit ekleyip chit sohbet desteği, veri kaynaklarınızı zaten varsa istemiyorsanız **hiçbiri**. 
   
![Oluşturma sırasında chit sohbet Ekle](../media/qnamaker-how-to-chit-chat/create-kb-chit-chat.png)

## <a name="add-chit-chat-to-an-existing-kb"></a>Mevcut bir KB olarak Chit sohbet Ekle
KB'ı seçin ve gitmek **ayarları** sayfası. Bir bağlantı ilgili tüm chit sohbet veri kümelerine **.tsv** biçimi. İstediğiniz kişilik indirin ve ardından dosya kaynağı olarak karşıya yükleyin. İndirin ve dosyayı karşıya yüklemeyi zaman biçimi veya meta veri düzenleme olmadan emin olun. 
  
![Sohbet chit mevcut KB olarak Ekle](../media/qnamaker-how-to-chit-chat/add-chit-chat-dataset.png)

## <a name="edit-your-chit-chat-questions-and-answers"></a>Sohbet chit sorular ve yanıtlar Düzenle
KB düzenlediğinizde chit Sohbeti, seçtiğiniz kişilik üzerinde temel için yeni bir kaynağı görürsünüz. Artık sorular değiştirilmiş ekleyebilir veya yanıtlar, gibi başka bir kaynağı ile düzenleyin. 

![Sohbet chit Bankalarıyla Düzenle](../media/qnamaker-how-to-chit-chat/edit-chit-chat.png)

Meta verileri görüntülemek için, araç çubuğunda **seçenekleri görüntüle** ' yi seçin ve **meta verileri göster**' i seçin.

## <a name="add-additional-chit-chat-questions-and-answers"></a>Ek chit sohbet sorularını ve yanıtlarını Ekle
Değil önceden tanımlanmış ayarlanmış olan yeni chit sohbet soru-cevap ekleyebilirsiniz. Zaten chit sohbet kümesinde kapsamında bir soru-cevap çifti çoğaltma değil emin olun. Tüm yeni chit sohbet soru-cevap'ı eklediğinizde, eklenen, **editoryal** kaynak. Derecelendiricisini anlayan chit sohbet olduğundan emin olmak için meta verileri anahtar/değer çifti Ekle "editoryal: chit sohbet", aşağıdaki görüntüde görebileceğiniz gibi:
   
![! [CHIT-chat QnAs Ekle] (.. /media/qnamaker-how-to-chit-chat/add-new-chit-chat.png)](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png#lightbox)

## <a name="delete-chit-chat-from-an-existing-kb"></a>Mevcut bir KB chit sohbet Sil
KB'ı seçin ve gitmek **ayarları** sayfası. Belirli chit sohbet kaynağınızı seçili kişilik adlı bir dosya olarak listelenir. Bu kaynak dosyası olarak silebilirsiniz.

![KB chit sohbet Sil](../media/qnamaker-how-to-chit-chat/delete-chit-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası içeri aktarma](../Tutorials/migrate-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
