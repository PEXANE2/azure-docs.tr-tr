---
title: Kimlik eşitlemesi ve yinelenen öznitelik dayanıklılığı | Microsoft Docs
description: Azure AD Connect kullanarak Dizin eşitleme sırasında, UPN veya ProxyAddress çakışmalarla nesneleri nasıl işleyeceğinizi gösteren yeni davranış.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5585f0cd04dca4145f0322db9d625e35372b24b5
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78298352"
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Kimlik eşitleme ve yinelenen öznitelik dayanıklılığı
Yinelenen öznitelik dayanıklılığı, Microsoft 'un eşitleme araçlarından birini çalıştırırken **userPrincipalName** ve SMTP **ProxyAddress** çakışmalarını ortadan kaldıran, Azure Active Directory bir özelliktir.

Bu iki öznitelik genellikle belirli bir Azure Active Directory kiracısındaki tüm **Kullanıcı**, **Grup**veya **kişi** nesneleri genelinde benzersiz olması için gereklidir.

> [!NOTE]
> Yalnızca kullanıcılar UPN 'leri içerebilir.
> 
> 

Bu özelliğin izin veren yeni davranış, eşitleme işlem hattının bulut bölümünde bulunur, bu nedenle istemci belirsiz ve Azure AD Connect, DirSync ve MıM + Bağlayıcısı dahil tüm Microsoft Eşitleme ürünleri için geçerlidir. "Sync Client" genel terimi, bu ürünlerden birini göstermek için bu belgede kullanılır.

## <a name="current-behavior"></a>Geçerli davranış
Bu benzersizlik kısıtlamasını ihlal eden bir UPN veya ProxyAddress değeri ile yeni bir nesne sağlama girişimi varsa Azure Active Directory bu nesnenin oluşturulmasını engeller. Benzer şekilde, bir nesne benzersiz olmayan bir UPN veya ProxyAddress ile güncelleştirilirse güncelleştirme başarısız olur. Hazırlama girişimi veya güncelleştirme, her bir dışarı aktarma döngüsünün üzerinde eşitleme istemcisi tarafından yeniden denenir ve çakışma çözümlenene kadar başarısız olmaya devam eder. Her denemeden sonra bir hata raporu e-postası oluşturulur ve eşitleme istemcisi tarafından bir hata kaydedilir.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılığı ile davranış
Yinelenen bir özniteliğe sahip bir nesneyi sağlamak veya güncelleştirmek yerine, benzersizlik kısıtlamasını ihlal eden yinelenen özniteliğe "quarantınes" Azure Active Directory. Bu öznitelik, UserPrincipalName gibi sağlama için gerekliyse hizmet bir yer tutucu değeri atar. Bu geçici değerlerin biçimi  
_**\<OriginalPrefix > +\<4DigitNumber >\@\<ınitialtenantdomain >. onmicrosoft. com**_ .

Öznitelik dayanıklılığı işlemi yalnızca UPN ve SMTP **ProxyAddress** değerlerini işler.

Öznitelik gerekli değilse, bir **ProxyAddress**gibi Azure Active Directory, yalnızca çakışma özniteliğini karantinaya alır ve nesne oluşturma veya güncelleştirme ile devam eder.

Özniteliği karantinaya alındıktan sonra, çakışma hakkındaki bilgiler, eski davranışta kullanılan aynı hata raporu e-postasına gönderilir. Ancak, bu bilgi yalnızca hata raporunda bir kez görünür, karantina gerçekleştiğinde gelecek e-postalara oturum açmaya devam etmez. Ayrıca, bu nesnenin dışarı aktarılması başarılı olduğundan, eşitleme istemcisi bir hatayı günlüğe almaz ve sonraki eşitleme döngüleri üzerinde oluşturma/güncelleştirme işlemini yeniden denemez.

Bu davranışı desteklemek için Kullanıcı, Grup ve Ilgili kişi nesne sınıflarına yeni bir öznitelik eklenmiştir:  
**DirSyncProvisioningErrors**

Bu, benzersizlik kısıtlamasını ihlal eden çakışan öznitelikleri depolamak için normal olarak kullanılan çok değerli bir özniteliktir. Çözümlenen yinelenen öznitelik çakışmalarını aramak için saatte bir çalışan Azure Active Directory bir arka plan Zamanlayıcı görevi etkinleştirilmiştir ve söz konusu nitelikleri karantinadan otomatik olarak kaldırır.

