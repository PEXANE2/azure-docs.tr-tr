---
title: Microsoft Commercial Market için SaaS teklifi planlaması
description: Microsoft AppSource, Azure Market 'te listeleme veya satma için yeni bir hizmet olarak yazılım (SaaS) teklifi planlaması veya Microsoft Iş Ortağı Merkezi 'nde ticari Market programını kullanan bulut çözümü sağlayıcısı (CSP) programı.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/30/2020
ms.openlocfilehash: 382a6056076179be0d25e0fee0d55b978a3b7169
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89420447"
---
# <a name="how-to-plan-a-saas-offer-for-the-commercial-marketplace"></a>Ticari Market için SaaS teklifi planlaması

Bu makalede, Microsoft Commercial Market 'e hizmet olarak yazılım (SaaS) teklifi yayımlamaya yönelik farklı seçenekler ve gereksinimler açıklanmaktadır. SaaS teklifleri, bireysel bilgisayarlara yerel yükleme yerine çevrimiçi bir abonelik aracılığıyla müşterilerinize yazılım çözümleri sunmanıza ve lisanslayamenize imkan tanır. Bu makale, Iş Ortağı Merkezi ile ticari Market 'te yayımlama teklifinizi hazırlamanıza yardımcı olur.

## <a name="listing-options"></a>Listeleme seçenekleri

Yeni bir SaaS teklifi yayımlamaya hazırlanırken, hangi _liste_ seçeneğinin seçileceğine karar vermeniz gerekir. Bu, Iş Ortağı Merkezi 'nde teklifinizi oluştururken sağlamanız gereken ek bilgileri saptayacaktır. **Teklif kurulumu** sayfasında, [ticari Market 'te SaaS teklifi oluşturma](create-new-saas-offer.md)bölümünde açıklandığı gibi liste seçeneğinizi tanımlayacaksınız.

Aşağıdaki tabloda, ticari Market 'teki SaaS teklifleri için listeleme seçenekleri gösterilmektedir.

| Listeleme seçeneği | İşlem işlemi |
| ------------ | ------------- |
| Benimle iletişim kurun | Müşteri, listeinizdeki bilgilerden doğrudan iletişim kurar.``*`` |
| Ücretsiz deneme sürümü | Müşteri, Azure Active Directory (Azure AD) aracılığıyla hedef URL 'nize yönlendirilir.``*`` |
| Şimdi alın (ücretsiz) | Müşteri, Azure AD aracılığıyla hedef URL 'nize yönlendirilir.``*`` |
| Microsoft ile satış  | Microsoft ile satılan tekliflere _transactable_ teklifleri denir. Transactable bir teklif, Microsoft 'un yayımcı adına bir yazılım lisansı için para değişimini kolaylaştıran bir tekliftir. Seçtiğiniz fiyatlandırma modelini kullanarak SaaS teklifleri faturyoruz ve müşteri işlemlerini sizin adınıza yönetirsiniz. Azure altyapı kullanım ücretlerinin size, iş ortağının doğrudan ücretlendirdiğini unutmayın. Fiyatlandırma modelinizdeki altyapı maliyetlerini hesaba almalısınız. Bu, aşağıdaki [SaaS faturalandırmasıyla](#saas-billing) daha ayrıntılı olarak açıklanmıştır.  |
|||

``*`` Yayımcılar, sipariş, karşılama, ölçüm, faturalandırma, faturalama, ödeme ve koleksiyon dahil olmak üzere, yazılım lisans işleminin tüm yönlerini desteklemekten sorumludur.

Bu liste seçenekleri hakkında daha fazla bilgi için bkz. [ticari Market Transact özellikleri](marketplace-commercial-transaction-capabilities-and-considerations.md).

Teklifiniz yayımlandıktan sonra teklifiniz için seçtiğiniz listeleme seçeneği, teklifinizin liste sayfasının sol üst köşesinde bir düğme olarak görünür. Örneğin, aşağıdaki ekran görüntüsünde, **benimle Iletişim kurun** ve **sürücü sına** düğmeleriyle Azure Marketi 'nde bir teklif listesi sayfası gösterilmektedir.

![Çevrimiçi mağazadaki bir teklif listesini gösterir.](./media/listing-options.png)

## <a name="technical-requirements"></a>Teknik gereksinimler

