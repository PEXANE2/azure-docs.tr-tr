---
title: Bulut pazarı için müşteri adayı yönetimi | Azure Marketi ve AppSource
description: Azure Marketi ve AppSource'a yayımlama teklifleri ve teknik eserlerle ilgili çeşitli konulara genel bakış
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: dsindona
ms.openlocfilehash: 94510d02a28e0364f1c715dbcf9ff641fe2b14fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286141"
---
# <a name="lead-management-for-cloud-marketplace"></a>Bulut marketi için müşteri adayı yönetimi


Müşteriler her iyi iş merkezidir. Günümüzün ürün satın almadönüşüm, pazarlamacılar doğrudan müşterilerle bağlantı ve bir ilişki kurma odaklanmak gerekir. Bu nedenle, yüksek kaliteli müşteri adayları oluşturmak satış döngünüz için hayati bir araçtır. Teklifinizi [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)listeledikten sonra, müşteri ilginizi gösterdikten veya ürününüzü pazarda dağıtdıktan hemen sonra, programlı bir şekilde müşteri iletişim bilgilerini almanıza olanak sağlayan araçlar vardır. 



## <a name="what-are-leads-in-the-marketplace"></a>Pazardaki müşteri adayları nelerdir?

Müşteri adayları, ürünlerinizi Pazar'dan ilgilenen veya dağıtan müşterilerden gelmektedir. Ürününuz Azure Marketi'nde veya AppSource'da listelenmiş olsun, CRM'nizden Bulut İş Ortağı Portalı'ndaki girişinize doğru şekilde ayarlandıktan sonra müşterilerden müşteri adayları alabilirsiniz 


## <a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>CRM sisteminizi bulut iş ortağı portalına bağlama

Müşteri adaylarını almaya başlamak için, Bulut İş Ortağı Portalı'ndaki Müşteri Adayı Yönetimi bağlayıcısı, CRM bilgilerinizle kolayca bağlanabilecek şekilde tasarlanmıştır. Artık, harici bir sistemle entegre olmak için önemli bir mühendislik çabası olmadan pazar tarafından oluşturulan müşteri adaylarını kolayca kullanabilirsiniz.

Olası müşteri adayı hedeflerinin her birini nasıl bağlayacaklarına ilişkin adım adım talimatlar aşağıda veda edebilirsiniz:

**Dynamics CRM Online** - Müşteri Adayları almak için Dynamics CRM Online'ı nasıl yapılandıracağınız la ilgili talimatları almak için[buraya tıklayın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)

**Marketo** - Müşteri adayları almak için Marketo Müşteri Adayı Yapılandırması'nı kurmak için talimatları almak için[buraya tıklayın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo)

**Salesforce** - Müşteri adayı almak için Salesforce örneğini ayarlama yönergelerini almak için[burayı tıklatın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce)

**Azure Tablosu** - Azure tablosunda müşteri adayları almak için Azure depolama hesabınızı ayarlama yönergelerini almak için[buraya tıklayın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table)

**Https Endpoint** - İpuçları almak için Https Endpoint'inizi ayarlama yönergelerini almak için[buraya tıklayın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https)

Müşteri adayı hedefinizi doğru şekilde yapılandırdıktan ve teklifiniz üzerine Yayımla'yı vurduktan sonra bağlantıyı doğrularız ve size bir test müşteri adayı göndeririz. Canlı yayına çıkmadan önce teklifi görüntülerken, teklifi önizleme ortamında kendiniz elde etmeye çalışarak müşteri adayı bağlantınızı da test edebilirsiniz. Müşteri adayı ayarlarınızın güncel kalmasını sağlamak önemlidir, böylece müşteri adayı kaybetmeyin, bu nedenle bir şey değiştiğinde bu bağlantıları güncelleştirdiğinizden emin olun.


### <a name="what-are-the-next-steps"></a>Sonraki adımlar nelerdir?

