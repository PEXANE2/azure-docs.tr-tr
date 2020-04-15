---
title: Sık sorulan sorular (SSS) - LUIS
description: Bu makale, Dil Anlama (LUIS) hakkında sık sorulan soruların yanıtlarını içerir.
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: diberry
ms.openlocfilehash: 235eba7f80778b8a60ba880616cf80f2c14ccba1
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382178"
---
# <a name="language-understanding-frequently-asked-questions-faq"></a>Dil Anlama Sık Sorulan Sorular (SSS)

Bu makale, Dil Anlama (LUIS) hakkında sık sorulan soruların yanıtlarını içerir.

## <a name="whats-new"></a>Yenilikler

Dil Anlama (LUIS)'deki yenilikler hakkında [daha fazla bilgi edinin.](whats-new.md)

<a name="luis-authoring"></a>

## <a name="authoring"></a>Yazma

### <a name="what-are-the-luis-best-practices"></a>LUIS en iyi uygulamaları nelerdir?
[Yazma Döngüsü](luis-concept-app-iteration.md)ile başlayın, sonra en [iyi uygulamaları](luis-concept-best-practices.md)okuyun.

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>UYGULAMAMI LUIS'te oluşturmaya başlamanın en iyi yolu nedir?

Uygulamanızı oluşturmanın en iyi yolu [artımlı](luis-concept-app-iteration.md)bir işlemdir.

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>Uygulamamın niyetini modellemek için iyi bir uygulama nedir? Daha spesifik veya daha genel niyetler oluşturmalı mıyım?

Örtüşecek kadar genel olmayan, ancak LUIS'in benzer amaçları ayırt etmesini zorlaştıracak kadar spesifik olmayan niyetleri seçin. Ayrımcı özel niyetler oluşturmak, LUIS modellemesi için en iyi uygulamalardan biridir.

### <a name="is-it-important-to-train-the-none-intent"></a>Yok niyetini eğitmek önemli mi?

Evet, diğer niyetlere daha fazla etiket **eklerken, None** niyetinizi daha fazla sözle eğitmek iyidir. İyi bir oran, bir amaca eklenen her 10 etiket için **Hiçbiri'ne** eklenen 1 veya 2 etikettir. Bu oran LUIS'in ayırıcı gücünü artırır.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Yazım hatalarını söyleyişlerle nasıl düzeltebilirim?

