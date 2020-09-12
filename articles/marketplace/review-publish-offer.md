---
title: Microsoft Commercial Market 'e yönelik bir teklifi İnceleme ve yayımlama
description: Teklifinizi önizlemek için Iş Ortağı Merkezi 'ni kullanın, teklifinizin önizlemesini yapın ve Microsoft ticari Market ' te yayımlayın.
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 08/12/2020
ms.openlocfilehash: eff1db3aca2cab77d18698634b84e8e1b1c99f7e
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89375898"
---
# <a name="how-to-review-and-publish-an-offer-to-the-commercial-marketplace"></a>Ticari Market 'e yönelik bir teklifi İnceleme ve yayımlama

Bu makalede, Iş Ortağı Merkezi 'ni kullanarak kendi teklifinizi yayınlama, teklifinizin önizlemesini oluşturma ve ardından ticari Market 'te yayımlama işlemlerinin nasıl yapılacağı gösterilmektedir. Yayımlama adımlarında ilerlerken yayımlama durumunuzu nasıl denetliyoruz de ele alınmaktadır. Yayımlamak istediğiniz bir teklifi önceden oluşturmuş olmanız gerekir.

## <a name="offer-status"></a>Teklif durumu

Teklif durumunuzu, [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/commercial-marketplace/overview)' nde ticari Market panosunun **genel bakış** sekmesinde gözden geçirebilirsiniz. Her teklifin **durum** sütununda aşağıdaki durum göstergelerden biri görüntülenir.

| Durum | Açıklama |
| ------------ | ------------- |
| Taslak | Teklif oluşturuldu ancak yayımlanmadı. |
| Yayımlama devam ediyor | Teklif, yayımlama sürecinde bu şekilde çalışır. |
| İlgilenilmesi gerekiyor | Sertifika sırasında veya başka bir yayımlama aşamasında kritik bir sorun tespit ettik. |
| Önizleme | Artık, yayımcı tarafından nihai bir doğrulamayı beklemiş olan teklifi sertifikalandırdık. Teklifi canlı yayımlamak için **canlı git** ' i seçin. |
| Canlı | Teklif, Market 'te canlı olur ve müşteriler tarafından görülebilir ve elde edilebilir. |
| Satışı durdur bekleniyor | Yayımcı bir teklif veya planda "satışı Durdur" seçeneğini seçti, ancak eylem henüz tamamlanmadı. |
| Market 'te yok | Market 'te daha önce yayınlanmış bir teklif kaldırılmıştır. |
|||

## <a name="validation-and-publishing-steps"></a>Doğrulama ve yayımlama adımları

Yayımlamaya yönelik bir teklif göndermeye hazırsanız, portalın sağ üst köşesinde **gözden geçir ve Yayımla** ' yı seçin. **İnceleme ve yayımlama** sayfasında, teklifiniz için her sayfanın durumu gösterilir ve bu, aşağıdakilerden biri olabilir:

   - **Başlatılmamış** – sayfa tamamlanmadı.
   - **Tamamlanmamış** – sayfada gerekli bilgiler eksik veya düzeltilmesi gereken hatalar var. Sayfaya dönüp güncelleştirmeniz gerekir.
   - **Tamam** – sayfa tamamlanmıştır. Gerekli tüm veriler sağlanmış ve hata yok.

Sayfaların herhangi birinde **tamamlanmamış**bir durum varsa, söz konusu sayfadaki sorunu düzeltmeniz ve sonra durumu şimdi **Tamam**olarak gösterir durumunu onaylamak için **İnceleme ve yayımlama** sayfasına geri dönmeniz gerekir. Bazı teklif türleri test gerektirir. Bu durumda, sertifika ekibine yönelik test yönergeleri ve uygulamanızı anlamak için yararlı olan tüm ek notlar sağlamanız gereken **sertifika alanı için bir not** görürsünüz.