### <a name="enabling-duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılığı etkinleştiriliyor
Yinelenen öznitelik dayanıklılığı, tüm Azure Active Directory kiracılarda yeni varsayılan davranış olacaktır. Bu, varsayılan olarak, eşitlemeyi etkinleştirilen tüm kiracılar için 22nd, 2016 veya üzeri sürümlerde ilk kez açık olacaktır. Bu tarihten önce eşitlemeyi etkinleştirilen kiracılar, özelliği toplu olarak etkin olur. Bu piyasaya çıkma, 2016 Eylül 'de başlar ve özellik etkinleştirilecek belirli bir tarihle her kiracının teknik bildirim iletişim ekibine bir e-posta bildirimi gönderilir.

> [!NOTE]
> Yinelenen öznitelik dayanıklılığı etkinleştirildikten sonra devre dışı bırakılamaz.

Özelliğin kiracınız için etkin olup olmadığını denetlemek için, Azure Active Directory PowerShell modülünün en son sürümünü indirerek ve çalıştıran şunları yapabilirsiniz:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> Artık, kiracı için kullanılmadan önce yinelenen öznitelik dayanıklılığı özelliğini önceden etkinleştirmek için set-MsolDirSyncFeature cmdlet 'ini kullanamazsınız. Özelliği test etmek için yeni bir Azure Active Directory kiracısı oluşturmanız gerekecektir.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>DirSyncProvisioningErrors ile nesneleri tanımlama
Yinelenen özellik çakışmaları, Azure Active Directory PowerShell ve [Microsoft 365 Yönetim Merkezi](https://admin.microsoft.com)nedeniyle bu hatalara sahip nesneleri belirlemek için şu anda iki yöntem vardır. Gelecekte ek portal tabanlı raporlamaya genişletecek planlar vardır.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Bu konudaki PowerShell cmdlet 'leri için aşağıdakiler doğrudur:

* Aşağıdaki cmdlet 'lerin tümü büyük/küçük harfe duyarlıdır.
* **– ErrorCategory PropertyConflict** her zaman eklenmelidir. Şu anda başka bir **ErrorCategory**türü yoktur, ancak bu durum gelecekte genişletilebilir.

İlk olarak, **Connect-MsolService** ' i çalıştırıp kiracı yöneticisinin kimlik bilgilerini girerek başlayın.

Ardından, hataları farklı yollarla görüntülemek için aşağıdaki cmdlet 'leri ve işleçleri kullanın:

1. [Tümünü gör](#see-all)
2. [Özellik türüne göre](#by-property-type)
3. [Çakışan değere göre](#by-conflicting-value)
4. [Dize araması kullanma](#using-a-string-search)
5. Sütununa
6. [Sınırlı miktarda veya tümü](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Tümünü gör
Bağlandıktan sonra, kiracı çalıştırmasında öznitelik sağlama hatalarının genel bir listesini görmek için:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Bu, aşağıdaki gibi bir sonuç üretir:  
 ![Get-MsolDirSyncProvisioningError](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Özellik türüne göre
Hataları özellik türüne göre görmek için, **userPrincipalName** veya **proxyAddresses** bağımsız değişkeniyle **-PropertyName** bayrağını ekleyin:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Veya

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Çakışan değere göre
Belirli bir özellikle ilgili hataları görmek için **-PropertyValue** bayrağını ekleyin (Bu bayrak eklenirken **-PropertyName** kullanılması gerekir):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Dize araması kullanma
Geniş bir dize arama yapmak için **-searchstring** bayrağını kullanın. Bu, her zaman gerekli olan **-ErrorCategory PropertyConflict**hariç olmak üzere yukarıdaki bayrakların tümüyle bağımsız olarak kullanılabilir:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Sınırlı miktarda veya tümü
1. **MaxResults \<Int >** , sorguyu belirli sayıda değerle sınırlandırmak için kullanılabilir.
2. **Tümü** , çok sayıda hata olması durumunda tüm sonuçların alındığından emin olmak için kullanılabilir.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="microsoft-365-admin-center"></a>Microsoft 365 Yönetim Merkezi
Dizin eşitleme hatalarını Microsoft 365 Yönetim merkezinde görüntüleyebilirsiniz. Microsoft 365 yönetim merkezindeki raporda yalnızca bu hatalar içeren **Kullanıcı** nesneleri görüntülenir. **Gruplar** ve **kişiler**arasındaki çakışmalar hakkında bilgi göstermez.

![Etkin kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1234.png "Etkin Kullanıcılar")

Microsoft 365 Yönetim merkezinde Dizin eşitleme hatalarını görüntüleme hakkında yönergeler için bkz. [Office 365 ' de dizin eşitleme hatalarını belirleme](https://support.office.com/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Kimlik eşitlemesi hata raporu
Yinelenen bir öznitelik çakışması olan bir nesne bu yeni davranışla birlikte işlenirse, kiracıya yönelik teknik bildirim ekibine gönderilen standart kimlik eşitleme hata raporu e-postasına bir bildirim eklenir. Ancak, Bu davranışta önemli bir değişiklik vardır. Geçmişte, yinelenen bir öznitelik çakışması hakkındaki bilgiler, çakışma çözümlenene kadar sonraki tüm hata raporuna dahil edilebilir. Bu yeni davranışla, belirli bir çakışma için hata bildirimi yalnızca bir kez görünür ve çakışan öznitelik karantinaya alınır.

E-posta bildiriminin bir ProxyAddress çakışması için nasıl göründüğünü bir örnek aşağıda verilmiştir:  
    ![Etkin kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "Etkin Kullanıcılar")  

## <a name="resolving-conflicts"></a>Çakışmaları çözme
Bu hatalar için sorun giderme stratejisi ve çözümlemesi, yinelenen öznitelik hatalarının geçmişte işlenme yöntemiyle farklı olmamalıdır. Tek fark, zamanlayıcı görevinin, çakışma çözümlendikten sonra, söz konusu özniteliği ilgili nesneye otomatik olarak eklemek için hizmet tarafındaki kiracı aracılığıyla EPS 'yi kapsar.

Aşağıdaki makalede çeşitli sorun giderme ve çözümleme stratejileri özetlenmektedir: [yinelenen veya geçersiz öznitelikler Office 365 ' de dizin eşitlemesini engelliyor](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bilinen sorunlar
Bu bilinen sorunlardan hiçbiri veri kaybına veya hizmet düşüşüne neden olur. Bunlardan bazıları Aesthetic Characteristics, diğer bir deyişle çakışma özniteliğini karantinaya almak yerine standart "*dayanıklılık öncesi*" yinelenen öznitelik hatalarının oluşturulmasına neden olur ve başka bir hata daha el ile daha fazla düzeltilmesi gerektirebilir.

**Çekirdek davranışı:**

1. Belirli öznitelik yapılandırmalarına sahip nesneler, karantinaya alınan yinelenen öznitelik (ler) in aksine dışarı aktarma hataları almaya devam eder.  
   Örnek:
   
    a. Yeni Kullanıcı AD 'de **ali\@contoso.com** ve ProxyAddress **smtp: alı\@contoso.com** UPN 'si ile oluşturulur
   
    b. Bu nesnenin özellikleri, ProxyAddress **SMTP: ali\@contoso.com**olan mevcut bir grupla çakışıyor.
   
    c. Dışarı aktarma sırasında, çakışma özniteliklerinin karantinaya alınması yerine bir **ProxyAddress çakışma** hatası oluşur. İşlem, dayanıklılık özelliği etkinleştirilmeden önce olduğu için sonraki her eşitleme döngüsüne yeniden denenir.
2. Şirket içinde aynı SMTP adresiyle iki grup oluşturulduysa, bir standart yinelenen **ProxyAddress** hatası ile ilk denemede bir tane sağlanamaz. Ancak, yinelenen değer bir sonraki eşitleme döngüsünün üzerinde doğru bir şekilde karantinaya alınır.

**Office portalı raporu**:

1. Bir UPN çakışma kümesindeki iki nesne için ayrıntılı hata iletisi aynıdır. Bu, her ikisinin UPN 'nin değiştirilme/karantinaya alındığı, aslında yalnızca birinin bir veri değiştiği anlamına gelir.
2. UPN çakışması için ayrıntılı hata iletisi, UPN 'si değiştirilmiş/karantinaya almış olan bir kullanıcı için yanlış displayName 'i gösterir. Örnek:
   
    a. İlk olarak **Kullanıcı A** , **UPN = User\@contoso.com**ile eşitler.
   
    b. **B kullanıcısının** daha sonra **UPN = User\@contoso.com**ile eşitlenmesi denendi.
   
    c. **B kullanıcısının** UPN, **User1234\@contoso.onmicrosoft.com** olarak değiştirilir ve **Kullanıcı\@contoso.com** **DirSyncProvisioningErrors**'e eklenir.
   
    d. **B kullanıcısının** hata iletisi, kullanıcının **bir** UPN olarak zaten **Kullanıcı\@contoso.com** olduğunu göstermelidir, ancak **b kullanıcısının** kendi DisplayName 'ini gösterir.

**Kimlik eşitlemesi hata raporu**:

*Bu sorunu çözme adımları* için bağlantı yanlış:  
    ![Etkin kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "Etkin Kullanıcılar")  

[https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency)işaret etmelidir.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
* [Office 365 ' de dizin eşitleme hatalarını tanımla](https://support.office.com/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

