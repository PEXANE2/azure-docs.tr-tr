---
title: 'Öğretici: Güç Sanal Ajanlar ile entegre - QnA Maker'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, aktif öğrenme ile bilgi tabanının kalitesini artırmak. Varolan soruları kaldırmadan veya değiştirmeden gözden geçirin, kabul edin veya reddedin veya ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 03/11/2020
ms.author: diberry
ms.openlocfilehash: 4557dee995c8a01067f7e6ad0e79bb7115b6ecdb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402825"
---
# <a name="tutorial-add-your-knowledge-base-to-power-virtual-agents"></a>Öğretici: Güç Sanal Ajanlar için bilgi tabanı ekleyin
Bilgi tabanınızdan yanıtlar sağlamak için Bir [Güç Sanal Aracılar](https://powervirtualagents.microsoft.com/) botu oluşturun ve genişletin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

<!-- green checkmark -->
> [!div class="checklist"]
> * Bir Güç Sanal Ajanlar bot oluşturma
> * Sistem geri dönüş konusu oluşturma
> * QnA Maker'ı bir konuya eylem olarak Güç Otomatikleştirme akışı olarak ekleyin
> * Güç Otomatikleştir çözümü oluşturun
> * Çözümünüze Güç Otomatikleştir akışı ekleyin
> * Güç Sanal Aracılar Yayınla
> * Güç Sanal Aracıları Test Edin ve QnA Maker bilgi tabanınızdan bir yanıt alın

## <a name="integrate-an-agent-with-a-knowledge-base"></a>Bir aracıyı bilgi tabanıyla bütünleştirme

[Güç Sanal Aracılar](https://powervirtualagents.microsoft.com/) ekipleri kılavuzlu, no-kod grafik arayüzü kullanarak güçlü botlar oluşturmak için izin verir. Veri bilimcileri veya geliştiriciler gerekmez.

Power Virtual Agents'te, eylem gerçekleştirerek kullanıcı sorularını yanıtlamak için bir dizi konu (konu alanı) içeren bir aracı oluşturursunuz. Bir yanıt bulunamazsa, bir sistem geri dönüşü bir yanıtı döndürebilir.

Sorunu bir konunun eyleminin bir parçası olarak veya *System Fallback* konu yolunun bir parçası olarak bilgi tabanınıza gönderecek aracıyı yapılandırın. Her ikisi de bilgi tabanınıza bağlanmak ve bir yanıt dönmek için bir eylem kullanır.

## <a name="power-automate-connects-to-generateanswer-action"></a>Power Automate eyleme `GenerateAnswer` bağlanır

Aracınızı bilgi tabanınıza bağlamak için eylemi oluşturmak için Power Automate'i kullanın. Power Automate, QnA Maker'ın `GenerateAnswer` API'sine bağlanan bir işlem akışı sağlar.

Akışı tasarlayıp kurtardıktan sonra, Power Automate çözümünden elde edilebilir. Bu çözümü aracınızda bir eylem olarak kullanın.

## <a name="connect-an-agent-to-your-knowledge-base"></a>Bilgi tabanınıza bir aracı bağlayın

Power Virtual Agents'teki bir aracıyı QnA Maker'daki bir bilgi üssüne bağlama adımlarının genel bir özeti aşağıda veda edebilirsiniz.

* [QnA Maker](https://www.qnamaker.ai/) portalında:
    * Bilgi tabanınızı oluşturun ve yayınlayın.
    * Kimlik, çalışma zamanı bitiş noktası anahtarı ve çalışma zamanı bitiş noktası ana bilgisayarı da dahil olmak üzere bilgi temel bilgilerinizi kopyalayın.
* Güç [Sanal Aracılar](https://powerva.microsoft.com/) portalında:
    * Aracı konusu oluşturun.
    * Bir eylemi çağırın (Akışı Otomatikleştir'e güç vermek için).
* Power [Automate](https://us.flow.microsoft.com/) portalında:
    * [QnA Maker'S GenerateAnswer](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)bir konektör ile bir akış oluşturun.
        * QnA Maker bilgi bankası bilgilerini yayınladı:
            * Bilgi bankası kimliği
            * QnA Maker kaynak bitiş noktası ana bilgisayarı
            * QnA Maker kaynak uç noktası anahtarı
        * Giriş - kullanıcı sorgusu
        * Çıktı - bilgi temel cevabı
    * Bir çözüm oluşturun ve akışı ekleyin.
* Güç Sanal Ajanlar dön:
    * Bir konu için bir ileti olarak çözümün çıktısını seçin.

## <a name="create-and-publish-a-knowledge-base"></a>Bilgi bankası oluşturma ve yayımlama

1. Bir bilgi tabanı oluşturmak için [hızlı başlatıizleyin.](../Quickstarts/create-publish-knowledge-base.md) Bir bot oluşturma hakkında, son bölümü tamamlamayın. Bunun yerine, Güç Sanal Aracılar ile bir bot oluşturmak için bu öğretici kullanın.

    > [!div class="mx-imgBorder"]
    > ![Yayınlanan bilgi tabanı ayarlarının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

    [QnA Maker](https://www.qnamaker.ai/) portalındaki **Ayarlar** sayfasında bulunan yayınlanmış bilgi tabanı ayarlarınızı girin. QnA Maker `GenerateAnswer` bağlantınızı yapılandırmak için [Power Automate adımı](#create-a-power-automate-flow-to-connect-to-your-knowledge-base) için bu bilgilere ihtiyacınız olacaktır.

1. QnA Maker portalında, **Ayarlar** sayfasında, bitiş noktası anahtarını, bitiş noktası ana bilgisayarını ve bilgi bankası kimliğini bulun.

## <a name="create-an-agent-in-power-virtual-agents"></a>Güç Sanal Aracılar bir aracı oluşturma

1. [Power Virtual Agents için oturum açın.](https://go.microsoft.com/fwlink/?LinkId=2108000&clcid=0x409) Okul unuzu veya iş e-posta hesabınızı kullanın.
1. Bu ilk botunsa, aracının **ana** sayfasında olacaksın. Bu ilk bot değilse, sayfanın sağ üst alanından bot seçin ve **+ Yeni Bot**seçin.

    > [!div class="mx-imgBorder"]
    > ![Power Virtual Agents ana sayfasının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-home.png)

1. [QnA Maker](https://www.qnamaker.ai/) portalındaki **Ayarlar** sayfasında bulunan yayınlanmış bilgi tabanı ayarlarınızı girin.

## <a name="topics-provided-in-the-bot"></a>Botta sağlanan konular

Aracı, konu alanınızdaki soruları yanıtlamak için konu koleksiyonunu kullanır. Bu öğreticide, aracının sizin için sağlanan birçok konu vardır, kullanıcı konuları ve sistem konuları ayrılmıştır.

> [!div class="mx-imgBorder"]
> ![Aracıda sağlanan konuların ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topics-provided.png)


## <a name="create-the-system-fallback-topic"></a>Sistem geri dönüş konusunu oluşturma

Aracı herhangi bir konudan bilgi tabanınıza bağlanabilse de, bu öğretici *sistem geri dönüş* konusunu kullanır. Geri dönüş konusu, aracı bir yanıt buladığında kullanılır. Aracı, kullanıcının metnini QnA Maker'ın `GenerateAnswer` API'sine aktarıyor, yanıtı bilgi tabanınızdan alır ve kullanıcıya ileti olarak görüntüler.

1. Power [Virtual Agents](https://powerva.microsoft.com/#/) portalında, sağ üst köşede **Ayarlar** 'ı (vites simgesi) seçin. Sonra **Sistem Fallback**seçin.

    > [!div class="mx-imgBorder"]
    > ![System Fallback için Power Virtual Agents menü öğesinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-system-fallback.png)

1. Sistem geri dönüş konusu eklemek için **+ Ekle'yi** seçin.

    > [!div class="mx-imgBorder"]
    > ![Geri dönüş konusu eklemenin ekran görüntüsü.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-add-fallback-topic.png)

1. Konu eklendikten sonra, yazar tuvalindeki geri dönüş konusunu yazarken **Geri Dönüş konusuna Git'i** seçin.

    > [!TIP]
    > Geri dönüş konusuna geri dönmeniz gerekiyorsa, **Sistem** konularının bir parçası olarak **Konular** bölümünde kullanılabilir.

## <a name="use-the-authoring-canvas-to-add-an-action"></a>Eylem eklemek için yazma tuvalini kullanma

Geri dönüş konusunu bilgi tabanınıza bağlamak için tuval yazan Power Virtual Agents'i kullanın. Konu tanınmayan kullanıcı metniyle başlar. Bu metni QnA Maker'a aktaran ve ardından yanıtı ileti olarak gösteren bir eylem ekleyin. Bir yanıtı görüntülemenin son [adımı,](#add-your-solutions-flow-to-power-virtual-agents)daha sonra bu öğreticide ayrı bir adım olarak işlenir.

Bu bölümde geri dönüş konu konuşma akışı oluşturur.

1. Yeni geri dönüş eylemi zaten konuşma akışı öğeleri olabilir. **Seçenekler** menüsünü seçerek **Artır öğesini** silin.

    > [!div class="mx-imgBorder"]
    > ![Güç Sanal Ajanlar geri dönüş konu ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fallback-topic-delete-escalate.png)

1. **İleti** **+** kutusundan akan bağlayıcıyı seçin ve ardından eylem **çağır'ı**seçin.

    > [!div class="mx-imgBorder"]
    > ![Eylem çağrısının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png)

1. **Akış oluştur**'u seçin. İşlem sizi Power Automate portalına götürür.

    > [!div class="mx-imgBorder"]
    > ![Akış Oluşturma ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/create-a-flow.png)

## <a name="create-a-power-automate-flow-to-connect-to-your-knowledge-base"></a>Bilgi tabanınıza bağlanmak için Güç Otomatikleştir akışı oluşturun

Aşağıdaki yordam, bir Güç Otomatikleştirme akışı oluşturur:
* Gelen kullanıcı metnini alır ve QnA Maker'a gönderir.
* QnA Maker üst yanıtını bir değişkene atar ve değişkeni (üst yanıt) yanıt olarak aracınıza geri gönderir.

1. **Power Automate'de** **Akış Şablonu** sizin için başlatılır. Power **Virtual Agents** akış öğesinde, aracıdan gelen giriş değişkenini bilgi tabanınıza yapılandırmak için **Edit'i** seçin. Metin tabanlı giriş değişkeni aracınızın kullanıcı tarafından gönderilen metin sorusudur.

    > [!div class="mx-imgBorder"]
    > ![Giriş değişkeninizi metin dizesi olarak yapılandırmak için Güç Otomatikleştirme seçeneğinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable.png)

1. Bir metin girişi ekleyin ve `InputText`değişkeni adı `IncomingUserQuestion`, . Bu adlandırma, giriş metnini daha sonra oluşturduğunuz çıktı metninden ayırt edinmesine yardımcı olur.

    > [!div class="mx-imgBorder"]
    > ![Giriş değişken adını ve açıklamasını yapılandırmak için Power Automate seçeneğinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable-name-and-description.png)

1. Akışa **+** yeni bir adım eklemek için **Power Virtual Agents** kutusundan akan bağlayıcıyı seçin **(Power Virtual Agent'a İade değeri(ler)** öncesinde). Ardından **eylem ekle'yi**seçin.

1. `Qna` **QnA Maker eylemlerini** bulmak için arama yapın ve ardından **yanıt oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Yanıt Oluştur ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/generate-answer-action-selected.png)

    QnA Maker için gerekli bağlantı ayarları eylemde ve aracının soru ayarlarında görünür.

    > [!div class="mx-imgBorder"]
    > ![Gerekli bağlantı ayarlarının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/generate-answer-knowledge-base-settings.png)

1. Eylemi bilgi bankası kimliğiniz, bitiş noktası ana bilgisayarınız ve bitiş noktası anahtarınızla yapılandırın. Bunlar bilgi tabanınızın **Ayarlar** sayfasında, QnA Maker portalında bulunur.

    > [!div class="mx-imgBorder"]
    > ![Yayınlanan bilgi tabanı ayarlarının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

1. **Soruyu**yapılandırmak için metin kutusunu seçin ve `InputText` ardından listeden seçin.

1. Akışa yeni bir adım eklemek için **+** **Yanıt oluştur** eylem kutusundan akan bağlayıcıyı seçin. Ardından **eylem ekle'yi**seçin.

1. Döndürülen yanıt metnini yakalamak için `GenerateAnswer`bir değişken eklemek `Initialize variable` için eylemi arayın ve seçin.

    Değişkenin adını '' `OutgoingQnAAnswer`olarak ayarlayın ve **String**olarak türünü seçin. **Değeri**ayarlama.

    > [!div class="mx-imgBorder"]
    > ![Çıktı değişkeninin başlatılmasının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/initialize-output-variable-for-qna-answer.png)

1. Akışa yeni bir adım eklemek **+** **için, Initialize değişken** eylem kutusundan akan bağlayıcıyı seçin. Ardından **eylem ekle'yi**seçin.

1. Değişkene tüm bilgi tabanı JSON yanıtını ayarlamak için`Apply to each` eylemi arayın ve seçin. 'yi `GenerateAnswer` `answers`seçin.

1. Yalnızca üst yanıtı döndürmek **için, her kutuya** aynı uygula'da **eylem ekle'yi**seçin. **Değişkeni Ayarla**ve Seç.

    Değişken **ilerletmek** için ayarla kutusunda Ad **için**metin kutusunu seçin ve ardından listeden **GidenQnAAnswer'ı** seçin.

    **Değer**için metin kutusunu seçin ve ardından listeden **Yanıtlar Yanıtı'nı** seçin.

    > [!div class="mx-imgBorder"]
    > ![Değişkenin adını ve değerini ayarlama ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-automate-flow-apply-to-each-set-variable.png)

1. Değişkeni (ve değerini) döndürmek **için, Power Virtual Agent akış öğesine İade değeri(ler) öğesini** seçin. Ardından **Edit** > **Çıktıekle'yi**seçin. Bir **Metin** çıktı türü seçin ve `FinalAnswer`sonra **Başlığı** girin. **Değer**için metin kutusunu seçin ve `OutgoingQnAAnswer` sonra değişkeni seçin.

    > [!div class="mx-imgBorder"]
    > ![İade değerini ayarlama ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-automate-flow-return-value.png)

1. Akışı kaydetmek için **Kaydet**’i seçin.

## <a name="create-a-solution-and-add-the-flow"></a>Bir çözüm oluşturun ve akış ekleyin

Aracının akışı bulabilmesi ve akışa bağlanması için, akış bir Power Automate çözümüne dahil edilmelidir.

1. Power Automate portalında yken, sol daki gezintiden **Çözümler'i** seçin.

1. **+ Yeni çözüm**’ü seçin.

1. Görünen ad girin. Çözüm listesi, kuruluşunuzdaki veya okulunuzdaki tüm çözümleri içerir. Sadece çözümlerinize filtre uygulamanıza yardımcı olan bir adlandırma kuralı seçin. Örneğin, e-postanızı çözüm adınıza önek atabilirsiniz: `jondoe-power-virtual-agent-qnamaker-fallback`.

1. Seçenekler listesinden yayımcınızı seçin.

1. Ad ve sürüm için varsayılan değerleri kabul edin.

1. İşlemi tamamlamak için **Oluştur'u** seçin.

## <a name="add-your-flow-to-the-solution"></a>Akışınızı çözüme ekleyin

1. Çözümler listesinde, yeni oluşturduğunuz çözümü seçin. Listenin başında olmalı. Değilse, çözüm adının bir parçası olan e-posta adınıza göre arama yapın.

1. Çözümde + **Varolan Ekle'yi**ve ardından listeden **Akış'ı** seçin.

1. Akışınızı bulun ve işlemi tamamlamak için **Ekle'yi** seçin. Çok sayıda akış varsa, en son akışı bulmak için **Değiştirilen** sütuna bakın.

## <a name="add-your-solutions-flow-to-power-virtual-agents"></a>Power Virtual Agents'e çözümünüzün akışını ekleyin

1. Power Virtual Agents'teki temsilcinizle tarayıcı sekmesine dönün. Yazma tuvali hala açık olmalıdır.

1. Akışa yeni bir adım eklemek için, **İleti** eylem **+** kutusunun altına bağlayıcıyı seçin. Ardından **eylem çağır'ı**seçin.

1. Yeni eylemde, **Tanınmayan Tetikleyici Tümceleme'nin**giriş değerini seçin. Bu, metni aracıdan akışa geçirir.

    > [!div class="mx-imgBorder"]
    > ![Tanınmayan tetikleyici tümceciği seçmek için Güç Sanal Aracılar seçeneğinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-select-unrecognized-trigger-phrase.png)

1. Akışa yeni bir adım eklemek **için, Eylem** kutusunun altına konektörü **+** seçin. Ardından **İletiyi göster'i**seçin.

1. İleti metnini `Your answer is:`girin, . Yerinde `FinalAnswer` araç çubuğunun işlevini kullanarak bağlam değişkeni olarak seçin.

    > [!div class="mx-imgBorder"]
    > ![İleti metnini girmek için Güç Sanal Aracılar seçeneğinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-show-message-final-answer.png)

1. Bağlam araç çubuğundan, konu için yazma tuval ayrıntılarını kaydetmek için **Kaydet'i**seçin.

İşte son ajan tuvali böyle görünüyor.

> [!div class="mx-imgBorder"]
> ![Son aracı tuvalin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

## <a name="test-the-agent"></a>Aracıyı test edin

1. Test bölmesinde, Konular **arasında Parça'yı**geçiş. Bu, konular arasındaki ilerlemeyi ve tek bir konu içinde izlemenizi sağlar.

1. Aşağıdaki sırada kullanıcı metni girerek aracıtest edin. Yazma tuvali, başarılı adımları yeşil onay işaretiyle bildirir.

    |Soru sırası|Test soruları|Amaç|
    |--|--|--|
    |1|Hello|Konuşmaya başlama|
    |2|Mağaza saatleri|Örnek konu. Bu sizin için herhangi bir ek çalışma olmadan yapılandırılır.|
    |3|Evet|Cevap olarak`Did that answer your question?`|
    |4|Mükemmel|Cevap olarak`Please rate your experience.`|
    |5|Evet|Cevap olarak`Can I help with anything else?`|
    |6|Bilgi bankası nedir?|Bu soru, metni yanıtlamak için bilgi tabanınıza gönderen geri dönüş eylemini tetikler. Sonra cevap gösterilir. |

> [!div class="mx-imgBorder"]
> ![Son aracı tuvalin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png)

## <a name="publish-your-bot"></a>Botunuzu yayımlayın

Aracıyı okulunuzun veya kuruluşunuzun tüm üyeleri için kullanılabilir hale getirmek için yayımlamanız gerekir.

1. Soldaki gezintiden **Yayımla'yı**seçin. Ardından **Publish** sayfada Yayınla'yı seçin.

1. Demo web sitesinde bot deneyin **(Yayınla**altında bağlantı arayın).

    Yeni bir web sayfası bot ile açılır. Bota aynı test sorusunu sorun:`What is a knowledge base?`

    > [!div class="mx-imgBorder"]
    > ![Son aracı tuvalin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/demo-chat-bot.png)

## <a name="share-your-bot"></a>Botunuzu paylaşın

Demo web sitesini paylaşmak için, bir kanal olarak yapılandırın.

1. Soldaki gezintiden Kanalları **Manage** > **Yönet'i**seçin.

1. Kanallar listesinden **Demo web sitesini** seçin.

1. Bağlantıyı kopyalayın ve **Kaydet'i**seçin. Demo web sitenizin bağlantısını okul veya kuruluş üyelerinize gönderilen bir e-postaya yapıştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi tabanıyla yaptığınız iş bittiğinde, Azure portalındaki QnA Maker kaynaklarını kaldırın.

## <a name="next-step"></a>Sonraki adım

[Bilgi bankanız hakkında analizler alma](../How-To/get-analytics-knowledge-base.md)

Aşağıdakiler hakkında daha fazla bilgi edinin:
* [Power Virtual Agents](https://docs.microsoft.com/power-virtual-agents/)
* [Power Automate](https://docs.microsoft.com/power-automate/)
* [QnA Maker konektörü](https://us.flow.microsoft.com/connectors/shared_cognitiveservicesqnamaker/qna-maker/) ve [konektör ayarları](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)