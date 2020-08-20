---
title: Ticari Market için lider yönetimi | Azure Marketi ve AppSource
description: Azure Marketi ve AppSource 'ta teklifleri ve teknik yapıtları yayımlamaya ilişkin çeşitli konulara genel bakış
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 04/14/2020
ms.openlocfilehash: b08324654a046b5a2296f6e533107c1b26a0e1fc
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88606892"
---
# <a name="lead-management-for-the-commercial-marketplace"></a>Ticari Market için lider yönetimi

Müşteriler her türlü iyi iş merkeziydi. Bugünün ürün alma dönüştürmesinin dönüşümünde, pazarlamacılar müşterilere doğrudan bağlanmaya ve bir ilişki oluşturmaya odaklanmalıdır. Yüksek kaliteli müşteri adaylarının oluşturulması, satış döngünüzde önemli bir araçtır. Teklifinizi [Iş Ortağı Merkezi](https://partner.microsoft.com/)' nde listeledikten sonra, müşteri iletişim bilgilerini bir müşterinin ilgilendiklerini belirledikten veya ürününüzü Market 'te dağıttığında hemen elde etmeniz için etkinleştirilen araçlar vardır. 

## <a name="what-are-leads-in-the-marketplace"></a>Market 'teki müşteri adayları nelerdir?

Müşteri adayları, ürünlerinizi Market 'ten ilgilenen veya dağıtmakta olan müşterilerden elde edilir. Ürününüzün Azure Market veya AppSource 'ta listelenip listelenmediğini, CRM 'nizden Iş Ortağı Merkezi 'ndeki listelerinize doğru bir şekilde ayarladıktan sonra müşterilerden müşteri adaylarını alabileceksiniz. 

## <a name="how-to-connect-your-crm-system-with-partner-center"></a>CRM sisteminizi Iş Ortağı Merkezi ile bağlama

Müşteri adaylarını almaya başlamak için, Iş Ortağı Merkezi 'nde lider yönetimi Bağlayıcısı, CRM bilgilerinizin mevcut olduğu bir CRM sistem listesine kolayca kolayca takılabilmesi için tasarlanmıştır. Artık, bir dış sistemle tümleştirme için önemli bir mühendislik çabası olmadan Market tarafından oluşturulan müşteri adaylarını kolayca kullanabilirsiniz.

Olası her bir müşteri adayı hedefini nasıl bağlayacağınız hakkında adım adım yönergeler aşağıda verilmiştir:

**DYNAMICS CRM Online** -müşteri adaylarını almak IÇIN Dynamics CRM Online 'ın nasıl yapılandırılacağı hakkında yönergeler için bkz. [Dynamics 365 for Customer Engagement için lider yönetimini yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) .

**Marketo** -müşteri adaylarını almak üzere Marketo lider yapılandırmasını ayarlamaya ilişkin yönergeler için bkz. [Marketo 'da lider yönetimi yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md) .

**Salesforce** -müşteri adaylarını almak için Salesforce örneğinizi ayarlama hakkında yönergeler için bkz. [Salesforce için lider yönetimi yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md) .

**Azure tablosu** -Azure tablosu 'nda müşteri adaylarını almak için Azure depolama hesabınızı ayarlamaya ilişkin yönergeler için bkz. [Azure tablosunu kullanarak müşteri adayı yönetimini yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md) .

**Https uç noktası** -müşteri adaylarını almak Için HTTPS uç noktanızı ayarlamaya ilişkin yönergeler IÇIN [bir HTTPS uç noktası kullanarak müşteri adayı yönetimini yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md) konusuna bakın.

Müşteri adayı hedefini doğru bir şekilde yapılandırdıktan ve teklifiniz üzerinde isabet yayımladıktan sonra, bağlantıyı doğrulayacağız ve size bir test lideri göndereceğiz. Teklifi, canlı çalışmadan önce görüntülerken, teklifi önizleme ortamında edinmeyi deneyerek lider bağlantınızı da test edebilirsiniz. Müşteri adaylarını kaybetmemeniz için müşteri adayı ayarlarınızın güncel kalmasını sağlamak önemlidir. bu nedenle, bu bağlantıları sonunda her değişiklik yapıldığında güncelleştirdiğinizden emin olun.

### <a name="what-are-the-next-steps"></a>Sonraki adımlar nelerdir?

Teknik kurulum hazır olduktan sonra, bu müşteri adaylarını geçerli satış & pazarlama stratejinize ve işlemsel süreçlerinize eklemeniz gerekir. Genel Satış sürecinizi daha iyi anlamak ve yüksek kaliteli müşteri adayları sağlamak ve size yeterli veri sağlamak için size yakın bir şekilde çalışmak istemeniz gerekir. Size bu müşterileri başarılı hale getirmeye yardımcı olmak için size ek veriler gönderdiğimiz müşteri adaylarını iyileştirebilecek ve geliştirebilmemiz için geri bildirimde bulunun. Satış ekibinizin Market müşteri adaylarıyla daha başarılı olmasını sağlamak için geri bildirim ve öneriler sağlamayı düşünüyorsanız bize bilgi verin.

## <a name="common-lead-configuration-errors-during-publishing-in-partner-center"></a><a id="publishing-config-errors"></a> Iş Ortağı Merkezi 'nde yayımlama sırasında ortak lider yapılandırma hataları

**Müşteri adayı Dynamics CRM 'ye kaydedilemedi. Dynamics CRM hesap ayarlarını kontrol edin. LastCRMError: Dynamics CRM 'de oturum açılamıyor, LastCRMException:** 

> O365 kimlik doğrulaması seçildiyse, Kullanıcı hesabının ve parolanın geçerli olup olmadığını denetleyin. AAD seçildiyse, kiracı KIMLIĞI, uygulama KIMLIĞI ve uygulama gizli anahtarı 'nın AAD 'de ayarlanmış olan ile eşleşip eşleşmediğini denetleyin. [Buradaki](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)yönergeleri izleyin. Hesap Kullanıcı adı/parola geçerliyse, lütfen Dynamics 365 ' e erişime sahip olduğundan ve atanmış bir lisansa sahip olduğundan emin olun (Office kullanıcısı kullanılıyorsa Azure Active Directory veya güvenlik ayarları kullanıyorsanız 11-15 adımları). 

**Müşteri adayı Dynamics CRM 'ye kaydedilemedi. Kullanıcı, lider varlığındaki leadsourcecode özniteliği için oluşturma izinlerine sahip değil** 

> Uygulama/kullanıcının Microsoft Market lider yazıcı için güvenlik rolleri eksik. [Burada](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)Office kullanıcısı kullanıyorsanız, Azure Active Directory veya güvenlik ayarlarını kullanıyorsanız 11-15 adımlarını izleyin.

**AAD kullanılarak Dynamics CRM 'ye müşteri adayı kaydedilemedi. Özel durum:: kiracı bulunamadı. Bu örnek, kiracı için etkin abonelikler yoksa gerçekleşebilir.**  

> Lider Yönetimi bölümünde belirtilen dizin kimliği geçerli bir dizin değil. Lütfen adım 2 ' deki yönergelere (Azure Active Directory altında, [buradan](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)) göre dizin kimliğini alın.

**Müşteri adayı Dynamics CRM 'ye kaydedilemedi. LastCRMError: SecLib:: RetrievePrivilegeForUser başarısız oldu-kullanıcıya hiçbir rol atanmadı.**  

> Çözüm: Microsoft Market lider yazıcısına güvenlik rolü atayın. Güvenlik ayarları altında [buradaki](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) yönergeleri izleyin.

**AAD kullanılarak Dynamics CRM 'ye müşteri adayı kaydedilemedi. Özel durum:: tanımlayıcılı uygulama dizinde bulunamadı** 

> Lider Yönetimi bölümünde belirtilen uygulama kimliği geçerli bir dizin değil. Lütfen 8. adımdaki yönergelere göre dizin kimliğini alın (Azure Active Directory altında, [buradan](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)). 

**AAD kullanılarak Dynamics CRM 'ye müşteri adayı kaydedilemedi. Özel durum:: Istenen kiracı tanımlayıcısı geçerli değil ve geçerli dış etki alanı biçimi değil** 

> Lider Yönetimi bölümünde belirtilen dizin kimliği geçerli bir dizin değil. Lütfen adım 2 ' deki yönergelere (Azure Active Directory altında, [buradan](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)) göre dizin kimliğini alın. 

**AAD kullanılarak Dynamics CRM 'ye müşteri adayı kaydedilemedi. Özel durum:: kimlik bilgileri doğrulanırken hata.: geçersiz istemci parolası belirtildi.** 

> Çözüm: Azure portal oturum açın, uygulama anahtarının Iş Ortağı Merkezi 'nde olan ile eşleşip eşleşmediğini denetleyin. Lütfen 10. adımdaki yönergeye (Azure Active Directory), [buradan](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)) göre parola oluşturun. 

**Müşteri adayı Dynamics CRM 'ye kaydedilemedi. LastCRMError: 00:02:00 sonrasında bir yanıt beklenirken istek kanalı zaman aşımına uğradı. Istek çağrısına geçirilen zaman aşımı değerini artırın veya bağlamada SendTimeout değerini artırın. Bu işlem için ayrılan süre daha uzun bir zaman aşımı değerinin bir bölümü olabilir.**  

> Çözüm: Iş Ortağı Merkezi 'nde oturum açın, storefront ayrıntılarını >> müşteri adayı hedefi >> URL 'sini denetleyin, geçerli bir dinamik CRM örneği olup olmadığını denetleyin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Müşteri adayları nedir ve neden Market 'te bir yayımcı olarak önemlidir?** 

Müşteri adayları, ürünlerinizi Market 'ten dağıtan müşterilerdir. Ürününüzün [Azure Market](https://azuremarketplace.microsoft.com) veya [appsource](https://appsource.microsoft.com/)'ta listelenip listelenmediğini, teklifiniz üzerinde müşteri adayı hedefini ayarladıysanız, ürününüzle ilgilenen müşterilerin müşteri adaylarını elde edebileceksiniz.  

**Lider hedefini ayarlama konusunda nereden yardım alabilirim?** 

[Müşteri adaylarını al](./partner-center-portal/commercial-marketplace-get-customer-leads.md) veya [Yardım ve destek '](https://aka.ms/marketplacepublishersupport)de destek bileti Gönder ' de belgeleri bulabilirsiniz. Teklif türü ve lider Yönetimi ' ni seçin. 

**Market 'te teklif yayımlamak için bir lider hedefi yapılandırmak istiyorum mu?**

Evet, bir benimle Iletişim kurun SaaS uygulaması veya danışmanlık hizmetleri yayımlıyorsanız.  

**Müşteri adayı yapılandırmasının doğru olduğunu nasıl doğrulayabilirim?**

Teklifinizi ve lider hedefini ayarladıktan sonra teklifinizi yayımlayın. Müşteri adayı doğrulama adımında Market, teklifiniz tarafından yapılandırılan lider hedefine bir test lideri gönderecek. 

**Test lideri nasıl bulabilirim?**

Müşteri adayı Hedefinizdeki "MSFT_TEST" araması yapın, örnek bir test lideri verileri aşağıda verilmiştir: 

```text
company = MSFT_TEST_636573304831318844 

country = US 

description = MSFT_TEST_636573304831318844 

email = MSFT_TEST_636573304831318844@test.com

encoding = UTF-8 

encoding = UTF-8 

first_name = MSFT_TEST_636573304831318844 

last_name = MSFT_TEST_636573304831318844 

lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844|\<Offer Name> 

oid = 00Do0000000ZHog 

phone = 1234567890 

title = MSFT_TEST_636573304831318844 
```

**Canlı bir teklifim var, ancak hiçbir müşteri adayı görmüyorum?**

Her müşteri adayının seçtiğiniz müşteri adayı hedefindeki alanlara geçirilmesi, müşteri adayları şu biçimde olacaktır: **Kaynak-eylem | Teklif**

- *Ğına*
  - AzureMarketplace
  - AzurePortal
  - TestDrive  
  - SPZA (AppSource kısaltması)

- *Eylem*
  - "INS"-yükleme anlamına gelir. Bu, bir müşteri, ürününüzü almak için düğmeyi her ziyaret eden Azure Market veya AppSource 'ta bulunur.
  - "PLT"-Iş ortağı LED deneme sürümünü temsil eder. Bu, bir müşteri benimle Iletişim kurma düğmesine her geldiğinde AppSource 'ta bulunur.
  - "DNC"-başvurmayın için temsil eder. Bu, uygulama sayfanızda çapraz olarak listelenen bir Iş ortağı ile iletişim kurulması istendiği her seferinde AppSource 'ta yer alır. Bu müşterinin uygulamanızda çapraz olarak listelenme kafalarını paylaşıyoruz, ancak bunlara başvurulmaları gerekmez.
  - "Oluştur"-Bu yalnızca Azure portal içindedir ve bir müşterinin kendi hesabına teklifinizi satın aldığı her zaman.
  - "StartTestDrive"-Bu yalnızca test sürücülerine yöneliktir ve bir müşterinin test sürücüsünü her başlattığında olur.

- *Sunar*
  - "Checkpoint. Check-Point-r77-10sg-KLG",
  - "bitnamı. openedxcypress",
  - "Docusign. 3701c77e-1cfa-4c56-91e6-3ed0b622145a"

*Müşteri bilgilerinin örnek verileri aşağıda verilmiştir*

```json
{ 
"FirstName":"John",
"LastName":"Smith",
"Email":"jsmith@microsoft.com",
"Phone":"1234567890",
"Country":"US",
"Company":"Microsoft",
"Title":"CTO"
}
```

[Müşteri adayı bilgileri](./partner-center-portal/commercial-marketplace-get-customer-leads.md)altında daha fazla bilgi edinin. 

**Azure BLOB 'U lider hedefi olarak yapılandırdım, neden müşteri adayını görmüyorum?** 

Lider yalnızca, müşteri adayı hedefi olarak Azure BLOB depolamayı seçtiğinizde yazılır. Müşteri adayının gerçek zamanını almak için Azure tablosuna geçin.

**Market 'ten bir e-posta aldım, neden CRM 'de müşteri adayını bulamıyorum?**  

Son kullanıcının e-posta etki alanı. edu 'dan olabilir. Gizlilik nedenleriyle,. edu etki alanından kişisel olarak tanımlanabilen verileri geçirmeiyoruz. [Yardım ve destek](https://aka.ms/marketplacepublishersupport)'te bir destek bileti gönderebilirsiniz.

**Azure Tablo/Azure blobu 'nı lider hedefim olarak yapılandırdım, müşteri adaylarını nasıl görüntüleyebilirim?** 

Azure portal blob veya tabloya erişebilir veya Azure depolama hesabınızın tablolarını/bloblarını görüntülemek için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) ücretsiz indirip yükleyebilir. 

**Azure tablosunu lider hedefim olarak yapılandırdım, Market tarafından yeni bir müşteri adayı gönderildiğinde bildirim alabilir miyim?** 

Evet, [buradaki](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md)belgelerde Azure tablo + işlevini ayarlamak için yönergeleri izleyin. 

**Salesforce 'um hedefi olarak Salesforce yapılandırdım, neden müşteri adaylarını bulamıyorum?**

Müşteri adayı formunun bir seçim listesine göre zorunlu bir alan olup olmadığını denetleyin. Yanıt Evet ise, alanı üzerinde zorunlu olmayan bir metin alanına geçin.  
 
**Lider hedefimde bir sorun oluştu ve bazı müşteri adaylarını kaçırdım. E-postada bana gönderilebilirim?** 

Gizlilik ilkeleri nedeniyle, güvenli olmayan e-posta ile müşteri adayı bilgilerini paylaşamazsınız. 

**Azure depolama 'yı (BLOB/tablo), lider hedefi olarak yapılandırdım, ne kadar ücret alınacaktır?** 

Öncü gen verileri düşüktür (neredeyse tüm yayımcılar için 1 GB <). Maliyet, bir 50 ayda 1.000 müşteri adayı alınmışsa, ücretlendirilecektir. 
