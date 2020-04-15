---
title: Ticari pazar için kurşun yönetimi | Azure Marketi ve AppSource
description: Azure Marketi ve AppSource'a yayımlama teklifleri ve teknik eserlerle ilgili çeşitli konulara genel bakış
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: 09b02d043e970c68cdff0e3dc4f97328c9d74c84
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383790"
---
# <a name="lead-management-for-commercial-marketplace"></a>Ticari pazar için müşteri adayı yönetimi


Müşteriler her iyi iş merkezidir. Günümüzün ürün satın almadönüşüm, pazarlamacılar doğrudan müşterilerle bağlantı ve bir ilişki kurma odaklanmak gerekir. Bu nedenle, yüksek kaliteli müşteri adayları oluşturmak satış döngünüz için hayati bir araçtır. Teklifinizi İş [Ortağı Merkezi'nde](https://partner.microsoft.com/)listelemeyaptıktan sonra, müşteri ilginizi gösterdikten veya ürününüzü pazarda dağıttıktan hemen sonra programlı bir şekilde müşteri iletişim bilgilerini almanıza olanak sağlayan araçlar vardır. 



## <a name="what-are-leads-in-the-marketplace"></a>Pazardaki müşteri adayları nelerdir?

Müşteri adayları, ürünlerinizi Pazar'dan ilgilenen veya dağıtan müşterilerden gelmektedir. Ürününuz Azure Marketi'nde veya AppSource'da listelenmiş olsun, CRM'nizden İş Ortağı Merkezi'ndeki girişinize doğru şekilde ayarlandıktan sonra müşterilerden müşteri adayları alabilirsiniz. 


## <a name="how-to-connect-your-crm-system-with-partner-center"></a>CRM sisteminizi İş Ortağı Merkezi'ne bağlama

Müşteri adayları almaya başlamak için, İş Ortağı Merkezi'ndeki Müşteri Adayı Yönetimi bağlayıcısı, CRM bilgilerinizle kolayca bağlanabilecek şekilde tasarlanmıştır. Artık, harici bir sistemle entegre olmak için önemli bir mühendislik çabası olmadan pazar tarafından oluşturulan müşteri adaylarını kolayca kullanabilirsiniz.

Olası müşteri adayı hedeflerinin her birini nasıl bağlayacaklarına ilişkin adım adım talimatlar aşağıda veda edebilirsiniz:

**Dynamics CRM Online** - Müşteri [katılımı için Dynamics 365](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) için müşteri adayı almak için Dynamics CRM Online'ı nasıl yapılandıracağınız hakkında talimatlar için müşteri adayı yönetimini yapılandırın.

**Marketo** - Müşteri adaylarını elde etmek için Marketo Müşteri Adayı Yapılandırması'nı kurma yla ilgili talimatlar için [Marketo'daki müşteri adayı yönetimini yapılandırın.](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md)

**Salesforce** - Müşteri adaylarını almak için Salesforce örneğini ayarlama yönergeleri için Salesforce için [müşteri adayı yönetimini yapılandırmaya](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md) bakın.

**Azure Tablosu** - Azure depolama hesabınızı bir Azure tablosunda müşteri adayı almak için ayarlama yönergeleri için [bir Azure Tablosu kullanarak müşteri adayı yönetimini yapılandırmaya](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md) bakın.

**Https Endpoint** - Müşteri adayı almak için Https Bitiş Noktanızı ayarlama yönergeleri için [bir HTTPS bitiş noktası kullanarak müşteri adayı yönetimini yapılandırın.](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md)

Müşteri adayı hedefinizi doğru şekilde yapılandırdıktan ve teklifiniz üzerine Yayımla'yı vurduktan sonra bağlantıyı doğrularız ve size bir test müşteri adayı göndeririz. Canlı yayına çıkmadan önce teklifi görüntülerken, teklifi önizleme ortamında kendiniz elde etmeye çalışarak müşteri adayı bağlantınızı da test edebilirsiniz. Müşteri adayı ayarlarınızın güncel kalmasını sağlamak önemlidir, böylece müşteri adayı kaybetmeyin, bu nedenle bir şey değiştiğinde bu bağlantıları güncelleştirdiğinizden emin olun.

