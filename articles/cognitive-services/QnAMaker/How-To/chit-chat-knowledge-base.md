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
ms.date: 11/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: a9a14056e6be62fc1c1b5e542c1a3acceb738eac
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78389265"
---
# <a name="add-chit-chat-to-a-knowledge-base"></a>Bilgi Bankası'na Chit sohbet Ekle

Botunuz için Sohbet chit ekleyerek daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici kolaylaştırır. Soru-cevap Oluşturucu chit sohbet özelliği üst chit-sohbet, önceden doldurulmuş bir dizi, Bilgi Bankası (KB) kolayca eklemenize olanak sağlar. Bu botunuzun ait kişilik için bir başlangıç noktası olabilir ve bu saat ve bunları sıfırdan yazmak maliyeti kaydedecek.  

Bu veri kümesinde, profesyonel, kolay ve WIT gibi birden çok kişinin sesinde, tek BT-sohbet için 100 senaryo vardır. Botunuzun ait ses en çok benzeyen bir kişi seçin. Kullanıcı sorgusu verildiğinde, soru-cevap Oluşturucu en yakın bilinen chit sohbet soru-cevap ile eşleştirmeye çalışır.  

Farklı kişiselleştirimler için bazı örnekler aşağıda verilmiştir. Tüm kişilik [veri kümelerini](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets) , kişiselleştirimler hakkındaki ayrıntılarla birlikte görebilirsiniz.

`When is your birthday?`Kullanıcı sorgusu için, her kişilik stilli bir yanıt vardır:

<!-- added quotes so acrolinx doesn't score these sentences -->
|Kişilik|Örnek|
|--|--|
|Professional|Yaş gerçekten bana uygulanmaz.|
|Uygun|Gerçekten bir yaşı yok.|
|Wtty|Kullanım süresi ücretsizdir.|
|Giriş|Yaşı yok.|
|Heyecanlandırdı|Ben bir bot kullanıyorum, bu yüzden bir yaşı yok.|
||


## <a name="language-support"></a>Dil desteği

CHIT-chat veri kümeleri aşağıdaki dillerde desteklenmektedir:

|Dil|
|--|
|Çince|
|Türkçe|
|Fransızca|
|Almanya|
|İtalyanca|
|Japonca|
|Korece|
|Portekizce|
|İspanyolca|


## <a name="add-chit-chat-during-kb-creation"></a>KB oluşturma sırasında chit sohbet ekleme
Kaynak URL ve dosyaları ekledikten sonra Bilgi Bankası oluşturma sırasında chit sohbet ekleme seçeneği mevcuttur. Sohbet chit temeliniz olarak kullanmak istediğiniz kişilik seçin. CHIT-chat eklemek istemiyorsanız veya veri kaynaklarınızda zaten bir sohbet desteğine sahipseniz, **hiçbiri**' ni seçin. 

## <a name="add-chit-chat-to-an-existing-kb"></a>Mevcut bir KB olarak Chit sohbet Ekle
KB 'nizi seçin ve **Ayarlar** sayfasına gidin. Uygun **. tsv** biçimindeki tüm CHIT-chat veri kümelerine bir bağlantı vardır. İstediğiniz kişilik indirin ve ardından dosya kaynağı olarak karşıya yükleyin. İndirin ve dosyayı karşıya yüklemeyi zaman biçimi veya meta veri düzenleme olmadan emin olun. 
  
![Sohbet chit mevcut KB olarak Ekle](../media/qnamaker-how-to-chit-chat/add-chit-chat-dataset.png)

## <a name="edit-your-chit-chat-questions-and-answers"></a>Sohbet chit sorular ve yanıtlar Düzenle
KB düzenlediğinizde chit Sohbeti, seçtiğiniz kişilik üzerinde temel için yeni bir kaynağı görürsünüz. Artık sorular değiştirilmiş ekleyebilir veya yanıtlar, gibi başka bir kaynağı ile düzenleyin. 

![Sohbet chit Bankalarıyla Düzenle](../media/qnamaker-how-to-chit-chat/edit-chit-chat.png)

Meta verileri görüntülemek için, araç çubuğunda **seçenekleri görüntüle** ' yi seçin ve **meta verileri göster**' i seçin.

## <a name="add-additional-chit-chat-questions-and-answers"></a>Ek chit sohbet sorularını ve yanıtlarını Ekle
Değil önceden tanımlanmış ayarlanmış olan yeni chit sohbet soru-cevap ekleyebilirsiniz. Zaten chit sohbet kümesinde kapsamında bir soru-cevap çifti çoğaltma değil emin olun. Yeni bir GIT-chat QnA eklediğinizde, **düzenleme** kaynağınıza eklenir. Derecelendiricisini 'ın bu tek bir sohbet olduğunu anlaması için, aşağıdaki görüntüde görüldüğü gibi "düzenleme: chitchat" meta veri anahtarını/değer çiftini ekleyin:
   
![! [CHIT-chat QnAs Ekle] (.. /media/qnamaker-how-to-chit-chat/add-new-chit-chat.png)](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png#lightbox)

## <a name="delete-chit-chat-from-an-existing-kb"></a>Mevcut bir KB chit sohbet Sil
KB 'nizi seçin ve **Ayarlar** sayfasına gidin. Belirli chit sohbet kaynağınızı seçili kişilik adlı bir dosya olarak listelenir. Bu kaynak dosyası olarak silebilirsiniz.

![KB chit sohbet Sil](../media/qnamaker-how-to-chit-chat/delete-chit-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası içeri aktarma](../Tutorials/migrate-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