Teknik kurulum yapıldıktan sonra, bu müşteri adaylarını mevcut satış & pazarlama stratejinize ve operasyonel süreçlerinize dahil etmelidir. Genel satış sürecinizi daha iyi anlamak istiyoruz ve başarılı olmak için yüksek kaliteli müşteri adayları ve yeterli veri sağlama konusunda sizinle yakın bir şekilde çalışmak istiyoruz. Bu müşterilerin başarılı olması için size gönderdiğimiz müşteri adaylarını nasıl optimize edebileceğimiz ve geliştirebileceğimiz hakkındaki görüşlerinizi memnuniyetle karşılıyoruz. Satış ekibinizin Market Müşteri Adayları ile daha başarılı olmasını sağlamak için geri bildirim ve öneriler sunmak istiyorsanız bize bildirin.



## <a name="common-lead-configuration-errors-during-publishing-on-cloud-partner-portal"></a>Bulut iş ortağı portalında yayımlama sırasında sık karşılaşılan müşteri adayı yapılandırma hataları 

**Dynamics CRM'ye giden ipucunu kaydedemedi. Dynamics CRM hesap ayarlarını kontrol edin. LastCRMError: Dynamics CRM'de oturum açamayan LastCRMException:** 

> O365 kimlik doğrulaması seçildiyse, kullanıcı hesabı nın ve parolanın geçerli olup olmadığını kontrol edin. AAD seçildiyse, kiracı kimliği, uygulama kimliği ve uygulama gizli anahtarının AAD'de ayarlananla eşleşip eşleşmediğini kontrol edin. Talimatları [buradan](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)takip edin. Hesap kullanıcı adı/parolası geçerliyse, lütfen Dynamics 365'e erişimi olduğundan ve atanmış bir lisansa sahip olduğundan emin olun (Office kullanıcısını kullanıyorsanız Azure Etkin Dizin veya Güvenlik Ayarları kullanıyorsanız 11-15 adım). 

 
**Dynamics CRM'ye giden ipucunu kaydedemedi. Kullanıcı, müşteri adayı varlığındaki kurşun kaynak kodu özniteliği için izin oluşturmuyor** 

> Uygulama/kullanıcı, Microsoft Market'in baş yazarı için güvenlik rolü(ler) eksiktir. [Burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)bir Office kullanıcıkullanıyorsanız Azure Etkin Dizin veya Güvenlik Ayarları kullanıyorsanız 11-15 adımlarını izleyin.

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. İstisna:: Kiracı bulunamadı. Kiracı için etkin abonelik yoksa bu örnek oluşabilir.**  

