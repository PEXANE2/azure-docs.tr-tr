---
title: Sorun giderme-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma hizmetiyle ilgili en sık sorulan soruların seçkin listesi, hizmeti daha hızlı ve daha iyi sonuçlarla benimsemenizi sağlayacak.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 09/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: c0ceea5c86af8d733c838e19f77eaeadc8a12dbb
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300231"
---
# <a name="troubleshooting-for-qna-maker"></a>Soru-Cevap Oluşturma için sorun giderme

Soru-Cevap Oluşturma hizmetiyle ilgili en sık sorulan soruların seçkin listesi, hizmeti daha hızlı ve daha iyi sonuçlarla benimsemenizi sağlayacak.


## <a name="how-to-get-the-qnamaker-service-hostname"></a>QnAMaker hizmet ana bilgisayar adını alma

QnAMaker desteği veya UserVoice başvurduğunuzda QnAMaker hizmeti ana bilgisayar adı hata ayıklama amaçları için yararlıdır. Ana bilgisayar adı şu biçimdeki bir URL: https:// *{hostname}* . azurewebsites.net.
    
1. QnAMaker hizmetinize (kaynak grubu) Git [Azure portalı](https://portal.azure.com)

    ![Azure portalında QnAMaker Azure kaynak grubu](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Soru-Cevap Oluşturma kaynakla ilişkili App Service seçin. Genellikle, adlar aynıdır.

     ![QnAMaker uygulama hizmetini seçin](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Konak adı URL genel bakış bölümünde kullanılabilir

    ![QnAMaker ana bilgisayar adı](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="use-the-help-bot-in-the-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında yardım bot 'ı kullanma

Soru-Cevap Oluşturma size yardımcı olmak için Soru-Cevap Oluşturma portalında bir **Yardım** bot sağlar. Yardım bot, her Web sayfasında kullanılabilir. Bot, yanıtları sağlamak için soru-cevap oluşturma kullanır ve kendi yanıt bot 'ınızla hızlıca çalışmaya devam edebilmeniz için bot [ C# Framework kod projesini](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-support) bot 'a sağlar. 

![! [Soru-Cevap Oluşturma size yardımcı olmak için Soru-Cevap Oluşturma portalında bir * * yardım * * bot sağlar.] (./Media/qnamaker-FAQ/Use-QNA-Maker-Help-bot-to-learn-QNA-Maker-Service.exe)](./media/qnamaker-faq/use-qna-maker-help-bot-to-learn-qna-maker-service.png#lightbox)

## <a name="manage-the-knowledge-base"></a>Bilgi Bankası'nı yönetme

### <a name="i-accidentally-deleted-a-part-of-my-qna-maker-what-should-i-do"></a>Yanlışlıkla sildiğim bir parçası my soru-cevap Oluşturucu, neler yapabilirim? 

Arama veya Web uygulaması gibi Soru-Cevap Oluşturma kaynakla birlikte oluşturulan Azure hizmetlerinden hiçbirini silmeyin. Bunlar Soru-Cevap Oluşturma çalışması için gereklidir, Soru-Cevap Oluşturma doğru çalışmayı durdurur.

Kalıcı soru ve cevap çiftlerini, dosyaları, URL'ler, özel sorular ve yanıtlar, bilgi bankalarından veya Azure kaynakları dahil olmak üzere tüm siler. Bilgi Bankası dışarı aktardığınızdan emin olun **ayarları** bilgi bankanızı herhangi bir bölümünü silmeden önce sayfa. 

### <a name="why-is-my-urlsfiles-not-extracting-question-answer-pairs"></a>Neden benim URL'lerini veya soru-cevap çiftlerini değil Ayıklanan dosyalar?

Soru-cevap Oluşturucu otomatik-geçerli SSS URL'lerinden soru-cevap (soru-cevap) içerikler ayıklama emin olamaz mümkündür. Böyle durumlarda soru-cevap içeriğini bir .txt dosyasına yapıştırma ve aracı, veri alabilen, bkz. Alternatif olarak, bilgi bankanızı düzenleyerek içerik bilgi bankanızı ekleyebileceğiniz [soru-cevap Oluşturucu portalı](https://qnamaker.ai).

### <a name="how-large-a-knowledge-base-can-i-create"></a>Ne kadar büyük bir bilgi bankası oluşturabilirim?

Soru-cevap Oluşturucu hizmeti oluştururken seçtiğiniz SKU, Azure Search'te Bilgi Bankası boyutuna bağlıdır. Okuma [burada](./Tutorials/choosing-capacity-qnamaker-deployment.md) daha fazla ayrıntı için.

### <a name="why-cant-i-see-anything-in-the-drop-down-when-i-try-to-create-a-new-knowledge-base"></a>Yeni Bilgi Bankası oluşturmaya çalıştığınızda neden açılan herhangi bir şey göremiyorum?

Azure'da henüz herhangi bir soru-cevap Oluşturucu hizmeti oluşturmadınız. Okuma [burada](./How-To/set-up-qnamaker-service-azure.md) bunun nasıl yapılacağını öğrenin.

### <a name="how-do-i-share-a-knowledge-base-with-others"></a>Nasıl Bilgi Bankası başkalarıyla paylaşırım?

Soru-cevap Oluşturucu hizmetini düzeyinde çalışır paylaşımı, diğer bir deyişle, hizmetteki tüm bilgi bankalarından paylaşılır. Okuma [burada](./How-To/collaborate-knowledge-base.md) nasıl Bilgi Bankası üzerinde işbirliği yapın.

### <a name="can-you-share-a-knowledge-base-with-a-contributor-that-is-not-in-the-same-aad-tenant-to-modify-a-knowledge-base"></a>Bilgi Bankası değiştirmek için aynı AAD kiracısında değil bir katkıda bulunan ile Bilgi Bankası paylaşabilir miyim? 

Paylaşımı Azure rol tabanlı erişim denetimi (RBAC) bağlıdır. Dilerseniz, _herhangi_ kaynak başka bir kullanıcı ile Azure'da soru-cevap Oluşturucu da paylaşabilirsiniz.

### <a name="if-you-have-an-app-service-plan-with-5-qnamaker-knowledge-bases-can-you-assign-readwrite-rights-to-5-different-users-so-each-of-them-can-access-only-1-qnamaker-knowledge-base"></a>Bir App Service planı ile 5 QnAMaker bilgi bankalarından varsa. Bunların her biri yalnızca 1 QnAMaker Bilgi Bankası erişebilmesi için 5 farklı kullanıcılara Okuma/Yazma hakkı atayabilirim miyim?

QnAMaker hizmetin tamamı, bireysel bilgi bankalarından paylaşabilirsiniz.

### <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>İyi bir eşleşme bulunduğunda varsayılan ileti nasıl değiştirebilirim?

Varsayılan ileti ayarları, App Service'te bir parçasıdır.
- Azure portalında uygulama hizmeti kaynağınıza gidin

![qnamaker appservice](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Tıklayarak **ayarları** seçeneği

![qnamaker appservice ayarları](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Değiştirin **DefaultAnswer** ayarı
- App service'ı yeniden başlatın

![qnamaker appservice yeniden başlatma](./media/qnamaker-faq/qnamaker-appservice-restart.png)

### <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Neden SharePoint bağlantımı ayıklanan değil mi?

Bkz: [veri kaynağı konumları](./Concepts/data-sources-supported.md#data-source-locations) daha fazla bilgi için.

### <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>My Bilgi Bankası'na yaptığım güncelleştirmeler bankamda. Neden?

Her düzenleme işleminden bir tablo güncelleştirme, test veya ayar olup olmadığını yayımlanabilmesi için önce kaydedilmesi gerekir. Tıkladığınızdan emin olun **kaydedin ve eğitme** her düzenleme işleminden sonra düğme.

### <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Multimedya ve Bilgi Bankası zengin veriler mu?

Bilgi bankası Markdown’ı destekler. Bununla birlikte, URL'lerden otomatik ayıklama HTML Markdown'a dönüştürme becerisi sınırlıdır. Tam özellikli Markdown kullanmak istiyorsanız, içeriğinizi doğrudan Tablo'da değiştirebilir ya da zengin içerikli bir Bilgi Bankası karşıya yükleyin.

Multimedya, resimler ve videolar gibi şu anda desteklenmiyor.

### <a name="does-qna-maker-support-non-english-languages"></a>Soru-cevap Oluşturucu İngilizce dışındaki dilleri destekler mi?

Daha fazla ayrıntı görmek [desteklenen diller](./Overview/languages-supported.md).

Birden çok dil içerik varsa, her dil için ayrı bir hizmet oluşturmak emin olun.

## <a name="manage-service"></a>Hizmeti yönetme

### <a name="when-should-i-restart-my-app-service"></a>App service ne zaman yeniden başlatmanız gerekir? 

Bilgi Bankası'ndaki sürüm değeri yanında uyarı simgesi olduğunda uygulama hizmetinizi yenileyin **uç nokta anahtarları** üzerinde tablo **kullanıcı ayarları** [sayfa](https://www.qnamaker.ai/UserSettings).

### <a name="i-deleted-my-existing-search-service-how-can-i-fix-this"></a>Var olan arama hizmetimi sildim. Bunu nasıl giderebilirim?

Bir Azure Search dizini silerseniz, işlem son ' dur ve Dizin kurtarılamıyor. 

### <a name="i-deleted-my-testkb-index-in-my-search-service-how-can-i-fix-this"></a>Arama hizmetimde `testkb` dizinimi sildim. Bunu nasıl giderebilirim? 

Eski verileriniz kurtarılamıyor. Yeni bir Soru-Cevap Oluşturma kaynağı oluşturun ve bilgi bankanızı tekrar oluşturun.

### <a name="when-should-i-refresh-my-endpoint-keys"></a>Uç nokta anahtarlarımı ne zaman yenilemeniz gerekir?

Uç nokta anahtarlarınızı geçirildiğini, şüpheleniyorsanız yenileyin.

### <a name="can-i-use-the-same-azure-search-resource-for-knowledge-bases-using-multiple-languages"></a>Aynı Azure Search kaynak kullanarak birden çok dil, bilgi bankaları için kullanabilir miyim?

Birden çok dil ve birden çok bilgi bankalarından kullanmak için her dil için bir soru-cevap Oluşturucu kaynak oluşturmak kullanıcı vardır. Bu, dil başına ayrı bir Azure Arama hizmeti oluşturur. Tek bir Azure arama hizmetinde farklı dil bilgi bankalarından karıştırma sonuçları düşürülmüş ilgi düzeyi neden olur.

### <a name="how-can-i-change-the-name-of-the-azure-search-resource-used-by-qna-maker"></a>Soru-cevap Oluşturucu tarafından kullanılan Azure Search kaynağın adını nasıl değiştirebilirim?

Azure Search kaynağın adını sonuna birkaç rastgele bir harf ile soru-cevap Oluşturucu kaynak adıdır. Bu soru-cevap Oluşturucu için birden çok arama kaynakları ayırt zorlaştırır. (İstediğiniz şekilde adlandırma) ayrı bir Azure Search hizmeti oluşturma ve soru-cevap hizmetinize bağlanın. Adımlar, [bir Azure aramasını yükseltmek](How-To/set-up-qnamaker-service-azure.md#upgrade-the-azure-search-service)için yapmanız gereken adımlara benzerdir.

### <a name="when-qna-maker-returns-runtime-core-is-not-initialized-how-do-i-fix-it"></a>Soru-cevap oluşturma, bunu `Runtime core is not initialized,` nasıl giderebilirim?

App Service 'niz için disk alanı dolu olabilir. Disk alanınızı çözme adımları:

1. [Azure Portal](https://portal.azure.com), soru-cevap oluşturma App Service ' i seçin ve ardından hizmeti durdurun.
1. Uygulama hizmetinde hala **geliştirme araçları**' nı ve **Gelişmiş Araçlar**' ı seçin, sonra da **Git**' i seçin. Bu, yeni bir tarayıcı penceresi açar.
1. Komut satırı aracını açmak için **hata ayıklama konsolu**' nu ve ardından **cmd** ' yi seçin. 
1. _Site/Wwwroot/veri/QnAMaker/_ dizine gidin.
1. Adı ile `rd`başlayan tüm klasörleri kaldırın. 

    Aşağıdakileri **silmeyin** :

    * KbIdToRankerMappings. txt dosyası
    * EndpointSettings. JSON dosyası
    * EndpointKeys klasörü 

1. App Service 'i başlatın.
1. Şimdi çalıştığını doğrulamak için bilgi bankaıza erişin. 


## <a name="integrate-with-other-services-including-bots"></a>Botlar gibi diğer hizmetlerle tümleştirme

### <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Soru-Cevap Oluşturucu’yu kullanmak için Bot Framework’ü kullanmam mı gerekir?

Hayır, Soru-Cevap Oluşturma ile [bot çerçevesini](https://github.com/Microsoft/botbuilder-dotnet) kullanmanız gerekmez. Ancak, Soru-Cevap Oluşturma [Azure bot hizmetinde](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)birkaç şablondan biri olarak sunulur. Bot hizmeti, Microsoft robot altyapısı aracılığıyla, hızla akıllı robot geliştirme sağlar ve bir sunucusuz ortamda çalışır.

### <a name="how-can-i-create-a-new-bot-with-qna-maker"></a>Soru-Cevap Oluşturma yeni bir bot nasıl oluşturabilirim?

Bölümündeki yönergeleri [bu](./Tutorials/create-qna-bot.md) ile Azure Bot hizmeti Botunuza oluşturulacağını belgeler.

### <a name="how-do-i-use-a-different-knowledge-base-with-an-existing-azure-bot-service"></a>Nasıl yaparım? mevcut bir Azure bot hizmeti ile farklı bir Bilgi Bankası mı kullanıyorsunuz?

Bilgi tabanınız hakkında aşağıdaki bilgilere sahip olmanız gerekir:

* Bilgi Bankası KIMLIĞI.
* Bilgi Bankası 'nın yayımlanmış uç nokta ana bilgisayar adı-yayımladıktan sonra **Ayarlar** sayfasında bulunur.
* Bilgi Bankası 'nın yayımlanmış uç nokta anahtarı-yayımladıktan sonra **Ayarlar** sayfasında bulunur. 

Bu bilgilerle, Azure portal bot 'ın App Service 'e gidin. **Ayarlar-> yapılandırma-> uygulama ayarları**altında bu değerleri değiştirin.  

Bilgi Bankası 'nın uç nokta anahtarı, ABS `QnAAuthkey` hizmetinde etiketlidir. 

### <a name="can-two-or-more-client-applications-share-a-knowledge-base"></a>İki veya daha fazla istemci uygulaması Bilgi Bankası 'nı paylaşabilir mi? 

Evet, Bilgi Bankası 'nda herhangi bir sayıda istemciden sorgulanabilecek. Bilgi bankasından gelen yanıt yavaş veya zaman aşımına uğrar görünüyorsa, Bilgi Bankası ile ilişkili App Service 'in hizmet katmanını yükseltmeyi göz önünde bulundurun.

### <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Soru-Cevap Oluşturucu hizmetini web siteme nasıl ekleyebilirim?

Soru-cevap Oluşturucu hizmetini Web sitenize bir web Sohbeti denetimi olarak eklemek için şu adımları izleyin:

1. Yönergeleri takip ederek, SSS Robotu oluşturun [burada](./Tutorials/create-qna-bot.md).
2. Adımları izleyerek web Sohbeti etkinleştirin [burada](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat)

## <a name="data-storage"></a>Veri depolama

### <a name="what-data-is-stored-and-where-is-it-stored"></a>Hangi verilerin depolanır ve nerede depolanır? 

Soru-cevap Oluşturucu hizmetinizi oluşturduğunuzda, bir Azure bölgesi seçildi. Bilgi bankaları ve günlük dosyaları, bu bölgede depolanır. 