### <a name="what-are-the-next-steps"></a>Sonraki adımlar nelerdir?

Teknik kurulum yapıldıktan sonra, bu müşteri adaylarını mevcut satış & pazarlama stratejinize ve operasyonel süreçlerinize dahil etmelidir. Genel satış sürecinizi daha iyi anlamak istiyoruz ve başarılı olmak için yüksek kaliteli müşteri adayları ve yeterli veri sağlama konusunda sizinle yakın bir şekilde çalışmak istiyoruz. Bu müşterilerin başarılı olması için size gönderdiğimiz müşteri adaylarını nasıl optimize edebileceğimiz ve geliştirebileceğimiz hakkındaki görüşlerinizi memnuniyetle karşılıyoruz. Satış ekibinizin Market Müşteri Adayları ile daha başarılı olmasını sağlamak için geri bildirim ve öneriler sunmak istiyorsanız bize bildirin.

## <a name="common-lead-configuration-errors-during-publishing-in-partner-center"></a>İş Ortağı Merkezi'nde yayımlama sırasında sık karşılaşılan müşteri adayı yapılandırma hataları

**Dynamics CRM'ye giden ipucunu kaydedemedi. Dynamics CRM hesap ayarlarını kontrol edin. LastCRMError: Dynamics CRM'de oturum açamayan LastCRMException:** 

> O365 kimlik doğrulaması seçildiyse, kullanıcı hesabı nın ve parolanın geçerli olup olmadığını kontrol edin. AAD seçildiyse, kiracı kimliği, uygulama kimliği ve uygulama gizli anahtarının AAD'de ayarlananla eşleşip eşleşmediğini kontrol edin. Talimatları [buradan](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)takip edin. Hesap kullanıcı adı/parolası geçerliyse, lütfen Dynamics 365'e erişimi olduğundan ve atanmış bir lisansa sahip olduğundan emin olun (Office kullanıcısını kullanıyorsanız Azure Etkin Dizin veya Güvenlik Ayarları kullanıyorsanız 11-15 adım). 

**Dynamics CRM'ye giden ipucunu kaydedemedi. Kullanıcı, müşteri adayı varlığındaki kurşun kaynak kodu özniteliği için izin oluşturmuyor** 

> Uygulama/kullanıcı, Microsoft Market'in baş yazarı için güvenlik rolü(ler) eksiktir. [Burada](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)bir Office kullanıcıkullanıyorsanız Azure Etkin Dizin veya Güvenlik Ayarları kullanıyorsanız 11-15 adımlarını izleyin.

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. İstisna:: Kiracı bulunamadı. Kiracı için etkin abonelik yoksa bu örnek oluşabilir.**  