> Müşteri adayı yönetimi bölümünde sağlanan Dizin Kimliği geçerli bir dizin değildir. Lütfen Adım 2'deki talimatlara göre Dizin Kimliği'ni alın (Azure Active Directory altında, [buradan](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) 

**Dynamics CRM'ye giden ipucunu kaydedemedi. LastCRMError: SecLib::RetrievePrivilegeForUser başarısız oldu - kullanıcıya hiçbir rol atanmadı.**  

> Çözüm: Güvenlik rolünü Microsoft Market baş yazarına atayın. Güvenlik ayarları altında [talimatları buradan](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) izleyin 

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. Özel Durum:: Tanımlayıcıile uygulama dizinde bulunamadı** 

> Müşteri adayı yönetimi bölümünde sağlanan Uygulama Kimliği geçerli bir dizin değildir. Lütfen Adım 8'deki yönergelere göre Dizin Kimliği'ni alın (Azure Etkin Dizini altında, [buradan).](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) 

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. Özel Durum:: İstenen kiracı tanımlayıcısı geçerli değildir ve geçerli harici etki alanı biçimi değildir** 

> Müşteri adayı yönetimi bölümünde sağlanan Dizin Kimliği geçerli bir dizin değildir. Lütfen Adım 2'deki yönergelere göre Dizin Kimliği'ni alın (Azure Etkin Dizini altında, [buradan).](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) 

**AAD kullanarak Dynamics CRM'ye giden müşteri adayı nı kaydedemedi. Özel Durum:: Kimlik bilgilerini doğrulayan hata.: Geçersiz istemci sırrı sağlanır.** 

> Çözüm: Azure Portalı'nda oturum açın, uygulama anahtarının Bulut İş Ortağı Portalı'ndakilerle eşleşip eşleşmeyini kontrol edin. Lütfen Adım 10'daki yönergeye göre (Azure Etkin Dizini altında) buradan parola [oluşturun.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) 

**Dynamics CRM'ye giden ipucunu kaydedemedi. LastCRMError: İstek kanalı 00:02:00'den sonra yanıt beklerken zaman doldu. İstek çağrısına geçen zaman dilimi değerini artırın veya Bağlama'daki SendTimeout değerini artırın. Bu işlem için ayrılan süre, daha uzun bir zaman aşımının bir parçası olabilir.**  

> Çözüm: Bulut İş Ortağı Portalında oturum açın, Müşteri Adayı hedef >> URL >> Storefront ayrıntılarını kontrol edin, geçerli bir Dinamik CRM örneği olup olmadığını kontrol edin

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**İpuçları nedir ve marketplace'te bir yayıncı olarak benim için neden önemlidir?** 

Müşteri adayları, ürünlerinizi Market'ten dağıtan müşterilerdir. Ürününuz [Azure Marketi'nde](https://azuremarketplace.microsoft.com/en-us) veya [AppSource'da](https://appsource.microsoft.com/)listelenmiş olsun, teklifinizdeki müşteri hedefini kursanız, ürününizle ilgilenen müşterilerin müşteri adaylarını alabilirsiniz.  


**Müşteri adayımın ayarlanması nda nereden yardım alabilirim?** 

Belgeleri burada bulabilirsiniz: [Müşteri müşteri adaylarını alın](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) veya teklif türünü ve müşteri adayı yönetimini seçmek aka.ms/marketplacepublishersupport destek bileti gönderin. 



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

    "Create" - This is inside Azure Portal only and is whenever a customer purchases your offer to their account. 

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

Müşteri Adayı [Bilgileri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)altında daha fazla bilgi edinin. 


**Azure BLOB'u müşteri adayı olarak yapılandırıldım, neden müşteri adayını göremiyorum?** 

Müşteri adayı yalnızca müşteri adayı olarak Azure BLOB depolama alanını seçtiğinizde yazılır. Müşteri adayını gerçek zamanlı olarak almak için Azure tablosuna geçme 


**Marketplace'ten bir e-posta aldım, crm'imde neden müşteri adayı bulamıyorum?**  

Son kullanıcının e-posta etki alanı .edu'dan olabilir. Gizlilik nedenlerinden dolayı .edu etki alanından kişisel bilgi verisi geçirmiyoruz. aka.ms/marketplacepublishersupport aracılığıyla destek bileti gönderin 


 **Müşteri adayı olarak Azure Tablo/Azure BLOB'u yapılandırıldım, müşteri adaylarını nasıl görüntüleyebilirim?** 

Azure Portalı'ndan blob veya tabloya erişebilir veya Azure depolama hesabınızın tablolarını/lekelerini görüntülemek için [Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) ücretsiz olarak indirip yükleyebilirsiniz. 


**Azure Tablosu'nu müşteri adayım olarak yapılandırıldım, Market tarafından yeni bir müşteri adayı gönderildiğinde bilgilendirilebilir miyim?** 

Evet, [buradaki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table)belgelerde Azure Tablo + İşlev'i ayarlamak için yönergeleri izleyin. 



**Satış Gücü'ni müşteri adayım olarak yapılandırıldım, neden müşteri adaylarını bulamıyorum?** 

Web'in yol formuna ilişkin bir seçim listesine dayalı zorunlu bir alan olup olmadığını denetleyin. Evet ise, alanı zorunlu olmayan bir metin alanına geçin.  
 

**Benim kurşun hedef ile ilgili bir sorun vardı ve bazı yol cevapsız. Bunları bana bir e-posta yla göndertebilir miyim?** 

Kişisel Bilgiler (Özel Tanımlanabilir Bilgiler) ilkeleri nedeniyle, müşteri adayı bilgilerini güvenli olmayan e-posta yoluyla paylaşamayız. 



**Müşteri adayım olarak Azure Depolama 'yı (BLOB/Tablo) yapılandırdım, ne kadara mal olacak?** 

Kurşun gen verileri düşüktür (hemen hemen tüm yayıncılar için 1 GB <). Maliyet, bir ay içinde 1.000 müşteri adayı alınırsa, yaklaşık 50 sente mal olacak. 