Tüm sayfalar tamamlandıktan ve ilgili test notlarını girdikten sonra, doğrulama ve yayımlama işlemlerine başlamak için **Yayımla** ' yı seçin. Aşamalar ve genel sıra, yayımladığınız teklifin türüne bağlı olarak farklılık gösterebilir. Aşağıdaki tabloda, olası bir yayımlama akışı gösterilmektedir. Her aşama aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

| Aşama | Ne olur? |
| ------------ | ------------- | ------------- |
| [Otomatik doğrulama](#automated-validation-phase) | Otomatik doğrulama kümesini işliyoruz. |
| [Sertifika](#certification-phase) | El ile doğrulama yaptık. |
| [Önizleme oluşturma](#preview-creation-phase) | Teklif önizlemeniz için liste sayfası önizleme bağlantısına sahip olan herkes tarafından kullanılabilir. Teklifiniz Microsoft (transactable) üzerinden satılıyorsa, yalnızca teklifinizin **ön izleme İzleyicisi** sayfasında belirttiğiniz hedef kitle, test için teklif satın alabilir ve bu teklifi erişebilir. |
| [Yayımcı oturumu kapatma](#publisher-sign-off-phase) | Teklifinizi önizlemenizi ve onaylamanıza yönelik bir istek içeren bir e-posta göndereceğiz. |
| [Yayımlama](#publish-phase) | Önizleme teklifinin ticari Market 'e canlı olarak yayımlandığını doğrulamak için bir dizi adımı çalıştırdık. |
|||

## <a name="automated-validation-phase"></a>Otomatik doğrulama aşaması

Yayımlama işlemindeki ilk adım bir otomatik doğrulama kümesidir. Her doğrulama adımı, teklifi oluştururken seçtiğiniz bir özelliğe karşılık gelir. Teklifin yayımlama işlemindeki bir sonraki adıma geçebilmesi için her doğrulama denetiminin tamamlanmış olması gerekir.

- **Teklif satın alma akışı kurulumu** (<10 dk)

   Teklifinizin, Azure portal aracılığıyla müşteriler tarafından satın alındığında karşılanabileceği güvence altına alınır. Bu adım yalnızca Microsoft ile satılan teklifler için geçerlidir.

- **Sınama sürücüsü veri doğrulaması** (~ 5 dakika)

   Teklifin teknik yapılandırma sayfasında verdiğiniz verileri doğrulamamız gerekir. Test sürücüsü işlevlerini test etme ve onaylama. Bu adım yalnızca bir test sürücüsü etkin olan teklifler için geçerlidir.

- **Sınama sürücüsü sağlama** (~ 30 dakika)

    Önceki adımda test sürücünüzün verileri ve işlevlerini doğruladıktan sonra, test sürücünüzün örneklerini dağıtıp çoğaltarak müşteri kullanımına hazırlarız. Bu adım yalnızca bir test sürücüsü etkin olan teklifler için geçerlidir.

- **Müşteri adayı yönetimi doğrulama ve kayıt** (<15 dk)

    Müşteri adayı yönetim sisteminizin müşteri adaylarını, teklif Kurulumu sayfasında verdiğiniz ayrıntılara göre **alacağından** emin veriyoruz. Bu adım yalnızca, lider yönetimi etkinleştirilmiş teklifler için geçerlidir.

## <a name="certification-phase"></a>Sertifika aşaması

Ticari Market 'e gönderilen tekliflerin yayımlanmadan önce sertifikalı olması gerekir. Daha ayrıntılı bir test, bazı otomatikleştirilmiş ve diğerleri el ile daha kapsamlı testler sunar. Daha fazla bilgi için bkz. [ticari Market sertifika ilkeleri](https://aka.ms/commercial-marketplace-certification-policies).

### <a name="types-of-validation-that-take-place-during-certification"></a>Sertifika sırasında gerçekleşen doğrulama türleri

Gönderilen her teklif için sertifika işlemine dahil edilen üç doğrulama düzeyi vardır.

- Yayımcı iş uygunluğu
- İçerik doğrulama
- Teknik doğrulama

#### <a name="publisher-business-eligibility"></a>Yayımcı iş uygunluğu

Her teklif türü, gerekli bir temel uygunluk ölçütü kümesini denetler. Bu ölçütler, yayımcı MPN durumu, Uzmanlıklar, uzmanlık düzeyleri vb. içerebilir.

#### <a name="content-validation"></a>İçerik doğrulama

Teklifinizi oluştururken girilen bilgiler kalite ve ilgi açısından denetlenir. Bu denetimler Market listeleme ayrıntıları, fiyatlandırma, kullanılabilirlik, ilişkili planlar vb. için girdilerinizi gözden geçirir. Microsoft AppSource ve Azure Marketi 'nin liste ölçütlerini karşılamak için teklifinizin şunları içerdiğini doğrulayacağız:

- Teklifi doğru şekilde açıklayan bir başlık
- Kapsamlı bir genel bakış ve değer teklifi sağlayan iyi yazılmış açıklamalar
- Kaliteli ekran görüntüleri ve videolar
- Teklifin Microsoft platformlarını ve araçlarını nasıl kullandığını gösteren bir açıklama.

[Genel listeleme ilkelerini](https://aka.ms/commercial-marketplace-certification-policies#100-general)okuyarak içerik doğrulama ölçütleriyle ilgili daha fazla bilgi edinin.

#### <a name="technical-validation"></a>Teknik doğrulama

Teknik doğrulama sırasında, teklif (paket veya ikili) aşağıdaki denetimleri altına girer.

- Kötü amaçlı yazılım için tarandı
- İzlenen ağ çağrıları
- Paket çözümlendi
- Teklifin işlevselliğinin tam olarak taranması

Teklif, güçlü olduğundan emin olmak için çeşitli platformlar ve sürümler arasında test edilir.

### <a name="certification-failure-report"></a>Sertifika hata raporu

Teklifiniz herhangi bir liste, teknik veya ilke denetimi başarısız olursa veya bu tür bir teklif göndermeye uygun değilse, size bir sertifika hatası raporu göndereceğiz.

Bu rapor, gözden geçirme notlarıyla birlikte başarısız olan ilkelerin açıklamalarını içerir. Bu e-posta raporunu gözden geçirin, herhangi bir sorunu çözün, sizin teklifinizde gereken güncelleştirmeleri yapın ve Iş Ortağı Merkezi ' nde [ticari Market portalı](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) 'nı kullanarak teklifi yeniden gönderin. Sertifikayı geçirmeden önce teklifi gerektiği kadar yeniden gönderebilirsiniz.

## <a name="preview-creation-phase"></a>Önizleme oluşturma aşaması

Önizleme oluşturma aşamasında, teklifinizin yalnızca teklifinizin **ön izleme İzleyicisi** sayfasında belirttiğiniz kitlelerin erişebileceği bir sürümünü oluşturacağız. Teklifinizi canlı olarak yayımlaana kadar, teklifinizin önizleme sürümü, önizleme hedef kitlesi dışındaki herkese açık olmayacaktır.

> [!NOTE]
> Kuruluşunuzun dışındaki kişilere bir teklifle görünürlük sağlamak için Önizleme kitleyi kullanmayın. Bunun yerine özel teklif seçeneğini kullanın. Bu noktada, teklifiniz tam olarak sınanmamıştır ve doğrulanmaz ve dağıtım dışı için hazırlanma.

## <a name="publisher-sign-off-phase"></a>Yayımcı oturum kapatma aşaması

Teklif gözden geçirmeniz ve oturumunuz devre dışı bırakıldığında size, teklif önizlemeinizi gözden geçirip onaylamanıza yönelik bir e-posta göndereceğiz. Teklifinizin Yayımcı oturum açma aşamasına ulaşmış olup olmadığını görmek için tarayıcınızda **teklif genel bakış** sayfasını da yenileyebilirsiniz. Varsa, **canlı çalış** düğmesi ve önizleme bağlantıları kullanılabilir hale gelir.

Aşağıdaki ekran görüntüsünde bir SaaS teklifinin **teklif genel bakış** sayfası gösterilmektedir. Bu sayfada göreceğiniz doğrulama adımları, teklif türüne ve teklifi oluştururken yaptığınız seçimlere bağlı olarak değişir.

![Iş Ortağı Merkezi 'nde teklif için teklif genel bakış sayfasını gösterir. Canlı çalış düğmesi ve önizleme bağlantıları gösterilir.](./media/publish-status-publisher-signoff.png)

### <a name="previewing-and-approving-your-offer"></a>Teklifinizi önizleme ve onaylama

> [!IMPORTANT]
> Uçtan uca satın alma ve kurulum akışını doğrulamak için, önizleme aşamasında olan teklifinizi satın alın. Bir ücreti işlememenizi sağlamak için öncelikle Microsoft 'un bir [destek bileti](https://aka.ms/marketplacesupport) olduğunu bildirin.

**Teklif genel bakış** sayfasında, **canlı çalış** düğmesinin altında Önizleme bağlantıları görüntülenir. Teklifinizi oluştururken seçtiğiniz seçeneklere bağlı olarak, AppSource Preview, Azure Marketi önizlemesi veya her ikisinin de bir bağlantısı olacaktır. Teklifinizi Microsoft aracılığıyla satmayı seçerseniz, önizleme izleyicisine eklenmiş olan herkes, bu aşamada gereksinimlerinizi karşıladığından emin olmak için teklifinizin Alım ve dağıtımını test edebilir.

Önizlemeyi onayladıktan sonra, teklifinizi ticari Market 'te canlı yayımlamak için **canlı ol** ' u seçin. 

Teklifin önizlemesini yaptıktan sonra değişiklik yapmak istiyorsanız, yayın isteğinizi düzenleyebilir ve yeniden gönderebilirsiniz. Teklifiniz zaten canlı ve Market 'te herkese açık ise, yaptığınız güncelleştirmeler **canlı git*' i seçene kadar canlı olmaz. Daha fazla bilgi için bkz [. ticari Market 'te mevcut teklifi güncelleştirme](./partner-center-portal/update-existing-offer.md)

## <a name="publish-phase"></a>Yayımlama aşaması

Teklifinizle canlı yapmayı seçtiniz ve bu, ticari Market 'te kullanılabilir hale getirir. canlı teklifin yalnızca teklifin önizleme sürümü gibi yapılandırıldığından emin olmak için bir dizi son doğrulama denetimi gerçekleştirdik.

- **Teklif satın alma akışı kurulumu** (>10 dk)

    Teklifinizin, Azure portal aracılığıyla müşteriler tarafından satın alındığında karşılanabileceği güvence altına alınır. Bu adım yalnızca Microsoft ile satılan teklifler için geçerlidir.

- **Sınama sürücüsü veri doğrulaması** (~ 5 dakika)

    Teklifin teknik yapılandırma sayfasında verdiğiniz verileri doğrulamamız gerekir. Test sürücüsü işlevlerini test etme ve onaylama. Bu adım yalnızca bir test sürücüsü etkin olan teklifler için geçerlidir.

- **Sınama sürücüsü sağlama** (~ 30 dakika)

    Test sürücünüzün örneklerini dağıtıp çoğalttık ve bu sayede müşteri kullanımı için hazırlanın. Bu adım yalnızca bir test sürücüsü etkin olan teklifler için geçerlidir.

- **Müşteri adayı yönetimi doğrulama ve kayıt** (>15 dk)

    Müşteri adaylarını teklif kurulum sayfanızda sunulan ayrıntılara göre müşteri adaylarını **alacağından** emin veriyoruz. Bu adım yalnızca, lider yönetimi etkinleştirilmiş teklifler için geçerlidir.

- **Son yayımlama (>30 dakika)**

    Teklifinizin Market 'te herkese açık hale geldiğini güvence altına aldığınızdan emin veriyoruz.

Bu doğrulama denetimleri tamamlandıktan sonra teklifiniz Market 'te canlı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Iş Ortağı Merkezi 'nde ticari Market için analitik raporlara erişin](partner-center-portal/analytics.md)
