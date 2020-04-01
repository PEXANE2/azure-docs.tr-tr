---
title: Sorun Giderme - QnA Maker
description: QnA Maker hizmeti ile ilgili en sık sorulan soruların küratörlüğünü, hizmeti daha hızlı ve daha iyi sonuçlarla benimsemenize yardımcı olacaktır.
ms.topic: troubleshooting
ms.date: 03/25/2020
ms.author: diberry
ms.openlocfilehash: 310cfe0cc8c1f647c09b8e0efb435a1ab326ff62
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474949"
---
# <a name="troubleshooting-for-qna-maker"></a>QnA Maker için sorun giderme

QnA Maker hizmeti ile ilgili en sık sorulan soruların küratörlüğünü, hizmeti daha hızlı ve daha iyi sonuçlarla benimsemenize yardımcı olacaktır.

<a name="how-to-get-the-qnamaker-service-hostname"></a>

## <a name="manage-predictions"></a>Tahminleri yönetme

<details>
<summary><b>Sorgu tahminleri için iş geleni performansını nasıl artırabilirim?</b></summary>

**Yanıt**: İş-iş performansı sorunları, hem Uygulama hizmetiniz hem de Bilişsel Aramanız için ölçeklendirmeniz gerektiğini gösterir. Performansı artırmak için Bilişsel Aramanıza bir yineleme eklemeyi düşünün.

[Fiyatlandırma katmanları](Concepts/azure-resources.md)hakkında daha fazla bilgi edinin.
</details>

<details>
<summary><b>QnAMaker hizmet bitiş noktası nasıl elde e</b></summary>

**Cevap**: QnAMaker hizmet bitiş noktası, QnAMaker Desteği veya UserVoice ile iletişim kurduğunuzda hata ayıklama amacıyla yararlıdır. Bitiş noktası bu biçimde bir `https://your-resource-name.azurewebsites.net`URL'dir: .

