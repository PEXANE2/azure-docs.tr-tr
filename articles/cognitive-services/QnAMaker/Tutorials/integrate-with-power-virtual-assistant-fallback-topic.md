---
title: 'Öğretici: Power Virtual Agents ile tümleştirme-Soru-Cevap Oluşturma'
description: Bu öğreticide, etkin öğrenme ile bilgi Bankalarınızın kalitesini geliştirebilirsiniz. Mevcut soruları kaldırmadan veya değiştirmeden gözden geçirin, kabul edin veya reddedin veya ekleyin.
ms.topic: tutorial
ms.date: 06/08/2020
ms.openlocfilehash: f1d51f6ad8892252161238eb71fbb02f463463fd
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "84635396"
---
# <a name="tutorial-add-your-knowledge-base-to-power-virtual-agents"></a>Öğretici: bilgi tabanınızı Power Virtual Agents 'e ekleme
Bilgi tabanınızdan yanıt sağlamak için bir [Power Virtual Agents](https://powervirtualagents.microsoft.com/) bot oluşturun ve genişletin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

<!-- green checkmark -->
> [!div class="checklist"]
> * Power sanal aracıları oluştur bot
> * Bir sistem geri dönüş konusu oluşturun
> * Power otomatikleştir akışı olarak bir konuya eylem olarak Soru-Cevap Oluşturma ekleme
> * Power otomatikleştir çözümü oluşturma
> * Çözümünüze Power otomatikleştir akışı ekleme
> * Power Virtual aracılarını yayımlama
> * Power Virtual aracılarını test edin ve Soru-Cevap Oluşturma bilgi tabanınızdan bir yanıt alın

## <a name="integrate-an-agent-with-a-knowledge-base"></a>Bir aracıyı Bilgi Bankası ile tümleştirme

[Güç sanal aracıları](https://powervirtualagents.microsoft.com/) , ekiplerin Kılavuzlu, kod içermeyen bir grafik arabirimi kullanarak güçlü botlar oluşturmalarına olanak tanır. Veri bilimcileri veya geliştiricilere ihtiyacınız yoktur.

Güç sanal aracılarında, eylemler gerçekleştirerek Kullanıcı sorularını yanıtlamak için bir dizi konuyla (konu alanı) bir aracı oluşturursunuz. Bir Yanıt bulunamazsa, sistem geri dönüşü bir yanıt döndürebilir.

Bir konunun eyleminin bir parçası olarak veya *sistem geri dönüş* konu yolunun bir parçası olarak soruyu bilgi bankasına göndermek için aracıyı yapılandırın. Bunlar her ikisi de bilgi tabanınızı bağlamak ve bir yanıt döndürmek için bir eylem kullanır.

## <a name="power-automate-connects-to-generateanswer-action"></a>Güç otomatikleştirme, `GenerateAnswer` eyleme bağlanır

Aracınızı bilgi tabanınızı bağlamak için, bu eylemi oluşturmak üzere güç otomatikleştirmesini kullanın. Güç otomatikleştirme, Soru-Cevap Oluşturma API 'sine bağlanan bir işlem akışı sağlar `GenerateAnswer` .

Akışı tasarladıktan ve kaydettikten sonra, Power otomatikleştir çözümünden erişilebilir. Bu çözümü aracısında bir eylem olarak kullanın.

## <a name="connect-an-agent-to-your-knowledge-base"></a>Bilgi bankasında bir aracıyı bağlama

Power sanal aracılarında bir aracıyı Soru-Cevap Oluşturma bir bilgi tabanına bağlama adımlarına genel bir bakış aşağıda verilmiştir.

* [Soru-cevap oluşturma](https://www.qnamaker.ai/) portalında:
    * Bilgi tabanınızı derleyin ve yayımlayın.
    * KIMLIK, çalışma zamanı uç noktası anahtarı ve çalışma zamanı uç noktası Konağı dahil Bilgi Bankası ayrıntılarınızı kopyalayın.
* [Power Virtual Agents](https://powerva.microsoft.com/) portalında:
    * Bir aracı konu başlığı oluşturun.
    * Bir eylem çağırın (akışı Power otomatikleştirmek için).
* [Power otomatikleştir](https://us.flow.microsoft.com/) portalında:
    * Soru-Cevap Oluşturma şablonu _kullanarak yanıt oluştur_ araması yapın
    * Akışı [soru-cevap oluşturma GenerateAnswer](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)kullanacak şekilde yapılandırmak için şablon kullanın.
        * Soru-Cevap Oluşturma yayımlanan Bilgi Bankası bilgileri:
            * Bilgi Bankası KIMLIĞI
            * Soru-Cevap Oluşturma kaynak uç noktası Konağı
            * Soru-Cevap Oluşturma kaynak uç noktası anahtarı
        * Giriş-Kullanıcı sorgusu
        * Çıkış-Bilgi Bankası yanıtı
    * Bir çözüm oluşturun ve akışı ekleyin veya mevcut çözüme akış ekleyin.
* Power sanal aracılarına geri dön:
    * Bir konu başlığı olarak çözümün çıkışını bir ileti olarak seçin.

## <a name="create-and-publish-a-knowledge-base"></a>Bilgi bankası oluşturma ve yayımlama

1. Bilgi Bankası oluşturmak için [hızlı](../Quickstarts/create-publish-knowledge-base.md) başlangıcı izleyin. Son bölümü, bir bot oluşturma hakkında tamammayın. Bunun yerine, Power sanal aracılarıyla bir bot oluşturmak için bu öğreticiyi kullanın.

    > [!div class="mx-imgBorder"]
    > ![Yayımlanan bilgi tabanı ayarlarının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

    [Soru-cevap oluşturma](https://www.qnamaker.ai/) portalındaki **Ayarlar** sayfasında bulunan yayımlanmış bilgi tabanı ayarlarınızı girin. Soru-Cevap Oluşturma bağlantınızı yapılandırmak için [Power otomatikleştir adımında](#create-a-power-automate-flow-to-connect-to-your-knowledge-base) bu bilgilere ihtiyacınız olacaktır `GenerateAnswer` .

1. Soru-Cevap Oluşturma portalında, **Ayarlar** sayfasında, uç nokta anahtarını, uç nokta konağını ve BILGI Bankası kimliğini bulun.

## <a name="create-an-agent-in-power-virtual-agents"></a>Power Virtual aracılarında bir aracı oluşturma

1. [Power sanal aracılarında oturum açın](https://go.microsoft.com/fwlink/?LinkId=2108000&clcid=0x409). Okul veya iş e-posta hesabınızı kullanın.

1. İlk bot ise, aracının **ana** sayfasında olursunuz. İlk bot sizin değilse, sayfanın sağ üst kısmından bot ' ı seçin ve **+ yeni bot**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Power Virtual Agents ana sayfasının ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-home.png)

## <a name="topics-provided-in-the-bot"></a>Bot 'ta sunulan konular

Aracı konu alanındaki soruları yanıtlamak için konu koleksiyonunu kullanır. Bu öğreticide, aracı sizin için sunulan birçok konuya sahiptir ve Kullanıcı konuları ve sistem konuları ' na bölünmüştür.

Bot tarafından sunulan konuları görmek için sol gezinmede bulunan **konuları** seçin.

> [!div class="mx-imgBorder"]
> ![Aracıda belirtilen konuların ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topics-provided.png)


## <a name="create-the-system-fallback-topic"></a>Sistem geri dönüş konusunu oluşturma

Aracı herhangi bir konudan bilgi tabanınızı bağlayabilse de, bu öğretici *sistem geri dönüş* konusunu kullanır. Aracı bir yanıt bulamadığında geri dönüş konusu kullanılır. Aracı kullanıcının metnini Soru-Cevap Oluşturma API 'sine geçirir `GenerateAnswer` , bilgi tabanınızdan yanıtı alır ve kullanıcıya ileti olarak görüntüler.

1. [Power Virtual Agents](https://powerva.microsoft.com/#/) portalında, sağ üst köşedeki **Ayarlar** ' ı (dişli simgesi) seçin. Ardından **sistem geri dönüş**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Sistem geri dönüşü için Power Virtual Agents menü öğesinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-system-fallback.png)

1. Bir sistem geri dönüş konusu eklemek için **+ Ekle** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Geri dönüş konusu ekleme ekranının ekran görüntüsü.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-add-fallback-topic.png)

1. Konu eklendikten sonra, yazma tuvalindeki geri dönüş konusunu yazmak için **geri dönüş konusuna git** ' i seçin.

    > [!TIP]
    > Geri dönüş konusuna geri dönmeniz gerekirse, **sistem** konularının bir parçası olarak **konular** bölümünde bulunur.

## <a name="use-the-authoring-canvas-to-add-an-action"></a>Bir eylem eklemek için yazma tuvali 'ni kullanma

Bilgi bankalarınıza geri dönüş konusunu bağlamak için Power Virtual Agents yazma tuvalinden yararlanabilirsiniz. Konu, tanınmayan Kullanıcı metniyle başlar. Bu metni Soru-Cevap Oluşturma geçiren ve sonra yanıtı ileti olarak gösteren bir eylem ekleyin. Bir yanıtı görüntülemenin son adımı, Bu öğreticinin ilerleyen bölümlerinde [ayrı bir adım](#add-your-solutions-flow-to-power-virtual-agents)olarak işlenir.

Bu bölüm, geri dönüş konusu konuşma akışını oluşturur.

1. Yeni geri dönüş eylemi, zaten konuşma akışı öğelerine sahip olabilir. **Seçenekler** menüsünü seçerek **ilerleme** öğesini silin.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/delete-escalate-action-using-option-menu.png" alt-text="Silme seçeneği vurgulanmış şekilde, konuşma akışının kısmi ekran görüntüsü.":::

1. **+** **İleti** kutusuna bağlayıcı akışını seçin ve ardından **eylem çağır**' ı seçin.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png" alt-text="Bir eylemi çağıran kısmi ekran görüntüsü.":::

1. **Akış oluştur**'u seçin. İşlem sizi Power otomatikleştir portalına götürür.

    > [!div class="mx-imgBorder"]
    > ![Akış oluştur ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/create-a-flow.png)


    Güç otomatikleştirme, yeni bir şablon olarak açılır. Bu yeni şablonu kullanmayacağız.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-automate-flow-initial-template.png" alt-text="Yeni akış şablonuyla Power otomatikleştirmenin kısmi ekran görüntüsü.":::

## <a name="create-a-power-automate-flow-to-connect-to-your-knowledge-base"></a>Bilgi tabanınızı bağlamak için Power otomatikleştir akışı oluşturma

Aşağıdaki yordam şu şekilde bir güç otomatikleştirme akışı oluşturur:
* Gelen Kullanıcı metnini alır ve Soru-Cevap Oluşturma gönderir.
* En iyi yanıtı aracıya geri döndürür.

1. **Power otomatikleştirmede**sol gezinti bölmesinde **Şablonlar** ' ı seçin. Tarayıcı sayfasından çıkmak isteyip istemediğiniz sorulursa, bırak seçeneğini kabul edin.

1. Şablonlar sayfasında, şablon **soru-cevap oluşturma kullanarak yanıt oluştur** ' u arayın ve şablonu seçin. Bu şablonda, Bilgi Bankası ayarlarınıza Soru-Cevap Oluşturma çağrısı yapmak ve en iyi yanıtı döndürmek için tüm adımlar bulunur.

1. Soru-Cevap Oluşturma akışının yeni ekranında **devam**' ı seçin.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-automate-qna-flow-template-continue.png" alt-text="Devam düğmesi vurgulanmış Soru-Cevap Oluşturma şablon akışının kısmi ekran görüntüsü.":::

1. **Yanıt oluştur** eylem kutusunu seçin ve [Bilgi Bankası oluşturma ve yayımlama](#create-and-publish-a-knowledge-base)başlıklı önceki bir bölümden soru-cevap oluşturma ayarlarınızı girin. Aşağıdaki görüntüdeki **hizmet ana** Bilgisayarınız, Bilgi Bankası ana bilgisayar **ana** bilgisayarınız ile ilgilidir ve biçiminde olur `https://YOUR-RESOURCE-NAME.azurewebsites.net/qnamaker` .


    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fill-in-generate-answer-settings.png" alt-text="Devam düğmesi vurgulanmış Soru-Cevap Oluşturma şablon akışının kısmi ekran görüntüsü.":::

1. Akışı kaydetmek için **Kaydet**’i seçin.

## <a name="create-a-solution-and-add-the-flow"></a>Bir çözüm oluşturun ve akışı ekleyin

Aracının akışı bulması ve akışa bağlanması için akışın bir güç otomatikleştirme çözümüne dahil olması gerekir.

1. Power otomatikleştir portalında hala, sol taraftaki gezinmede **çözümler** ' i seçin.

1. **+ Yeni çözüm**’ü seçin.

1. Görünen ad girin. Çözüm listesi, kuruluşunuzdaki veya okulunuzdaki her çözümü içerir. Yalnızca çözümlerinizi filtrelemenize yardımcı olacak bir adlandırma kuralı seçin. Örneğin, e-postanızı çözümünüz adına önek olarak ekleyebilirsiniz: `jondoe-power-virtual-agent-qnamaker-fallback` .

1. Seçenekler listesinden yayımcıyı seçin.

1. Ad ve sürüm için varsayılan değerleri kabul edin.

1. İşlemi tamamlaması için **Oluştur** ' u seçin.

## <a name="add-your-flow-to-the-solution"></a>Akışınızı çözüme ekleyin

1. Çözümler listesinde, az önce oluşturduğunuz çözümü seçin. Listenin en üstünde olmalıdır. Aksi takdirde, çözüm adının bir parçası olan e-posta adınızla arama yapın.

1. Çözümde **+ Varolanı Ekle**' yi seçin ve listeden **Flow** ' u seçin.

1. **Dış çözümler** listesinden akışınızı bulun ve ardından **Ekle** ' yi seçerek işlemi sona erdirin. Çok sayıda akış varsa, en son akışı bulmak için **değiştirilen** sütununa bakın.

## <a name="add-your-solutions-flow-to-power-virtual-agents"></a>Çözümünüzün akışını Power Virtual Agent 'a ekleyin

1. Güç sanal aracılarında aracınızın bulunduğu tarayıcı sekmesine dönün. Yazma tuvali hala açık olmalıdır.

1. Akışa yeni bir adım eklemek için **ileti** eylemi kutusunun üstünde **+** bağlayıcısını seçin. Sonra **bir eylem çağır**' ı seçin.

1. **Akış** açılır penceresinde, **soru-cevap oluşturma Bilgi Bankası 'Nı kullanarak yanıtları oluştur**adlı yeni akışı seçin... Yeni eylem akışta görüntülenir.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-flow-after-adding-action.png" alt-text="Soru-Cevap Oluşturma Flow eklendikten sonra Power Virtual Agent konusunun konuşma tuvalinin kısmi ekran görüntüsü.":::

1. Giriş değişkenini Soru-Cevap Oluşturma eyleme doğru şekilde ayarlamak için, **değişken Seç**' i seçin ve ardından bot ' ı seçin **. UnrecognizedTriggerPhrase**.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-selection-action-input.png" alt-text="Power Virtual Agent konusunun konuşma tuvali 'nin giriş değişkenini seçen kısmi ekran görüntüsü.":::


1. Çıkış değişkenini Soru-Cevap Oluşturma eyleme doğru şekilde ayarlamak için, **ileti** eyleminde, **Unrecognizedtriggertümcecik**' i seçin, sonra bir değişken eklemek için simgeyi seçin ve `{x}` ardından **FinalAnswer**' ı seçin.

1. Bağlam araç çubuğundan **Kaydet**' i seçerek konunun yazma tuvali ayrıntılarını kaydedin.

Son aracı tuvali aşağıda verilmiştir.

> [!div class="mx-imgBorder"]
> ![Son aracı tuvalinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

## <a name="test-the-agent"></a>Aracıyı test etme

1. Test bölmesinde, **konular arasında izleme**' yi değiştirin. Bu, konular arasındaki ilerlemeyi ve tek bir konu dahilinde izlemenize olanak sağlar.

1. Kullanıcı metnini aşağıdaki sırayla girerek aracıyı test edin. Yazma tuvali, başarılı adımları yeşil onay işaretiyle bildirir.

    |Soru sırası|Test soruları|Amaç|
    |--|--|--|
    |1|Hello|Konuşmaya başla|
    |2|Depolama saatleri|Örnek konu. Bu, sizin için sizin için herhangi bir ek iş olmadan yapılandırılır.|
    |3|Evet|Yanıt olarak `Did that answer your question?`|
    |4|Mükemmel|Yanıt olarak `Please rate your experience.`|
    |5|Evet|Yanıt olarak `Can I help with anything else?`|
    |6|Sorgu tahminleri için üretilen iş performansını nasıl geliştirebilirim?|Bu soru, yanıt vermek için metin bilgi tabanınızı gönderen geri dönüş eylemini tetikler. Ardından Yanıt gösterilir. tek tek eylemler için yeşil onay işaretleri her bir eylem için başarıyı gösterir.|

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png" alt-text="Başarılı eylemler için yeşil onay işaretlerini gösteren tuval ile sohbet bot ekran görüntüsü.":::

## <a name="publish-your-bot"></a>Botunuzu yayımlayın

Aracıyı okulunuzun veya kuruluşunuzun tüm üyeleri için kullanılabilir hale getirmek için, yayımlamanız gerekir.

1. Soldaki gezinmede **Yayımla**' yı seçin. Sonra sayfada **Yayımla** ' yı seçin.

1. Tanıtım Web sitesinde botunuzu deneyin ( **Yayımla**altındaki bağlantıyı arayın).

    Bot 'unuzla yeni bir Web sayfası açılır. Bot 'a aynı test sorusunu sorun: `How can I improve the throughput performance for query predictions?`

    > [!div class="mx-imgBorder"]
    > ![Son aracı tuvalinin ekran görüntüsü](../media/how-to-integrate-power-virtual-agent/demo-chat-bot.png)

## <a name="share-your-bot"></a>Botunuzu paylaşma

Tanıtım Web sitesini paylaşmak için kanal olarak yapılandırın.

1. Sol taraftaki gezinmede kanalları **Yönet**' i seçin  >  **Channels**.

1. Kanallar listesinden **demo web sitesi** ' ni seçin.

1. Bağlantıyı kopyalayın ve **Kaydet**' i seçin. Tanıtım Web sitenizin bağlantısını okulunuza veya kuruluş üyelerinize bir e-postaya yapıştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi Bankası ile işiniz bittiğinde, Azure portal Soru-Cevap Oluşturma kaynakları kaldırın.

## <a name="next-step"></a>Sonraki adım

[Bilgi bankanız hakkında analizler alma](../How-To/get-analytics-knowledge-base.md)

Aşağıdakiler hakkında daha fazla bilgi edinin:
* [Power Virtual Agents](https://docs.microsoft.com/power-virtual-agents/)
* [Power Automate](https://docs.microsoft.com/power-automate/)
* [Soru-cevap oluşturma Bağlayıcısı](https://us.flow.microsoft.com/connectors/shared_cognitiveservicesqnamaker/qna-maker/) ve [bağlayıcı ayarları](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)