> Müşteri adayı yönetimi bölümünde sağlanan Dizin Kimliği geçerli bir dizin değildir. Lütfen Adım 2'deki talimatlara göre Dizin Kimliği'ni alın (Azure Active Directory altında, [buradan.](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)

**Dynamics CRM'ye giden ipucunu kaydedemedi. LastCRMError: SecLib::RetrievePrivilegeForUser başarısız oldu - kullanıcıya hiçbir rol atanmadı.**  

> Çözüm: Güvenlik rolünü Microsoft Market baş yazarına atayın. Güvenlik ayarları altında [talimatları buradan](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) takip edin.

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. Özel Durum:: Tanımlayıcıile uygulama dizinde bulunamadı** 

> Müşteri adayı yönetimi bölümünde sağlanan Uygulama Kimliği geçerli bir dizin değildir. Lütfen Adım 8'deki yönergelere göre Dizin Kimliği'ni alın (Azure Etkin Dizini altında, [buradan).](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) 

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. Özel Durum:: İstenen kiracı tanımlayıcısı geçerli değildir ve geçerli harici etki alanı biçimi değildir** 

> Müşteri adayı yönetimi bölümünde sağlanan Dizin Kimliği geçerli bir dizin değildir. Lütfen Adım 2'deki yönergelere göre Dizin Kimliği'ni alın (Azure Etkin Dizini altında, [buradan).](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) 

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. Özel Durum:: Kimlik bilgilerini doğrulayan hata.: Geçersiz istemci sırrı sağlanır.** 

> Çözüm: Azure portalında oturum açın, uygulama anahtarının Ortak Merkezi'ndekilerle eşleşip eşleşmeyini kontrol edin. Lütfen Adım 10'daki yönergeye göre (Azure Etkin Dizini altında) buradan parola [oluşturun.](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) 

**Dynamics CRM'ye giden ipucunu kaydedemedi. LastCRMError: İstek kanalı 00:02:00'den sonra yanıt beklerken zaman doldu. İstek çağrısına geçen zaman dilimi değerini artırın veya Bağlama'daki SendTimeout değerini artırın. Bu işlem için ayrılan süre, daha uzun bir zaman aşımının bir parçası olabilir.**  

> Çözüm: İş Ortağı Merkezi'nde oturum açın, Müşteri Adayı >> storefront ayrıntılarını kontrol edin >> URL'si, geçerli bir Dinamik CRM örneği olup olmadığını kontrol edin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**İpuçları nedir ve marketplace'te bir yayıncı olarak benim için neden önemlidir?** 

Müşteri adayları, ürünlerinizi Market'ten dağıtan müşterilerdir. Ürününuz [Azure Marketi'nde](https://azuremarketplace.microsoft.com) veya [AppSource'da](https://appsource.microsoft.com/)listelenmiş olsun, teklifinizdeki müşteri hedefini kursanız, ürününizle ilgilenen müşterilerin müşteri adaylarını alabilirsiniz.  

**Müşteri adayımın ayarlanması nda nereden yardım alabilirim?** 

Belgeleri burada bulabilirsiniz: [Müşteri müşteri adaylarını alın](./partner-center-portal/commercial-marketplace-get-customer-leads.md) veya teklif türünü ve müşteri adayı yönetimini seçmek aka.ms/marketplacepublishersupport destek bileti gönderin. 

**Market'te bir teklif yayınlamak için bir müşteri adayı nı yapılandırmam gerekiyor mu?**

Evet, Bir İletişim Me SaaS uygulaması veya Danışmanlık Hizmetleri yayınlıyorsanız.  
 
**Müşteri adayı yapılandırmasının doğru olduğunu nasıl doğrulayabilirim?**

Teklifinizi ayarladıktan sonra ve varış noktasını belirledikten sonra teklifinizi yayınlayın. Müşteri adayı doğrulama adımında, Marketplace teklifinizde yapılandırılan müşteri adayı hedefine bir test müşteri adayı gönderir. 

**Test ipucunu nasıl bulabilirim?**

Müşteri adayı hedefinizde "MSFT_TEST" araması yapın, örnek test müşteri adayı verilerini aşağıda bulabilirsiniz: 

şirket = MSFT_TEST_636573304831318844 

ülke = ABD 

açıklama = MSFT_TEST_636573304831318844 

e-posta =MSFT_TEST_636573304831318844@test.com

kodlama = UTF-8 

kodlama = UTF-8 

first_name = MSFT_TEST_636573304831318844 

last_name = MSFT_TEST_636573304831318844 

lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844| \<Teklif Adı> 

oid = 00Do0000000ZHog 

telefon = 1234567890 

başlık = MSFT_TEST_636573304831318844 

**Canlı bir teklifim var ama bir ipucu göremiyorum.**

Her müşteri adayı seçtiğiniz müşteri adayı hedefindeki alanlarda veri aktarılacak, müşteri adayları bu biçimde gelecektir: **Kaynak-Eylem| Teklif** 

  *Kaynak:*

    "AzureMarketplace", 
    "AzurePortal", 
    "TestDrive",  
    "SPZA" (acronym for AppSource) 

  *Eylem:*

    "INS" - Stands for Installation. This is on Azure Marketplace or AppSource whenever a customer hits the button to acquire your product. 
    "PLT" - Stands for Partner Led Trial. This is on AppSource whenever a customer hits the Contact me button. 

    "DNC" - Stands for Do Not Contact. This is on AppSource whenever a Partner who was cross listed on your app page gets requested to be contacted. We are sharing the heads up that this customer was cross listed on your app, but they do not need to be contacted. 

    "Create" - This is inside Azure portal only and is whenever a customer purchases your offer to their account. 

    "StartTestDrive" - This is for Test Drives only and is whenever a customer starts their test drive. 


  *Sunmaktadır:*

    "checkpoint.check-point-r77-10sg-byol", 
    "bitnami.openedxcypress", 
    "docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145a" 

 

  *İşte müşteri bilgilerinin örnek verileri*

    { 

    "FirstName":"John", 

    "LastName":"Smith", 

    "Email":"jsmith@microsoft.com", 

    "Phone":"1234567890", 

    "Country":"US", 

    "Company":"Microsoft", 

    "Title":"CTO" 

    } 

Müşteri Adayı [Bilgileri](./partner-center-portal/commercial-marketplace-get-customer-leads.md)altında daha fazla bilgi edinin. 

**Azure BLOB'u müşteri adayı olarak yapılandırıldım, neden müşteri adayını göremiyorum?** 

Müşteri adayı yalnızca müşteri adayı olarak Azure BLOB depolama alanını seçtiğinizde yazılır. Müşteri adayını gerçek zamanlı olarak almak için Azure tablosuna geçin.

**Marketplace'ten bir e-posta aldım, crm'imde neden müşteri adayı bulamıyorum?**  

Son kullanıcının e-posta etki alanı .edu'dan olabilir. Gizlilik nedenlerinden dolayı .edu etki alanından kişisel olarak tanımlanabilir verileri geçirmiyoruz. Destek [bileti](https://aka.ms/marketplacepublishersupport)gönderin.

**Müşteri adayı olarak Azure Tablo/Azure BLOB'u yapılandırıldım, müşteri adaylarını nasıl görüntüleyebilirim?** 

Azure portalından blob veya tabloya erişebilir veya Azure depolama hesabınızın tablolarını/lekelerini görüntülemek için [Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) ücretsiz olarak indirip yükleyebilirsiniz. 

**Azure Tablosu'nu müşteri adayım olarak yapılandırıldım, Market tarafından yeni bir müşteri adayı gönderildiğinde bilgilendirilebilir miyim?** 

Evet, [buradaki](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md)belgelerde Azure Tablo + İşlev'i ayarlamak için yönergeleri izleyin. 

**Satış Gücü'ni müşteri adayım olarak yapılandırıldım, neden müşteri adaylarını bulamıyorum?** 

Web'in yol formuna ilişkin bir seçim listesine dayalı zorunlu bir alan olup olmadığını denetleyin. Evet ise, alanı zorunlu olmayan bir metin alanına geçin.  
 
**Benim kurşun hedef ile ilgili bir sorun vardı ve bazı yol cevapsız. Bunları bana bir e-posta yla göndertebilir miyim?** 

Gizlilik politikaları nedeniyle, müşteri adayı bilgilerini güvenli olmayan e-posta yoluyla paylaşamayız. 

**Müşteri adayım olarak Azure Depolama 'yı (BLOB/Tablo) yapılandırdım, ne kadara mal olacak?** 

Kurşun gen verileri düşüktür (hemen hemen tüm yayıncılar için 1 GB <). Maliyet, bir ay içinde 1.000 müşteri adayı alınırsa, yaklaşık 50 sente mal olacak. 
