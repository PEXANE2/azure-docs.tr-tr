---
title: Müşteri müşteri adaylarını yapılandırma | Azure Marketi
description: Ticari pazarda müşteri müşteri adaylarını yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 05b166b2ec46900d9e3972025efb581d9619ec6a
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81252678"
---
# <a name="customer-leads-from-your-marketplace-offer"></a>Market teklifinizden müşteri adayları

Müşteri adayları, tekliflerinizi [Azure Marketi'nden](https://azuremarketplace.microsoft.com) veya [AppSource'dan](https://appsource.microsoft.com)ilgilenen veya dağıtan müşterilerdir. Teklifiniz pazara yayınlandığında müşteri müşteri adayları alacaksınız. Bu makalede açıklayacağız:

* Pazar yeri teklifinizin müşteri müşteri adaylarını nasıl oluşturur, iş fırsatlarını kaçırmamanızı sağlar. 
* Müşteri adaylarınızı tek bir merkezi konumda yönetebilmeniz için CRM'nizi teklifinize bağlayın.
* Size gönderdiğimiz müşteri verilerini anlayın, böylece size ulaşan müşterileri takip edebilirsiniz.

## <a name="generate-customer-leads"></a>Müşteri müşteri adayları oluşturma

Müşteri adayının oluşturulduğu yerler şunlardır:

1. Bir müşteri pazardan "Bana Ulaşın" seçeneğini seçtikten sonra bilgilerini paylaşmayı kabul ettiğinde. Bu müşteri, ürününüzü almakla ilgilendiğini ifade eden müşteri hakkındaki bilgileri paylaştığımız bir **başlangıç müşteri** adayıdır. Kurşun, satın alma hunisinin en üst kısmıdır.

      ![Dynamics 365 Bana Ulaşın](./media/commercial-marketplace-get-customer-leads/dynamics-365-contact-me.png)

2. Bir müşteri teklifinizi almak için "Şimdi Al" veya "Oluştur" seçeneğini seçtiğinde [(Azure portalında)](https://portal.azure.com/)bu müşteri, ürününüzü dağıtmaya başlayan bir müşteri hakkında bilgi paylaştığımız etkin bir **müşteri dir.**

    ![SQL Şimdi Alın](./media/commercial-marketplace-get-customer-leads/sql-get-it-now.png)

    ![Windows Server Oluştur](./media/commercial-marketplace-get-customer-leads/windows-server-create.png)

3. Bir müşteri bir "Test Sürüşü" alır veya teklifinizin "Ücretsiz Deneme" sini başlatır. Test Sürüşleri veya ücretsiz denemeler, herhangi bir giriş engeli olmaksızın işinizi potansiyel müşterilerle anında paylaşmanız için hızlandırılmış fırsatlardır.

    ![Dynamics 365 Test Sürüşü](./media/commercial-marketplace-get-customer-leads/dynamics-365-test-drive.png)

    ![Dynamics 365 Test Sürüşü](./media/commercial-marketplace-get-customer-leads/dynamics-365-free-trial.png)

## <a name="connect-to-your-crm-system"></a>CRM sisteminize bağlanın

[!INCLUDE [Test drive content](./includes/connect-lead-management.md)]

## <a name="understand-lead-data"></a>Müşteri adayı verilerini anlama

Müşteri edinme işlemi sırasında aldığınız her müşteri adayının belirli alanlarda verileri vardır. Dikkat etmek için ilk alan `LeadSource` bu biçimi izleyen alandır: **Kaynak-Eylem** | **Teklifi**.

**Kaynaklar**: Bu alanın değeri, kurşunu oluşturan pazara göre doldurulur. Olası değerler `"AzureMarketplace"` `"AzurePortal"`, `"AppSource (SPZA)"`ve .

**Eylemler**: Bu alanın değeri, müşterinin pazarda yaptığı ve müşteriyi oluşturan eyleme göre doldurulur. 

Olası değerler şunlardır:

- "INS" -- Kurulum. Bu eylem, bir müşteri ürününüzü satın aldığında Azure Marketi veya AppSource'da yapılır.
- "PLT" -- Ortak Led Trial anlamına gelir. Bu eylem, bir müşteri Bana Ulaşın seçeneğini kullandığında AppSource'ta dır.
- "DNC" -- Temas etmeyin. Bu eylem, uygulama sayfanızda çapraz listelenen bir İş Ortağıyla iletişime geçilmesi istendiğinde AppSource'da yer alır. Bu müşterinin uygulamanızda çapraz listeye alındığına dair kafaları paylaşıyoruz, ancak bunlarla iletişime geçmelerine gerek yok.
- "Oluştur" -- Bu eylem yalnızca Azure portalının içindedir ve bir müşteri teklifinizi kendi hesabına satın aldığında oluşturulur.
- "StartTestDrive" -- Bu eylem yalnızca Test Sürücüleri içindir ve bir müşteri test sürüşüne başladığında oluşturulur.

**Teklifler**: Pazarda birden fazla teklif olabilir. Bu alanın değeri, müşteri adayını oluşturan teklife göre doldurulur. Publisher ID ve Teklif Kimliği bu alanda gönderilir ve teklifi pazara yayınladığınızda sağladığınız değerlerdir.

Aşağıdaki örnekler, örnek değerleri beklenen `publisherid.offerid`biçimde gösterir: 

1. `checkpoint.check-point-r77-10sg-byol`
1. `bitnami.openedxcypress`
1. `docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145`

## <a name="customer-info"></a>Müşteri Bilgileri

Müşterinin bilgileri birden çok alan üzerinden gönderilir. Aşağıdaki örnek, müşteri adayında bulunan müşteri bilgilerini gösterir.

- Ad soyad: John
- Soyadı: Smith
- E-posta:\@jsmith microsoft.com
- Telefon numarası: 1234567890
- Ülke: ABD
- Şirket: Microsoft
- Başlık: CTO

>[!Note]
>Önceki örnekteki tüm veriler her müşteri adayı için her zaman kullanılabilir değildir. Müşteri Adayları bölümünde belirtildiği gibi birden çok adımdan müşteri adayları alacağınız için, müşteri adaylarını işlemenin en iyi yolu kayıtları çoğaltmak ve takipleri kişiselleştirmektir. Bu şekilde her müşteri uygun bir ileti alıyor ve siz benzersiz bir ilişki oluşturuyorsunuz.

## <a name="best-practices-for-lead-management"></a>Lider yönetimi için en iyi uygulamalar

1. *İşlem* - Kilometre taşları, KPM'ler ve net ekip sahipliği yle net bir satış süreci tanımlayın.
2. *Nitelik* - Bir müşteri adayının tam olarak kalifiye olup olmadığını gösteren ön koşulları tanımlayın. Satış veya pazarlama temsilcilerinin müşteri adaylarını tam satış sürecine geçmeden önce dikkatli bir şekilde hak kazandığından emin olun.
3. *Takip -* Takip etmeyi unutmayın, tipik işlemin 5 ila 12 takip çağrısı gerektirmesini bekleyin
4. *Nurture* - Daha yüksek bir kar marjı yolunda almak için, yol yetiştirmek.

## <a name="leads-frequently-asked-questions"></a>Sık sorulan sorulara yol açar

### <a name="where-can-i-get-help-in-setting-up-my-lead-destination"></a>Müşteri adayımın ayarlanması nda nereden yardım alabilirim?

Belgeleri [burada](#connect-to-your-crm-system) bulabilir veya aka.ms/marketplacepublishersupport üzerinden bir destek bileti gönderebilir, ardından **'teklif oluşturma'** → **teklif türünü** → **'müşteri adayı yönetimi yapılandırması'nı seçebilirsiniz.**

### <a name="am-i-required-to-configure-a-lead-destination-in-order-to-publish-an-offer-on-the-marketplace"></a>Pazarda bir teklif yayınlamak için bir müşteri adayı nı yapılandırmam gerekiyor mu?

Yanıt, yayımladırdığınız tekliftürüne bağlıdır. Müşteri Katılımı için SaaS ve Dynamics 365 'Contact Me', tüm Dynamics 365 for Operations teklifleri, tüm Dynamics 365 Business Central teklifleri ve tüm Danışmanlık Hizmeti teklifleri bir lider hedefe bağlantı gerektirir. Teklif türünüz listelenmediyse, gerekli değildir. Ancak, iş fırsatlarını kaçırmamak için bir müşteri adayı hedefini yapılandırmanız önerilir.

### <a name="how-can-i-find-the-test-lead"></a>Test ipucunu nasıl bulabilirim?

Müşteri `"MSFT_TEST"` adayı hedefinizi arayın, microsoft'tan örnek bir test müşteri adayı aşağıda verilmiştir:

```
company = MSFT_TEST_636573304831318844
country = US
description = MSFT_TEST_636573304831318844
email = MSFT_TEST_636573304831318844@test.com
encoding = UTF-8
encoding = UTF-8
first_name = MSFT_TEST_636573304831318844
last_name = MSFT_TEST_636573304831318844
lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844|<Offer Name>
oid = 00Do0000000ZHog
phone = 1234567890
title = MSFT_TEST_636573304831318844
```

### <a name="i-have-a-live-offer-but-im-not-seeing-any-leads"></a>Canlı bir teklifim var ama bir ipucu göremiyorum.

Müşteri adayı hedefine bağlantınızın geçerli olduğundan emin olun. Partner Center'daki teklifinizi yayınladıktan sonra size bir test ipucu göndereceğiz. Test müşteri adayını görürseniz, bağlantı geçerlidir. Ayrıca, pazardaki girişte "şimdi al", "benimle iletişim edeyim" veya "ücretsiz deneme" seçeneğini tıklayarak, önizleme adımı sırasında teklif önizlemesini elde etmeye çalışarak müşteri adayı bağlantınızı test edebilirsiniz.

Ayrıca, doğru verileri aradığınızdan emin olun. Bu belgenin [Veri Verilerini Anla](#understand-lead-data) bölümündeki içerik, müşteri adayı hedefinize gönderdiğimiz müşteri adayı verilerini açıklar.

### <a name="i-have-configured-azure-blob-as-my-lead-destination-why-dont-i-see-the-lead"></a>Azure BLOB'u müşteri adayı olarak yapılandırıldım, neden müşteri adayını göremiyorum?

Azure Blob müşteri adayı hedefi artık desteklenmez, bu nedenle teklifiniz tarafından oluşturulan müşteri müşteri müşteri adaylarını kaçırırsınız. Diğer müşteri adayı [hedef seçeneklerinden](./commercial-marketplace-get-customer-leads.md)herhangi biri için geçin. 

### <a name="i-received-an-email-from-marketplace-why-cant-i-find-the-lead-in-my-crm"></a>Marketplace'ten bir e-posta aldım, crm'imde neden müşteri adayı bulamıyorum?

Son kullanıcının e-posta etki alanı .edu'dan olabilir. Gizlilik nedenleriyle, .edu etki alanından özel tanımlayıcı bilgiler geçirmeyiz. aka.ms/marketplacepublishersupport üzerinden destek bileti gönderin.

### <a name="i-have-configured-azure-table-as-my-lead-destination-how-can-i-view-the-leads"></a>Azure Tablosu'nu müşteri adayı olarak yapılandırıldım, müşteri adaylarını nasıl görüntüleyebilirim?

Azure portalından Azure Tablosu'nda depolanan müşteri adayı verilerine erişebilir veya Azure depolama hesabınızın tablo verilerini görüntülemek için [Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) ücretsiz olarak indirip yükleyebilirsiniz.

### <a name="i-have-configured-azure-table-as-my-lead-destination-can-i-get-notified-whenever-a-new-lead-is-sent-by-marketplace"></a>Azure Tablosu'nu müşteri adayım olarak yapılandırıldım, Market tarafından yeni bir müşteri adayı gönderildiğinde bilgilendirilebilir miyim?

Evet, [buradaki](./commercial-marketplace-lead-management-instructions-azure-table.md)belgelerde Azure Tablosuna bir müşteri adayı eklenirse e-posta gönderen bir Microsoft akışı ayarlamak için yönergeleri izleyin.

### <a name="i-have-configured-salesforce-as-my-lead-destination-why-cant-i-find-the-leads"></a>Satış Gücü'ni müşteri adayım olarak yapılandırıldım, neden müşteri adaylarını bulamıyorum?

"Web to lead" formunun bir seçim listesine dayalı zorunlu bir alan olup olmadığını kontrol edin. Evet ise, alanı zorunlu olmayan bir metin alanına geçin.

### <a name="there-was-an-issue-with-my-lead-destination-and-i-missed-some-leads-can-i-have-them-sent-to-me-in-an-email"></a>Benim kurşun hedef ile ilgili bir sorun vardı ve bazı yol cevapsız. Bunları bana bir e-posta yla göndertebilir miyim?

Özel tanımlanabilir bilgi ilkeleri nedeniyle, müşteri adayı bilgilerini güvenli olmayan e-posta yoluyla paylaşamayız.

### <a name="i-have-configured-azure-table-as-my-lead-destination-how-much-will-it-cost"></a>Müşteri adayım olarak Azure Tablosu'nu yapılandırıldım, ne kadara mal olur?

Kurşun gen verileri düşüktür (hemen hemen tüm yayıncılar için 1 GB <). Maliyet, bir ay içinde 1.000 müşteri adayı alınırsa, yaklaşık 50 sente mal olacak. Depolama fiyatlandırması hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/pricing/details/storage/)

Sorunuz hala yanıtlanmamışsa, aka.ms/marketplacepublishersupport aracılığıyla Destek'e başvurun, ardından **'teklif oluşturma'** → **teklif türünü** → **'müşteri adayı yönetimi yapılandırması'** seçeneğini belirleyin. 

### <a name="i-am-receiving-email-notifications-when-new-customer-leads-are-received-how-can-i-configure-who-to-receive-these-emails"></a>Yeni müşteri müşteri adayları geldiğinde e-posta bildirimleri alıyorum. Bu e-postaları kimin alacağını nasıl yapılandırabilirim?

İş Ortağı Merkezi'ndeki teklifinize erişin ve **Teklif kurulum** sayfasına gidin -> Müşteri **Adayı Yönetimi** -> **Düzenle.** **İletişim e-postası** alanı altındaki e-posta adreslerini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Teknik kurulum yapıldıktan sonra, bu müşteri adaylarını mevcut satış & pazarlama stratejinize ve operasyonel süreçlerinize dahil etmelidir. Genel satış sürecinizi daha iyi anlamak istiyoruz ve başarılı olmak için yüksek kaliteli müşteri adayları ve yeterli veri sağlama konusunda sizinle yakın bir şekilde çalışmak istiyoruz. Bu müşterilerin başarılı olması için size gönderdiğimiz müşteri adaylarını nasıl optimize edebileceğimiz ve geliştirebileceğimiz hakkındaki görüşlerinizi memnuniyetle karşılıyoruz. Satış ekibinizin Market Müşteri Adayları ile daha başarılı olmasını sağlamak için [geri bildirim](mailto:AzureMarketOnboard@microsoft.com) ve öneriler sunmak istiyorsanız bize bildirin.
