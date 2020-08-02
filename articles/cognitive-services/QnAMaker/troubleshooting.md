---
title: Sorun giderme-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma hizmetiyle ilgili en sık sorulan soruların seçkin listesi, hizmeti daha hızlı ve daha iyi sonuçlarla benimsemenizi sağlayacak.
ms.topic: troubleshooting
ms.date: 03/25/2020
ms.openlocfilehash: 50823e29eb5c276c00a1c8f0e3227ec67999a703
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87502588"
---
# <a name="troubleshooting-for-qna-maker"></a>Soru-Cevap Oluşturma için sorun giderme

Soru-Cevap Oluşturma hizmetiyle ilgili en sık sorulan soruların seçkin listesi, hizmeti daha hızlı ve daha iyi sonuçlarla benimsemenizi sağlayacak.

<a name="how-to-get-the-qnamaker-service-hostname"></a>

## <a name="manage-predictions"></a>Tahminleri yönetme

<details>
<summary><b>Sorgu tahminleri için üretilen iş performansını nasıl geliştirebilirim?</b></summary>

**Cevap**: performans sorunları, hem App Service hem de bilişsel arama için ölçeği büyütme yapmanız gerektiğini gösterir. Performansı artırmak için Bilişsel Arama bir çoğaltma eklemeyi düşünün.

[Fiyatlandırma katmanları](Concepts/azure-resources.md)hakkında daha fazla bilgi edinin.
</details>

<details>
<summary><b>QnAMaker hizmet uç noktasını alma</b></summary>

**Yanıt**: qnaoluşturucu hizmeti uç noktası, Qnamaker desteği veya UserVoice ile iletişim kurmak için hata ayıklama amacıyla faydalıdır. Uç noktası bu biçimdeki bir URL 'dir: `https://your-resource-name.azurewebsites.net` .

