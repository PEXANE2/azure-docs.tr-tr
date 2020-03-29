---
title: Çok dönüşlü konuşmalar - QnA Maker
description: Botunuzun bir sorudan diğerine çoklu dönüşolarak bilinen çoklu dönüşlerini yönetmek için istemleri ve bağlamı kullanın. Çoklu dönüş, önceki sorunun bağlamınınbir sonraki soru ve yanıtı etkilediği bir ileri ve geri konuşma yapma yeteneğidir.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: abdde09fbb1f6b066772366c5cea933824cb5864
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220627"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Birden çok konuşma geçişi oluşturmak için takip istemlerini kullanma

Botunuzun bir sorudan diğerine çoklu dönüşler olarak bilinen, birden çok _dönüşleri_yönetmek için izleme istemlerini ve bağlamını kullanın.

Çok döndürülerek nasıl çalıştığını görmek için aşağıdaki tanıtım videosunu görüntüleyin:

[![QnA Maker'da çoklu dönüşlü konuşma](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample)

## <a name="what-is-a-multi-turn-conversation"></a>Çok dönüşlü konuşma nedir?

Bazı sorular tek bir dönüşte cevaplanamaz. İstemci uygulamanızı (sohbet robotu) konuşmalarınızı tasarlarken, bir kullanıcı doğru yanıtı belirlemek için filtre edilmesi veya rafine edilmesi gereken bir soru sorabilir. Kullanıcıya *takip istemleri*sunarak bu akışı sorular üzerinden mümkün kılarsınız.

Bir kullanıcı bir soru sorduğunda, QnA Maker yanıtı _ve_ herhangi bir takip istemlerini döndürür. Bu yanıt, takip sorularını seçenek olarak sunmanızı sağlar.

> [!CAUTION]
> Çok dönüşlü istemler SSS belgelerinden ayıklanmaz. Çok döndürülme gerekiyorsa, QnA çiftlerini SSS olarak belirleyen soru işaretlerini kaldırın.

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Sohbet botu ile örnek çok yönlü konuşma

Çoklu dönüşle, bir sohbet botu aşağıdaki resimde gösterildiği gibi son yanıtı belirlemek için bir kullanıcıyla yapılan bir konuşmayı yönetir:

![Bir konuşma boyunca bir kullanıcıya yol gösteren istemleri içeren çok döndürme iletişim kutusu](../media/conversational-context/conversation-in-bot.png)

Önceki resimde, bir kullanıcı **hesabımı**girerek bir konuşma başlattı. Bilgi tabanının birbirine bağlı üç soru-cevap çifti vardır. Yanıtı hassaslaştırmak için, kullanıcı bilgi tabanındaki üç seçenekten birini seçer. Soru (#1), üç seçenek (#2) olarak sohbet bot sunulan üç takip istemleri vardır.

Kullanıcı bir seçenek (#3) seçtiğinde, bir sonraki rafine etme seçenekleri (#4) listesi sunulur. Bu sıra kullanıcı doğru, son yanıtı (#6) belirleyene kadar (#5) devam eder.


### <a name="use-multi-turn-in-a-bot"></a>Botta çoklu dönüş kullanma

KB'nizi yayımladıktan sonra, QnA Maker botunuzu Azure bot hizmetine dağıtmak için **Bot Oluştur** düğmesini seçebilirsiniz. İstemler, botunuz için etkinleştirdiğiniz sohbet istemcilerinde görünür.

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Belgenin yapısından çok döndürme oluşturma

Bir bilgi tabanı oluşturduğunuzda, **KB'nizin Doldur** bölümü **URL'lerden çok döndürme etkinleştir, .pdf veya .docx dosyaları** onay kutusunu görüntüler.

![Çok döndürme sağlamak için onay kutusu](../media/conversational-context/enable-multi-turn.png)

Bu seçeneği seçtiğinizde, QnA Maker belge yapısında bulunan hiyerarşiyi ayıklar. Hiyerarşi istemleri izlemek için dönüştürülür ve hiyerarşinin kökü üst QnA olarak hizmet vermektedir. Bazı belgelerde hiyerarşinin kökünde yanıt olarak kullanılabilecek içerik yoktur, bu tür hiyerarşileri ayıklamak için yedek yanıt metni olarak kullanılmak üzere 'Varsayılan Yanıt Metni'ni sağlayabilirsiniz.

Çok döndürülebilir yapı yalnızca URL'lerden, PDF dosyalarından veya DOCX dosyalarından çıkarılabilir. Yapı örneği olarak, Microsoft Surface [kullanım kılavuzu PDF dosyasının](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf)görüntüsünü görüntüleyin.

![! [Kullanım kılavuzundaki yapı örneği] (.. /media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

### <a name="building-your-own-multi-turn-document"></a>Kendi çok dönüşlü belgenizi oluşturma

Çok döndürülerek belge oluşturuyorsanız, lütfen aşağıdaki yönergeleri göz önünde bulundurun:

* Hiyerarşiyi belirtmek için başlıkları ve alt başlıkları kullanın. Örneğin, hızlı olarak alınması gereken QnA'yı belirtmek için üst QnA ve h2'yi belirtmek için h1 yapabilirsiniz. Sonraki hiyerarşiyi belirtmek için küçük başlık boyutunu kullanın. Belgenizdeki yapıyı ima etmek için stil, renk veya başka bir mekanizma kullanmayın, QnA Maker çok döndürülme istemlerini ayıklamaz.

* Başlığın ilk karakteri büyük harfle yazılmalı.

* Bir başlığı soru işaretiyle bitirmeyin. `?`

* [Örnek belgeyi](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx) kendi çok döndürme belgenizi oluşturmak için örnek olarak kullanabilirsiniz.

### <a name="adding-files-to-a-multi-turn-kb"></a>Çok dönüşlü KB'ye dosya ekleme

Hiyerarşik bir belge eklediğinizde, QnA Maker konuşma akışı oluşturmak için yapıdan izleme istemlerini belirler.

1. QnA Maker'da, **URL'lerden,.pdf veya .docx dosyalarından çoklu dönüş çıkarma etkinleştirme** ile oluşturulmuş varolan bir bilgi tabanını seçin. Etkin.
1. **Ayarlar** sayfasına gidin, eklemek için dosyayı veya URL'yi seçin.
1. Bilgi tabanını **kaydedin ve eğitin.**

> [!Caution]
> Yeni veya boş bilgi tabanı için veri kaynağı olarak dışa aktarılan Bir TSV veya XLS çok döndüryetenekli bilgi tabanı dosyasının kullanılmasına yönelik destek desteklenmez. Bir bilgi tabanına dışa aktarılan çok döndürülme istemlerini eklemek için bu dosya türünü QnA Maker portalının **Ayarlar** sayfasından **almanız** gerekir.


## <a name="create-knowledge-base-with-multi-turn-prompts-with-the-create-api"></a>Create API ile çok döndürme istemleri ile bilgi tabanı oluşturma

[QnA Maker Create API'yi](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)kullanarak çok dönüşlü istemleri olan bir bilgi durumu oluşturabilirsiniz. İstemler özelliğin `context` `prompts` dizisinde eklenir.

## <a name="show-questions-and-answers-with-context"></a>Soruları ve yanıtları bağlamla göster

Görüntülenen soru-yanıt çiftlerini yalnızca bağlamsal konuşmalara sahip olanlara indirin.

**Seçenekleri Görüntüle'yi**seçin ve ardından **bağlamı göster'i**seçin. Liste, izleme istemleri içeren soru-cevap çiftleri görüntüler.

![Soru-yanıt çiftlerini bağlamsal konuşmalara göre filtreleme](../media/conversational-context/filter-question-and-answers-by-context.png)

Çok döndürme bağlamı ilk sütunda görüntülenir.

![! ["Bağlam (PREVIEW)" sütunu] (.. /media/conversational-context/surface-manual-pdf-follow-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

Önceki resimde, **#1** sütunda geçerli soruyu ifade eden kalın metni gösterir. Üst soru satırdaki en üst öğedir. Aşağıdaki tüm sorular bağlantılı soru-cevap çiftleridir. Bu öğeler seçilebilir, böylece hemen diğer bağlam öğelerine gidebilirsiniz.

## <a name="add-an-existing-question-and-answer-pair-as-a-follow-up-prompt"></a>İzleme istemi olarak varolan bir soru-yanıt çiftini ekleme

Orijinal soru, **Hesabım**, Hesapları ve oturum **açma**gibi takip istemleri vardır .

!["Hesaplar ve oturum açma" yanıtları ve takip istemleri](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Şu anda bağlı olmayan varolan bir soru-cevap çiftine bir izleme istemi ekleyin. Soru herhangi bir soru-cevap çiftine bağlı olmadığından, geçerli görünüm ayarının değiştirilmesi gerekir.

1. Varolan bir soru-cevap çiftini izleme istemi olarak bağlamak için, soru-cevap çifti için satırı seçin. Surface kılavuzu için, listeyi azaltmak için **Oturum Aç'ı** arayın.
1. **Oturum Aç'ın**satırında , **Yanıt** sütununda, takip **istemi ekle'yi**seçin.
1. **İzleme istemi** açılır penceresindeki alanlara aşağıdaki değerleri girin:

    |Alan|Değer|
    |--|--|
    |Görüntü metni|Girin **Cihazı kapat.** Bu, izleme isteminde görüntülenecek özel bir metindir.|
    |Yalnızca bağlam| Bu onay kutusunu seçin. Yanıt, yalnızca soru bağlamı belirtirse döndürülür.|
    |Yanıtlamak için bağlantı|Varolan soru-cevap çiftini bulmak için **oturum açma ekranını** girin.|


1.  Bir eşleşme döndürülür. İzleme olarak bu yanıtı seçin ve sonra **Kaydet'i**seçin.

    !["İzleme istemi (PREVIEW)" sayfası](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Takip istemini ekledikten sonra **Kaydet'i** seçin ve en üstteki navigasyonda train'i seçin.

### <a name="edit-the-display-text"></a>Görüntü metnini edin

Bir takip istemi oluşturulduğunda ve varolan bir soru-cevap çifti **Yanıtbağlantısı**olarak girildiğinde, yeni **Görüntüle metni**girebilirsiniz. Bu metin varolan sorunun yerini almaz ve yeni bir alternatif soru eklemez. Bu değerlerden ayrıdır.

1. Görüntü metnini deletmek için **Bağlam** alanındaki soruyu arayın ve seçin.
1. Bu sorunun satırında, yanıt sütunundaki izleme istemini seçin.
1. Görüntülemek istediğiniz ekran metnini seçin ve ardından **Edit'i**seçin.

    ![Görüntü metni için Edit komutu](../media/conversational-context/edit-existing-display-text.png)

1. İzleme **istemi** açılır penceresinde, varolan ekran metnini değiştirin.
1. Görüntü metnini düzenlemeyi bitirdikten sonra **Kaydet'i**seçin.
1. Üst navigasyon çubuğunda, **Kaydet ve tren**.


## <a name="add-a-new-question-and-answer-pair-as-a-follow-up-prompt"></a>Takip istemi olarak yeni bir soru-cevap çifti ekleme

Bilgi tabanına yeni bir soru-cevap çifti eklediğinizde, her çift bir izleme istemi olarak varolan bir soruya bağlanmalıdır.

1. Bilgi tabanı araç çubuğunda, Hesaplar için varolan soru-cevap çiftini arayın ve seçin **ve oturum aç.'**

1. Bu sorunun **Yanıt** sütununda, **takip istemi ekle'yi**seçin.
1. **İzleme istemi (PREVIEW)** altında, aşağıdaki değerleri girerek yeni bir izleme istemi oluşturun:

    |Alan|Değer|
    |--|--|
    |Görüntü metni|*Bir Windows Hesabı oluşturun.* İzleme isteminde görüntülenecek özel metin.|
    |Yalnızca bağlam|Bu onay kutusunu seçin. Bu yanıt yalnızca soru bağlamı belirtirse döndürülür.|
    |Yanıtlamak için bağlantı|Cevap olarak aşağıdaki metni girin:<br>*Yeni veya varolan bir e-posta hesabıyla bir Windows hesabı [oluşturun.](https://account.microsoft.com/) *<br>Veritabanını kaydedip eğitdiğinizde, bu metin dönüştürülür. |
    |||

    ![Yeni bir istemi soru ve yanıt oluşturma](../media/conversational-context/create-child-prompt-from-parent.png)


1. **Yeni Oluştur'u**seçin ve sonra **Kaydet'i**seçin.

    Bu eylem yeni bir soru-cevap çifti oluşturur ve seçili soruyu bir izleme istemi olarak bağlar. **Bağlam** sütunu, her iki soru için de bir izleme istemi ilişkisini gösterir.

1. **Görünüm seçeneklerini**seçin ve ardından [**bağlamı Göster'i (PREVIEW)**](#show-questions-and-answers-with-context)seçin.

    Yeni soru nasıl bağlantılı olduğunu gösteriyor.

    ![Yeni bir izleme istemi oluşturma](../media/conversational-context/new-qna-follow-up-prompt.png)

    Üst soru seçeneklerinden biri olarak yeni bir soru görüntüler.

    ![! [Bağlam sütunu, her iki soru için de, bir izleme istemi ilişkisini gösterir] (.. /media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Takip istemini ekledikten sonra **Kaydet'i** seçin ve en üstteki navigasyon çubuğunda train'i seçin.

## <a name="enable-multi-turn-during-testing-of-follow-up-prompts"></a>İzleme istemlerinin test edilmesi sırasında çoklu dönüş etkinleştirme

Testi **bölmesinde** takip istemleri ile soruyu test ettiğinizde, **çoklu dönüş eve ver'i**seçin ve ardından sorunuza girin. Yanıt, izleme istemlerini içerir.

![Yanıt, takip istemlerini içerir](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Çok dönüşü etkinleştirmezseniz, yanıt döndürülür, ancak izleme istemleri döndürülmez.

## <a name="a-json-request-to-return-an-initial-answer-and-follow-up-prompts"></a>JSON ilk yanıtı ve takip isteklerini döndürmek için bir istek

Kullanıcının `context` sorusunun yanıtını istemek ve izleme istemlerini eklemek için boş nesneyi kullanın.

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="a-json-response-to-return-an-initial-answer-and-follow-up-prompts"></a>İlk yanıtı ve takip istemlerini döndürmek için bir JSON yanıtı

Önceki bölümde bir cevap istenir ve hesaplara herhangi bir takip ister **ve oturum açar.** Yanıt, *yanıtlar[0].context'da*bulunan istem bilgilerini ve kullanıcıya görüntülenecek metni içerir.

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 100.0,
            "id": 15,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    }
                ]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start, and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 27.53,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

Dizi `prompts` `displayText` özellik ve `qnaId` değer metin sağlar. Bu yanıtları konuşma akışında bir sonraki görüntülenen seçenekler olarak `qnaId` gösterebilir ve ardından seçilenleri aşağıdaki istekte QnA Maker'a geri gönderebilirsiniz.

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts"></a>JSON'un ilk yanıt ve takip istemlerini döndürme isteği

Nesneyi `context` önceki bağlamı içerecek şekilde doldurun.

Aşağıdaki JSON isteğinde, geçerli soru *oturum imzalamak için Windows Hello kullanın* ve önceki soru hesapları ve oturum *açma*oldu .

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "qnaId": 17,
  "context": {
    "previousQnAId": 15,
    "previousUserQuery": "accounts and signing in"
  }
}
```

##  <a name="a-json-response-to-return-a-non-initial-answer-and-follow-up-prompts"></a>İlk yanıt olmayan bir yanıtı ve takip istemlerini döndürmek için bir JSON yanıtı

QnA Maker _GenerateAnswer_ JSON `context` `answers` yanıtı, nesnedeki ilk öğenin özelliğindeki izleme istemlerini içerir:

```JSON
{
    "answers": [
        {
            "questions": [
                "Use Windows Hello to sign in"
            ],
            "answer": "**Use Windows Hello to sign in**\n\nSince Surface Pro 4 has an infrared (IR) camera, you can set up Windows Hello to sign in just by looking at the screen. \n\nIf you have the Surface Pro 4 Type Cover with Fingerprint ID (sold separately), you can set up your Surface sign you in with a touch. \n\nFor more info, see What is Windows Hello? on Windows.com. ",
            "score": 100.0,
            "id": 17,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Meet Surface Pro 4"
            ],
            "answer": "**Meet Surface Pro 4**\n\nGet acquainted with the features built in to your Surface Pro 4. \n\nHere’s a quick overview of Surface Pro 4 features: \n\n\n\n\n\n\n\nPower button \n\n\n\n\n\nPress the power button to turn your Surface Pro 4 on. You can also use the power button to put it to sleep and wake it when you’re ready to start working again. \n\n\n\n\n\n\n\nTouchscreen \n\n\n\n\n\nUse the 12.3” display, with its 3:2 aspect ratio and 2736 x 1824 resolution, to watch HD movies, browse the web, and use your favorite apps. \n\nThe new Surface G5 touch processor provides up to twice the touch accuracy of Surface Pro 3 and lets you use your fingers to select items, zoom in, and move things around. For more info, see Surface touchscreen on Surface.com. \n\n\n\n\n\n\n\nSurface Pen \n\n\n\n\n\nEnjoy a natural writing experience with a pen that feels like an actual pen. Use Surface Pen to launch Cortana in Windows or open OneNote and quickly jot down notes or take screenshots. \n\nSee Using Surface Pen (Surface Pro 4 version) on Surface.com for more info. \n\n\n\n\n\n\n\nKickstand \n\n\n\n\n\nFlip out the kickstand and work or play comfortably at your desk, on the couch, or while giving a hands-free presentation. \n\n\n\n\n\n\n\nWi-Fi and Bluetooth® \n\n\n\n\n\nSurface Pro 4 supports standard Wi-Fi protocols (802.11a/b/g/n/ac) and Bluetooth 4.0. Connect to a wireless network and use Bluetooth devices like mice, printers, and headsets. \n\nFor more info, see Add a Bluetooth device and Connect Surface to a wireless network on Surface.com. \n\n\n\n\n\n\n\nCameras \n\n\n\n\n\nSurface Pro 4 has two cameras for taking photos and recording video: an 8-megapixel rear-facing camera with autofocus and a 5-megapixel, high-resolution, front-facing camera. Both cameras record video in 1080p, with a 16:9 aspect ratio. Privacy lights are located on the right side of both cameras. \n\nSurface Pro 4 also has an infrared (IR) face-detection camera so you can sign in to Windows without typing a password. For more info, see Windows Hello on Surface.com. \n\nFor more camera info, see Take photos and videos with Surface and Using autofocus on Surface 3, Surface Pro 4, and Surface Book on Surface.com. \n\n\n\n\n\n\n\nMicrophones \n\n\n\n\n\nSurface Pro 4 has both a front and a back microphone. Use the front microphone for calls and recordings. Its noise-canceling feature is optimized for use with Skype and Cortana. \n\n\n\n\n\n\n\nStereo speakers \n\n\n\n\n\nStereo front speakers provide an immersive music and movie playback experience. To learn more, see Surface sound, volume, and audio accessories on Surface.com. \n\n\n\n\n",
            "score": 21.92,
            "id": 3,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 19.04,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-maker-id"></a>Bilgi tabanını QnA Maker kimliğiyle sorgula

Çok dönüşözelliğini kullanarak özel bir uygulama oluşturuyorsanız. İlk sorunun yanıtında, herhangi bir takip istemleri `qnaId` ve ilişkili döndürülür. Artık kimliğiniz olduğuna göre, bunu takip isteminin istek gövdesinde geçirebilirsiniz. İstek gövdesi `qnaId`, ve bağlam nesnesi (önceki QnA Maker özelliklerini içeren) içeriyorsa, GenerateAnswer soruyu yanıt metnine göre bulmak için sıralama algoritmasını kullanmak yerine tam soruyu kimlikle döndürür.


## <a name="display-order-is-supported-in-the-update-api"></a>Görüntüleme sırası Güncelleştirme API'sinde desteklenir

JSON yanıtında döndürülen [görüntü metni ve görüntü sırası,](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto)Güncelleştirme [API'sı](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update)tarafından düzenlenmek üzere desteklenir.

## <a name="add-or-delete-multi-turn-prompts-with-the-update-api"></a>Güncelleştirme API'si ile çok dönüşlü istemler ekleme veya silme

[QnA Maker Update API'sini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update)kullanarak çok dönüşlü istemler ekleyebilir veya silebilirsiniz.  İstemler özelliğin `context` `promptsToAdd` dizisinde ve diziye `promptsToDelete` eklenir.

## <a name="export-knowledge-base-for-version-control"></a>Sürüm denetimi için bilgi tabanını dışa aktarma

QnA Maker, dışa aktarılan dosyaya çok döndürerek konuşma adımlarını ekleyerek sürüm denetimini destekler.

## <a name="next-steps"></a>Sonraki adımlar

Bu [iletişim örneğinden](https://aka.ms/qnamakermultiturnsample) bağlamsal konuşmalar hakkında daha fazla bilgi edinin veya [çok döndürülme konuşmaları için kavramsal bot tasarımı](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0)hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Bilgi bankasını geçirme](../Tutorials/migrate-knowledge-base.md)