1. [Azure portalındaki](https://portal.azure.com) QnAMaker hizmetinize (kaynak grubu) gidin

    ![Azure portalında QnAMaker Azure kaynak grubu](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. QnA Maker kaynağıyla ilişkili Uygulama Hizmeti'ni seçin. Genellikle, adlar aynıdır.

     ![QnAMaker Uygulama Hizmetini Seçin](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Bitiş noktası URL'si Genel Bakış bölümünde mevcuttur

    ![QnAMaker uç noktası](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)

</details>

## <a name="manage-the-knowledge-base"></a>Bilgi tabanını yönetin

<details>
<summary><b>QnA Maker'ımın bir parçasını yanlışlıkla sildim, ne yapmalıyım?</b></summary>

**Yanıt**: Arama veya Web Uygulaması gibi QnA Maker kaynağıyla birlikte oluşturulan Azure hizmetlerinin hiçbirini silmeyin. Bunlar QnA Maker için gereklidir, eğer birini silerseniz, QnA Maker doğru çalışmayı durdurur.

Soru ve yanıt çiftleri, dosyalar, URL'ler, özel sorular ve yanıtlar, bilgi tabanları veya Azure kaynakları dahil olmak üzere tüm silmeler kalıcıdır. Bilgi tabanınızın herhangi bir bölümünü silmeden önce bilgi tabanınızı **Ayarlar** sayfasından dışa aktardığınızdan emin olun.

</details>

<details>
<summary><b>Neden URL(ler)/dosya(lar) soru-cevap çiftlerini ayıklamıyor?</b></summary>

**Cevap**: QnA Maker'ın geçerli SSS URL'lerinden bazı soru-cevap (QnA) içeriğini otomatik olarak ayıklayamamış olması mümkündür. Bu gibi durumlarda, QnA içeriğini bir .txt dosyasına yapıştırabilir ve aracın onu yutup yutup yutamadığını görebilirsiniz. Alternatif olarak, [QnA Maker portalı](https://qnamaker.ai)aracılığıyla bilgi tabanınıza editoryal içerik ekleyebilirsiniz.

</details>

<details>
<summary><b>Ne kadar büyük bir bilgi tabanı oluşturabilirim?</b></summary>

**Yanıt**: Bilgi tabanının boyutu, QnA Maker hizmetini oluştururken seçtiğiniz Azure aramasının SKU'suna bağlıdır. Daha fazla bilgi için [burayı](./Tutorials/choosing-capacity-qnamaker-deployment.md) okuyun.

</details>

<details>
<summary><b>Yeni bir bilgi tabanı oluşturmaya çalıştığımda neden açılır da bir şey göremiyorum?</b></summary>

**Yanıt**: Azure'da henüz herhangi bir QnA Maker hizmeti oluşturmadın. Bunu nasıl yapacağınızı öğrenmek için [burayı](./How-To/set-up-qnamaker-service-azure.md) okuyun.

</details>

<details>
<summary><b>Bir bilgi tabanını başkalarıyla nasıl paylaşırım?</b></summary>

**Cevap**: QnA Maker hizmeti düzeyinde paylaşım çalışmaları, yani hizmetteki tüm bilgi üsleri paylaşılacaktır. [Burada](./How-To/collaborate-knowledge-base.md) nasıl bir bilgi tabanında işbirliği okuyun.

</details>

<details>
<summary><b>Bir bilgi tabanını değiştirmek için aynı AAD kiracı olmayan bir katılımcı ile bir bilgi tabanı paylaşabilir miyim?</b></summary>

**Yanıt**: Paylaşım, Azure rol tabanlı erişim denetimine (RBAC) dayanır. Azure'daki _herhangi bir_ kaynağı başka bir kullanıcıyla paylaşabiliyorsanız, QnA Maker'ı da paylaşabilirsiniz.

</details>

<details>
<summary><b>5 QnAMaker bilgi tabanına sahip bir Uygulama Hizmet Planınız varsa. Her biri sadece 1 QnAMaker bilgi tabanına erişebilsin diye 5 farklı kullanıcıya okuma/yazma hakları atayabilir misiniz?</b></summary>

**Cevap**: Bireysel bilgi tabanlarını değil, tüm QnAMaker hizmetini paylaşabilirsiniz.

</details>

<details>
<summary><b>İyi eşleşme bulunamadında varsayılan iletiyi nasıl değiştirebilirim?</b></summary>

**Yanıt**: Varsayılan ileti, Uygulama hizmetinizdeki ayarların bir parçasıdır.
- Azure portalındaki Uygulama hizmeti kaynağınıza gidin

![qnamaker appservice](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- **Ayarlar** seçeneğine tıklayın

![qnamaker uygulama ayarları](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- **Varsayılan Yanıt** ayarının değerini değiştirme
- Uygulama hizmetinizi yeniden başlatın

![qnamaker appservice yeniden](./media/qnamaker-faq/qnamaker-appservice-restart.png)


</details>

<details>
<summary><b>SharePoint bağlantım neden çıkarılamıyor?</b></summary>

**Cevap**: Daha fazla bilgi için [Veri kaynağı konumları](./Concepts/knowledge-base.md#data-source-locations) bakın.

</details>

<details>
<summary><b>Bilgi tabanıma yaptığım güncelleştirmeler yayımlamaya yansıtılmıyor. Neden olmasın?</b></summary>

**Cevap**: Tablo güncelleştirmesinde, test edilsin veya ayaryapken her bir edit işleminin yayımlanabilmesi için kaydedilmesi gerekir. Her bir düzenlenen işlemden sonra **Kaydet ve train** düğmesini tıklattığınızdan emin olun.

</details>

<details>
<summary><b>Bilgi bankası zengin verileri veya multimedyayı destekliyor mu?</b></summary>

**Cevap**:

#### <a name="multimedia-auto-extraction-for-files-and-urls"></a>Dosyalar ve URL'ler için multimedya otomatik çıkarma

* URL'ler - sınırlı HTML'den Markdown'a dönüştürme özelliği.
* Dosyalar - desteklenmiyor

#### <a name="answer-text-in-markdown"></a>İşaretlemetin yanıtla
QnA kümeleri bilgi tabanında olduğunda, genel URL'lerden kullanılabilen ortambağlantıları içerecek şekilde bir yanıtın işaretleme metnini edinebilirsiniz.


</details>

<details>
<summary><b>QnA Maker İngilizce olmayan dilleri destekliyor mu?</b></summary>

**Cevap**: Desteklenen [diller](./Overview/languages-supported.md)hakkında daha fazla bilgi için bkz.

Birden çok dilden içeriğiniz varsa, her dil için ayrı bir hizmet oluşturduğunuzdan emin olun.

</details>

## <a name="manage-service"></a>Hizmeti yönetme

<details>
<summary><b>Uygulama hizmetimi ne zaman yeniden başlatmalıyım?</b></summary>

**Cevap**: **Kullanıcı Ayarları** [sayfasındaki](https://www.qnamaker.ai/UserSettings) **Bitiş Noktası tuşları** tablosundaki bilgi tabanının sürüm değerinin yanında dikkat simgesi olduğunda uygulama hizmetinizi yenileyin.

</details>

<details>
<summary><b>Varolan Arama hizmetimi sildim. Bunu nasıl düzeltebilirim?</b></summary>

**Yanıt**: Bir Azure Bilişsel Arama dizinini silerseniz, işlem son işlemdir ve dizin kurtarılamaz.

</details>

<details>
<summary><b>Arama hizmetimde dizinimi `testkb` sildim. Bunu nasıl düzeltebilirim?</b></summary>

**Cevap**: Eski verileriniz kurtarılamaz. Yeni bir QnA Maker kaynağı oluşturun ve bilgi tabanınızı yeniden oluşturun.

</details>

<details>
<summary><b>Son nokta anahtarlarımı ne zaman yenilemem gerekiyor?</b></summary>

**Cevap**: Son nokta anahtarlarınızı, açığa alındıklarına şüpheleniyorsanız yenileyin.

</details>

<details>
<summary><b>Birden çok dil kullanarak bilgi tabanları için aynı Azure Bilişsel Arama kaynağını kullanabilir miyim?</b></summary>

**Cevap**: Birden çok dil ve birden çok bilgi tabanı nı kullanmak için, kullanıcının her dil için bir QnA Maker kaynağı oluşturması zorundadır. Bu, dil başına ayrı bir Azure arama hizmeti oluşturur. Tek bir Azure arama hizmetinde farklı dil bilgisi tabanlarının karıştırılması, sonuçların alaka düzeyinin düşmesine neden olur.

</details>

<details>
<summary><b>QnA Maker tarafından kullanılan Azure Bilişsel Arama kaynağının adını nasıl değiştirebilirim?</b></summary>

**Cevap**: Azure Bilişsel Arama kaynağının adı, sonunda bazı rasgele harflerekilen QnA Maker kaynak adıdır. Bu, QnA Maker için birden çok Arama kaynağı ayırt etmeyi zorlaştırır. Ayrı bir arama hizmeti oluşturun (istediğiniz şekilde adlandırın) ve QnA Hizmetinize bağlayın. Adımlar, [Azure aramasını yükseltmek](How-To/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service)için yapmanız gereken adımlara benzer.

</details>

<details>
<summary><b>QnA Maker `Runtime core is not initialized,` döndüğünde nasıl düzeltebilirim?</b></summary>

**Cevap**: Uygulama hizmetinizin disk alanı dolu olabilir. Disk alanınızı düzeltme adımları:

1. Azure [portalında](https://portal.azure.com)QnA Maker'ın Uygulama hizmetini seçin ve ardından hizmeti durdurun.
1. Uygulama hizmeti nde yken, **Geliştirme Araçları'nı**seçin, sonra **Gelişmiş Araçlar,** sonra **Git**. Bu yeni bir tarayıcı penceresi açar.
1. Komut satırı aracını açmak için **Hata Ayıklama konsolunu,** ardından **CMD'yi** seçin.
1. _Siteye/wwwroot/Data/QnAMaker/_ dizinine gidin.
1. Adı `rd`ile başlayan tüm klasörleri kaldırın

    Aşağıdakileri **silmeyin:**

    * KbIdToRankerMappings.txt dosyası
    * EndpointSettings.json dosyası
    * EndpointKeys klasörü

1. Uygulama hizmetini başlatın.
1. Şimdi çalıştığını doğrulamak için bilgi tabanınıza erişin.

</details>

## <a name="integrate-with-other-services-including-bots"></a>Botlar da dahil olmak üzere diğer hizmetlerle tümleştirme

<details>
<summary><b>QnA Maker'ı kullanmak için Bot Framework'u kullanmam gerekiyor mu?</b></summary>

**Cevap**: Hayır, QnA Maker ile [Bot Framework](https://github.com/Microsoft/botbuilder-dotnet) kullanmanız gerekmez. Ancak, QnA Maker [Azure Bot Hizmeti'ndeki](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)birkaç şablondan biri olarak sunulur. Bot Hizmeti, Microsoft Bot Framework aracılığıyla hızlı akıllı bot geliştirme sağlar ve sunucusuz bir ortamda çalışır.

</details>

<details>
<summary><b>QnA Maker ile nasıl yeni bir bot oluşturabilirim?</b></summary>

**Cevap**: Azure Bot Hizmeti ile Botunuzu oluşturmak için [bu](./Quickstarts/create-publish-knowledge-base.md) belgelerdeki yönergeleri izleyin.

</details>

<details>
<summary><b>Varolan bir Azure bot hizmetiyle farklı bir bilgi tabanını nasıl kullanırım?</b></summary>

**Cevap**: Bilgi tabanınız hakkında aşağıdaki bilgilere sahip olmanız gerekir:

* Bilgi bankası kimliği.
* Bilgi bankasının yayımlandıktan sonra **Ayarlar** sayfasında `host`bulunan , " olarak bilinen endpoint özel alt etki alanı adı.
* Bilgi tabanının yayınlanan uç nokta anahtarı - yayımladıktan sonra **Ayarlar** sayfasında bulunur.

Bu bilgilerle, Azure portalındaki botunuzun uygulama hizmetine gidin. **Ayarlar -> Yapılandırma -> Uygulama ayarları**altında, bu değerleri değiştirin.

Bilgi tabanının uç nokta anahtarı `QnAAuthkey` ABS hizmetinde etiketlenir.

</details>

<details>
<summary><b>İki veya daha fazla istemci uygulaması bir bilgi tabanını paylaşabilir mi?</b></summary>

**Cevap**: Evet, bilgi bankası herhangi bir sayıda istemciden sorgulanabilir. Bilgi tabanından gelen yanıt yavaş veya zaman dışında görünüyorsa, bilgi tabanıyla ilişkili uygulama hizmeti için hizmet katmanını yükseltmeyi düşünün.

</details>

<details>
<summary><b>QnA Maker hizmetini web siteme nasıl yerleştirebilirim?</b></summary>

**Cevap**: QnA Maker hizmetini web sitenize web sohbet kontrolü olarak yerleştirmek için aşağıdaki adımları izleyin:

1. [Burada](./Quickstarts/create-publish-knowledge-base.md)yönergeleri izleyerek SSS bot oluşturun.
2. [Buradaki](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat) adımları izleyerek web sohbetini etkinleştirin

</details>

## <a name="data-storage"></a>Veri depolama

<details>
<summary><b>Hangi veriler depolanır ve nerede depolanır?</b></summary>

**Cevap**:

QnA Maker hizmetinizi oluştururken, bir Azure bölgesi seçtiniz. Bilgi üsleriniz ve günlük dosyalarınız bu bölgede depolanır.

</details>