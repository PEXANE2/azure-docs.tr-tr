---
title: Azure’da Cloudyn için sık sorulan sorular
description: Cloudyn portalını kullanarak yaygın dolaylı kurumsal kurulum sorunlarını çözmeyi ve sık sorulan diğer soruları yanıtlamayı öğrenin.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: benshy
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: b9d19923302a40985906fa3c2e0e183045a95860
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460465"
---
# <a name="frequently-asked-questions-for-cloudyn"></a>Cloudyn için sık sorulan sorular

Bu makalede Cloudyn ile ilgili sık sorulan sorulara yer verilmiştir. Cloudyn hakkında sorularınız varsa [Cloudyn Hakkında SSS](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-cloudyn-cost-management?forum=Cloudyn) sayfasından sorabilirsiniz.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Genel dolaylı kurumsal kurulum sorunlarını nasıl çözebilirim?

Cloudyn portalını ilk kullandığınızda, Kurumsal Anlaşma veya Bulut Çözümü Sağlayıcı (CSP) kullanıcısıysanız şu iletileri görebilirsiniz:

- **Cloudyn'i Kurma** sihirbazında "Belirtilen API anahtarı, üst düzey bir kayıt anahtarı değil" iletisi görüntülenir.
- Kurumsal Anlaşma portalında "Doğrudan Kayıt - Hayır" görüntülenir.
- Azure Maliyet Yönetimi portalında "Son 30 gün için kullanım verisi bulunamadı. Cloudyn portalında, Azure hesabınız için işaretlemenin etkinleştirildiğinden emin olmak için lütfen dağıtımcınızla görüşün" iletisi görüntülenir.

Önceki ileti, bir kurumsal bayi veya CSP aracılığıyla Azure Kurumsal Anlaşma satın aldığınızı belirtir. Cloudyn'de verilerinizi görüntüleyebilmeniz için satıcınızın veya CSP’nin Azure hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir.

Sorunların çözümü:

1. Kurumsal bayinin hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir. Yönergeler için bkz. [Dolaylı Müşteri Ekleme Kılavuzu](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Cloudyn ile kullanılmak için Azure Kurumsal Anlaşma anahtarını üretirsiniz. Yönergeler için bkz. [Azure EA'nızı Ekleme](quick-register-ea.md#register-with-cloudyn) veya [EA Kayıt Kimliğinizi ve API Anahtarınızı Bulma](https://youtu.be/u_phLs_udig).

Yalnızca bir Azure hizmet yöneticisi Cloudyn'i etkinleştirebilir. Ortak yönetici izinleri yeterli değil.

Cloudyn'i kurmak için Azure Kurumsal Anlaşma API anahtarını oluşturabilmeniz için aşağıda belirtilen kaynaklarda yer alan yönergeleri izleyerek Azure Faturalama API’sini etkinleştirmeniz gerekir:

- [Kurumsal müşteriler için Raporlama API’lerine genel bakış](../manage/enterprise-api.md)
- **API’lere veri erişimini etkinleştirme** bölümünde [Microsoft Azure kurumsal portal Raporlama API’si](https://ea.azure.com/helpdocs/reportingAPI)


Departman yöneticilerine, hesap sahiplerine ve kurumsal yöneticilere Faturalama API’si ile _ücretleri görüntüleme_ izni vermeniz de gerekebilir.

## <a name="why-dont-i-see-optimizer-recommendations"></a>İyileştirici önerilerini neden göremiyorum?

Öneri bilgileri yalnızca etkinleştirilen hesaplarda kullanılabilir. Aşağıdakiler dahil olmak üzere *etkinleştirilmemiş* durumdaki hesaplar için **İyileştirici** rapor kategorilerinde öneri bilgileri görüntülenmez:

- İyileştirme Yöneticisi
- Boyut İyileştirme
- Verimsizlikler

İyileştirici öneri verilerini görüntüleyemiyorsanız büyük ihtimalle hesaplarınız etkinleştirilmemiş olabilir. Bir hesabı etkinleştirmek için Azure kimlik bilgilerinizle kaydetmeniz gerekir.

Bir hesabı etkinleştirmek için:

1.    Cloudyn portalında, sağ üst kısımdaki **Ayarlar**’a tıklayın ve **Bulut Hesapları**’nı seçin.
2.    Microsoft Azure Hesapları sekmesinde **etkinleştirilmiş** aboneliğe sahip olan hesapları bulun.
3.    Etkinleştirilmemiş hesabın sağ tarafındaki kaleme benzeyen **düzenleme** simgesine tıklayın.
4.    Kiracı kimliğiniz ve ücret kimliğiniz otomatik olarak algılanır. **İleri**’ye tıklayın.
5.    Azure portalına yeniden yönlendirilirsiniz. Portalda oturum açın ve Cloudyn Toplayıcısına Azure verilerinize erişim izni verin.
6.    Sonraki adımda Cloudyn Hesapları yönetim sayfasına yeniden yönlendirilirsiniz ve aboneliğiniz **etkin** Hesap Durumu ile güncelleştirilir. Yanında yeşil renkli onay işareti gösterilir.
7.    Aboneliklerden biri veya daha fazlası için yeşil bir onay işareti simgesi görmezseniz, bu abonelik için okuyucu uygulamasını (CloudynCollector) oluşturma izninizin olmadığı anlamına gelir. Abonelik için daha yüksek izinleri olan bir kullanıcının 3. ve 4. adımları yinelemesi gerekir.  

Yukarıdaki adımları tamamladıktan sonra, İyileştirici önerilerini bir veya iki gün içinde görüntüleyebilirsiniz. Ancak tam iyileştirme verilerinin kullanılabilir duruma gelmesi beş gün sürebilir.


## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Askıya alınmış veya kilitlenmiş kullanıcıları nasıl etkinleştirebilirim?

Öncelikle kullanıcı hesaplarının *initiallySuspended* durumuna geçmesine neden olan en yaygın senaryoyu inceleyelim.

> Admin1, bir Microsoft Bulut Çözümü Sağlayıcısı veya Kurumsal Anlaşma kullanıcısı olabilir. Kuruluşu, Cloudyn'i kullanmaya başlamaya hazırlanıyor.  Azure portalından kaydoluyor ve Cloudyn portalında oturum açıyor. Cloudyn hizmetini kaydeden ve Cloudyn portalında oturum açan Admin1, *birincil yönetici* oluyor. Admin1 herhangi bir kullanıcı hesabı oluşturmuyor. Ancak Cloudyn portalını kullanarak Azure hesapları oluşturuyor ve bir varlık hiyerarşisi belirliyor. Admin1, kiracı yöneticisi olan Admin2'ye Cloudyn'e kaydolması ve Cloudyn portalında oturum açması gerektiğini bildiriyor.
>
> Admin2, Azure portalından kaydoluyor. Ancak Cloudyn portalında oturum açmaya çalıştığında hesabının **askıya alındığını** belirten bir hatayla karşılaşıyor. Birincil yönetici olan Admin1'i hesabın askıya alındığına dair bilgilendiriyor. Admin1'in Admin2’nin hesabını etkinleştirmesi ve uygun varlıklar için *yönetici varlığı erişimi* vermesi, kullanıcı yönetimi erişimi sağlaması ve kullanıcı hesabını etkinleştirmesi gerekiyor.


Bir kullanıcıya erişim izni verme isteği içeren bir uyarı alırsanız kullanıcı hesabını etkinleştirmeniz gerekir.

Kullanıcı hesabını etkinleştirmek için:

1. Cloudyn'i kurmak için kullandığınız Azure yöneticisi kullanıcı hesabını kullanarak Cloudyn'de oturum açın. Yönetici erişimi verilmiş olan bir kullanıcı hesabıyla da oturum açabilirsiniz.
2. Sağ üst kısımdaki dişli simgesine tıklayın ve **Kullanıcı Yönetimi**'ni seçin.
3. Kullanıcıyı bulun, kalem simgesini seçin ve kullanıcıyı düzenleyin.
4. **Kullanıcı durumu** bölümünde durumu **Askıya alındı** yerine **Etkin** olarak değiştirin.

Cloudyn kullanıcı hesapları, Azure'dan çoklu oturum açma ile bağlanır. Parolasını yanlış yazan bir kullanıcının Cloudyn hesabı kilitlenebilir ancak bu kullanıcı Azure'a erişmeye devam edebilir.

Cloudyn'deki e-posta adresinizi Azure'daki varsayılan adresten farklı olacak şekilde değiştirirseniz hesabınız kilitlenebilir. "durum initiallySuspended" ifadesi görüntülenebilir. Kullanıcı hesabınızın kilitlenmesi halinde başka bir yöneticiden hesabınızı kilitlemesini isteyin.

Birinin kilitlenme ihtimaline karşı en az iki Cloudyn yönetici hesabı oluşturmanız önerilir.

Cloudyn portalında oturum açamıyorsanız Cloudyn'de oturum açmak için doğru URL'yi kullandığınızdan emin olun. [https://azure.cloudyn.com](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade) adresini kullanın.

Doğrudan Cloudyn'in URL'sini (`https://app.cloudyn.com`) kullanmaktan kaçının.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Etkinleştirilmemiş hesapları Azure kimlik bilgileriyle nasıl etkinleştirebilirim?

Azure hesaplarınız Cloudyn tarafından keşfedildikten hemen sonra maliyet tabanlı raporlar halinde maliyet verileri sağlanır. Ancak Cloudyn'in kullanım ve performans verilerini sunabilmesi için Azure kimlik bilgilerinizi hesaplara kaydetmeniz gerekir. Yönergeler için bkz. [Hesap ekleme veya aboneliği güncelleştirme](activate-subs-accounts.md#add-an-account-or-update-a-subscription).

Bir hesaba Azure kimlik bilgilerini eklemek için Cloudyn portalında hesap adının (aboneliğin değil) sağ tarafındaki kalem simgesini seçin.

Azure kimlik bilgileriniz Cloudyn'e eklenene kadar hesap _etkinleştirilmemiş_ olarak görünür.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Mevcut aboneliğe nasıl birden fazla hesap ve varlık nasıl ekleyebilirim?

Cloudyn aboneliğine ek Kurumsal Anlaşma eklemek için ek varlıklar kullanılır. Daha fazla bilgi için bkz. [Varlıkları oluşturma ve yönetme](tutorial-user-access.md#create-and-manage-entities).

CSP'ler için:

Bir varlığa ek CSP hesapları eklemek için yeni varlığı oluştururken **Kurumsal** yerine **MSP Erişimi**'ni seçin. Hesabınız Kurumsal Anlaşma olarak kayıtlıysa ve CSP kimlik bilgileri eklemek istiyorsanız Cloudyn destek ekibinin hesap ayarlarınızı değiştirmesi gerekebilir. Ücretli bir Azure abonesiyseniz, Azure portalında yeni bir destek isteği oluşturabilirsiniz. **Yardım + destek** ve ardından **Yeni destek isteği**'ni seçin.

## <a name="currency-symbols-in-cloudyn-reports"></a>Cloudyn raporlarındaki para birimi simgeleri

Farklı para birimlerini kullanan birden fazla Azure hesabına sahip olabilirsiniz. Ancak Cloudyn'deki raporlar, rapor başına yalnızca bir para birimi türü gösterir.

Farklı para birimlerini kullanan birden fazla aboneliğiniz varsa, üst varlık ve onun alt varlığı olan para birimleri ABD doları **$** olarak görüntülenir. Önerilen en iyi yöntemlerden biri, aynı varlık hiyerarşisinde farklı para birimlerini kullanmaktan kaçınmaktır. Başka bir deyişle bir varlık yapısı altında düzenlenmiş olan tüm abonelikleriniz aynı para birimini kullanmalıdır.

Cloudyn, Kurumsal Anlaşma aboneliğinizdeki para birimini otomatik olarak algılar ve bunu raporlarda doğru şekilde sunar.  Ancak Cloudyn, CSP ve doğrudan web Azure hesapları için yalnızca ABD doları **$** para birimini görüntüler.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Cloudyn veri yenileme zaman çizelgeleri nedir?

Cloudyn aşağıdaki veri yenileme zaman çizelgelerine sahiptir:

- **Başlangıç**: Ayarladıktan sonra, maliyet verilerinin Cloudyn'de görüntülemesi 24 saate kadar sürebilir. Ayrıca Cloudyn'in boyutlandırma önerilerini görüntülemesi için yeterli verilerin toplanması 10 güne kadar sürebilir.
- **Günlük**: Cloudyn, her ayın onuncu gününden son gününe kadar bir sonraki gün yaklaşık UTC+3 sonrasında önceki günle ilgili güncel verileri göstermelidir.
- **Aylık**: Cloudyn, her ayın ilk gününden onuncu gününe kadar yalnızca önceki ayın sonuna kadar olan verilerinizi gösterebilir.

Cloudyn, önceki güne ait tüm veriler kullanılabilir duruma geldiğinde bunları işler. Önceki günün verileri genellikle Cloudyn'de mevcuttur ancak her gün yaklaşık UTC+3 civarında verilir. Etiketler gibi bazı verileri işlemek için ayrı bir 24 saat sürebilir.

Geçerli aya ait veriler, her ayın başında toplanmak için uygun değildir. Hizmet sağlayıcıları bu süre boyunca önceki ayın faturasını tamamlar. Önceki ayın verileri Cloudyn'de yeni ay başladıktan 5-10 gün sonra görünür. Bu süre boyunca yalnızca önceki aya ait amorti edilen maliyetleri görebilirsiniz. Günlük faturalandırma veya kullanım verilerini göremeyebilirsiniz. Veriler kullanılabilir hale geldiğinde Cloudyn bunları geriye dönük olarak işler. İşlem tamamlandıktan sonra her ayın beşinci günü ile onuncu günü arasında aylık verilerin tamamı görüntülenir.

Verilerin Azure'dan Cloudyn'e gönderilmesi sırasında bir gecikme yaşanırsa veriler Azure'da kaydedilmeye devam eder. Bağlantı kurulduğunda bu veriler Cloudyn'e aktarılır.

## <a name="cost-fluctuations-in-cloudyn-cost-reports"></a>Cloudyn Maliyet Raporlarındaki maliyet dalgalanmaları

Bulut hizmeti sağlayıcılarının güncelleştirilmiş fatura dosyaları göndermesi durumunda maliyet raporlarında dalgalanmalar olabilir. Maliyet dalgalanmalarının nedeni, bulut hizmeti sağlayıcısından olağan günlük veya aylık raporlama zamanlaması haricinde yeni dosya gelmesidir. Maliyet değişikliklerinin nedeni Cloudyn hesaplamaları değildir.

Ay boyunca bulut hizmeti sağlayıcınız tarafından gönderilen tüm faturalandırma dosyaları, günlük maliyetlerinizin tahmini değerini gösterir. Bazen veriler sıklıkla (genellikle günde birkaç kaz) güncelleştirilir. AWS'de Azure'da olduğundan daha sık güncelleştirme yapılır. Önceki ay için faturalandırma toplamı tamamlandığında ve son faturalandırma dosyası alındığında maliyet toplamları sabit kalmalıdır. Bu da genellikle ayın 10'u olur.

Bulut hizmeti sağlayıcınızdan maliyet ayarlamaları aldığınızda değişiklikler oluşur. Buna örnek olarak kredi almak verilebilir. İlgili ay kapatıldıktan aylar sonra değişiklik yapılabilir. Bulut hizmeti sağlayıcınız tarafında yeniden hesaplama yapıldığında ilgili değişiklikler gösterilir. Cloudyn, tüm ayarlamaların yeniden hesaplanmasını sağlamak için geçmiş verilerini güncelleştirir. Ayrıca maliyetlerin raporlarda doğru şekilde gösterildiğini de doğrular.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Doğrudan CSP'ler, dolaylı CSP müşterileri veya iş ortakları için Cloudyn erişimini nasıl yapılandırabilir?

Yönergeler için bkz. [Cloudyn’de dolaylı CSP erişimini yapılandırma](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>İyileştirici menü öğesi neden kayboldu?

Azure Resource Manager erişimi eklendikten ve veriler toplandıktan sonra **İyileştirici** seçeneğinin gelmesi gerekir. Azure Resource Manager erişimini etkinleştirmek için bkz. [Etkinleştirilmemiş hesapları Azure kimlik bilgileriyle nasıl etkinleştirebilirim?](#how-do-i-activate-unactivated-accounts-with-azure-credentials)

## <a name="is-cloudyn-agent-based"></a>Cloudyn, aracı tabanlı mıdır?

Hayır. Aracı kullanılmaz. VM'ler için Azure sanal makine ölçüm verileri, Microsoft Insights API'sinden alınır. Azure VM'lerinden ölçüm verilerini almak istiyorsanız tanılama ayarlarının etkinleştirilmesi gerekir.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Cloudyn raporları, rapor başına birden fazla AD kiracısı gösteriyor mu?

Evet. Sahip olduğunuz her bir AD kiracısı için [karşılık gelen bir bulut hesabı varlığı oluşturabilirsiniz](tutorial-user-access.md#create-and-manage-entities). Daha sonra Azure AD kiracı verilerinizin yanı sıra Amazon Web Services ve Google Cloud Platform dahil olmak üzere diğer bulut platformu sağlayıcılarını görüntüleyebilirsiniz.
