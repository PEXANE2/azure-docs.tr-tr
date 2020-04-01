---
title: 'Öğretici: Güç Sanal Ajan ile entegre - QnA Maker'
description: Bu eğitimde, aktif öğrenme ile bilgi tabanının kalitesini artırmak. Varolan soruları kaldırmadan veya değiştirmeden gözden geçirin, kabul edin veya reddedin.
ms.topic: tutorial
ms.date: 03/11/2020
ms.openlocfilehash: 283667c587e395a1d712f82f3385582b4c5c3227
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398880"
---
# <a name="tutorial-add-knowledge-base-to-power-virtual-agent"></a>Öğretici: Power Virtual Agent bilgi tabanı ekle
Bilgi tabanınızdan yanıtlar sağlamak için bir [Power Virtual Agent](https://powervirtualagents.microsoft.com/) bot oluşturun ve genişletin.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Güç Sanal Aracı oluştur
> * Sistem geri dönüş konusunu oluşturma
> * Power Automate akışı olarak konuya eylem olarak QnA Maker ekleyin
> * Power Automate çözümü oluşturun
> * Çözüme Güç Otomatikleştirme akışı ekle
> * Güç Sanal Aracıyayım
> * Test Güç Sanal Ajan, QnA Maker bilgi tabanından cevap almak

## <a name="integrate-a-power-virtual-agent-with-a-knowledge-base"></a>Bir Güç Sanal Aracıyı bilgi tabanıyla bütünleştirin

[Güç Sanal Aracılar](https://powervirtualagents.microsoft.com/) kolayca veri bilim adamları veya geliştiriciler için gerek kalmadan bir kılavuzlu, no-kod grafik arayüzü kullanarak güçlü botlar oluşturmak için izin verir.

Bir Güç Sanal Aracısı, eylem gerçekleştirerek kullanıcı sorularını yanıtlamak için bir dizi konu (konu alanları) ile oluşturulur. Bir yanıt bulunamazsa, bir sistem geri dönüşü bir yanıtı döndürebilir.

Sorunu bir konunun eyleminin bir parçası olarak veya **System Fallback** konu yolunun bir parçası olarak bilgi tabanınıza gönderecek aracıyı yapılandırın. Her ikisi de bilgi tabanınıza bağlanmak ve bir yanıt dönmek için aynı eylem mekanizmasını kullanır.

## <a name="power-automate-connects-to-generateanswer-action"></a>Power Otomatikleştir' i GenerateAnswer eylemine bağlar

Aracınızı bilgi tabanınıza bağlamak için eylemi oluşturmak için Power Automate'i kullanın. Power Automate, QnA Maker'ın GenerateAnswer API'sine bağlanan bir işlem **akışı**sağlar.

**Akış** tasarlanıp kaydedildikten sonra, Power Automate **Solution'dan**edinilebilir.  GenerateAnswer akışı bir çözüme eklendikten sonra, bu çözümü aracınızda bir eylem olarak kullanın.

## <a name="process-steps-to-connect-an-agent-to-your-knowledge-base"></a>Bir aracıyı bilgi tabanınıza bağlamak için işlem adımları

Aşağıdaki adımlar, adımların bir Power Virtual Agent'ı QnA Maker bilgi tabanına bağlama hedefiyle nasıl ilişkili olduğunu anlamanıza yardımcı olacak bir genel bakış olarak sunulur.

QnA Maker ile Power Virtual aracısını kullanma adımları:
* [QnA Maker](https://www.qnamaker.ai/) portalında
    * Bilgi tabanı oluşturma ve yayımlama
    * Bilgi bankası kimliği, çalışma zamanı bitiş noktası anahtarı ve çalışma zamanı bitiş noktası ana bilgisayar larını içeren bilgi tabanı ayrıntılarını kopyalayın.
* [In Power Virtual Agent](https://powerva.microsoft.com/) portalı
    * Aracı oluşturma konusu
    * Bir eylem çağırın (Akışı Otomatikleştirin)
* [Güç Otomatikleştir](https://us.flow.microsoft.com/) portalında
    * [QnA Maker'ın GenerateAnswer'ına konektörle](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/) akış oluşturun
        * QnA Maker bilgi bankası bilgilerini yayınladı
            * Bilgi bankası kimliği
            * QnA Maker kaynak bitiş noktası ana bilgisayarı
            * QnA Maker kaynak uç noktası anahtarı
        * Giriş - kullanıcı sorgusu
        * Çıktı - bilgi temel cevabı
    * Çözüm oluşturma ve akış ekleme
* Power Virtual Agent dön
    * Konu için mesaj olarak çözümün çıktısını seçin

## <a name="create-and-publish-a-knowledge-base"></a>Bilgi bankası oluşturma ve yayımlama

1. Bir bilgi tabanı oluşturmak için [hızlı başlatıizleyin.](../Quickstarts/create-publish-knowledge-base.md) Bir bot oluşturmak için son bölümü tamamlamayın. Bu öğretici quickstart Bot Framework bot yerine, bir bot oluşturmak için Güç Sanal Aracısı kullandığından, quickstart son bölümü için bir yedektir.

    > [!div class="mx-imgBorder"]
    > ![[QnA Maker]https://www.qnamaker.ai/) portalındaki **Ayarlar** sayfasında bulunan yayınlanmış bilgi tabanı ayarlarınızı girin.](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

    QnA Maker GenerateAnswer bağlantınızı yapılandırmak için [Power Automate adımı](#create-power-automate-flow-to-connect-to-your-knowledge-base) için bu bilgilere ihtiyacınız olacaktır.

1. QnA Maker portalındaki **Ayarlar** sayfasında bitiş noktası anahtarını, bitiş noktası ana bilgisayarını ve bilgi temel kimliğini bulun.

## <a name="create-power-virtual-agent"></a>Güç Sanal Aracı oluştur

1. Okul veya iş e-posta hesabınızla Power Virtual [Agent'da oturum açın.](https://go.microsoft.com/fwlink/?LinkId=2108000&clcid=0x409)
1. Bu ilk bot ise, aracının **Ana** sayfasında olmalıdır. Bu ilk Power Virtual Agent değilse, sağ üst navigasyon bot seçin ve **+ Yeni Bot**seçin.

    > [!div class="mx-imgBorder"]
    > ![[QnA Maker]https://www.qnamaker.ai/) portalındaki **Ayarlar** sayfasında bulunan yayınlanmış bilgi tabanı ayarlarınızı girin.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-home.png)

## <a name="several-topics-are-provided-in-the-bot"></a>Bot'ta çeşitli konular sağlanır

Aracı, konu alanınızdaki soruları yanıtlamak için konu koleksiyonunu kullanır. Bu öğreticide, aracı kullanıcı **konuları** ve **sistem konular**bölünmüş, sizin için sağlanan birçok konu vardır.

> [!div class="mx-imgBorder"]
> ![Aracı, konu alanınızdaki soruları yanıtlamak için konu koleksiyonunu kullanır. Bu öğreticide, aracının **Kullanıcı Konuları** ve **Sistem konuları**'ye bölünmüş birçok konu bulunmaktadır.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topics-provided.png)


## <a name="create-power-virtual-agents-system-fallback-topic"></a>Güç Sanal Aracı'nın Sistemi geri dönüş konu oluşturma

Aracı herhangi bir konudan bilgi tabanınıza bağlanabilir iken, bu öğretici Sistem **Fallback** konu kullanır. Geri dönüş konusu, aracı bir yanıt buladığında kullanılır. Aracı, kullanıcının metnini QnA Maker'ın GenerateAnswer API'sine geçirir, yanıtı bilgi tabanınızdan alır ve kullanıcıya ileti olarak geri görüntüler.

1. Power [Virtual Agents](https://powerva.microsoft.com/#/) portalında, navigasyonun sağ üst köşesinde **Ayarlar** sayfasını seçin. Bu sayfanın simgesi dişli. **Sistem Fallback**seçin.

    > [!div class="mx-imgBorder"]
    > ![System Fallback için Power Virtual aracı menü öğesi](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-system-fallback.png)

1. Açılır **Ayarlar** penceresinde, System Fallback konularını eklemek için **+ Ekle'yi** seçin.

    > [!div class="mx-imgBorder"]
    > ![Ayarlar penceresinde geri dönüş konusunu ekleyin.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-add-fallback-topic.png)

1. Konu eklendikten sonra, yazma tuvalindeki Fallback konusunu yazarken **Fallback konusuna git'i** seçin.

    > [!TIP]
    > Geri Dönüş konusuna geri dönmeniz gerekiyorsa, **Sistem** konularının bir parçası olarak **Konular** bölümünde kullanılabilir.

## <a name="use-authoring-canvas-to-add-an-action"></a>Eylem eklemek için yazma tuvalini kullanma

Fallback konusunu bilgi tabanınıza bağlamak için tuval yazan Power Virtual Agents'i kullanın. Konu **tanınmayan kullanıcı metniyle**başlar. Bu metni QnA Maker'a aktaran bir eylem ekleyin ve ardından yanıtı bir ileti olarak gösterir. Bir yanıtı görüntülemenin son adımı, bu öğreticide daha sonra ayrı bir [adım](#add-solutions-flow-to-power-virtual-agent) olarak işlenir.

Bu bölümde geri dönüş konu konuşma akışı oluşturur.

1. Yeni Fallback eylem zaten konuşma akışı öğeleri olabilir. Seçenekler menüsünü seçerek **Artır öğesini** silin.

    > [!div class="mx-imgBorder"]
    > ![Tetikleyici tümceciklerle geri dönüş eylemini başlatma](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fallback-topic-delete-escalate.png)

1. **İleti** **+** kutusundan akan bağlayıcıyı seçin, ardından eylem **çağır'ı**seçin.

    > [!div class="mx-imgBorder"]
    > ![Eylem çağrısı](../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png)

1. **Akış oluştur**'u seçin. Bu işlem sizi farklı bir tarayıcı tabanlı portal olan **Power Automate'e**götürür.

    > [!div class="mx-imgBorder"]
    > ![Eylem çağrısı](../media/how-to-integrate-power-virtual-agent/create-a-flow.png)

## <a name="create-power-automate-flow-to-connect-to-your-knowledge-base"></a>Bilgi tabanınıza bağlanmak için Güç Otomatikleştir Akış Oluşturun

Aşağıdaki yordam, bir **Güç Otomatikleştirme** akışı oluşturur:
* gelen kullanıcı metnini alır
* QnA Maker gönderir
* bir değişkene QnA Maker üst yanıtı atar
* değişkeni (üst yanıt) aracınıza yanıt olarak gönderir

1. **Power Automate'de** **Akış Şablonu** sizin için başlatılır. Power **Virtual Agents** akış öğesinde, aracıdan gelen giriş değişkenini bilgi tabanınıza yapılandırmak için **Edit'i** seçin. Metin tabanlı giriş değişkeni aracınızın kullanıcı tarafından gönderilen metin sorusudur.

    > [!div class="mx-imgBorder"]
    > ![Giriş değişkeninizi metin dizesi olarak yapılandırma](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable.png)

1. Bir metin girişi ekleyin ve `InputText` değişkeni `IncomingUserQuestion`. Bu adlandırma, giriş metnini daha sonra oluşturduğunuz çıktı metninden ayırt edinmesine yardımcı olur.

    > [!div class="mx-imgBorder"]
    > ![Metin girişi ekleyin ve 'UserQuestion' açıklamasıyla 'Giriş Metni' değişkenini adlandırın](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable-name-and-description.png)

1. Power **+** **Virtual Agents** kutusundan akan bağlayıcıyı seçin, akışa yeni bir adım eklemek için **(İade değeri(ler)'den önce Power Virtual Agent'a),** ardından **eylem ekle'yi**seçin.

1. `Qna` **QnA Maker eylemlerini** bulmak için arama yapın ve ardından **yanıt oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![**QnA Maker** eylemlerini bulmak için 'Qna' araması yapın ve ardından **Yanıt Oluştur**'yi seçin**](../media/how-to-integrate-power-virtual-agent/generate-answer-action-selected.png)

    QnA Maker için gereken üç (3) bağlantı ayarı, Power Virtual Agent'ın eylemve soru ayarlarında görünür.

    > [!div class="mx-imgBorder"]
    > ![QnA Maker'ın bağlantı ayarları eylemde görünür.](../media/how-to-integrate-power-virtual-agent/generate-answer-knowledge-base-settings.png)

1. İşlemi bilgi bankası kimliğiniz, bitiş noktası ana bilgisayarınız ve bitiş noktası anahtarınızla yapılandırın. Bunlar bilgi tabanınızın **Ayarlar** sayfasında, QnA Maker portalında bulunur.

    > [!div class="mx-imgBorder"]
    > ![[QnA Maker]https://www.qnamaker.ai/) portalındaki **Ayarlar** sayfasında bulunan yayınlanmış bilgi tabanı ayarlarınızı girin.](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

1. **Soruyu**yapılandırmak için metin kutusunu seçin ve `InputText` ardından listeden seçin.

1. Akışa yeni bir adım eklemek için **+** **Yanıt oluştur** eylem kutusundan akan bağlayıcıyı seçin ve ardından **eylem ekle'yi**seçin.

1. GenerateAnswer'tan döndürülen yanıt metnini yakalamak için bir `Initialize variable` değişken eklemek için eylemi arayın ve seçin.

    Değişkenin adını '' `OutgoingQnAAnswer`olarak ayarlayın ve **String**olarak türünü seçin. **Değeri**ayarlama.

    > [!div class="mx-imgBorder"]
    > ![Değişkenin adını 'QnAAnswer' olarak ayarlayın ve türünü **String** olarak seçin](../media/how-to-integrate-power-virtual-agent/initialize-output-variable-for-qna-answer.png)

1. Akışa yeni bir adım eklemek **+** **için, Initialize değişken** eylem kutusundan akan bağlayıcıyı seçin ve ardından **eylem ekle'yi**seçin.

1. Değişkene tüm bilgi tabanı JSON yanıtını ayarlamak için`Apply to each` eylemi arayın ve seçin. GenerateAnswer'ı `answers`seçin.

1. Yalnızca üst yanıtı döndürmek **için, her kutuya** aynı uygula'da **eylem ekle'yi**seçin. **Değişkeni Ayarla**ve Seç.

    Değişken **ilerletmek** için ayarla değişken kutusunda, **Ad**için metin kutusunu seçin, ardından listeden **GidenQnAAnswer'ı** seçin.

    **Değer**için metin kutusunu seçin, ardından listeden **Yanıtlar Yanıtı'nı** seçin.

    > [!div class="mx-imgBorder"]
    > ![Değişkenin adını ve değerini ayarlama](../media/how-to-integrate-power-virtual-agent/power-automate-flow-apply-to-each-set-variable.png)

1. Değişkeni (ve değerini) döndürmek **için, Power Virtual Agent akış öğesine İade değeri(ler)** seçeneğini belirleyin ve ardından **Edit'i**seçin, ardından **çıktı ekleyin.** Bir **Metin** çıktısı türü seçin `FinalAnswer`ve '' ' ın **başlığını** girin. **Değer**için metin kutusunu seçin, `OutgoingQnAAnswer` sonra değişkeni seçin.

    > [!div class="mx-imgBorder"]
    > ![İade değerini ayarlama](../media/how-to-integrate-power-virtual-agent/power-automate-flow-return-value.png)

1. Akışı kaydetmek için **Kaydet'i** seçin.

## <a name="create-solution-and-add-flow"></a>Çözüm oluşturma ve akış ekleme

Power Virtual Agent'ın akışı bulabilmesi ve akışa bağlanabilmesi için, akış bir Power Automate Solution'a dahil edilmelidir.

1. Power Automate portalında yken, sol daki gezintiden **Çözümler'i** seçin.

1. **+ Yeni çözüm**’ü seçin.

1. Görünen ad girin. Çözüm listesi, kuruluşunuzdaki veya okulunuzdaki tüm çözümleri içerir. E-postanızı çözüm adınıza önceden yapıştırma gibi çözümlerinize filtre uygulamanıza yardımcı olan bir adlandırma kuralı seçin: `jondoe-power-virtual-agent-qnamaker-fallback`.

1. Seçenekler listesinden yayımcınızı seçin.

1. Ad ve sürüm için varsayılan değerleri kabul edin.

1. İşlemi tamamlamak için **Oluştur'u** seçin.

## <a name="add-flow-to-solution"></a>Çözüme akış ekleme

1. Çözümler listesinde, yeni oluşturduğunuz çözümü seçin. Listenin başında olmalı. Değilse, çözüm adının bir parçası olan e-posta adınıza göre arama yapın.

1. Çözümde + **Varolan ekle'yi**seçin, ardından listeden **Akış'ı** seçin.

1. Akışınızı bulun ve işlemi tamamlamak için **Ekle'yi** seçin. Çok sayıda akış varsa, en son akışı bulmak için **Değiştirilen** sütuna bakın.

## <a name="add-solutions-flow-to-power-virtual-agent"></a>Power Virtual Agent'a çözüm akışını ekleyin

1. Power Virtual Agent'ınızla tarayıcı sekmesine dönün. Yazma tuvali hala açık olmalıdır.

1. Akışa **+** yeni bir adım eklemek için **İleti** eylem kutusunun altındaki bağlayıcıyı seçin ve ardından **eylem çağır'ı**seçin.

1. Yeni eylemde, **Tanınmayan Tetikleyici Tümceleme'nin**giriş değerini seçin. Bu, metni aracıdan akışa geçirir.

    > [!div class="mx-imgBorder"]
    > ![Yeni eylemde**UnrecognizedTriggerPhrase**'nin giriş değerini seçin.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-select-unrecognized-trigger-phrase.png)

1. Akışa **+** yeni bir adım eklemek için **Eylem** kutusunun altındaki bağlayıcıyı seçin ve ardından **İletiyi göster'i**seçin.

1. İleti metnini `Your answer is:`girin ve `FinalAnswer` yerinde araç çubuğunun işlevini kullanarak bağlam değişkeni olarak seçin.

    > [!div class="mx-imgBorder"]
    > ![Power Otomatikleştir Akış'tan ileti metnini ve 'FinalAnswer'ı girin.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-show-message-final-answer.png)

1. Konu için yazma tuval ayrıntılarını kaydetmek için bağlam araç çubuğundan **Kaydet'i** seçin.

Son tuval aşağıda gösterilmiştir.

> [!div class="mx-imgBorder"]
> ![Son ajan tuval](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

## <a name="test-power-virtual-agent"></a>Test Gücü Sanal Aracı

1. Test bölmesinde, Konular **arasında Parça'yı**geçiş. Bu, tek bir konunun yanı sıra konular arasındaki ilerlemeyi de izlemenizi sağlar.

1. Aşağıda verilen sıraya kullanıcı metni girerek aracıyı test edin. Yazma tuvali, başarılı adımları yeşil onay işaretiyle bildirir.

|Soru sırası|Test soruları|Amaç|
|--|--|--|
|1|Hello|Konuşmaya başlama|
|2|Mağaza saatleri|Örnek konu - sizin tarafınızdan herhangi bir ek çalışma olmadan sizin için yapılandırılmıştır.|
|3|Evet|Cevap olarak`Did that answer your question?`|
|4|Mükemmel|Cevap olarak`Please rate your experience.`|
|5|Evet|Cevap olarak`Can I help with anything else?`|
|6|Bilgi bankası nedir?|Bu soru, metni yanıtlamak üzere bilgi tabanınıza gönderen geri dönüş eylemini tetikler, ardından yanıt görüntülenir. |

> [!div class="mx-imgBorder"]
> ![Son ajan tuval](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png)

## <a name="publish-your-bot"></a>Botunuzu yayımlayın

Aracıyı okulunuzun veya kuruluşunuzun tüm üyeleri nezdinde kullanılabilir hale getirmek için, aracıyayınlamanız gerekir.

1. Sol **Publish** navigasyondan Yayımla'yı seçin, ardından sayfada **Yayınla'yı** seçin.

1. **Yayın** düğmesinin altında bir bağlantı olarak sağlanan demo web sitesinde bot deneyin.

    Yeni bir web sayfası bot ile açılır. Bota aynı test sorusunu sorun:`What is a knowledge base?`

    > [!div class="mx-imgBorder"]
    > ![Son ajan tuval](../media/how-to-integrate-power-virtual-agent/demo-chat-bot.png)

## <a name="share-your-bot"></a>Botunuzu paylaşın

Demo web sitesini paylaşmak için, bir kanal olarak yapılandırır.

1. Sol **navigasyondan** **Kanalları Yönet'i** seçin.

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