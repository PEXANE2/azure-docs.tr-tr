---
title: Microsoft ticari Market teklifinizden müşteri adaylarını yapılandırma
description: Müşteri adaylarını Microsoft ticari marketi 'nde yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 54c67656d7847b44c8fc83b33a4e03be3838cf76
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82131084"
---
# <a name="customer-leads-from-your-marketplace-offer"></a>Market teklifinizden müşteri adayları

Müşteri adayları, Microsoft [Appsource](https://appsource.microsoft.com) ve [Azure Marketi](https://azuremarketplace.microsoft.com)'nden tekliflerinizi ilgilenen veya dağıtmakta olan müşteriler. Teklifiniz Market 'te yayımlandıktan sonra müşteri adaylarını alacaksınız. Bu makalede şunları açıklanmaktadır:

* Market teklifinizin, iş fırsatlarını kaçırmadığınızdan emin olmak için müşteri adaylarını nasıl üretdirebileceği. 
* Müşteri ilişkileri yönetimi (CRM) sisteminizi teklifinize bağlamak, böylelikle müşteri adaylarını tek bir merkezi konumda yönetebilmenizi sağlar.
* Size ulaşan müşterileri takip edebilmeniz için size gönderdiğimiz müşteri adayı verileri.

## <a name="generate-customer-leads"></a>Müşteri adayları oluşturma

Bir müşteri adayının oluşturulduğu yer şunlardır:

- Müşteri, ticari Market 'ten **benimle Iletişim kurun** ' i seçtikten sonra bilgilerini paylaşma. Bu müşteri adayı, bir *ilk faiz* lideri. Ürününüzü alma konusunda ilgilendiğiniz müşteri hakkındaki bilgileri sizinle paylaşıyoruz. Müşteri adayı, Alım huninin en üstü.

    ![Dynamics 365 benimle Iletişim kurun](./media/commercial-marketplace-get-customer-leads/dynamics-365-contact-me.png)

- Müşteri, teklifinizi almak için **Şimdi al** (veya [Azure Portal](https://portal.azure.com/) **Oluştur** ' A seçer) öğesini seçer. Bu müşteri adayı *etkin* bir müşteri adaydır. Ürününüzü dağıtmayı Başlatan müşteriyle ilgili bilgileri sizinle paylaşıyoruz.

    ![SQL Şimdi Al düğmesi](./media/commercial-marketplace-get-customer-leads/sql-get-it-now.png)

    ![Windows Server oluştur düğmesi](./media/commercial-marketplace-get-customer-leads/windows-server-create.png)

- Müşteri, teklifinizi denemek için **test sürücüsü** veya **ücretsiz deneme sürümünü** seçer. Sınama sürücüleri veya ücretsiz denemeler, herhangi bir giriş konusunda herhangi bir engelleri olmadan işletmenizin potansiyel müşterilerle anında paylaşılması için hızlandırılmış fırsatlardır.

    ![Dynamics 365 test sürücüsü düğmesi](./media/commercial-marketplace-get-customer-leads/dynamics-365-test-drive.png)

    ![Dynamics 365 ücretsiz deneme düğmesi](./media/commercial-marketplace-get-customer-leads/dynamics-365-free-trial.png)

## <a name="connect-to-your-crm-system"></a>CRM sisteminize bağlanma

[!INCLUDE [Test drive content](./includes/connect-lead-management.md)]

## <a name="understand-lead-data"></a>Müşteri adayı verilerini anlama

Müşteri alma işlemi sırasında aldığınız her bir müşteri, belirli alanlardaki verilere sahiptir. İçin ilk `LeadSource` aranacak alan, şu biçimi izleyen alandır: **Kaynak-eylem** | **teklifi**.

**Kaynaklar**: Bu alanın değeri, müşteri adayını oluşturan Market 'e göre doldurulur. Olası değerler şunlardır `"AzureMarketplace"`, `"AzurePortal"`ve `"AppSource (SPZA)"`.

**Eylemler**: Bu alanın değeri, müşterinin müşteri adayını oluşturan Market 'te aldığı eyleme göre doldurulur.

Olası değerler şunlardır:

- **"INS"**: *yükleme*anlamına gelir. Bu eylem, bir müşteri ürününüzü satın aldığında Azure Market veya AppSource 'ta bulunur.
- **"Plt"**: *iş ortağının LED deneme sürümünü*temsil eder. Bu eylem, bir müşteri **benimle Iletişim kurma** seçeneğini seçtiğinde appsource 'ta bulunur.
- **"DNC"**: *başvurmayın*için temsil eder. Bu eylem, uygulama sayfanızda çapraz listeye alınan bir iş ortağı ile iletişim kurulması istendiğinde AppSource 'ta yer alır. Bu müşterinin uygulamanızda çapraz listelenme bildirimini paylaşıyoruz, ancak bunlara başvurulmaları gerekmez.
- **"Oluştur"**: Bu eylem yalnızca Azure Portal içindedir ve bir müşteri, teklifinizi kendi hesaplarına satın aldığında oluşturulur.
- **"Starttestdrive"**: Bu eylem yalnızca **Test sürücü** seçeneği içindir ve bir müşteri test sürücüsünü başlattığında oluşturulur.

**Teklifler**: ticari Market 'te birden fazla teklifiniz olabilir. Bu alanın değeri, müşteri adayını oluşturan teklifine göre doldurulur. Yayımcı KIMLIĞI ve teklif KIMLIĞI bu alana gönderilir ve teklifi Market 'e yayımladığınızda verdiğiniz değerlerdir.

Aşağıdaki örnekler, değerleri beklenen biçimde `publisherid.offerid`göstermektedir: 

- `checkpoint.check-point-r77-10sg-byol`
- `bitnami.openedxcypress`
- `docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145`

## <a name="customer-information"></a>Müşteri bilgileri

Müşterinin bilgileri birden çok alan aracılığıyla gönderilir. Aşağıdaki örnek, bir müşteri adayıyla bulunan müşteri bilgilerini gösterir:

- FirstName: John
- Soyadı: Smith
- E-posta:\@jsmith Microsoft.com
- Telefon: 1234567890
- Ülke: ABD
- Şirket: Microsoft
- Başlık: CTO

>[!NOTE]
>Önceki örnekteki verilerin hepsi her bir müşteri adayı için her zaman kullanılabilir değildir. "Müşteri adayları oluşturma" bölümünde belirtildiği gibi birden çok adımdan müşteri adaylarını alacağınız için, müşteri adaylarını işlemenin en iyi yolu kayıtları yeniden çoğaltmanız ve takip eden pencereleri kişiselleştirmesidir. Bu şekilde, her müşteri uygun bir ileti alır ve benzersiz bir ilişki oluşturursunuz.

## <a name="best-practices-for-lead-management"></a>Müşteri adayı yönetimi için en iyi yöntemler

- **İşlem**: kilometre taşları, analiz ve açık ekip sahipliğiyle açık bir satış işlemi tanımlayın.
- **Nitelik**: bir müşteri adayının tam nitelikli olup olmadığını gösteren önkoşulları tanımlayın. Satış veya pazarlama temsilcilerinin, müşteri adaylarını tam satış süreci boyunca almadan önce dikkatli bir şekilde nitelediğinizden emin olun.
- **İzleme**: izlemeyi unutmayın. Tipik işlemin, 5-12 izleme çağrısı gerektirmesini bekler.
- **Nurra**: daha yüksek bir kâr marjına yol açmak için müşteri adaylarına ulaşın.

## <a name="leads-frequently-asked-questions"></a>Müşteri adayları hakkında sık sorulan sorular

### <a name="where-can-i-get-help-in-setting-up-my-lead-destination"></a>Lider hedefini ayarlama konusunda nereden yardım alabilirim?

[CRM sisteminize bağlanma](#connect-to-your-crm-system)bölümündeki adımları Izleyin veya [Iş Ortağı Merkezi Yardım ve destek](https://partner.microsoft.com/support/v2/?stage=1)aracılığıyla bir destek bileti gönderebilirsiniz. Teklif oluşturma**müşteri adayı yönetimi yapılandırma****türünden** >  **teklif oluşturmayı** > seçin.

### <a name="am-i-required-to-configure-a-lead-destination-in-order-to-publish-an-offer-in-the-commercial-marketplace"></a>Ticari Market 'te teklif yayımlamak için bir müşteri adayı hedefi yapılandırmak istiyorum mu?

Yanıt, yayımlamakta olduğunuz teklifin türüne bağlıdır. Hizmet olarak yazılım (SaaS) ve Dynamics 365 müşteri katılımı kullanımı, tüm Dynamics 365 finans ve Işlemler tekliflerini, tüm Dynamics 365 Iş Merkezi tekliflerini ve tüm danışmanlık hizmeti tekliflerini listelemek için **benimle Iletişim kurun** . Sonuç olarak, bir müşteri adayı hedefine bağlantı gerektirir. Teklif türü listede yoksa, bir müşteri adayı hedefine bağlantı gerekli değildir. İş fırsatlarını kaçırmamak için bir lider hedefi yapılandırmanızı öneririz.

### <a name="how-can-i-find-the-test-lead"></a>Test lideri nasıl bulabilirim?

`"MSFT_TEST"` Müşteri adayı Hedefinizdeki arama yapın. Microsoft 'tan örnek bir test lideri aşağıda verilmiştir:

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

### <a name="i-have-a-live-offer-but-why-am-i-not-seeing-any-leads"></a>Canlı teklifim var, ancak neden hiçbir müşteri adaylarını görmüyorum?

Müşteri adayı hedefine olan bağlantınızın geçerli olduğundan emin olun. Iş Ortağı Merkezi 'nde teklifiniz üzerinde **Yayımla** ' yı seçtikten sonra size bir test lideri göndereceğiz. Test lideri görürseniz, bağlantı geçerli olur. Ayrıca, önizleme adımı sırasında teklif önizlemeyi almaya çalışırken, müşteri adayı bağlantınızı test edebilirsiniz. Ticari Market 'teki listede **Şimdi al**, **benimle Iletişim kurun**veya **ücretsiz deneme sürümü** ' nü seçin.

Ayrıca, doğru verileri baktığınızdan emin olun. Bu makalenin [müşteri adayı verilerini anlama](#understand-lead-data) bölümünde bulunan içerik, müşteri adayı hedefize gönderdiğimiz müşteri adayı verilerini açıklar.

### <a name="i-configured-azure-blob-storage-as-my-lead-destination-but-why-dont-i-see-the-lead"></a>Azure Blob depolamayı lider hedefim olarak yapılandırdım, ancak neden müşteri adayını görmüyorum?

Azure Blob depolama, artık bir lider hedefi olarak desteklenmediğinden teklifiniz tarafından oluşturulan müşteri adaylarını kaçırıyoruz. Diğer [lider hedefi seçeneklerinden](./commercial-marketplace-get-customer-leads.md)herhangi birine geçin. 

### <a name="i-received-an-email-from-the-commercial-marketplace-but-why-cant-i-find-the-lead-in-my-crm"></a>Ticari Market 'ten bir e-posta aldım, ancak neden CRM 'de müşteri adayını bulamıyorum?

Son kullanıcının e-posta etki alanı. edu 'dan olabilir. Gizlilik nedenleriyle, kişisel olarak tanımlanabilir bilgileri. edu etki alanından geçirmedik. [Iş Ortağı Merkezi Yardım ve destek](https://partner.microsoft.com/support/v2/?stage=1)aracılığıyla bir destek bileti gönderebilirsiniz.

### <a name="i-configured-an-azure-table-as-my-lead-destination-how-can-i-view-the-leads"></a>Müşteri adayı hedefi olarak bir Azure tablosu yapılandırdım. Müşteri adaylarını nasıl görüntüleyebilirim?

Azure tablosunda depolanan müşteri adayı verilerine Azure portal erişebilirsiniz. Ayrıca, Azure depolama hesabınızın tablo verilerini görüntülemek için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) ücretsiz olarak indirip yükleyebilirsiniz.

### <a name="i-configured-an-azure-table-as-my-lead-destination-can-i-get-notified-whenever-a-new-commercial-marketplace-lead-is-sent"></a>Müşteri adayı hedefi olarak bir Azure tablosu yapılandırdım. Her yeni ticari market lideri gönderildiğinde bildirim alabilir miyim?

Evet. Azure tablosuna bir müşteri adayı eklendiğinde bir e-posta gönderen Microsoft Flow ayarlamak için [Azure tablosu kullanarak müşteri adayı yönetimini yapılandırma](./commercial-marketplace-lead-management-instructions-azure-table.md) bölümündeki yönergeleri izleyin.

### <a name="i-configured-salesforce-as-my-lead-destination-but-why-cant-i-find-the-leads"></a>Salesforce 'um hedefi olarak Salesforce yapılandırdım, ancak neden müşteri adaylarını bulamıyorum?

Web 'den müşteri adayı formunun seçim listesine göre zorunlu bir alan olup olmadığını denetleyin. Varsa, alanı zorunlu olmayan bir metin alanına geçirin.

### <a name="there-was-an-issue-with-my-lead-destination-and-i-missed-some-leads-can-i-have-them-sent-to-me-in-an-email"></a>Lider hedefimde bir sorun oluştu ve bazı müşteri adaylarını kaçırdım. E-postada bana gönderilebilirim?

Kişisel olarak tanımlanabilir bilgi ilkeleri nedeniyle, sağlama bilgilerini güvenli olmayan e-posta ile paylaşamıyoruz.

### <a name="i-configured-an-azure-table-as-my-lead-destination-how-much-will-it-cost"></a>Müşteri adayı hedefi olarak bir Azure tablosu yapılandırdım. Ne kadar ücret alınacaktır?

Müşteri adayı oluşturma verileri düşüktür. Neredeyse tüm yayımcılar için 1 GB 'tan az. Maliyet, alınan müşteri adaylarının sayısına bağlıdır. Örneğin, bir ayda 1.000 müşteri adayı alındıysa, maliyet 50 ilaaydır. Depolama fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure depolama genel bakış fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Sorunuz yanıtlanmazsa, [Iş Ortağı Merkezi Yardım ve destek](https://aka.ms/marketplacepublishersupport)aracılığıyla Microsoft desteği başvurun. Teklif oluşturma**müşteri adayı yönetimi yapılandırma****türünden** >  **teklif oluşturmayı** > seçin.

### <a name="im-receiving-email-notifications-when-new-customer-leads-are-received-how-can-i-configure-someone-else-to-receive-these-emails"></a>Yeni müşteri adayları alındığında e-posta bildirimleri alıyorum. Başka birini bu e-postaları alacak şekilde nasıl yapılandırabilirim?

İş Ortağı Merkezi ' nde teklifinizi erişin ve **teklif kurulumu** sayfasına gidin > **müşteri adayı yönetimi** > 'ni**düzenleyin**. **İletişim e-postası** alanındaki e-posta adreslerini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Teknik kurulum hazır olduktan sonra, bu müşteri adaylarını geçerli satış ve pazarlama stratejinize ve işletimsel süreçlerinize ekleyin. Genel Satış sürecinizi daha iyi anlamak ve yüksek kaliteli müşteri adayları sağlamak için sizinle birlikte çalışmak ve başarılı olmak için yeterli veri sunmak istiyoruz. Size bu müşterileri başarılı hale getirmeye yardımcı olmak için size ek veriler gönderdiğimiz müşteri adaylarını iyileştirebilecek ve geliştirebilmemiz için geri bildirimde bulunun. Satış ekibinizin ticari Market liderlerine göre daha başarılı olmasını sağlamak için [geri bildirim](mailto:AzureMarketOnboard@microsoft.com) ve öneriler sağlamayı düşünüyorsanız bize bilgi verin.