Bing [Yazım Denetimi API V7](luis-tutorial-bing-spellcheck.md) öğreticibakın. LUIS, Bing Spell Check API V7 tarafından uygulanan sınırları uygular.

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>LUIS uygulamamı programlı olarak nasıl düzenli olarak düzenliyorum?
LUIS uygulamanızı programlı olarak güncellemek için [Yazma API'sini](https://go.microsoft.com/fwlink/?linkid=2092087)kullanın. Bkz. [API'yi yazan Call LUIS](./get-started-get-model-rest-apis.md) ve Yazarlık API'sini nasıl arayacağına dair örnekler için [Node.js kullanarak programlı bir ŞEKILDE bir LUIS uygulaması oluşturun.](./luis-tutorial-node-import-utterances-csv.md) Yazma API'si, bitiş noktası anahtarı yerine [yazma anahtarı](luis-concept-keys.md#azure-resources-for-luis) kullanmanızı gerektirir. Programlı yazarlık ayda 1.000.000'a kadar arama ve saniyede beş işlem yapılmasına olanak tanır. LUIS ile kullandığınız tuşlar hakkında daha fazla bilgi için, [bkz.](./luis-concept-keys.md)

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Düzenli ifade eşleştirmesini sağlayan Desen özelliği nerededir?
Önceki **Desen özelliği** şu anda amortismana hazır, **[desenler](luis-concept-patterns.md)** ile değiştirilir.

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Doğru verileri çekmek için bir varlığı nasıl kullanırım?
Bkz. [varlıklar](luis-concept-entity-types.md) ve [veri ayıklama.](luis-concept-data-extraction.md)

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Örnek bir söyleyiş varyasyonları noktalama dahil mi?
Aşağıdaki çözümlerden birini kullanın:
* [Noktalama işaretlerini](luis-reference-application-settings.md#punctuation-normalization) yoksay
* Niyete örnek olarak farklı varyasyonlar ekleyin
* Noktalama işaretlerini [yoksaymak için sözdizimi](luis-concept-patterns.md#pattern-syntax) ile örnek söyleyiş deseni ekleyin.

### <a name="does-luis-currently-support-cortana"></a>LUIS şu anda Cortana'yı destekliyor mu?

Cortana önceden oluşturulmuş uygulamalar 2017 yılında amortismana uğradı. Artık desteklenmisler.

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>LUIS uygulamasının sahipliğini nasıl aktarım?
Bir LUIS uygulamasını farklı bir Azure aboneliğine aktarmak için, LUIS uygulamasını dışa aktarın ve yeni bir hesap kullanarak içe aktarın. Onu çağıran istemci uygulamasındaLUIS uygulama kimliğini güncelleştirin. Yeni uygulama orijinal uygulamadan biraz farklı LUIS puanları döndürebilir.

### <a name="a-prebuilt-entity-is-tagged-in-an-example-utterance-instead-of-my-custom-entity-how-do-i-fix-this"></a>Önceden oluşturulmuş bir varlık, özel varlığım yerine örnek bir sözcükte etiketlenir. Bunu nasıl düzeltebilirim?

LUIS portalında, metni ayıklamak istediğiniz tam varlık için etiketleyebilirsiniz. LUIS portalı doğru varlık tahminini göstermiyorsa, daha fazla sözcük eklemeniz ve varlığı metin içinde etiketlemeniz veya tanımlayıcı (özellik gibi) eklemeniz gerekebilir.

### <a name="i-tried-to-import-an-app-or-version-file-but-i-got-an-error-what-happened"></a>Bir uygulama veya sürüm dosyası almaya çalıştım ama bir hata var, ne oldu?

[Sürüm alma hataları](luis-how-to-manage-versions.md#import-errors)hakkında daha fazla bilgi edinin.

<a name="luis-collaborating"></a>

## <a name="collaborating-and-contributing"></a>İşbirliği ve katkıda bulunmak

### <a name="how-do-i-give-collaborators-access-to-luis-with-azure-active-directory-azure-ad-or-role-based-access-control-rbac"></a>Ortak çalışanlara Azure Etkin Dizin (Azure AD) veya Rol tabanlı erişim denetimi (RBAC) ile LUIS'e nasıl erişim sağlarım?

Ortak çalışanlara nasıl erişim sağlayacağınızı öğrenmek için [Azure Etkin Dizin kaynakları](luis-how-to-collaborate.md#azure-active-directory-resources) ve Azure Etkin [Dizin kiracı sıcaä](luis-how-to-collaborate.md#azure-active-directory-tenant-user) ına bakın.

<a name="luis-endpoint"></a>

## <a name="endpoint"></a>Uç Nokta

### <a name="i-received-an-http-403-error-status-code-how-do-i-fix-it"></a>Bir HTTP 403 hata durum kodu aldım. Nasıl düzeltebilirim?

Fiyatlandırma katmanınız için saniyede yapılan hareketleri veya aylık hareketleri aştığınızda 403 ve 429 hata durum kodları alırsınız. Fiyatlandırma katmanınızı artırın veya Dil Anlama [kapsayıcılarını](luis-container-howto.md)kullanın.

Tüm bu ücretsiz 1000 uç nokta sorgularını kullandığınızda veya fiyatlandırma katmanınızın aylık işlem kotası aşıldığında, bir HTTP 403 hata durum kodu alırsınız.

Bu hatayı gidermek [için, fiyatlandırma katmanınızı](luis-how-to-azure-subscription.md#change-pricing-tier) daha yüksek bir katmana değiştirmeniz veya [yeni bir kaynak oluşturmanız](get-started-portal-deploy-app.md#create-the-endpoint-resource) ve [uygulamanıza atamanız](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal)gerekir.

Bu hatanın çözümleri şunlardır:

* Azure [portalında](https://portal.azure.com), Dil Anlama kaynağınızda, **Kaynak Yönetimi -> Fiyatlandırma katmanında,** fiyatlandırma katmanınızı daha yüksek bir TPS katmanına değiştirin. Kaynağınız Dil Anlama uygulamanıza zaten atanmışsa, Dil Anlama portalında hiçbir şey yapmanız gerekmez.
*  Kullanımınız en yüksek fiyatlandırma katmanını aşarsa, önlerinde yük dengeleyicisi bulunan daha fazla Dil Bilgisi kaynağı ekleyin. Kubernetes veya Docker Compose ile [Dil Anlama konteynerbu](luis-container-howto.md) nda yardımcı olabilir.

### <a name="i-received-an-http-429-error-status-code-how-do-i-fix-it"></a>Bir HTTP 429 hata durum kodu aldım. Nasıl düzeltebilirim?

Fiyatlandırma katmanınız için saniyede yapılan hareketleri veya aylık hareketleri aştığınızda 403 ve 429 hata durum kodları alırsınız. Fiyatlandırma katmanınızı artırın veya Dil Anlama [kapsayıcılarını](luis-container-howto.md)kullanın.

Bu durum kodu, saniyedeki işlemleriniz fiyatlandırma katmanınızı aştığında döndürülür.

Çözümler şunlardır:

* En yüksek katmanda değilseniz [fiyatlandırma katmanınızı artırabilirsiniz.](luis-how-to-azure-subscription.md#change-pricing-tier)
* Kullanımınız en yüksek fiyatlandırma katmanını aşarsa, önlerinde yük dengeleyicisi bulunan daha fazla Dil Bilgisi kaynağı ekleyin. Kubernetes veya Docker Compose ile [Dil Anlama konteynerbu](luis-container-howto.md) nda yardımcı olabilir.
* Bu durum kodunu aldığınızda kendi başınıza uyguladığınız [bir yeniden deneme ilkesiyle](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) istemci uygulama isteklerinizi kapılayabilirsiniz.

### <a name="my-endpoint-query-returned-unexpected-results-what-should-i-do"></a>Bitiş noktası sorgum beklenmeyen sonuçları döndürdü. Ne yapmalıyım?

Beklenmeyen sorgu tahmin sonuçları, yayımlanan modelin durumuna bağlıdır. Modeli düzeltmek için modeli değiştirmeniz, eğitmeniz ve yeniden yayımlamanız gerekebilir.

Modeli düzeltmek aktif [öğrenme](luis-how-to-review-endpoint-utterances.md)ile başlar.

Tüm eğitim verilerini kullanmak için uygulama sürüm [ayarları API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) güncelleyerek deterministik olmayan eğitimi kaldırabilirsiniz.

Diğer ipuçları için [en iyi uygulamaları](luis-concept-best-practices.md) gözden geçirin.

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>LUIS neden sözcüklerin etrafında veya ortasındaki sorguya boşluk ekler?
LUIS [kültüre](luis-language-support.md#tokenization)dayalı söyleyiş [tokenizes](luis-glossary.md#token) . Hem özgün değer hem de belirteç değeri [veri ayıklama](luis-concept-data-extraction.md#tokenized-entity-returned)için kullanılabilir.

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>LUIS uç nokta anahtarını nasıl oluşturabilir ve atayacağım?
[Hizmet](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) düzeyiniz için [Azure'da uç nokta anahtarını oluşturun.](luis-how-to-azure-subscription.md) **[Anahtarı Azure Kaynakları](luis-how-to-azure-subscription.md)** [sayfasındaatay.](luis-how-to-azure-subscription.md) Bu eylem için karşılık gelen BIR API yoktur. Daha sonra [yeni bitiş noktası anahtarını kullanmak](luis-concept-keys.md)için HTTP isteğini bitiş noktasına değiştirmeniz gerekir.

### <a name="how-do-i-interpret-luis-scores"></a>LUIS puanlarını nasıl yorumlayabilirim?
Sisteminiz değeri ne olursa olsun en yüksek puanlama niyetini kullanmalıdır. Örneğin, 0,5'in altındaki bir puan (%50'den az) luis'in kendine güveninin düşük olduğu anlamına gelmez. Daha fazla eğitim verisi sağlamak, en olası amacın [puanını](luis-concept-prediction-score.md) artırmaya yardımcı olabilir.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Uygulamamın Panosunda uç nokta isabetlerimi neden göremiyorum?
Uygulamanızın Panosundaki toplam bitiş noktası isabetleri düzenli aralıklarla güncellenir, ancak Azure portalındaki LUIS uç nokta anahtarınızla ilişkili ölçümler daha sık güncelleştirilir.

Pano'da güncelleştirilmiş uç nokta isabetlerini görmüyorsanız, Azure portalında oturum açın ve LUIS uç nokta anahtarınızla ilişkili kaynağı bulun ve **Toplam Aramalar** metrikünü seçmek için **Ölçümler'i** açın. Bitiş noktası anahtarı birden fazla LUIS uygulaması için kullanılıyorsa, Azure portalındaki metrik, onu kullanan tüm LUIS uygulamalarından gelen toplam arama sayısını gösterir.

### <a name="is-there-a-powershell-command-get-to-the-endpoint-quota"></a>PowerShell komutu bitiş noktası kotasına ulaşmak var mı?

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bitiş noktası kotasını görmek için PowerShell komutunu kullanabilirsiniz:

```powershell
Get-AzCognitiveServicesAccountUsage -ResourceGroupName <your-resource-group> -Name <your-resource-name>
```

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>Benim LUIS uygulaması dün çalışıyordu ama bugün 403 hata alıyorum. Uygulamayı değiştirmedim. Nasıl düzeltebilirim?
Luis uç noktası anahtarı oluşturmak ve uygulamaya atamak için bu [yönergeleri](#how-do-i-create-and-assign-a-luis-endpoint-key) izleyin. Ardından, [yeni bitiş noktası anahtarını kullanmak](luis-concept-keys.md)için istemci uygulamanın HTTP isteğini bitiş noktasına değiştirmeniz gerekir. Farklı bir bölgede yeni bir kaynak oluşturduysanız, HTTP istemci isteğinin bölgesini de değiştirin.

### <a name="how-do-i-secure-my-luis-endpoint"></a>LUIS bitiş noktamı nasıl güvenebilirim?
Bkz. [Bitiş noktasının güvenliğini sağlama](luis-concept-keys.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>LUIS sınırları içinde çalışmak

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>Bir LUIS uygulamasının desteklenebildiği maksimum niyet ve varlık sayısı nedir?
[Sınır](luis-boundaries.md) başvurusuna bakın.

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Maksimum niyet sayısına sahip bir LUIS uygulaması oluşturmak istiyorum. Ne yapmalıyım?

[Bkz. Niyetler için en iyi uygulamalar.](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents)

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>LUIS'te maksimum varlık sayısından fazlasına sahip bir uygulama oluşturmak istiyorum. Ne yapmalıyım?

Varlıklar [için en iyi uygulamalara](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities) bakın

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>İfade listelerinin sayısı ve boyutundaki sınırlar nelerdir?
Bir [tümcecik listesinin](./luis-concept-feature.md)en uzun uzunluğu [için, sınır](luis-boundaries.md) başvurusuna bakın.

### <a name="what-are-the-limits-on-example-utterances"></a>Örnek sözlerle ilgili sınırlar nelerdir?
[Sınır](luis-boundaries.md) başvurusuna bakın.

## <a name="testing-and-training"></a>Test ve eğitim

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Uygulamamdaki bazı modeller için toplu iş testi bölmesinde bazı hatalar görüyorum. Bu sorunu nasıl çözebilirim?

Hatalar, etiketlerinizle modellerinizdeki öngörüler arasında bazı tutarsızlıklar olduğunu gösterir. Sorunu gidermek için aşağıdaki görevlerden birini veya her ikisini yapın:
* LUIS'in niyetler arasındaki ayrımcılığı iyileştirmesine yardımcı olmak için daha fazla etiket ekleyin.
* LUIS'in daha hızlı öğrenmesine yardımcı olmak için, etki alanına özgü kelime dağarcığı tanıtan ifade listesi özellikleri ekleyin.

Toplu [İşlem testi](luis-tutorial-batch-testing.md) öğreticisine bakın.

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Bir uygulama dışa aktarıldığında yeni bir uygulamaya (yeni bir uygulama kimliğiyle) yeniden aktarılır, LUIS tahmin puanları farklıdır. Bu neden oluyor?

Bkz. [Aynı uygulamanın kopyaları arasındaki Tahmin farklılıkları.](luis-concept-prediction-score.md#review-intents-with-similar-scores)

### <a name="some-utterances-go-to-the-wrong-intent-after-i-made-changes-to-my-app-the-issue-seems-to-disappear-at-random-how-do-i-fix-it"></a>Uygulamamda değişiklik yaptıktan sonra bazı söyleyişler yanlış niyete gidiyor. Sorun rastgele yok gibi görünüyor. Nasıl düzeltebilirim?

[Tüm verilerle Tren'e](luis-how-to-train.md#train-with-all-data)bakın.

## <a name="app-publishing"></a>Uygulama yayımlama

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>"Uygulamanıza anahtar ekle" penceresindeki kiracı kimliği nedir?
Azure'da kiracı, bir hizmetle ilişkili istemciyi veya kuruluşu temsil eder. **Azure Etkin Dizin** > **Yönetme** > **Özellikleri'ni**seçerek, **Dizin Kimliği** kutusundaki Azure portalında kiracı kimliğinizi bulun.

![Azure portalında kiracı kimliği](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

<a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>
<a name="why-are-there-more-endpoint-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>


### <a name="why-are-there-more-endpoint-keys-assigned-to-my-app-than-i-assigned"></a>Uygulamama neden benim aparat ettiğimden daha fazla uç nokta anahtarı atanmıştır?
Her LUIS uygulaması, son nokta listesinde kolaylık sağlamak için yazma/başlangıç anahtarına sahiptir. Bu anahtar, LUIS'i deneyebilmeniz için yalnızca birkaç uç nokta isabetine izin verir.

UYGULAMANIZ LUIS genel kullanıma sunulmadan önce (GA) varsa, aboneliğinizdeki LUIS uç nokta anahtarları otomatik olarak atanır. Bu GA geçiş kolaylaştırmak için yapıldı. Azure portalındaki yeni LUIS uç nokta anahtarları otomatik olarak LUIS'e _atanmez._

## <a name="key-management"></a>Anahtar yönetimi

### <a name="how-do-i-know-what-key-i-need-where-i-get-it-and-what-i-do-with-it"></a>Hangi anahtara ihtiyacım olduğunu, nereden aldığımı ve onunla ne yaptığımı nasıl bileceğim?

Yazarlık anahtarı ile tahmin çalışma zamanı anahtarı arasındaki farklar hakkında bilgi edinmek için [LUIS'teki yazma ve sorgu tahmin bitiş noktası](luis-concept-keys.md) anahtarlarına bakın.

### <a name="i-got-an-error-about-being-out-of-quota-how-do-i-fix-it"></a>Kotanın dışında kalma konusunda bir hata maldım. Nasıl düzeltebilirim?

Daha fazla bilgi edinmek için HTTP durum kodu [403](#i-received-an-http-403-error-status-code-how-do-i-fix-it) ve [429'u](#i-received-an-http-429-error-status-code-how-do-i-fix-it) düzeltin.

### <a name="i-need-to-handle-more-endpoint-queries-how-do-i-do-that"></a>Daha fazla uç nokta sorgusu ele almam gerekiyor. Bunu nasıl yapabilirim?

Daha fazla bilgi edinmek için HTTP durum kodu [403](#i-received-an-http-403-error-status-code-how-do-i-fix-it) ve [429'u](#i-received-an-http-429-error-status-code-how-do-i-fix-it) düzeltin.

### <a name="i-created-an-authoring-key-but-it-isnt-showing-in-the-luis-portal-what-happened"></a>Bir yazma anahtarı oluşturdum ama LUIS portalında görünmüyor. Ne oldu?

Yazma tuşları, [yazma anahtar deneyimine geçtikten](luis-migration-authoring.md)sonra LUIS portalında mevcuttur.

## <a name="app-management"></a>Uygulama yönetimi

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Kullanıcı söyleyişlerinin bir günlüğünü nasıl indirebilirim?
Varsayılan olarak, LUIS uygulamanız kullanıcılardan gelen günlükleri kaydeder. Kullanıcıların LUIS uygulamanıza gönderdiği bir günlük sözcük günlüğünü indirmek için **Uygulamalarım'a**gidin ve uygulamayı seçin. Bağlamsal araç çubuğunda, **Uç Nokta Günlüklerini Dışa Aktar'ı**seçin. Günlük virgülle ayrılmış değer (CSV) dosyası olarak biçimlendirilir.

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Söyleyiş lerin günlüğe kaydedilmesini nasıl devre dışı dışı bebilirim?
İstemci uygulamanızın LUIS sorgusunda kullandığı `log=false` Endpoint URL'sini ayarlayarak kullanıcı yla ilgili gönderimlerin günlüğe kaydedilmesini kapatabilirsiniz. Ancak, günlük özelliğini kapatmak, LUIS uygulamanızın söz çıkarma önerebilme veya etkin [öğrenmeye](luis-concept-review-endpoint-utterances.md#what-is-active-learning)dayalı performansı artırma yeteneğini devre dışı bırakarak devre dışı bırakabilirsiniz. Veri gizliliği `log=false` yle ilgili endişeler nedeniyle ayarlarsanız, bu kullanıcı söyleyişlerinin kaydını LUIS'ten indiremez veya uygulamanızı geliştirmek için bu sözlerle kullanamazsınız.

Günlük, söyleyişlerin tek deposudur.

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>Neden tüm uç nokta lı sözlerimin günlüğe kaydolmasını istemiyorum?
Günlükünüzü tahmin analizi için kullanıyorsanız, günlüğünuzdeki test sözcüklerini yakalamayın.

## <a name="data-management"></a>Veri yönetimi

### <a name="can-i-delete-data-from-luis"></a>LUIS'ten gelen verileri silebilir miyim?

* LUIS'i eğitmek için kullanılan örnek açıklamaları her zaman silebilirsiniz. LUIS uygulamanızdan bir örnek açıklama silerseniz, bu açıklama LUIS web hizmetinden kaldırılır ve dışa aktarılmaz.
* Luis'in **Gözden Geçir uç nokta açıklamaları** sayfasında önerdiği kullanıcı açıklamaları listesinden çıkan açıklamaları silebilirsiniz. Bu listeden gelen açıklamalarıssilinmesi, bu açıklamaların önerilmesini engeller, ancak günlüklerden silmez.
* Bir hesabı silerseniz, tüm uygulamalar, örnek açıklamaları ve günlükleriyle birlikte silinir. Veriler, kalıcı olarak silinmeden önce sunucularda 60 gün süreyle saklanır.

### <a name="how-does-microsoft-manage-data-i-send-to-luis"></a>Microsoft, LUIS'e gönderdiğim verileri nasıl yönetir?

[Güven Merkezi,](https://www.microsoft.com/trustcenter) Azure Hizmetleri'nde veri yönetimi ve erişim için taahhütlerimizi ve seçeneklerinizi açıklar.

## <a name="language-and-translation-support"></a>Dil ve çeviri desteği

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Ben bir dilde bir uygulama var ve başka bir dilde paralel bir uygulama oluşturmak istiyorum. Bunu yapmanın en kolay yolu nedir?
1. Uygulamanızı dışa aktarın.
2. Dışa aktarılan uygulamanın JSON dosyasındaki etiketli deyişlerleri hedef dile çevirin.
3. Niyet ve varlıkların adlarını değiştirmeniz veya oldukları gibi bırakmanız gerekebilir.
4. Son olarak, hedef dilde bir LUIS uygulaması na sahip olmak için uygulamayı içeri aktarın.

## <a name="app-notification"></a>Uygulama bildirimi

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Neden kotamın bitmek üzere olduğunu söyleyen bir e-posta aldım?
Yazma/başlangıç anahtarına ayda yalnızca 1000 uç nokta sorgusu na izin verilir. Luis uç noktası anahtarı (ücretsiz veya ücretli) oluşturun ve bitiş noktası sorguları yaparken bu anahtarı kullanın. Bir bot veya başka bir istemci uygulamasından uç nokta sorguları yapıyorsanız, orada LUIS bitiş noktası anahtarını değiştirmeniz gerekir.

## <a name="bots"></a>Bot

### <a name="my-luis-bot-isnt-working-what-do-i-do"></a>Luis botum çalışmıyor. Ne yapmalıyım?

İlk sorun, sorun LUIS ile ilgili veya LUIS ara dışında olur ya da izole etmektir.

#### <a name="resolve-issue-in-luis"></a>LUIS'teki sorunu çözümle
[Luis uç noktasından](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint)LUIS için aynı söyleyiş pass . Bir hata alırsanız, hata artık döndürülene kadar LUIS'teki sorunu çözümle. Sık karşılaşılan hatalar şunlardır:

* `Out of call volume quota. Quota will be replenished in <time>.`- Bu sorun, yazarlık anahtarından [bitiş noktası anahtarına](luis-how-to-azure-subscription.md) değiştirmeniz veya [hizmet katmanlarını](luis-how-to-azure-subscription.md#change-pricing-tier)değiştirmeniz gerektiğini gösterir.

#### <a name="resolve-issue-in-azure-bot-service"></a>Azure Bot Hizmeti'nde sorunu giderme

Azure Bot Hizmetini kullanıyorsanız ve sorun **Web Sohbetindeki** `Sorry, my bot code is having an issue`Test'in iade ettiğiyse günlüklerinizi kontrol edin:

1. Azure portalında, botuniçin, **Bot yönetimi** bölümünden **Yapı'yı**seçin.
1. Çevrimiçi kod düzenleyicisini açın.
1. Üst, mavi navigasyon çubuğunda, bot adını (sağdaki ikinci öğe) seçin.
1. Ortaya çıkan açılır **listede, Kudu Konsolu Aç'ı**seçin.
1. **LogFiles'ı**seçin, ardından **Uygulama'yı**seçin. Tüm günlük dosyalarını gözden geçirin. Uygulama klasöründe hata görmüyorsanız, **LogFiles**altındaki tüm günlük dosyalarını gözden geçirin.
1. C# gibi derlenmiş bir dil kullanıyorsanız projenizi yeniden oluşturmayı unutmayın.

> [!Tip]
> Konsol paketleri de yükleyebilir.

#### <a name="resolve-issue-while-debugging-on-local-machine-with-bot-framework"></a>Bot Framework ile yerel makinede hata ayıklarken sorunu giderin.

Bir botun yerel hata ayıklama hakkında daha fazla bilgi için bir [bot hata ayıklama](https://docs.microsoft.com/azure/bot-service/bot-service-debug-bot?view=azure-bot-service-4.0)bakın.

## <a name="integrating-luis"></a>LUIS'i entegre etme

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>Azure web uygulaması bot abonelik işlemi sırasında LUIS uygulamam nerede oluşturulur?
Bir LUIS şablonu seçerseniz ve şablon bölmesinde **Seç** düğmesini seçerseniz, sol taraftaki bölme şablon türünü içerecek şekilde değişir ve LUIS şablonu oluşturmak için hangi bölgede sorar. Web uygulaması bot işlemi olsa bir LUIS abonelik oluşturmaz.

![LUIS şablonu web uygulaması bot bölgesi](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>Hangi LUIS bölgeleri Bot Framework konuşma priming desteği?
[Konuşma astarı](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) yalnızca merkezi (ABD) örneğindeki LUIS uygulamaları için desteklenir.

## <a name="api-programming-strategies"></a>API Programlama Stratejileri

### <a name="how-do-i-programmatically-get-the-luis-region-of-a-resource"></a>Bir kaynağın LUIS bölgesini programlı olarak nasıl edinebilirim?

C# veya Node.Js kullanarak bölgeyi programlı olarak [bulmak](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/find-region) için LUIS örneğini kullanın.

## <a name="luis-service"></a>LUIS hizmeti

### <a name="is-language-understanding-luis-available-on-premises-or-in-private-cloud"></a>Dil Anlama (LUIS) şirket içinde mi yoksa özel bulutta mı kullanılabilir?

Evet, sayaç kullanımına gerekli bağlantıvarsa, bu senaryolar için LUIS [kapsayıcısını](luis-container-howto.md) kullanabilirsiniz.

## <a name="migrating-to-the-next-version"></a>Bir sonraki sürüme geçiş

### <a name="how-do-i-migrate-to-preview-v3-api"></a>V3 API önizlemesine nasıl geçirebilirim?

[LUIS uygulamaları için API v2 ile v3 Geçiş kılavuzuna](luis-migration-api-v3.md) bakın

## <a name="build-2019-conference-announcements"></a>Build 2019 Konferans duyuruları

Build 2019 Konferansı'nda aşağıdaki özellikler yayınlandı:

* [V3 API geçiş kılavuzuönizlemesi](luis-migration-api-v3.md)
* [Geliştirilmiş analiz panosu](luis-how-to-use-dashboard.md)
* [Geliştirilmiş önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md)
* [Dinamik liste varlıkları](luis-migration-api-v3.md#dynamic-lists-passed-in-at-prediction-time)
* [Dış varlıklar](luis-migration-api-v3.md#external-entities-passed-in-at-prediction-time)

Videolar:

* [İşletmenizi bir sonraki nesil için ölçeklendirmek için Azure Konuşma AI'yi nasıl kullanılır?](https://www.youtube.com/watch?v=_k97jd-csuk&feature=youtu.be)

## <a name="next-steps"></a>Sonraki adımlar

LUIS hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:
* [Yığın Taşma soruları LUIS ile etiketlendi](https://stackoverflow.com/questions/tagged/luis)
* [MSDN Dil Anlayışı Akıllı Hizmetler (LUIS) Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS)
