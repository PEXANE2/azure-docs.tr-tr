---
title: QnA Maker bilgi tabanına chit-chat ekleme
titleSuffix: Azure Cognitive Services
description: Botunuza kişisel sohbet eklemek, bir KB oluşturduğunuzda daha konuşkan ve ilgi çekici hale getirir. QnA Maker kolayca üst chit-chat önceden doldurulur bir dizi eklemek için izin verir, KB içine.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220715"
---
# <a name="add-chit-chat-to-a-knowledge-base"></a>Bilgi tabanına Chit-chat ekleme

Botunuzun chit-chat ekleme daha konuşma ve ilgi çekici hale getirir. QnA maker'daki chit-chat özelliği, bilgi tabanınıza (KB) önceden doldurulmuş bir üst chit-chat kümesini kolayca eklemenize olanak tanır. Bu bot kişiliği için bir başlangıç noktası olabilir, ve size zaman ve sıfırdan bunları yazma maliyeti kazandıracak.  

Bu dataset profesyonel, dostu ve Esprili gibi birden fazla kişi, ses chit-sohbet yaklaşık 100 senaryoları vardır. Botunuzun sesine en çok benzeyen kişiliği seçin. Bir kullanıcı sorgusu göz önüne alındığında, QnA Maker en yakın bilinen chit-chat QnA ile eşleştirmeye çalışır.  

Farklı kişiliklerin bazı örnekleri aşağıdadır. Tüm kişilik [veri kümelerini](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets) kişiliklerin ayrıntılarıyla birlikte görebilirsiniz.

Kullanıcı sorgusu `When is your birthday?`için, her kişiliğin stilde bir yanıtı vardır:

<!-- added quotes so acrolinx doesn't score these sentences -->
|Kişilik|Örnek|
|--|--|
|Professional|Yaş benim için geçerli değil.|
|Dostu|Aslında bir yaşım yok.|
|Esprili|Yaşım yok.|
|Bakım|Benim bir yaşım yok.|
|Hevesli|Ben bir robotum, o yüzden yaşım yok.|
||


## <a name="language-support"></a>Dil desteği

Chit-chat veri kümeleri aşağıdaki dillerde desteklenir:

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


## <a name="add-chit-chat-during-kb-creation"></a>KB oluşturma sırasında chit-chat ekleme
Bilgi tabanı oluşturma sırasında, kaynak URL'lerinizi ve dosyalarınızı ekledikten sonra, chit-chat ekleme seçeneği vardır. Chit-chat tabanınız olarak istediğiniz kişiliği seçin. Chit-chat eklemek istemiyorsanız veya veri kaynaklarınızda zaten chit-chat desteği varsa, **Yok'u**seçin. 

## <a name="add-chit-chat-to-an-existing-kb"></a>Varolan bir KB'ye Chit-chat ekleme
KB'nizi seçin ve **Ayarlar** sayfasına gidin. Uygun **.tsv** formatında tüm chit-chat veri kümelerine bir bağlantı vardır. İstediğinizi kişiliği indirin ve dosya kaynağı olarak yükleyin. Dosyayı indirip yüklerken biçimi veya meta verileri yüklemediğinizde bunları değil. 
  
![Mevcut KB'ye chit-chat ekleme](../media/qnamaker-how-to-chit-chat/add-chit-chat-dataset.png)

## <a name="edit-your-chit-chat-questions-and-answers"></a>Sohbet sorularınızı ve yanıtlarınızı edin
KB'nizi yaptığınızda, seçtiğiniz kişiliğe göre sohbet için yeni bir kaynak görürsünüz. Artık başka bir kaynakta olduğu gibi değiştirilmiş sorular ekleyebilir veya yanıtları değiştirebilirsiniz. 

![Chit-chat Qna'ları edin](../media/qnamaker-how-to-chit-chat/edit-chit-chat.png)

Meta verileri görüntülemek için araç çubuğunda **Seçenekleri Görüntüle'yi** seçin ve ardından meta verileri **göster'i**seçin.

## <a name="add-additional-chit-chat-questions-and-answers"></a>Ek chit-chat soruları ve yanıtları ekleyin
Önceden tanımlanmış kümede olmayan yeni chit-chat QnA ekleyebilirsiniz. Zaten chit-chat kümesinde kapsanan bir QnA çiftini çoğaltmadığınızdan emin olun. Yeni bir chit-chat QnA eklediğinizde, **bu içerik Editoryal** kaynağınıza eklenir. Ranker bu chit-chat olduğunu anladığını sağlamak için, meta veri anahtarı / değer çifti ekleyin "Editoryal: chitchat", aşağıdaki resimde görüldüğü gibi:
   
![! [Chit-chat QnAs ekle] (.. /media/qnamaker-how-to-chit-chat/add-new-chit-chat.png)](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png#lightbox)

## <a name="delete-chit-chat-from-an-existing-kb"></a>Chit-chat'i varolan bir KB'den silme
KB'nizi seçin ve **Ayarlar** sayfasına gidin. Belirli chit-chat kaynağınız, seçili kişilik adı olan bir dosya olarak listelenir. Bunu kaynak dosya olarak silebilirsiniz.

![KB'den chit-chat silme](../media/qnamaker-how-to-chit-chat/delete-chit-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi tabanı alma](../Tutorials/migrate-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