1. [Azure Portal](https://portal.azure.com) QnAMaker hizmetinize (kaynak grubu) gidin

    ![Azure portal 'de QnAMaker Azure Kaynak grubu](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Soru-Cevap Oluşturma kaynakla ilişkili App Service seçin. Genellikle, adlar aynıdır.

     ![QnAMaker App Service seçin](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Uç nokta URL 'SI genel bakış bölümünde kullanılabilir

    ![QnAMaker uç noktası](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)

</details>

## <a name="manage-the-knowledge-base"></a>Bilgi Bankası 'nı yönetme

<details>
<summary><b>Soru-Cevap Oluşturma bir parçasını yanlışlıkla sildim, ne yapmam gerekir?</b></summary>

**Cevap**: arama veya Web uygulaması gibi soru-cevap oluşturma kaynakla birlikte oluşturulan Azure hizmetlerinden hiçbirini silmeyin. Bunlar Soru-Cevap Oluşturma çalışması için gereklidir, Soru-Cevap Oluşturma doğru çalışmayı durdurur.

Soru ve yanıt çiftleri, dosyalar, URL 'Ler, özel sorular ve yanıtlar, bilgi tabanları veya Azure kaynakları dahil olmak üzere tüm silmeler kalıcıdır. Bilgi Bankalarınızın herhangi bir bölümünü silmeden önce, bilgi bankaınızı **Ayarlar** sayfasından dışarı aktardığınızdan emin olun.

</details>

<details>
<summary><b>URL 'lerim neden soru-cevap çiftlerini ayıklamıyor?</b></summary>

**Cevap**: soru-cevap oluşturma geçerli SSS URL 'lerinden bazı soru-cevap (QNA) içeriğini otomatik olarak ayıklayamıyor olabilir. Bu gibi durumlarda, QnA içeriğini bir. txt dosyasına yapıştırabilir ve aracın bunu alıp alabildiğinden bkz. Alternatif olarak, [soru-cevap oluşturma portalı](https://qnamaker.ai)aracılığıyla bilgi bankasında içerik ekleyebilirsiniz.

</details>

<details>
<summary><b>Bilgi bankasını ne kadar büyük bir şekilde oluşturabilirim?</b></summary>

**Cevap**: bilgi bankasındaki boyut, soru-cevap oluşturma hizmeti oluştururken seçtiğiniz Azure Search SKU 'suna bağlıdır. Daha ayrıntılı bilgi için [burayı](./Tutorials/choosing-capacity-qnamaker-deployment.md) okuyun.

</details>

<details>
<summary><b>Yeni bir Bilgi Bankası oluşturmaya çalıştığımda neden açılan kutuda hiçbir şey göremiyorum?</b></summary>

**Cevap**: Azure 'da henüz hiç soru-cevap oluşturma hizmeti oluşturmadınız. Bunu nasıl yapacağınızı öğrenmek için [buradan](./How-To/set-up-qnamaker-service-azure.md) okuyun.

</details>

<details>
<summary><b>Bilgi Bankası 'nı başkalarıyla paylaşmak Nasıl yaparım? mı?</b></summary>

**Cevap**: soru-cevap oluşturma bir hizmet düzeyinde paylaşım çalışır, diğer bir deyişle, hizmette tüm bilgi tabanları paylaşılır. Bilgi Bankası 'nda nasıl [işbirliği yapılacağını okuyun](./How-To/collaborate-knowledge-base.md) .

</details>

<details>
<summary><b>Bilgi Bankası 'nı değiştirmek için aynı AAD kiracısında olmayan bir katkıda bulunan bir Bilgi Bankası paylaşabilir misiniz?</b></summary>

**Yanıt**: paylaşım, Azure rol tabanlı erişim denetimi 'Ne (Azure RBAC) dayanır. Azure 'da _herhangi_ bir kaynağı başka bir kullanıcıyla paylaşabilmek için soru-cevap oluşturma de paylaşabilirsiniz.

</details>

<details>
<summary><b>5 QnAMaker bilgi tabanı içeren bir App Service planınız varsa. Her birinin yalnızca 1 QnAMaker Bilgi Bankası 'na erişebilmeleri için 5 farklı kullanıcıya okuma/yazma hakları atayabilir misiniz?</b></summary>

**Cevap**: tek bir bilgi bankasını değil, tüm QnAMaker hizmetini paylaşabilirsiniz.

</details>

<details>
<summary><b>Hiçbir iyi eşleşme bulunamadığında varsayılan iletiyi nasıl değiştirebilirim?</b></summary>

**Cevap**: varsayılan Ileti, App Service 'teki ayarların bir parçasıdır.
- Azure portal App Service kaynağına gidin

![qnamaker appservice](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- **Ayarlar** seçeneğine tıklayın

![qnamaker appservice ayarları](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- **Defaultanswer** ayarının değerini değiştirme
- App Service 'i yeniden başlatın

![qnamaker appservice yeniden başlatma](./media/qnamaker-faq/qnamaker-appservice-restart.png)


</details>

<details>
<summary><b>Neden SharePoint bağlantım ayıklanmıyor?</b></summary>

**Cevap**: daha fazla bilgi için bkz. [veri kaynağı konumları](./Concepts/knowledge-base.md#data-source-locations) .

</details>

<details>
<summary><b>Bilgi tabanmda yaptığım güncelleştirmeler Yayımla ' ya yansıtılmıyor. Neden olmasın?</b></summary>

**Cevap**: bir tablo güncelleştirmesinde, testte veya ayarda, yayımlanmadan önce kaydedilmesi gereken her düzenleme işlemi. Her düzenleme işleminden sonra **Kaydet ve eğitme** düğmesine tıkladığınızdan emin olun.

</details>

<details>
<summary><b>Bilgi Bankası zengin veri veya multimedyayı destekliyor mu?</b></summary>

**Cevap**:

#### <a name="multimedia-auto-extraction-for-files-and-urls"></a>Dosyalar ve URL 'Ler için çoklu ortam otomatik ayıklama

* URL 'ler-HTML-Markaşağı dönüştürme özelliği sınırlıdır.
* Dosyalar-desteklenmiyor

#### <a name="answer-text-in-markdown"></a>Markaşağı 'da yanıt metni
Bilgi Bankası 'nda QnA çiftleri olduktan sonra, bir yanıtın markaşağı metnini düzenleyerek genel URL 'lerden kullanılabilen medyaya bağlantılar ekleyebilirsiniz.


</details>

<details>
<summary><b>Soru-Cevap Oluşturma Ingilizce olmayan dilleri destekliyor mu?</b></summary>

**Cevap**: [desteklenen diller](./Overview/languages-supported.md)hakkında daha fazla ayrıntı görüntüleyin.

Birden çok dilde içeriğiniz varsa, her dil için ayrı bir hizmet oluşturmayı unutmayın.

</details>

## <a name="manage-service"></a>Hizmeti yönetme

<details>
<summary><b>App Service 'i ne zaman yeniden başlatmalıyım?</b></summary>

**Cevap**: **Kullanıcı ayarları** [sayfasındaki](https://www.qnamaker.ai/UserSettings) **Endpoint Keys** tablosunda Bilgi Bankası 'nın sürüm değerinin yanında uyarı simgesi olduğunda App Service 'i yenileyin.

</details>

<details>
<summary><b>Var olan arama hizmetimi sildim. Bunu nasıl giderebilirim?</b></summary>

**Cevap**: bir Azure bilişsel arama dizinini silerseniz, işlem nihai ve Dizin kurtarılamıyor.

</details>

<details>
<summary><b>`testkb`Arama hizmetimde dizinimi sildim. Bunu nasıl giderebilirim?</b></summary>

**Cevap**: eski verileriniz kurtarılamıyor. Yeni bir Soru-Cevap Oluşturma kaynağı oluşturun ve bilgi bankanızı tekrar oluşturun.

</details>

<details>
<summary><b>Uç nokta anahtarlarımı ne zaman yenilemem gerekir?</b></summary>

**Cevap**: Bu anahtarların tehlikede olduğunu düşünüyorsanız, uç nokta anahtarlarınızı yenileyin.

</details>

<details>
<summary><b>Birden çok dil kullanarak Bilgi Bankası için aynı Azure Bilişsel Arama kaynağını kullanabilir miyim?</b></summary>

**Cevap**: birden çok dil ve birden çok bilgi tabanı kullanmak için, kullanıcının her dil için bir soru-cevap oluşturma kaynağı oluşturması gerekir. Bu, dil başına ayrı bir Azure Arama hizmeti oluşturur. Tek bir Azure Search hizmetinde farklı dil bilgisi esaslarını karışmaya, sonuçların uygunluğu düşürüldü.

</details>

<details>
<summary><b>Soru-Cevap Oluşturma tarafından kullanılan Azure Bilişsel Arama kaynağının adını nasıl değiştirebilirim?</b></summary>

**Cevap**: Azure bilişsel arama kaynağının adı, sonunda bazı rastgele harfler eklenmiş soru-cevap oluşturma kaynak adıdır. Bu, Soru-Cevap Oluşturma için birden çok arama kaynağı arasında ayrım yapmayı zorlaştırır. Ayrı bir arama hizmeti oluşturun (istediğiniz şekilde adlandırın) ve QnA hizmetinize bağlayın. Adımlar, [bir Azure aramasını yükseltmek](How-To/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service)için yapmanız gereken adımlara benzerdir.

</details>

<details>
<summary><b>Soru-Cevap Oluşturma, `Runtime core is not initialized,` bunu nasıl giderebilirim?</b></summary>

**Cevap**: App Service için disk alanı dolu olabilir. Disk alanınızı çözme adımları:

1. [Azure Portal](https://portal.azure.com), soru-cevap oluşturma App Service ' i seçin ve ardından hizmeti durdurun.
1. Uygulama hizmetinde hala **geliştirme araçları**' nı ve **Gelişmiş Araçlar**' ı seçin, sonra da **Git**' i seçin. Bu, yeni bir tarayıcı penceresi açar.
1. Komut satırı aracını açmak için **hata ayıklama konsolu**' nu ve ardından **cmd** ' yi seçin.
1. _Site/Wwwroot/veri/QnAMaker/_ dizine gidin.
1. Adı ile başlayan tüm klasörleri kaldırın `rd` .

    Aşağıdakileri **silmeyin** :

    * KbIdToRankerMappings.txt dosyası
    * Dosya üzerinde EndpointSettings.js
    * EndpointKeys klasörü

1. App Service 'i başlatın.
1. Şimdi çalıştığını doğrulamak için bilgi bankaıza erişin.

</details>

## <a name="integrate-with-other-services-including-bots"></a>Botlar dahil diğer hizmetlerle tümleştirin

<details>
<summary><b>Soru-Cevap Oluşturma kullanmak için bot Framework 'Ü kullanmam gerekir mi?</b></summary>

**Cevap**: hayır, soru-cevap oluşturma Ile [bot çerçevesini](https://github.com/Microsoft/botbuilder-dotnet) kullanmanıza gerek yoktur. Ancak, Soru-Cevap Oluşturma [Azure bot hizmetinde](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)birkaç şablondan biri olarak sunulur. Bot hizmeti, Microsoft bot Framework aracılığıyla hızlı akıllı bot geliştirmesi sunar ve sunucu-daha az bir ortamda çalışır.

</details>

<details>
<summary><b>Soru-Cevap Oluşturma yeni bir bot nasıl oluşturabilirim?</b></summary>

**Cevap**: Azure bot hizmeti ile bot 'nizi oluşturmak için [Bu](./Quickstarts/create-publish-knowledge-base.md) belgelerde verilen yönergeleri izleyin.

</details>

<details>
<summary><b>Nasıl yaparım? mevcut bir Azure bot hizmeti ile farklı bir Bilgi Bankası mı kullanıyorsunuz?</b></summary>

**Cevap**: bilgi tabanınız hakkında aşağıdaki bilgilere sahip olmanız gerekir:

* Bilgi Bankası KIMLIĞI.
* Bilgi Bankası 'nın yayımlanan bitiş noktası özel alt etki alanı adı, `host` Yayımlama sonrasında **Ayarlar** sayfasında bulunur.
* Bilgi Bankası 'nın yayımlanmış uç nokta anahtarı-yayımladıktan sonra **Ayarlar** sayfasında bulunur.

Bu bilgilerle, Azure portal bot 'ın App Service 'e gidin. **Ayarlar-> yapılandırma-> uygulama ayarları**altında bu değerleri değiştirin.

Bilgi Bankası 'nın uç nokta anahtarı, `QnAAuthkey` ABS hizmetinde etiketlidir.

</details>

<details>
<summary><b>İki veya daha fazla istemci uygulaması Bilgi Bankası 'nı paylaşabilir mi?</b></summary>

**Cevap**: Evet, Bilgi Bankası herhangi bir sayıda istemciden sorgulanabilir. Bilgi bankasından gelen yanıt yavaş veya zaman aşımına uğrar görünüyorsa, Bilgi Bankası ile ilişkili App Service 'in hizmet katmanını yükseltmeyi göz önünde bulundurun.

</details>

<details>
<summary><b>Soru-Cevap Oluşturma Hizmeti web siteme Nasıl yaparım? eklemek mi istiyorsunuz?</b></summary>

**Cevap**: soru-cevap oluşturma hizmetini Web sitenize web sohbeti denetimi olarak eklemek için aşağıdaki adımları izleyin:

1. [Buradaki](./Quickstarts/create-publish-knowledge-base.md)YÖNERGELERI izleyerek SSS bot uygulamanızı oluşturun.
2. [Buradaki](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat) adımları izleyerek Web sohbetini etkinleştirin

</details>

## <a name="data-storage"></a>Veri depolama

<details>
<summary><b>Hangi veriler depolanır ve nerede depolanır?</b></summary>

**Cevap**:

Soru-Cevap Oluşturma hizmetinizi oluşturduğunuzda bir Azure bölgesi seçtiniz. Bilgi tabanlarınız ve günlük dosyalarınız bu bölgede depolanır.

</details>