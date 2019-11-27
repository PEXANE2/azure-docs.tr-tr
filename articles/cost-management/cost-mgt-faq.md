---
title: Cloudyn Azure için sık sorulan sorular | Microsoft Docs
description: Bu makalede, Cloudyn hakkında genel soruların yanıtları sağlanır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: troubleshooting
ms.service: cost-management-billing
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: faa7181ebc7b886fbe06e4ac01c704016d4b0c51
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230090"
---
# <a name="frequently-asked-questions-for-cloudyn"></a>Cloudyn için sık sorulan sorular

Bu makalede Cloudyn hakkında bazı yaygın sorular ele alınmıştır. Cloudyn hakkında sorularınız varsa [Cloudyn Için SSS](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-cloudyn-cost-management?forum=Cloudyn)adresinde sorun bulabilirsiniz.

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Sık karşılaşılan dolaylı Kurumsal kurulum sorunlarını nasıl giderebilirim?

Cloudyn portalını ilk kullandığınızda, Kurumsal Anlaşma veya Bulut Çözümü Sağlayıcı (CSP) kullanıcısıysanız şu iletileri görebilirsiniz:

- "Belirtilen API anahtarı, **Cloudyn ayarlama** Sihirbazı 'nda görünen bir üst düzey kayıt anahtarı değil"
- "– Hayır Kurumsal Anlaşma portalında gösterilen doğrudan kayıt".
- "Son 30 güne ait kullanım verisi bulunamadı. Biçimlendirme Azure hesabınız için etkinleştirildi emin olmak için dağıtımcı başvurun", Cloudyn portalında görüntülenmez.

Önceki ileti, bir kurumsal bayi veya CSP aracılığıyla Azure Kurumsal Anlaşma satın aldığınızı belirtir. Cloudyn'de verilerinizi görüntüleyebilmeniz için satıcınızın veya CSP’nin Azure hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir.

Sorunların çözümü:

1. Kurumsal bayinin hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir. Yönergeler için bkz. [Dolaylı Müşteri Ekleme Kılavuzu](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Cloudyn ile kullanılmak için Azure Kurumsal Anlaşma anahtarını üretirsiniz. Yönergeler için bkz. [Azure EA 'Nizi ekleme](quick-register-ea.md#register-with-cloudyn) veya [EA kayıt kimliğinizi ve API anahtarınızı bulma](https://youtu.be/u_phLs_udig).

Yalnızca bir Azure hizmet yöneticisi Cloudyn'i etkinleştirebilir. Ortak yönetici izinleri yeterli değil.

Cloudyn'i kurmak için Azure Kurumsal Anlaşma API anahtarını oluşturabilmeniz için aşağıda belirtilen kaynaklarda yer alan yönergeleri izleyerek Azure Faturalama API’sini etkinleştirmeniz gerekir:

- [Kurumsal müşteriler için Raporlama API’lerine genel bakış](../billing/billing-enterprise-api.md)
- [API’lere veri erişimini etkinleştirme](https://ea.azure.com/helpdocs/reportingAPI) bölümünde **Microsoft Azure kurumsal portal Raporlama API’si**


Departman yöneticilerine, hesap sahiplerine ve kurumsal yöneticilere Faturalama API’si ile _ücretleri görüntüleme_ izni vermeniz de gerekebilir.

## <a name="why-dont-i-see-optimizer-recommendations"></a>İyileştirici önerileri neden göremiyorum?

Öneri bilgiler, yalnızca etkinleştirilen hesapları için kullanılabilir. *Etkin*olmayan hesaplara yönelik **iyileştirici** rapor kategorilerinde hiçbir öneri bilgisi görmezsiniz, örneğin:

- En iyi duruma getirme Yöneticisi
- Boyutlandırma optimizasyonu
- Verimsizlikleri

İyileştirici öneri verileri görüntüleyemezsiniz sonra büyük olasılıkla etkinleştirilmemiş olan hesapların varsa. Bir hesabı etkinleştirmek için Azure kimlik bilgilerinizle kaydetmeniz gerekir.

Bir hesabı etkinleştirmek için:

1.  Cloudyn portalında, sağ üst kısımdaki **Ayarlar**’a tıklayın ve **Bulut Hesapları**’nı seçin.
2.  Microsoft Azure hesapları sekmesinde, **etkinleştirilmiş** aboneliği olmayan hesaplara bakın.
3.  Etkin olmayan bir hesabın sağında, bir kaleme benzeyen **düzenleme** simgesine tıklayın.
4.  Kiracı kimliği ve oran Kimliğinizi otomatik olarak algılanır. **İleri**’ye tıklayın.
5.  Azure portalına yönlendirilirsiniz. Portalda oturum açın ve Azure verilerinize erişmek için Cloudyn Toplayıcı yetkisi verin.
6.  Daha sonra Cloudyn hesap yönetimi sayfasına yönlendirilirsiniz ve aboneliğiniz **etkin** hesap durumuyla güncelleştirilir. Bu, yeşil onay işareti simgesi gösterir.
7.  Bir veya daha fazla abonelik için bir yeşil onay işareti simgesini görmüyorsanız, (CloudynCollector) abonelik için bir okuyucu uygulaması oluşturmak için gerekli izinlere sahip değilsiniz demektir. Abonelik için daha yüksek izinlere sahip bir kullanıcı, 3 ve 4 gerekiyor.  

Yukarıdaki adımları tamamladıktan sonra bir veya iki gün içinde iyileştirici önerileri görüntüleyebilirsiniz. Ancak, uygulamanın tam iyileştirme veri kullanılabilir olmadan önce en fazla beş gün sürebilir.


## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Askıya alınmış veya kilitlenmiş kullanıcıları nasıl etkinleştirebilirim?

İlk olarak, Kullanıcı hesaplarının *ınitiallysuspbiterini*almasına neden olan en yaygın senaryoya göz atalım.

> Admin1 bir Microsoft Bulut çözüm sağlayıcısı veya Kurumsal Anlaşma Kullanıcı olabilir. Kuruluşları Cloudyn kullanmaya başlamaya hazırlanıyor.  Kendisi, Azure portalı üzerinden kaydeder ve Cloudyn portalında oturum açar. Cloudyn hizmetini kaydeden ve Cloudyn portalında oturum açan kişi, admin1 *birincil yönetici*olur. Tüm kullanıcı hesaplarını admin1 oluşturmaz. Ancak, Cloudyn portalını kullanarak Azure hesapları oluşturur ve bir varlık hiyerarşisi ayarlar. Admin1, Cloudyn 'e kaydolmaları ve Cloudyn portalında oturum açması için gereken Admin2, kiracı yöneticisi 'ne bildirir.
>
> Azure portalı üzerinden Admin2 kaydeder. Ancak Cloudyn portalında oturum açmaya çalıştıklarında, hesaplarının **askıya alındığını**belirten bir hata alırlar. Admin1, birincil yönetici hesabı askıya alınması bildirilir. Admin1 Admin2's hesabı 'nı etkinleştirip uygun varlıklar için *yönetici varlığı erişimi* vermelidir ve Kullanıcı yönetimi erişimine ve etkin kullanıcı hesabına izin verir.


Bir istek, bir kullanıcı için erişim izni olan bir uyarı alırsanız, kullanıcı hesabı'nı etkinleştirmeniz gerekir.

Kullanıcı hesabını etkinleştirmek için:

1. İçin Cloudyn, Cloudyn ' için kullandığınız Azure yönetici kullanıcı hesabıyla oturum açın. Veya yönetici erişim izni bir kullanıcı hesabıyla oturum açın.
2. Sağ üst köşedeki dişli simgesini seçin ve **Kullanıcı yönetimi**' ni seçin.
3. Kullanıcıyı bulun, kalem simgesini seçin ve ardından kullanıcı düzenleyin.
4. **Kullanıcı durumu**' nun altında, durumu **askıya alındı** ' dan **etkin**' e değiştirin.

Cloudyn kullanıcı hesapları, çoklu oturum açmayı azure'dan kullanarak bağlanın. Bir kullanıcı parolasını mistypes, Azure yine de erişebilmeleri olsa da bunlar Cloudyn dışında saldırdığında.

Cloudyn ' deki e-posta adresinizi Azure 'daki varsayılan adresten değiştirirseniz, hesabınız kilitlenebilir. "Durum ınitiallysuspsonsonu" gösterebilir. Kullanıcı hesabınız kilitlendi, hesabınızı sıfırlamak için başka bir yönetici ile iletişime geçin.

Hesaplardan birini kilitli durumunda en az iki Cloudyn yöneticisi hesabı oluşturmanızı öneririz.

Cloudyn portalında oturum açamıyorsanız Cloudyn'e oturum açmak için doğru URL'yi kullandığınızdan emin olun. [https://azure.cloudyn.com](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade)kullanın.

Cloudyn doğrudan URL 'sini kullanmaktan kaçının https://app.cloudyn.com.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Azure kimlik etkinleştirilmemiş hesaplarıyla nasıl etkinleştirebilirim?

Hesaplarınız Cloudyn tarafından bulunduktan hemen sonra maliyet verilerini hemen maliyet tabanlı raporlara sağlanır. Bununla birlikte, Cloudyn kullanım ve performans verilerini sağlamak hesapları için Azure kimlik bilgilerinizi kaydetmeniz gerekir. Yönergeler için bkz. [Hesap ekleme veya abonelik güncelleştirme](activate-subs-accounts.md#add-an-account-or-update-a-subscription).

Azure'ı eklemek için hesap adı, abonelik sağındaki düzenleme simgesi Cloudyn portalında, bir hesabın kimlik bilgilerini seçin.

Azure kimlik bilgileriniz Cloudyn 'e ekleninceye kadar, hesap _devre dışı_olarak görünür.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Mevcut bir aboneliğe birden çok hesap ve varlıkları nasıl ekleyebilirim?

Ek varlıklar Cloudyn aboneliğine ek Kurumsal anlaşmalar eklemek için kullanılır. Daha fazla bilgi için bkz. [varlıkları oluşturma ve yönetme](tutorial-user-access.md#create-and-manage-entities).

CSP'ler için:

Bir varlığa ek CSP hesapları eklemek için, yeni varlığı oluştururken **Enterprise** yerine **MSP erişimi** ' ni seçin. Hesabınızı bir Kurumsal Anlaşma kaydedilir ve CSP kimlik bilgileri eklemek istediğiniz, Cloudyn destek personeli, hesap ayarlarını değiştirmek gerekebilir. Ücretli bir Azure abonesi iseniz, Azure portalında yeni bir destek isteği oluşturabilirsiniz. **Yardım ve destek**' i seçin ve ardından **Yeni destek isteği**' ni seçin.

## <a name="currency-symbols-in-cloudyn-reports"></a>Cloudyn raporlarında para birimi sembolleri

Farklı para birimlerinin kullanarak birden çok Azure hesabında olabilir. Bununla birlikte, Cloudyn maliyet raporlarında, rapor başına birden fazla para birimi türü gösterme.

Farklı para birimleri kullanan birden çok aboneliğiniz varsa, bir üst varlık ve onun alt varlık para birimleri USD **$** görüntülenir. Bizim önerilen en iyi uygulama, aynı varlık hiyerarşideki farklı para birimlerinin kullanarak önlemek içindir. Diğer bir deyişle, bir varlık yapısında düzenlenmiş tüm abonelikler aynı para kullanmanız gerekir.

Cloudyn, otomatik olarak, Kurumsal Anlaşma abonelik para algılar ve düzgün şekilde raporlar sunar.  Ancak Cloudyn yalnızca CSP ve web doğrudan Azure hesapları için **$** USD görüntüler.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Cloudyn veri nelerdir zaman çizelgeleri yenilensin mi?

Cloudyn, aşağıdaki veri yenileme zaman çizelgeleri sahiptir:

- **İlk**: ayarladıktan sonra Cloudyn 'de maliyet verilerinin görüntülemesi 24 saate kadar sürebilir. Ayrıca, Cloudyn boyutlandırma önerileri görüntülemek için yeterli veri toplamak üzere 10 güne kadar da sürebilir.
- **Günlük**: onuncu günden her ayın sonuna kadar, Cloudyn, ertesi günden sonra UTC + 3 ' ten sonraki bir günden sonra verilerinizi güncel olarak göstermelidir.
- **Aylık**: ilk günden her ayın onuncu gününe kadar, Cloudyn verilerinizi yalnızca önceki ayın sonuna kadar gösterebilir.

Cloudyn, önceki günün önceki gün tam veri kullanılabilir olduğunda verileri işler. Önceki günün verileri her gün genellikle UTC + 3 hakkında Cloudyn tarafından kullanılabilir. Etiketler gibi bazı veriler ilaveten 24 işlemek için saat sürebilir.

Geçerli ay için verileri, her ayın başında toplamalarında kullanılamaz. Dönemi boyunca, hizmet sağlayıcıları önceki ay için bunların faturalandırma sonlandırın. Önceki ayın verilerini 10 gün sonra her ay başı Cloudyn 5'te görünür. Bu süre boyunca, yalnızca önceki ayın amorti edilmiş maliyet görebilirsiniz. Günlük faturalandırma ya da kullanım verilerini göremeyebilirsiniz. Cloudyn veri kullanılabilir olduğunda, geriye dönük olarak işler. İşlemden sonra tüm aylık verileri görüntülenir her ayın on günü beşinci arasında.

Cloudyn'e Azure'dan veri gönderen bir gecikme varsa, verileri Azure'da hala kaydedilir. Bağlantı geri geldiğinde, veriler Cloudyn'e aktarılır.

## <a name="cost-fluctuations-in-cloudyn-cost-reports"></a>Cloudyn maliyet raporlarında maliyet dalgalanmaları

Her bulut hizmeti sağlayıcıları güncelleştirilmiş fatura dosyaları gönderdiğinizde maliyet raporlarında maliyet dalgalanmaları gösterebilirsiniz. Yeni dosyaları bir bulut hizmeti sağlayıcısından dışında normal günlük veya aylık zamanlama bildirimi alındığında dalgalı maliyetler oluşur. Maliyet değişiklikler Cloudyn yeniden hesaplanmasına neden yoktur.

Ay boyunca, bulut hizmet sağlayıcınız tarafından gönderilen tüm faturalandırma günlük maliyetlerinizi tahmini dosyalarıdır. Bazen verileri sık sık güncelleştirilir — birden çok kez günde bazen. AWS ile Azure sık güncelleştirmeler. Önceki aya ait fatura hesaplama ve son fatura dosya alındığında, maliyet toplamları kararlı kalmalıdır. Genellikle, 10 ay tarafından.

Değişiklikler, bulut hizmeti sağlayıcısından maliyet ayarlamaları aldığında gerçekleşir. Kredisi alma örnek verilebilir. İlgili ay kapatıldıktan sonra değişiklikleri ay ortaya çıkabilir. Değişiklikler yeniden hesaplama bulut hizmet sağlayıcınız tarafından yapılan her. Cloudyn, tüm ayarlarını hesaplanır emin olmak için geçmiş verilerini güncelleştirir. Ayrıca maliyetleri doğru içinde görüntülendiğini doğrular, bildirir.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Nasıl doğrudan bir CSP dolaylı CSP müşterileri veya iş ortakları için Cloudyn erişimi yapılandırabilirsiniz?

Yönergeler için bkz. [Cloudyn 'de dolaylı CSP erişimini yapılandırma](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>Görüntülenecek iyileştirici menü öğesi nedeni nedir?

Azure Resource Manager erişim ekledikten ve veriler toplandıktan sonra, **iyileştirici** seçeneğini görmeniz gerekir. Azure Resource Manager erişimi etkinleştirmek için bkz. [Azure kimlik bilgileriyle etkinleştirilmemiş hesapları etkinleştirme nasıl yaparım?](#how-do-i-activate-unactivated-accounts-with-azure-credentials) .

## <a name="is-cloudyn-agent-based"></a>Cloudyn Aracısı bağlıdır?

Hayır. Aracıları kullanılmaz. Microsoft Insights API'den VM'ler için Azure sanal makine ölçüm verileri toplanır. Azure sanal makinelerinden ölçüm verilerini toplamak istiyorsanız, tanılama ayarları etkin olması gerekir.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Cloudyn raporlarını, rapor başına birden fazla AD Kiracı göstermemesidir?

Evet. Sahip olduğunuz her AD kiracısı için [karşılık gelen bir bulut hesabı varlığı oluşturabilirsiniz](tutorial-user-access.md#create-and-manage-entities) . Ardından, tüm Azure AD Kiracı verilerinizi ve Amazon Web Services ve Google Cloud Platform dahil diğer bulut platform sağlayıcıları görüntüleyebilirsiniz.
