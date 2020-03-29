---
title: Açıklama Tasarım Desenleri
titleSuffix: Azure Cognitive Services
description: Tasarım desenleri ve ifşaat için en iyi uygulamalar.
services: cognitive-services
author: sharonlo101
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: angle
ms.openlocfilehash: 3e7d8ee2b156a30b11cda79798a8af8a8ecf4f64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74776631"
---
# <a name="disclosure-design-patterns"></a>Tasarım desenlerini açıklama
Artık sentetik ses deneyiminiz için doğru [açıklama düzeyini](concepts-disclosure-guidelines.md#disclosure-assessment) belirlediğinize&#39;, potansiyel tasarım modellerini keşfetmek için iyi bir zaman&#39;.
## <a name="overview"></a>Genel Bakış
Sentetik ses deneyiminize uygulayabileceğiniz bir tasarım örüntüsü spektrumu vardır. Açıklama değerlendirmenizin sonucu 'Yüksek İfşaat' ise, sentetik sesin kökenini açıkça iletmek anlamına gelen [**açık açıklama**](#explicit-disclosure)yapmanızı öneririz. [**Örtük açıklama,**](#implicit-disclosure) gerekli açıklama düzeylerinin yüksek veya düşük olup olmadığı konusunda ses deneyimlerine yarar sağlayan ipuçları nı ve etkileşim kalıplarını içerir.
![Açıklama desenleri spektrumu](media/responsible-ai/disclosure-patterns/affordances.png)






| Açık açıklama desenleri                                                                                                                                                                                    | Örtük açıklama kalıpları                                                                 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
|[Şeffaf Giriş](#transparent-introduction)<br> [Sözel Şeffaf Giriş](#verbal-transparent-introduction)<br>  [Açık Byline](#explicit-byline)<br>  [Özelleştirme ve Kalibrasyon](#customization-and-calibration)<br> [Ebeveyn İfşası](#parental-disclosure)<br> [Sesin nasıl üretiliş edildiği hakkında daha fazla bilgi edinmek için fırsatlar sunmak](#providing-opportunities-to-learn-more-about-how-the-voice-was-made) | [Yetenek Açıklama](#capability-disclosure)<br>[Örtük İşaretler ve Geri Bildirim](#implicit-cues--feedback)<br> [Konuşma Şeffaflığı](#conversational-transparency) |



Sentetik sesinize doğrudan uygulanan desenlere başvurmak için aşağıdaki grafiği kullanın. Bu grafikteki diğer koşullardan bazıları senaryonuz için de geçerli olabilir:<br/>



| Sentetik ses deneyiminiz ise... | Öneriler | Tasarım desenleri |
| --- | --- | --- |
| Yüksek İfşa Gerektirir  | Kullanıcıların ilişki kurmalarına yardımcı olmak için en az bir açık desen ve örtülü ipuçlarını önceden kullanın. |[Açık Açıklama](#explicit-disclosure)<br>[Örtük Açıklama](#implicit-disclosure)  |
| Düşük Açıklama Gerektirir | Açıklama en az veya gereksiz olabilir, ancak bazı örtük desenler yararlanabilir. | [Yetenek Açıklama](#capability-disclosure)<br>[Konuşma Şeffaflığı](#conversational-transparency)  |
| Yüksek düzeyde bir katılıma sahiptir | Uzun vadede oluşturun ve kullanıcı yolculuğu boyunca ifşaat için birden fazla giriş noktası sunun. Bu son derece bir onboarding deneyimi olması tavsiye edilir. | [Şeffaf Giriş](#transparent-introduction)<br>[Özelleştirme ve Kalibrasyon](#customization-and-calibration)<br>[Yetenek Açıklama](#capability-disclosure) |
| Birincil hedef kitle olarak çocukları içerir | Ebeveynleri birincil açıklama kitlesi olarak hedefle ve çocuklara ifşaatı etkili bir şekilde iletebilmelerine dikkat edin.  | [Ebeveyn İfşası](#parental-disclosure)<br>[Sözel Şeffaf Giriş](#verbal-transparent-introduction)<br> [Örtük Açıklama](#implicit-disclosure)<br> [Konuşma Şeffaflığı](#conversational-transparency)  |
| Görme engelli kullanıcıları veya görme yetisi düşük kişileri birincil hedef kitle olarak içerir  | Tüm kullanıcıları dahil edin ve herhangi bir görsel açıklama biçiminin alternatif metin veya ses efektleri ile ilişkili olduğundan emin olun. Kontrast oranı ve görüntü boyutu için erişilebilirlik standartlarına uyun. İfşaatı iletmek için işitsel ipuçlarını kullanın.  | [Sözel Şeffaf Giriş](#verbal-transparent-introduction) <br>[İşitsel İpuçları](#implicit-cues--feedback)<br>[Haptic İpuçları](#implicit-cues--feedback)<br>[Konuşma Şeffaflığı](#conversational-transparency)<br>[Erişilebilirlik Standartları](https://www.microsoft.com/accessibility) |
| Ekransız, aygıtsız mı dır veya birincil veya tek etkileşim modu olarak sesi kullanır | İfşaatı iletmek için işitsel ipuçlarını kullanın. | [Sözel Şeffaf Giriş](#verbal-transparent-introduction) <br> [İşitsel İpuçları](#implicit-cues--feedback)  |
| Potansiyel olarak birden fazla kullanıcı/dinleyici içerir (örn. birden fazla evde kişisel asistan)  | Çeşitli kullanıcı bağlamlarına ve anlayış düzeylerine dikkat edin ve kullanıcı yolculuğunda ifşa etmek için birden fazla fırsat sunun.  | [Şeffaf Giriş (Return User)](#transparent-introduction)<br> [Sesin nasıl üretiliş edildiği hakkında daha fazla bilgi edinmek için fırsatlar sunmak](#providing-opportunities-to-learn-more-about-how-the-voice-was-made)<br> [Konuşma Şeffaflığı](#conversational-transparency)  |



## <a name="explicit-disclosure"></a>Açık açıklama
Sentetik ses deneyiminiz yüksek ifşaat gerektiriyorsa, sentetik niteliği açıkça belirtmek için aşağıdaki açık desenlerden en az birini kullanmak en iyisidir.
### <a name="transparent-introduction"></a>Şeffaf Giriş

Ses deneyimi başlamadan önce, sesinin kökeni ve yetenekleri hakkında tamamen şeffaf olarak dijital asistanı tanıtın. Bu deseni kullanmak için en uygun an, yeni bir kullanıcıya binerken veya geri dönen bir kullanıcıya yeni özellikler sunarken. Bir giriş sırasında örtük ipuçları nın uygulanması, kullanıcıların dijital ajanın sentetik doğası hakkında zihinsel bir model oluşturmalarına yardımcı olur.

#### <a name="first-time-user-experience"></a>İlk kez kullanıcı deneyimi

![İlk çalıştırma deneyimi sırasında saydam giriş](media/responsible-ai/disclosure-patterns/transparent-intro-first.png) <br>
*Sentetik ses yeni bir kullanıcı onboarding sırasında tanıtıldı.*

Öneriler
- Sesin yapay olduğunu açıklayın (örn. &quot;dijital)&quot;
- Aracının neler yapabileceğini açıklayın
- Ses&#39;kökenlerini açıkça belirtin
- Sentetik ses hakkında daha fazla bilgi edinmek için bir giriş noktası sunun

#### <a name="returning-user-experience"></a>Dönen kullanıcı deneyimi

Bir kullanıcı biniş deneyimini atlarsa, kullanıcı sesi ilk kez tetikleyene kadar Saydam Giriş deneyimine giriş noktaları sunmaya devam edin.
<br/>

![İade kullanıcı deneyimi sırasında saydam giriş](media/responsible-ai/disclosure-patterns/transparent-intro-return.png)<br/>
*Sentetik ses deneyimine tutarlı bir giriş noktası sağlayın. Kullanıcı yolculuğunun herhangi bir noktasında sesi ilk kez tetiklediğinde kullanıcının biniş deneyimine geri dönmesine izin verin.*


### <a name="verbal-transparent-introduction"></a>Sözel Şeffaf Giriş

Dijital asistanın&#39;sesinin kökenini belirten bir sözlü istem, kendi başına ifşaata ulaşmak için yeterince açık. Bu desen, sesin tek etkileşim biçimi olduğu yüksek açıklama senaryoları için en iyisidir.
<br/>

![Sözel olarak konuşulan şeffaf giriş](media/responsible-ai/disclosure-patterns/spoken-prompt-1.png)
<br/>*Kullanıcı deneyiminde bir kişiyi&#39;sesini zaten tanıtabileceğiniz veya atfedebileceğiniz anlar olduğunda saydam bir giriş kullanın.*


![Birinci şahısta sözlü olarak konuşulan şeffaf giriş](media/responsible-ai/disclosure-patterns/spoken-prompt-2.png)<br/>
*Daha fazla şeffaflık için, ses aktörü sentetik sesin kökenini ilk kişide açıklayabilir.*

### <a name="explicit-byline"></a>Açık Byline

Kullanıcı sesi tetiklemek için bir ses oynatıcı veya etkileşimli bileşenle etkileşimde bulunacaksa bu deseni kullanın.


![Haber medyası senaryosunda açık satır ara](media/responsible-ai/disclosure-patterns/explicit-byline.png) <br/>
*Açık bir satır, sesin nereden geldiğinin atfedildiğidir.*

Öneriler

- Sentezlenen ses hakkında daha fazla bilgi edinmek için giriş noktası teklif edin

### <a name="customization-and-calibration"></a>Özelleştirme ve Kalibrasyon

Kullanıcılara dijital asistanın bunlara nasıl yanıt vereceği (örneğin, sesin nasıl geldiği) üzerinde denetim sağlayın.  Bir kullanıcı bir sistemle kendi şartlarında ve belirli hedefleri göz önünde bulundurarak etkileşime geçtiğinde, tanım gereği bunun gerçek bir kişi olmadığını&#39;zaten anlamış olur.

#### <a name="user-control"></a>Kullanıcı Denetimi

Sentetik ses deneyimi üzerinde anlamlı ve belirgin bir etkisi olan seçenekler sunun.

![Kullanıcı tercihleri](media/responsible-ai/disclosure-patterns/customization-user-control.png)<br/>
*Kullanıcı tercihleri, kullanıcıların deneyimlerini özelleştirmelerine ve geliştirmelerine olanak tanır.*

Öneriler

- Kullanıcıların sesi özelleştirmesine izin verin (örn. dil ve ses türünü seçin)
- Kullanıcılara, sisteme benzersiz sesine yanıt vermelerini (örn. ses kalibrasyonu, özel komutlar) öğretecek bir yol sağlayın
- Kullanıcı tarafından oluşturulan veya bağlamsal etkileşimler için optimize edin (örn. anımsatıcılar)

#### <a name="persona-customization"></a>Persona Özelleştirme

Dijital asistanın sesini&#39;özelleştirmenin yollarını sunun. Ses bir ünlüye veya yaygın olarak tanınabilir bir kişiye dayanıyorsa, kullanıcılar sesi önizlediğinde hem görsel hem de sözlü tanıtımları kullanmayı düşünün.

![Ses özelleştirme](media/responsible-ai/disclosure-patterns/customization-voice-type.png)<br/>
*Bir dizi ses arasından seçim yapma olanağı sunmak yapay doğayı aktarmaya yardımcı olur.*

Öneriler
- Kullanıcıların her sesin sesini önizlemesine izin verme
- Her ses için özgün bir giriş kullanın
- Sentezlenen ses hakkında daha fazla bilgi edinmek için giriş noktaları sunun

### <a name="parental-disclosure"></a>Ebeveyn İfşası

COPPA yönetmeliklerine uymanın yanı sıra, birincil hedef kitleniz küçük çocuklarsa ve maruz kalma seviyeniz yüksekse ebeveynlere ifşa atalım. Hassas kullanımlar için, bir yetişkin sentetik sesin kullanımını kabul edene kadar deneyimi gating düşünün. Ebeveynleri, bu mesajı çocuklarına iletmeye teşvik edin.

![Ebeveynler için açıklama](media/responsible-ai/disclosure-patterns/parental-disclosure.png)<br/>
*Ebeveynler için optimize edilmiş şeffaf bir giriş, bir yetişkinin, bir çocuk onunla etkileşime girmeden önce sesin sentetik doğasından haberdar edilmesini sağlar.*

Öneriler

- Açıklama için birincil hedef kitle olarak hedef ebeveynler
- Ebeveynleri çocuklarına ifşaat iletmeye teşvik edin
- Sentezlenen ses hakkında daha fazla bilgi edinmek için giriş noktaları sunun
- Velilere ifşaatı okuduğunu &quot;göstermek&quot; için basit bir koruma sorusu sorarak deneyimi kapılayın

### <a name="providing-opportunities-to-learn-more-about-how-the-voice-was-made"></a>Sesin nasıl üretiliş edildiği hakkında daha fazla bilgi edinmek için fırsatlar sunmak

Sentetik ses teknolojisi hakkında daha fazla bilgi sağlayan bir sayfaya, açılır pencereye veya harici siteye içeriğe duyarlı giriş noktaları sunun. Örneğin, biniş sırasında veya kullanıcı konuşma sırasında daha fazla bilgi istediğinde daha fazla bilgi edinmek için bir bağlantıyı yüzebilirsiniz.

![Daha fazla bilgi için giriş noktası](media/responsible-ai/disclosure-patterns/learn-more-entry-point.png)<br/>
*Sentezlenen ses hakkında daha fazla bilgi edinme fırsatı sunan bir giriş noktası örneği.*

Bir kullanıcı sentetik ses hakkında daha fazla bilgi istediğinde, birincil amaç onları sentetik sesin kökeni hakkında eğitmek ve teknoloji hakkında şeffaf olmaktır.

![Kullanıcılara sentetik ses hakkında daha fazla bilgi sağlayın](media/responsible-ai/disclosure-patterns/learn-more.png)<br/>
*Daha fazla bilgi harici bir site yardım sitesinde sunulabilir.*

Öneriler

- Karmaşık kavramları basitleştirin ve yasal ve teknik jargon kullanmaktan kaçının
- Bu içeriği gizlilik ve kullanım koşulları bildirimlerinde gömmeyin
- Kullanılabilir olduğunda içeriği kısa tutun ve görüntüleri kullanın

## <a name="implicit-disclosure"></a>Örtük açıklama

Tutarlılık, kullanıcı yolculuğu boyunca dolaylı olarak ifşaat elde etmek için anahtardır. Aygıtlar ve etkileşim modları arasında görsel ve işitsel ipuçlarının tutarlı kullanımı, örtük desenler ve açık açıklama arasında ilişki kurulmasına yardımcı olabilir.

![Örtük ipuçlarının tutarlılığı](media/responsible-ai/disclosure-patterns/consistency.png)

### <a name="implicit-cues--feedback"></a>Örtük İpuçları & Geribildirim

Antropomorfizm, ajanın gerçek görsel temsilinden sese, seslere, ışık kalıplarına, sıçrayan şekillere ve hatta bir cihazın titreşimine kadar farklı şekillerde tezahür edebilir. Kişiliğinizi tanımlarken, insana benzer bir avatarı hedeflemek yerine örtük ipuçlarından ve geri bildirim kalıplarından yararlanın. Bu, daha açık bir açıklama gereksinimini en aza indirmenin bir yoludur.

![Görsel ipuçları ve geri bildirim](media/responsible-ai/disclosure-patterns/visual-affordances.png)<br/>
*Bu ipuçları çok insan gibi olmadan ajan antropomorfize yardımcı olur. Ayrıca zaman içinde sürekli kullanıldığında kendi başlarına etkili ifşa mekanizmaları haline gelebilirler.*

Aşağıdaki ipuçlarını bir araya getirirken deneyiminizin farklı etkileşim modlarını göz önünde bulundurun:

| Görsel İpuçları                                                                                                                                                               | İşitsel İpuçları                                                      | Haptic İpuçları |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|-------------|
|  Avatar <br>Yanıt veren gerçek zamanlı ipuçları (örn. animasyonlar)<br> Ekran dışı ipuçları (örn. bir cihazdaki ışıklar ve desenler)<br>  | Sonicon (örn. kısa bir farklı ses, müzik notaları serisi) | Titreşim   |

### <a name="capability-disclosure"></a>Yetenek Açıklama

Dijital asistanın neler yapabileceğine ilişkin doğru beklentiler ayarlayarak dolaylı olarak ifşaat elde edilebilir. Kullanıcıların dijital asistanla nasıl etkileşimde bulunacaklarını öğrenebilmeleri ve deneyimin erken aşamalarında sentetik ses hakkında daha fazla bilgi edinmek için bağlamsal yardım sunabilmeleri için örnek komutlar sağlayın.

![Görsel ipuçları ve geri bildirim](media/responsible-ai/disclosure-patterns/capability-disclosure.png)<br/>

### <a name="conversational-transparency"></a>Konuşma Şeffaflığı

Konuşmalar beklenmeyen yollara düştüğünde, beklentileri sıfırlamaya, saydamlığı güçlendirmeye ve kullanıcıları başarılı yollara yönlendirmeye yardımcı olabilecek varsayılan yanıtlar oluşturmayı düşünün. Konuşmada açık açıklamayı kullanmak için de fırsatlar vardır.

![Beklenmeyen yolları işleme](media/responsible-ai/disclosure-patterns/conversational-transparency-1.png)<br/>

<br/>
Görev dışı &quot;veya&quot; aracıya yöneltilen kişisel sorular, kullanıcılara aracının sentetik doğasını hatırlatmak ve onları uygun şekilde etkileşime sokmaya veya gerçek bir kişiye yönlendirmeye yönlendirmek için iyi bir zamandır.

![Görev sorularını işleme](media/responsible-ai/disclosure-patterns/conversational-transparency-2.png)<br/>

## <a name="when-to-disclose"></a>Ne zaman ifşa edilsin

Kullanıcı yolculuğu boyunca ifşa ataak için birçok fırsat vardır. İlk kullanım için tasarım, ikinci kullanım, nth kullanımı &quot;..., ama aynı zamanda şeffaflık vurgulamak için başarısızlık&quot; anları kucaklamak-sistem bir hata yaptığında veya kullanıcı aracı&#39;yetenekleri bir sınırlama keşfettiğinde gibi.

![Kullanıcı yolculuğu boyunca açıklama fırsatları](media/responsible-ai/disclosure-patterns/touchpoints.png)<br/>

Çeşitli açıklama fırsatlarını vurgulayan standart bir dijital asistan kullanıcı yolculuğu örneği.

### <a name="up-front"></a>Ön

İfşaat için en uygun an, bir kişinin sentetik sesle ilk etkileşimidir.Kişisel bir sesli asistan senaryosunda, bu, biniş sırasında veya kullanıcının deneyimi ilk kez neredeyse unbox'ları olarak ortaya çıkan bir senaryo olacaktır. Diğer senaryolarda, sentetik bir sesin bir web sitesindeki içeriği ilk okuması veya bir kullanıcının sanal bir karakterle etkileşimde ilk etkileşimi olabilir.

- [Şeffaf Giriş](#transparent-introduction)
- [Yetenek Açıklama](#capability-disclosure)
- [Özelleştirme ve Kalibrasyon](#customization-and-calibration)
- [Örtük İpuçları](#implicit-cues--feedback)

### <a name="upon-request"></a>İstek üzerine

Kullanıcılar, istendiğinde kullanıcı yolculuğu sırasında herhangi bir noktada ek bilgilere kolayca erişebilmeli, tercihleri kontrol edebilmeli ve saydam iletişim alabilmelidir.

- [Sesin nasıl üretiliş edildiği hakkında daha fazla bilgi edinmek için fırsatlar sunmak](#providing-opportunities-to-learn-more-about-how-the-voice-was-made)
- [Özelleştirme ve Kalibrasyon](#customization-and-calibration)
- [Konuşma Şeffaflığı](#conversational-transparency)

### <a name="continuously"></a>Sürekli

Kullanıcı deneyimini sürekli olarak geliştiren örtük tasarım desenlerini kullanın.

- [Yetenek Açıklama](#capability-disclosure)
- [Örtük İpuçları](#implicit-cues--feedback)

### <a name="when-the-system-fails"></a>Sistem başarısız olduğunda

İfşaatı incelikle başarısız olmak için bir fırsat olarak kullanın.

- [Konuşma Şeffaflığı](#conversational-transparency)
- [Sesin nasıl üretiliş edildiği hakkında daha fazla bilgi edinmek için fırsatlar sunmak](#providing-opportunities-to-learn-more-about-how-the-voice-was-made)
- [İnsana teslim](#conversational-transparency)



## <a name="additional-resources"></a>Ek kaynaklar
- [Microsoft Bot Yönergeleri](https://www.microsoft.com/research/uploads/prod/2018/11/Bot_Guidelines_Nov_2018.pdf)
- [Cortana Tasarım Yönergeleri](https://docs.microsoft.com/cortana/voice-commands/voicecommand-design-guidelines)
- [Microsoft Windows UWP Konuşma Tasarımı Yönergeleri](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)
- [Microsoft Windows Karışık Gerçeklik Sesli Komut Yönergeleri](https://docs.microsoft.com/windows/mixed-reality/voice-design#top-things-users-should-know-about-speech-in-mixed-reality)

## <a name="reference-docs"></a>Referans dokümanları

* [Ses Yetenek için Açıklama](https://aka.ms/disclosure-voice-talent)
* [Sentetik Ses Teknolojisinin Sorumlu Dağıtımına İlişkin Yönergeler](concepts-guidelines-responsible-deployment-synthetic.md)
* [Gating Genel Bakış](concepts-gating-overview.md)
* [Nasıl Açıklar?](concepts-disclosure-guidelines.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Ses Yetenek için Açıklama](https://aka.ms/disclosure-voice-talent)
