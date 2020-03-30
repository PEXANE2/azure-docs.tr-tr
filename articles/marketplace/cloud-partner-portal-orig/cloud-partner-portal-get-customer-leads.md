---
title: Müşteri müşteri adaylarını yapılandırma | Azure Marketi
description: Bulut İş Ortağı Portalı'ndaki müşteri müşteri adaylarını yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: 56012fb2a907a6db6f87554660ee36b99a3dcbf9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280329"
---
<a name="get-customer-leads"></a>Müşteri adayları alma
==================

Bu makalede, Bulut İş Ortağı Portalı'nı kullanarak müşteri müşteri adaylarının nasıl oluşturulacakları açıklanmaktadır. Bu müşteri adaylarını CRM sisteminize bağlayabilir ve satış ardınıza entegre edebilirsiniz.

## <a name="leads"></a>Leads

Müşteri adayları, ürünlerinizi Azure Marketi'nden veya [Azure Marketplace](https://azuremarketplace.microsoft.com/) [AppSource'dan](https://appsource.microsoft.com)ilgilenen veya dağıtan müşterilerdir.

### <a name="azure-marketplace"></a>Azure Market

1.  Müşteri teklifinizin bir "Test Sürüşü" alır. Test Sürüşleri, herhangi bir giriş engeli olmaksızın işinizi potansiyel müşterilerle anında paylaşmanız için hızlandırılmış bir fırsat. Tüm Test Sürücüleri, ürününüzün daha fazla bilgi edinimi denemek isteyen bir müşteri için bir müşteri adayı oluşturur. [Azure Marketi Test Sürüşü'nde](https://azuremarketplace.azureedge.net/documents/azure-marketplace-test-drive-program.pdf)Test Sürücüleri hakkında daha fazla bilgi edinin.

    ![Pazar yeri test sürüşü örnekleri](./media/cloud-partner-portal-get-customer-leads/test-drive-offer.png)
 

<!-- -->

1. Müşteri ,"Hemen alın" seçeneğini seçtikten sonra bilgilerini paylaşmayı kabul eder. Bu müşteri, ürününüzü almakla ilgilendiğini ifade eden müşteri hakkındaki bilgileri paylaştığımız bir **başlangıç müşteri** adayıdır. Kurşun, satın alma hunisinin en üst kısmıdır.

   ![Şimdi alın seçeneği](./media/cloud-partner-portal-get-customer-leads/get-it-now-button.png)

1. Müşteri, ürününüzü almak için [Azure Portalı'nda](https://portal.azure.com/) "Satın Alma"yı seçer. Bu müşteri adayı, ürününüzü dağıtmaya başlayan bir müşteri hakkında bilgi paylaştığımız **etkin** bir müşteri dir.

   ![Satın alma seçeneği](./media/cloud-partner-portal-get-customer-leads/purchase-button.png)


### <a name="appsource"></a>AppSource

1.  Müşteri teklifiniz için bir "Test Sürüşü" aldı. Test Sürüşleri, herhangi bir giriş engeli olmaksızın işinizi potansiyel müşterilerle anında paylaşmanız için hızlandırılmış bir fırsat. Tüm Test Sürücüleri, ürününüzü daha fazla bilgi edinmek için denemek isteyen bir müşterinin müşteri adayını oluşturur. [AppSource Test Drive'da](https://appsource.microsoft.com/blogs/want-to-try-an-app-take-a-test-drive)Test Sürüşleri hakkında daha fazla bilgi edinin.

    ![Test sürüşü örneği](./media/cloud-partner-portal-get-customer-leads/test-drive-offer-2.png)

2.  Müşteri ,"Hemen alın" seçeneğini seçtikten sonra bilgilerini paylaşmayı kabul eder. Bu müşteri, ürününüzü almakla ilgilendiğini ifade eden müşteri hakkındaki bilgileri paylaştığımız bir **başlangıç müşteri** adayıdır. Kurşun, satın alma hunisinin en üst kısmıdır.

      ![Şimdi alın seçeneği](./media/cloud-partner-portal-get-customer-leads/get-it-now-button-2.png)


3.  Müşteri teklifinizde "Bana Ulaşın" seçeneğini seçer. Bu müşteri, ürününuz hakkında takip edilmeyi isteyen bir müşteri hakkında bilgi paylaştığımız **etkin** bir müşteri dir.

    ![Bana ulaşın seçeneği](./media/cloud-partner-portal-get-customer-leads/contact-me-image.png)

<a name="lead-data"></a>Müşteri Adayı Verileri
---------

Müşteri edinme işlemi sırasında aldığınız her müşteri adayının belirli alanlarda verileri vardır. Birden çok adımdan müşteri adayları alacağınızdan, müşteri adaylarını işlemenin en iyi yolu, sonraki yanıtları çoğaltmak ve kişiselleştirmektir. Bu şekilde her müşteri uygun bir ileti alıyor ve siz benzersiz bir ilişki oluşturuyorsunuz.

### <a name="lead-source"></a>Müşteri Adayı Kaynağı

Bir müşteri adayı kaynağının biçimi **Kaynak**-**Eylem** |  **Teklifi'dir**

**Kaynaklar**: "AzureMarket", "AzurePortal", "TestDrive" ve "AppSource (SPZA)"

**Eylemler**:
- "INS" -- Kurulum. Bu eylem, bir müşteri ürününüzü satın aldığında Azure Marketi veya AppSource'da yapılır.
- "PLT" -- Ortak Led Trial anlamına gelir. Bu eylem, bir müşteri Bana Ulaşın seçeneğini kullandığında AppSource'ta dır.
- "DNC" -- Temas etmeyin. Bu eylem, uygulama sayfanızda çapraz listelenen bir İş Ortağıyla iletişime geçilmesi istendiğinde AppSource'da yer alır. Bu müşterinin uygulamanızda çapraz listeye alındığına dair kafaları paylaşıyoruz, ancak bunlarla iletişime geçmelerine gerek yok.
- "Oluştur" -- Bu eylem yalnızca Azure Portalı'nın içindedir ve bir müşteri teklifinizi kendi hesabına satın aldığında oluşturulur.
- "StartTestDrive" -- Bu eylem yalnızca Test Sürücüleri içindir ve bir müşteri test sürüşüne başladığında oluşturulur.

**Teklifler**

Aşağıdaki örnekler, bir yayımcıya ve belirli bir teklife atanan benzersiz tanımlayıcıları gösterir: checkpoint.check-point-r77-10sg-byol, bitnami.openedxcypress ve docusign.3701c77e-1cfa-4c56-91e6-3ed0b62145a.


### <a name="customer-info"></a>Müşteri Bilgileri

Aşağıdaki örnekteki alanlar, müşteri adayında bulunan müşteri bilgilerini gösterir.
- Ad soyad: John
- Soyadı: Smith
- E-posta:\@jsmith microsoft.com
- Telefon numarası: 1234567890
- Ülke: ABD
- Şirket: Microsoft
- Başlık: CTO

>[!Note]
>Önceki örnekteki tüm veriler her müşteri adayı için her zaman kullanılabilir değildir.

Müşteri adaylarını geliştirmek için aktif olarak çalışıyoruz, bu nedenle burada görmediğiniz ancak sahip olmak istediğiniz bir veri alanı varsa, lütfen [geri bildiriminizi bize gönderin.](mailto:AzureMarketOnboard@microsoft.com)

<a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>CRM sisteminizi Bulut İş Ortağı Portalı'na bağlama
------------------------------------------------------------

Müşteri adayları almaya başlamak için, CrM bilgilerinizi kolayca takabilmeniz için Bulut İş Ortağı Portalı'nda Müşteri Adayı Yönetimi bağlayıcımızı kurduk ve bağlantıyı sizin için yapacağız. Artık, harici bir sistemle entegre olmak için önemli bir mühendislik çabası olmadan pazar tarafından oluşturulan müşteri adaylarını kolayca kullanabilirsiniz.

![Müşteri adayı yönetimi konektörü](./media/cloud-partner-portal-get-customer-leads/lead-management-connector.png)

Müşteri adaylarını çeşitli CRM sistemlerine veya doğrudan müşteri adaylarını istediğiniz gibi yönetebileceğiniz bir Azure Depolama Tablosuna yazabiliriz. Aşağıdaki bağlantıların her biri olası müşteri adayı hedeflerine bağlanmak için yönergeler sağlar:

-   [Dynamics CRM Online](./cloud-partner-portal-lead-management-instructions-dynamics.md) müşteri adayları almak için Dynamics CRM Online'ı yapılandırmak için nasıl talimatlar alırsınız.
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md) müşteri adayları almak için Marketo Kurşun Yapılandırma kurmak için talimatları almak için.
-    [Satış Gücü](./cloud-partner-portal-lead-management-instructions-salesforce.md) müşteri adaylarını almak için Salesforce örneğini ayarlamak için yönergeler almak için.
-    [Azure depolama](./cloud-partner-portal-lead-management-instructions-azure-table.md) hesabınızı bir Azure tablosunda müşteri adayları almak için ayarlama yönergelerini almak için Azure Tablosu.
-   [Https Endpoint](./cloud-partner-portal-lead-management-instructions-https.md) müşteri adayları almak için Https Endpoint'inizi ayarlama yönergelerini almak için.

Müşteri adayı hedefinizi yapılandırdıktan ve teklifinizi yayımladıktan sonra bağlantıyı doğrular ve size bir test müşteri adayı göndeririz. Canlı yayına çıkmadan önce teklifi görüntülerken, teklifi önizleme ortamında kendiniz elde etmeye çalışarak müşteri adayı bağlantınızı da test edebilirsiniz. Müşteri adayı ayarlarınızın güncel kalmasını sağlamak önemlidir, böylece müşteri adayı kaybetmeyin, bu nedenle bir şey değiştiğinde bu bağlantıları güncelleştirdiğinizden emin olun.

<a name="what-next"></a>Sırada ne var?
----------

Teknik kurulum yapıldıktan sonra, bu müşteri adaylarını mevcut satış & pazarlama stratejinize ve operasyonel süreçlerinize dahil etmelidir. Genel satış sürecinizi daha iyi anlamak la çok ilgileniyoruz ve başarılı olmak için yüksek kaliteli müşteri adayları ve yeterli veri sağlama konusunda sizinle yakın bir şekilde çalışmak istiyoruz. Bu müşterilerin başarılı olması için size gönderdiğimiz müşteri adaylarını nasıl optimize edebileceğimiz ve geliştirebileceğimiz hakkındaki görüşlerinizi memnuniyetle karşılıyoruz. Satış ekibinizin Market Müşteri Adayları ile daha başarılı olabilmesi için [geri bildirim](mailto:AzureMarketOnboard@microsoft.com) ve öneriler sunmak istiyorsanız lütfen bize bildirin.