Teknik gereksinimler, teklifiniz için seçtiğiniz listeleme seçeneğine göre farklılık gösterir.

_Benimle iletişim_ listeleme seçeneğinin hiçbir teknik gereksinimi yoktur. Müşteri adaylarını yönetmek için bir CRM sistemini bağlama seçeneğiniz vardır ve bu makalede daha sonra bu makalenin ilerleyen bölümlerinde [Müşteri](#customer-leads) adayları bölümünde açıklanan müşteri adaylarını yönetebilirsiniz.

_Şimdi al (ücretsiz)_, _ücretsiz deneme_ve Microsoft listeleme seçenekleri _üzerinden satış_ aşağıdaki teknik gereksinimlere sahiptir:

- SaaS uygulamanız çok kiracılı bir çözüm olmalıdır.
- Kullanıcıların kimliğini doğrulamak için hem Microsoft hesaplarını (MSA) hem de [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) etkinleştirebilirsiniz.
- Bir giriş sayfası oluşturmanız gerekir. Bir Kullanıcı teklifinizi satın aldıktan sonra, gereken ek sağlama veya kurulum işlemlerini kolaylaştırmak için giriş sayfasına yönlendirilir. Giriş sayfasını oluşturma hakkında yönergeler için şu makalelere bakın:
  - [Ticari Market 'te transactable SaaS teklifinizin giriş sayfasını oluşturun](azure-ad-transactable-saas-landing-page.md)
  - [Ticari Market 'te ücretsiz veya deneme SaaS teklifinizin giriş sayfasını oluşturun](azure-ad-free-or-trial-landing-page.md)

Bu ek teknik gereksinimler yalnızca Microsoft 'a _Satış_ (transactable) listesi seçeneği için geçerlidir:

- Çoklu oturum açma (SSO) kimlik yönetimi ve kimlik doğrulaması ile Azure AD gereklidir. Ayrıntılı kılavuz için bkz. [ticari Market 'Te Azure AD ve transactable SaaS teklifleri](azure-ad-saas.md).
- Azure Marketi ve Microsoft AppSource tümleştirme için [SaaS karşılama API 'lerini](./partner-center-portal/pc-saas-fulfillment-api-v2.md) kullanmanız gerekir. Bir kullanıcı hesabı ve hizmet planı oluşturmak, güncelleştirmek ve silmek için SaaS aboneliğiyle etkileşime girebilen bir hizmeti kullanıma sunabilmeniz gerekir. Kritik API değişikliklerinin 24 saat içinde desteklenmesi gerekir. Kritik olmayan API değişiklikleri düzenli olarak serbest bırakılır. Toplanan alanların kullanımını açıklayan diyagramlar ve ayrıntılı açıklamalar [API 'ler](./partner-center-portal/pc-saas-fulfillment-api-v2.md)için belgelerde bulunur.
- Teklifiniz için en az bir plan oluşturmanız gerekir. Planınız, yayımlamadan önce seçtiğiniz fiyatlandırma modeline göre fiyatlandırılır: _düz fiyat_ veya _Kullanıcı başına_. [Planlar](#plans) hakkında daha fazla ayrıntı bu makalenin ilerleyen kısımlarında verilmiştir.
- Müşteri teklifinizi dilediğiniz zaman iptal edebilir.

### <a name="technical-information"></a>Teknik bilgiler

Transactable teklifi oluşturuyorsanız, **Teknik yapılandırma** sayfası için aşağıdaki bilgileri toplamanız gerekir. Transactable teklifi oluşturmak yerine işlemleri bağımsız olarak işlemeyi tercih ederseniz, bu bölümü atlayın ve [test sürücüleri](#test-drives)' ne gidin.

- **Giriş sayfası URL 'si**: `https://contoso.com/signup` kullanıcının ticari Market 'ten teklifinizi aldıktan sonra, yeni oluşturulan SaaS aboneliğinden yapılandırma işlemini tetikleyen SaaS site URL 'si (örneğin:). Bu URL, etkileşimli kayıt sayfanız için sağlama ayrıntılarını almak üzere karşılama API 'Lerini çağırmak için kullanılabilecek bir belirteç alır.

  Bu URL, belirli müşterinin SaaS satın alımını benzersiz bir şekilde tanımlayan Market satın alma tanımlama belirteci parametresi ile çağırılır. Bu belirteci, [Çözümle API](./partner-center-portal/pc-saas-fulfillment-api-v2.md#resolve-a-purchased-subscription)'sini kullanarak ilgili SaaS aboneliği ayrıntıları için değiş tokuş etmeniz gerekir. Bu ayrıntılar ve toplamak istediğiniz tüm diğerleri, müşteri kaydını tamamlamaya ve satın alımlarını etkinleştirmeye yönelik deneyiminize yerleştirilmiş bir müşteri etkileşimli Web sayfasının parçası olarak kullanılmalıdır. Bu sayfada, Kullanıcı Azure Active Directory (Azure AD) kullanarak tek tıklamayla kimlik doğrulamasından kaydolmalıdır.

  Bu URL, Market satın alma tanımlama belirteci parametresi, müşteri Azure portal veya M365 yönetim merkezinden yönetilen SaaS deneyimi başlattığında de çağrılır. Her iki akışı da işlemeniz gerekir: belirteç yeni bir müşteri satın alma sonrasında ilk kez sağlandığında ve SaaS çözümünü yöneten mevcut bir müşteri için yeniden sağlandığında.

    Yapılandırdığınız giriş sayfası 24/7 olmalıdır ve çalışıyor olmalıdır. Bu, ticari Market 'te sunulan SaaS tekliflerinizin yeni satınalmaları veya bir teklifin etkin bir aboneliği için yapılandırma istekleri hakkında bildirim almanın tek yoludur.

- **Bağlantı Web kancası**: Microsoft 'un size gönderilmesi gereken tüm zaman uyumsuz olaylar için (örneğin, bir SaaS aboneliği iptal edildiğinde), bağlantı Web kancası URL 'si sağlamanız gerekir. Bu URL 'YI olay üzerinde bilgilendirmek için çağıracağız.

  Sağladığınız Web kancası, ticari Market aracılığıyla satın alınan müşterilerinizin SaaS abonelik24/7 leriyle ilgili güncelleştirmeler hakkında bildirim almanız gereken tek yöntemdir.

  > [!NOTE]
  > Azure portal içinde, iki hizmetimiz arasındaki bağlantının kimliğini doğrulamak için bir Azure Uygulama KIMLIĞININ kullanılmasını sağlamak üzere tek kiracılı bir [Azure Active Directory (Azure AD) uygulaması](../active-directory/develop/howto-create-service-principal-portal.md) oluşturmanız gerekir. [KIRACı kimliğini](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)bulmak için Azure Active Directory gidin ve **Özellikler**' i seçin, sonra listelenen dizin kimliği numarasını bulun. Örneğin, `50c464d3-4930-494c-963c-1e951d15360e`.

- **Azure Active Directory KIRACı kimliği**: (dizin kimliği olarak da bilinir). Azure portal içinde, bu uygulamayı çağırma yetkiniz olduğundan emin olmak için API 'nin erişim denetim listesine (ACL) ekleyebilmemiz için [bir Azure Active Directory (ad) uygulamasını kaydetmeniz](../active-directory/develop/howto-create-service-principal-portal.md) gerekir. Azure Active Directory (AD) uygulamanıza ait kiracı KIMLIĞINI bulmak için Azure Active Directory [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) dikey penceresine gidin. **Görünen ad** sütununda, uygulamayı seçin. Ardından listelenen **Dizin (kiracı) kimliği** numarasını (örneğin, `50c464d3-4930-494c-963c-1e951d15360e` ) arayın.

- **Uygulama kimliği Azure Active Directory**: [Uygulama Kimliğiniz](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)da gerekir. Değerini almak için Azure Active Directory [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) dikey penceresine gidin. **Görünen ad** sütununda, uygulamayı seçin. Ardından listelenen uygulama (istemci) KIMLIĞI numarasını (örneğin, `50c464d3-4930-494c-963c-1e951d15360e` ) arayın.

  Azure AD uygulama KIMLIĞI, Iş Ortağı Merkezi hesabınızdaki yayımcı KIMLIĞINIZLE ilişkilidir. Bu hesaptaki tüm teklifler için aynı uygulama KIMLIĞINI kullanmanız gerekir.

  > [!NOTE]
  > Yayımcının Iş Ortağı Merkezi 'nde iki veya daha fazla farklı hesabı varsa, her biri hesaplarından biri için iki veya daha fazla farklı Azure AD uygulama kimliği kullanılmalıdır. Iş Ortağı Merkezi 'ndeki her iş ortağı hesabı, bu hesap aracılığıyla yayınlanan tüm SaaS teklifleri için benzersiz bir Azure AD uygulama KIMLIĞI kullanmalıdır.

## <a name="test-drives"></a>Sınama sürücüleri
SaaS uygulamanız için bir test sürücüsü etkinleştirmeyi seçebilirsiniz. Test sürücüleri, müşterilere sabit sayıda saat boyunca önceden yapılandırılmış bir ortama erişim sağlar. Herhangi bir yayımlama seçeneği için test sürücüleri etkinleştirebilirsiniz, ancak bu özellik ek gereksinimlere sahiptir. Sınama sürücüleri hakkında daha fazla bilgi için bkz. [test sürücüsü nedir?](what-is-test-drive.md). Farklı türlerde test sürücüleri yapılandırma hakkında daha fazla bilgi için bkz. [Test Drive Technical Configuration](test-drive-technical-configuration.md).

> [!TIP]
> Bir test sürücüsü, [ücretsiz deneme](plans-pricing.md#free-trials)sürümünden farklıdır. Bir sınama sürücüsü, ücretsiz deneme veya her ikisini birden sağlayabilirsiniz. Bunlar her ikisi de müşterilerinizi sabit bir süre için sağlar. Ancak, bir sınama sürücüsü Ayrıca, ürününüzün temel özelliklerine ve avantajlarına yönelik uygulamalı, kendinden kılavuzlu bir tura sahip olan gerçek hayatta bir uygulama senaryosunda gösterilmekte olan avantajlı bir geliştirme içerir.

## <a name="customer-leads"></a>Müşteri liderleri

Müşteri bilgilerini toplamak için teklifinizi müşteri ilişkileri yönetimi (CRM) sisteminize bağlamanız gerekir. Müşterinin, bilgilerini paylaşması için izin istenir. Teklif adı, KIMLIĞI ve çevrimiçi mağazalarla birlikte bu müşteri ayrıntıları, yapılandırdığınız CRM sistemine gönderilir. Ticari Market, Azure tablosu kullanma veya Power otomatikleştirmeyi kullanarak bir HTTPS uç noktası yapılandırma seçeneğiyle birlikte çeşitli CRM sistemlerini destekler.

Teklif oluşturma sırasında veya sonrasında bir CRM bağlantısını dilediğiniz zaman ekleyebilir veya değiştirebilirsiniz. Ayrıntılı kılavuz için bkz. [ticari Market Için lider yönetimi](lead-management-for-cloud-marketplace.md).

## <a name="selecting-an-online-store"></a>Çevrimiçi mağaza seçme

SaaS teklifini yayımladığınızda, Microsoft AppSource, Azure Marketi veya her ikisinde de listelenir. Her çevrimiçi mağaza, benzersiz müşteri gereksinimlerine hizmet eder. AppSource iş çözümleri içindir ve Azure Marketi BT çözümleri içindir. Teklif türü, Transact özellikleri ve kategorileriniz, teklifinizin nerede yayımlanacağını belirleyecektir. Kategoriler ve alt kategoriler, çözüm türüne göre her bir çevrimiçi mağazaya eşlenir. Çevrimiçi mağaza seçme hakkında ayrıntılı bilgi için bkz. [çevrimiçi mağaza seçme](determine-your-listing-type.md#selecting-an-online-store).

## <a name="legal-contracts"></a>Yasal sözleşmeler

Müşteriler için satın alma sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için, Microsoft, ticari Market 'teki tekliflerinizi kullanabileceğiniz standart bir sözleşme sunar. Yazılımınızı standart sözleşme kapsamında sunışınızda, müşterilerin yalnızca bir kez okuyup kabul etmesi ve özel hüküm ve koşullar oluşturmanız gerekmez.

Standart sözleşmeyi kullanmayı seçerseniz, standart sözleşmeye genel değişiklik koşullarını ve en fazla 10 özel değişikliği ekleme seçeneğiniz vardır. Standart sözleşme yerine kendi hüküm ve koşullarınızı da kullanabilirsiniz. Bu ayrıntıları **Özellikler** sayfasında yöneteceksiniz. Ayrıntılı bilgi için bkz. [Microsoft Commercial Market Için standart sözleşme](standard-contract.md).

> [!NOTE]
> Ticari Market için standart sözleşmeyi kullanarak bir teklifi yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Bu bir "veya" senaryosudur. Çözümünüzü standart sözleşme kapsamında ya da kendi hüküm ve koşullarınız için sunun. Standart sözleşmenin şartlarını değiştirmek istiyorsanız, standart sözleşme değişiklikleri aracılığıyla bunu yapabilirsiniz.

## <a name="offer-listing-details"></a>Teklif listeleme ayrıntıları

Iş Ortağı Merkezi 'nde [Yeni bir SaaS teklifi oluşturduğunuzda](create-new-saas-offer.md) , **teklif listesi** sayfasına metin, görüntü, isteğe bağlı videolar ve diğer ayrıntıları girersiniz. Bu, aşağıdaki örnekte gösterildiği gibi, müşterilerin, ticari Market 'te teklif listelerinizi bulduklarında göreceği bilgiler.

:::image type="content" source="./media/example-saas-1.png" alt-text="Bu teklifin Microsoft AppSource nasıl göründüğünü gösterir.":::

**Çağrı açıklamaları**

1. Logo
2. Kategoriler
3. Sektörler
4. Destek adresi (bağlantı)
5. Kullanım koşulları
6. Gizlilik ilkesi
7. Teklif adı
8. Özet
9. Description
10. Ekran görüntüleri/videolar
11. Belgeler

Aşağıdaki örnekte Azure portal bir teklif listesi gösterilmektedir.

![Azure portal bir teklif listesi gösterir.](./media/example-managed-services.png)

**Arama açıklamaları**

1. Başlık
1. Açıklama
1. Yararlı bağlantılar
1. Ekran görüntüleri

> [!NOTE]
> Teklif açıklamasının "Bu uygulama yalnızca [Ingilizce olmayan dil]" ifadesi ile başlıyorsa, teklif listesi içeriğinin Ingilizce olması gerekmez.

Teklifinizi daha kolay bir şekilde oluşturmaya yardımcı olmak için bu öğelerin bazılarını zaman önce hazırlayın. Aksi belirtilmediği takdirde, aşağıdaki öğeler gereklidir.

- **Ad**: Bu ad, ticari Market 'teki teklif listelerinizin başlığı olarak görünür. Ad trademarked olabilir. Bu, emojıs (ticari marka ve telif hakkı sembolleri olmadıkları müddetçe) içeremez ve 50 karakterle sınırlı olmalıdır.
- **Arama sonuçları Özeti**: teklifinizin amacını veya işlevini 100 karakter veya daha az satır sonu olmadan tek bir cümle olarak tanıtın. Bu Özet, ticari Market listeleme arama sonuçlarında kullanılır.
- **Açıklama**: Bu açıklama ticari Market dökümüne genel bakış bölümünde görüntülenecektir. Bir değer teklifi, önemli avantajlar, hedeflenen kullanıcı tabanı, herhangi bir kategori ya da sektör ilişkilendirmesi, uygulama içi satın alma fırsatları, gerekli ön kapanışlar ve daha fazla bilgi edinmek için bir bağlantı dahil etmeyi göz önünde bulundurun.
    
    Bu metin kutusunda, açıklamanızı daha ilgi çekici hale getirmek için kullanabileceğiniz zengin metin düzenleyici denetimleri vardır. Ayrıca, açıklamanızı biçimlendirmek için HTML etiketlerini de kullanabilirsiniz. Bu kutuya HTML biçimlendirmesi dahil olmak üzere en fazla 3.000 karakter girebilirsiniz. Ek ipuçları için bkz. [harika bir uygulama açıklaması yazma](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description).

- Başlarken **yönergeleri**: teklifinizi Microsoft (transactable teklifini) ile satmayı seçerseniz, bu alan gereklidir. Bunlar, müşterilerin SaaS teklifinizin bağlanmasına yardımcı olan yönergelerdir. Daha ayrıntılı çevrimiçi belgelere en çok 3.000 karakter ve bağlantı ekleyebilirsiniz.
- **Anahtar sözcükleri ara** (isteğe bağlı): müşterilerin Market 'te teklifinizi bulmak için kullanabileceği en fazla üç arama anahtar sözcüğü sağlayın. Teklif **adı** ve **açıklaması**dahil etmeniz gerekmez: Bu metin, aramaya otomatik olarak eklenir.
- **Gizlilik ilkesi bağlantısı**: şirketinizin gizlilik ilkesinin URL 'si. Geçerli bir gizlilik ilkesi sağlamanız gerekir ve uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan sorumludur.
- **İletişim bilgileri**: aşağıdaki kişileri kuruluşunuzdan atamanız gerekir:
  - **Destek kişisi**: müşterileriniz sorguları açtıklarında kullanılacak Microsoft iş ortakları için ad, telefon ve e-posta adresi sağlayın. Destek Web sitenizin URL 'sini de eklemeniz gerekir.
  - **Mühendislik ilgili kişisi**: Teklifinizle ilgili sorunlar olduğunda, Microsoft 'un doğrudan kullanacağı adı, telefonu ve e-postayı belirtin. Bu iletişim bilgileri Market 'te listelenmez.
  - **CSP program kişisi** (isteğe bağlı): CSP programını kabul ediyorsanız ad, telefon ve e-posta sağlayın. bu nedenle, söz konusu iş ortakları sizinle herhangi bir soru ile iletişim kurabilmesidir. Pazarlama malzemelerinize da bir URL ekleyebilirsiniz.
- **Faydalı bağlantılar** (isteğe bağlı): teklifinizin kullanıcıları için çeşitli kaynaklara bağlantılar sağlayabilirsiniz. Örneğin, forumlar, SSS ve sürüm notları.
- **Destekleyici belgeler**: teknik incelemeler, broşürler, denetim listeleri veya PowerPoint sunuları gibi en fazla üç müşteriye yönelik belge sağlayabilirsiniz.
- **Medya – logolar**: **büyük** boyutlu amblem için bir PNG dosyası sağlayın. İş Ortağı Merkezi, **küçük** ve **Orta ölçekli** bir amblem oluşturmak için bunu kullanacaktır. İsterseniz bunları daha sonra farklı görüntülerle değiştirebilirsiniz.

   - Büyük (216 x 216-350 x 350 px, gereklidir)
   - Orta (90 x 90 piksel, isteğe bağlı)
   - Küçük (48 x 48 piksel, isteğe bağlı)

  Bu amblemler çevrimiçi mağazalarda farklı yerlerde kullanılır:

  -  Küçük logo Azure Marketi arama sonuçlarında Microsoft AppSource ve ana sayfa ve arama sonuçları sayfasında görünür.
  -  Orta logo, Microsoft Azure yeni bir kaynak oluşturduğunuzda görüntülenir.
  -  Büyük logo, Azure Marketi 'nde teklif listesi sayfanızda ve Microsoft AppSource görünür.

- **Medya-ekran görüntüleri**: teklifinizin nasıl çalıştığını gösteren aşağıdaki gereksinimlere sahip en az bir ve en fazla beş ekran görüntüsü eklemeniz gerekir:
  - 1280 x 720 piksel
  - . png dosyası
  - Bir resim yazısı içermelidir
- **Medya-videolar** (isteğe bağlı): aşağıdaki gereksinimlere sahip en fazla dört video ekleyebilirsiniz, bu da teklifinizi gösterir:
  - Name
  - URL: yalnızca YouTube veya Vimeo 'da barındırılmalıdır.
  - Küçük resim: 1280 x 720. png dosyası

> [!Note]
> Teklifinizin, ticari Market 'te yayımlanacak genel [ticari Market sertifika ilkelerini](https://docs.microsoft.com/legal/marketplace/certification-policies#100-general) ve [hizmet ilkesi olarak yazılımlarını](https://docs.microsoft.com/legal/marketplace/certification-policies#1000-software-as-a-service-saas) karşılaması gerekir.

## <a name="preview-audience"></a>İzleyiciyi Önizle
Önizleme hedef kitlesi, canlı yayımlamadan önce uçtan uca işlevselliği test etmek için çevrimiçi mağazalarda canlı olarak yayınlanmadan önce teklifinizin erişimine açabilir. **Izleyiciyi Önizle** sayfasında, sınırlı bir önizleme hedef kitlesi tanımlayabilirsiniz. Teklifinizi Microsoft aracılığıyla satma yerine işlemleri bağımsız olarak işlemeyi tercih ediyorsanız bu ayar kullanılamaz. Bu durumda, bu bölümü atlayabilir ve [ek satış fırsatlarına](#additional-sales-opportunities)gidebilirsiniz.

> [!NOTE]
> Önizleme hedef kitlesi özel bir plandan farklıdır. Özel bir plan yalnızca seçtiğiniz belirli bir hedef kitle için kullanılabilir hale getirir. Bu, belirli müşterilerle özel bir plana anlaşma yapmanızı sağlar. Daha ayrıntılı bilgi için sonraki bölüme bakın: planlar.

Davetleri Microsoft hesabı (MSA) veya Azure Active Directory (Azure AD) e-posta adreslerine gönderebilirsiniz. 10 ' a kadar e-posta adresi ekleyin veya bir. csv dosyası ile 20 ' ye kadar içeri aktarın. Teklifiniz zaten canlı ise, teklifiniz için herhangi bir değişikliği veya güncelleştirmeyi test etmek üzere bir önizleme hedef kitlesi tanımlayabilirsiniz.

## <a name="plans"></a>Planlar

Transactable teklifler en az bir plan gerektirir. Bir plan, çözüm kapsamını ve sınırlarını ve ilişkili fiyatlandırmayı tanımlar. Müşterilerinize farklı teknik ve fiyatlandırma seçenekleri sunmak için teklifiniz için birden çok plan oluşturabilirsiniz. İşlemleri bir transactable teklifi oluşturmak yerine bağımsız olarak işlemeyi tercih ederseniz, **planlar** sayfası görünür olmaz. Bu durumda, bu bölümü atlayın ve [ek satış fırsatlarına](#additional-sales-opportunities)gidin.

Fiyatlandırma modelleri, ücretsiz denemeler ve özel planlar dahil olmak üzere planlar hakkında genel rehberlik teklifleri için bkz. [planlar ve fiyatlandırma](plans-pricing.md) . Aşağıdaki bölümlerde SaaS tekliflerine özgü ek bilgiler ele alınmaktadır.

### <a name="saas-pricing-models"></a>SaaS fiyatlandırma modelleri

SaaS teklifleri, her plan ile iki fiyatlandırma modelinden birini kullanabilir: _sabit fiyat_ veya _Kullanıcı başına_. Aynı teklifinizin tüm planların aynı fiyatlandırma modeliyle ilişkilendirilmesi gerekir. Örneğin, bir teklifin sabit fiyat ve Kullanıcı başına başka bir plan olan bir plana sahip olması gerekir.

**Düz** Fiyat: tek bir aylık veya yıllık sabit fiyat fiyatı ile teklifinizin erişimini etkinleştirin. Bu, bazen site tabanlı fiyatlandırma olarak adlandırılır. Bu fiyatlandırma modeliyle isteğe bağlı olarak, sabit fiyat kapsamında olmayan kullanımlar için müşterileri ücretlendirmede Market ölçüm hizmeti API 'sini kullanan tarifeli planlar tanımlayabilirsiniz. Tarifeli faturalandırma hakkında daha fazla bilgi için bkz. [ticari Market ölçüm hizmeti kullanılarak SaaS Için ölçülen faturalandırma](./partner-center-portal/saas-metered-billing.md). SaaS hizmetiniz için kullanım davranışı bursts 'de ise de bu seçeneği kullanmanız gerekir.

**Kullanıcı başına** – teklifine erişebilen veya hiç oturmayan kullanıcı sayısına bağlı olarak teklifiniz için fiyata erişimi etkinleştirin. Bu Kullanıcı tabanlı modelle, plan tarafından desteklenen en düşük ve en fazla kullanıcı sayısını ayarlayabilirsiniz. Kullanıcı sayısına göre farklı fiyat noktalarını yapılandırmak için birden çok plan oluşturabilirsiniz. Bu alanlar isteğe bağlıdır. Sol tarafta seçili değilse, Kullanıcı sayısı bir sınıra sahip değildir (en az 1 ve en fazla, hizmetinizin destekleyebiliyor) olarak yorumlanır. Bu alanlar, planınızdaki bir güncelleştirmenin parçası olarak düzenlenebilir.

> [!IMPORTANT]
> Teklifiniz yayımlandıktan sonra, fiyatlandırma modelini değiştiremezsiniz. Ayrıca, aynı teklif için tüm planlar aynı fiyatlandırma modelini paylaşmalıdır.

### <a name="saas-billing"></a>SaaS faturalandırma

(Yayımcının) Azure aboneliğinde çalışan SaaS uygulamaları için altyapı kullanımı doğrudan size faturalandırılır; müşteriler, gerçek altyapı kullanım ücretlerini görmez. Çözümü çalıştırmak için dağıttığınız altyapının maliyetini dengelemek için, Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmasına paketlemelidir.

SaaS uygulaması, Microsoft tarafından aylık veya yıllık faturalandırmaya, Kullanıcı başına veya [ölçümlü faturalandırma hizmeti](./partner-center-portal/saas-metered-billing.md)kullanılarak sunulan tüketim ücretlerine dayalı olarak satılan bir ücret karşılığında satılabilir. Ticari Market, yayımcıların fiyatları, Microsoft ürün müşterileri ve Microsoft 'un bir kurum ücretini stopajına göre gelir ödediği bir kurum modeli üzerinde çalışır.

Bu, kurumlar modelini göstermek için maliyetlerin ve ödemlerden oluşan örnek bir dökümdir. Bu örnekte, Microsoft, yazılım lisansınızın $100,00 ' i müşteriye faturalandırır ve Yayımcı için $80,00 ' i ödeder.

| Lisans maliyetiniz | $100/ay |
| ------------ | ------------- |
| Azure kullanım maliyeti (D1/1-çekirdek) | Müşteriye değil doğrudan yayımcıya faturalandırılır |
| Müşteri Microsoft tarafından faturalandırılır | aylık $100,00 (yayımcı, lisans ücretindeki herhangi bir tahakkuk eden veya geçiş altyapı maliyeti için hesap olmalıdır) |
| **Microsoft faturaları** | **$100/ay** |
| Microsoft, lisans maliyetinizi %80 oranında ödetir<br>`*` Microsoft, tam SaaS uygulamaları için lisans maliyetlerinizin %90 ' ını öder| $80,00/ay<br>``*`` $90,00/ay |
|||

** `*` Daha az Market servis ücreti** – Microsoft 'un ticari Market 'te yayımladığı belirli SaaS teklifleri için, Microsoft 'un Market hizmet ücretini %20 ' den (Microsoft Publisher anlaşmasında açıklandığı gibi) %10 ' a azaltacak. Tekliflerinizin uygun olmasını sağlamak için teklifinizin Microsoft tarafından Azure IP ortak satışı incentivized olarak belirlenmiş olması gerekir. Uygunluk, ay için en az Market servis ücretini almak üzere her bir takvim ayının sonundan önce en az beş (5) iş gününe ulaşılmalıdır. Daha düşük Market hizmet ücreti, Azure IP ortak satış incentivized VM 'Leri, yönetilen uygulamalar ve ticari Market aracılığıyla sunulan diğer tüm nitelikli transactable IaaS teklifleri için de geçerlidir.

## <a name="additional-sales-opportunities"></a>Ek satış fırsatları

Microsoft tarafından desteklenen pazarlama ve satış kanallarını kabul edebilirsiniz. Iş Ortağı Merkezi 'nde teklifinizi oluştururken işlemin sonuna doğru iki sekme görürsünüz:

- **CSP 'ler aracılığıyla yeniden Satım**: Microsoft bulut çözüm SAĞLAYıCıLARıNıN (CSP) iş ortaklarının, çözümünüzü bir paketlenmiş teklifin parçası olarak yeniden satmasına izin vermek için bu seçeneği kullanın. Daha fazla bilgi için bkz. [bulut çözümü sağlayıcısı programı](cloud-solution-providers.md) .

- **Microsoft Ile ortak satış**: Bu seçenek, Microsoft satış ekiplerinin, müşterilerinin IHTIYAÇLARıNı değerlendirirken IP ortak satış için uygun çözümünüzü kabul etmenize olanak tanır. Teklifinizi değerlendirme için hazırlama hakkında ayrıntılı bilgi edinmek için [Iş Ortağı Merkezi 'Nde ortak satış seçeneğine](./partner-center-portal/commercial-marketplace-co-sell.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market 'te SaaS teklifi oluşturma](create-new-saas-offer.md)
- [En iyi teklif listeleme deneyimleri](gtm-offer-listing-best-practices.md)
