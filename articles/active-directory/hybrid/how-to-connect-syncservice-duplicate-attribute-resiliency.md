---
title: Kimlik senkronizasyonu ve yinelenen öznitelik esnekliği | Microsoft Dokümanlar
description: Azure AD Connect'i kullanarak dizin eşitleme sırasında UPN veya ProxyAddress çakışmaları olan nesnelerin nasıl işleyeceğinin yeni davranışı.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78298352"
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Kimlik eşitleme ve yinelenen öznitelik dayanıklılığı
Yinelenen Öznitelik Esnekliği, Azure Active Directory'de, Microsoft'un eşitleme araçlarından birini çalıştırırken **UserPrincipalName** ve SMTP **ProxyAddress** çakışmalarının neden olduğu sürtünmeyi ortadan kaldıran bir özelliktir.

Bu iki özniteliğin genellikle belirli bir Azure Etkin Dizin kiracısındaki tüm **Kullanıcı,** **Grup**veya **Kişi** nesneleri arasında benzersiz olması gerekir.

> [!NOTE]
> Yalnızca Kullanıcılar UPN'lere sahip olabilir.
> 
> 

Bu özelliğin etkinleştirtiği yeni davranış eşitleme ardışık bölümünün bulut bölümünde olduğundan, istemci agnostiktir ve Azure AD Connect, DirSync ve MIM + Bağlayıcısı gibi microsoft senkronizasyon ürünleri için alakalıdır. Bu belgede bu ürünlerden herhangi birini temsil etmek için genel "eşitleme istemcisi" terimi kullanılır.

## <a name="current-behavior"></a>Geçerli davranış
Bu benzersizlik kısıtlamasını ihlal eden bir UPN veya ProxyAddress değeriyle yeni bir nesne sağlama girişimi varsa, Azure Etkin Dizin inesnenin oluşturulmasını engeller. Benzer şekilde, bir nesne benzersiz olmayan bir UPN veya ProxyAddress ile güncelleştirilirse, güncelleştirme başarısız olur. Sağlama girişimi veya güncelleştirme, her dışa aktarma döngüsünde eşitleme istemcisi tarafından yeniden denenir ve çakışma çözülene kadar başarısız olmaya devam ediyor. Her denemede bir hata raporu e-postası oluşturulur ve eşitleme istemcisi tarafından bir hata günlüğe kaydedilir.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Yinelenen Öznitelik Esnekliği ile Davranış
Azure Etkin Dizini, yinelenen öznitelik içeren bir nesneyi tamamen sağlamamak veya güncelleştirmek yerine, teklik kısıtlamasını ihlal edecek yinelenen özniteliği "karantinaya alır". Bu öznitelik UserPrincipalName gibi sağlama için gerekliyse, hizmet bir yer tutucu değeri atar. Bu geçici değerlerin biçimi  
OriginalPrefix>_**+\<4DigitNumber \@ \<>InitialTenantDomain>.onmicrosoft.com . \<**_

Öznitelik esnekliği işlemi yalnızca UPN ve SMTP **ProxyAddress** değerlerini işler.

**ProxyAddress**gibi öznitelik gerekli değilse, Azure Etkin Dizin ilerlediği çakışmayı karantinaya alır ve nesne oluşturma veya güncelleştirmeyle devam eder.

Özniteliği karantinaya aldıktan sonra, çakışma hakkındaki bilgiler eski davranışta kullanılan aynı hata raporu e-postasında gönderilir. Ancak, bu bilgi yalnızca bir kez hata raporunda görünür, karantina gerçekleştiğinde, gelecekteki e-postalarda günlüğe kaydolmaya devam etmez. Ayrıca, bu nesne için dışa aktarma başarılı olduğundan, eşitleme istemcisi bir hata günlüğe kaydetmez ve sonraki eşitleme döngüleri üzerine oluşturma / güncelleştirme işlemi yeniden denemez.

Bu davranışı desteklemek için Kullanıcı, Grup ve İlgili Kişi nesne sınıflarına yeni bir öznitelik eklendi:  
**DirSyncProvisioning Hatalar**

Bu, normal olarak eklenmesi halinde benzersizlik kısıtlamasını ihlal edecek çakışan öznitelikleri depolamak için kullanılan çok değerli bir özniteliktir. Azure Etkin Dizini'nde, çözülmüş yinelenen öznitelik çakışmalarını aramak için her saat çalışan ve söz konusu öznitelikleri karantinadan otomatik olarak kaldıran bir arka plan zamanlayıcısı görevi etkinleştirildi.

### <a name="enabling-duplicate-attribute-resiliency"></a>Yinelenen Öznitelik Esnekliğini Etkinleştirme
Yinelenen Öznitelik Esnekliği, tüm Azure Etkin Dizin kiracılarında yeni varsayılan davranış olacaktır. 22 Ağustos 2016 veya daha sonra ilk kez eşitlemi etkinleştiren tüm kiracılar için varsayılan olarak açık olacaktır. Bu tarihten önce eşitlemeyi etkinleştiren kiracılar, özelliği toplu olarak etkinleştirir. Bu kullanıma eylül 2016'da başlayacak ve her kiracının teknik bildirim ilgiliekibine özelliğin etkinleştirileceği belirli tarihe ait bir e-posta bildirimi gönderilecektir.

> [!NOTE]
> Yinelenen Öznitelik Esnekliği açık olduktan sonra devre dışı tutulamaz.

Özelliğin kiracınız için etkin olup olmadığını kontrol etmek için, bunu Azure Active Directory PowerShell modülünün en son sürümünü indirip çalıştırarak yapabilirsiniz:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> Yinelenen Öznitelik Esnekliği özelliğini kiracınız için etkinleştirmek için Set-MsolDirSyncFeature cmdlet'i artık kullanamazsınız. Özelliği sınayabilmek için yeni bir Azure Etkin Dizin kiracısı oluşturmanız gerekir.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>DirSyncProvisioningErrors ile Nesneleri Tanımlama
Şu anda yinelenen özellik çakışmaları nedeniyle bu hatalara sahip nesneleri tanımlamak için iki yöntem vardır, Azure Active Directory PowerShell ve [Microsoft 365 yönetici merkezi.](https://admin.microsoft.com) Gelecekte ek portal tabanlı raporlama genişletme planları vardır.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Bu konuda PowerShell cmdlets için aşağıdaki doğrudur:

* Aşağıdaki cmdletlerin tümü duruma duyarlıdır.
* **–ErrorCategory PropertyConflict** her zaman eklenmelidir. Şu anda Başka **hata**kategorisi türü yoktur, ancak bu gelecekte uzatılabilir.

İlk olarak, **Connect-MsolService'i** çalıştırarak ve kiracı yönetici için kimlik bilgilerini girerek başlayın.

Ardından, hataları farklı şekillerde görüntülemek için aşağıdaki cmdlets ve işleçleri kullanın:

1. [Tümlerini Gör](#see-all)
2. [Özellik Türüne Göre](#by-property-type)
3. [Çakışan Değere Göre](#by-conflicting-value)
4. [String Arama kullanma](#using-a-string-search)
5. Sıra -lanmış
6. [Sınırlı Miktarda veya Tümü](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Tümünü göster
Bağlandıktan sonra, kiracı çalışmasında öznitelik sağlama hatalarının genel bir listesini görmek için:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Bu, aşağıdaki gibi bir sonuç üretir:  
 ![Get-MsolDirSyncProvisioningError](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Özellik türüne göre
Özellik türüne göre hataları görmek için **UserPrincipalName** veya **ProxyAdresleri** bağımsız değişkenine **-PropertyName** bayrağını ekleyin:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Veya

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Çakışan değere göre
Belirli bir özellik ile ilgili hataları görmek için **-PropertyValue** bayrağı ekleyin (**-PropertyName** de bu bayrağı eklerken kullanılmalıdır):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Dize araması kullanma
Geniş bir dize araması yapmak için **-SearchString** bayrağını kullanın. Bu, her zaman gerekli olan **-ErrorCategory PropertyConflict**dışında, yukarıdaki bayrakların tümünden bağımsız olarak kullanılabilir:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Sınırlı miktarda veya tüm
1. **MaxResults \<Int>** sorguyu belirli bir değer sayısıyla sınırlamak için kullanılabilir.
2. **Tüm** hatalar, çok sayıda hata olması durumunda tüm sonuçların alınmasını sağlamak için kullanılabilir.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="microsoft-365-admin-center"></a>Microsoft 365 yönetim merkezi
Microsoft 365 yönetici merkezinde dizin eşitleme hatalarını görüntüleyebilirsiniz. Microsoft 365 yönetici merkezindeki rapor yalnızca bu hatalara sahip **Kullanıcı** nesnelerini görüntüler. **Gruplar** ve **Kişiler**arasındaki çakışmalar hakkında bilgi göstermez.

![Etkin Kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1234.png "Etkin Kullanıcılar")

Microsoft 365 yönetici merkezindeki dizin eşitleme hatalarının nasıl görüntülenebildiğini öğrenmek için [bkz.](https://support.office.com/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

### <a name="identity-synchronization-error-report"></a>Kimlik eşitleme hata raporu
Yinelenen öznitelik çakışması olan bir nesne bu yeni davranışla işlendiğinde, bildirim, kiracı için Teknik Bildirim ilgili kişisine gönderilen standart Kimlik Eşitleme Hata Raporu e-postasına dahil edilir. Ancak, bu davranışta önemli bir değişiklik vardır. Geçmişte, yinelenen öznitelik çakışması hakkındaki bilgiler, çakışma çözülene kadar sonraki her hata raporuna dahil edilirdi. Bu yeni davranışta, belirli bir çakışma için hata bildirimi yalnızca bir kez görünür- çakışan öznitelik karantinaya alındığı anda.

ProxyAddress çakışması için e-posta bildiriminin nasıl göründüğüne bir örnek aşağıda verilmiştir:  
    ![Etkin Kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "Etkin Kullanıcılar")  

## <a name="resolving-conflicts"></a>Çakışmaları çözme
Bu hatalar için sorun giderme stratejisi ve çözüm taktikleri, yinelenen öznitelik hatalarının geçmişte işlenme şeklinden farklı olmamalıdır. Tek fark, zamanlayıcı görevinin, çakışma çözüldükten sonra söz konusu özniteliği otomatik olarak uygun nesneye eklemek için hizmet tarafındaki kiracıyı süpürmesidir.

Aşağıdaki makalede çeşitli sorun giderme ve çözüm stratejileri özetlenir: [Yinelenen veya geçersiz öznitelikler, Office 365'te dizin eşitlemesini önler.](https://support.microsoft.com/kb/2647098)

## <a name="known-issues"></a>Bilinen sorunlar
Bu bilinen sorunların hiçbiri veri kaybına veya hizmet bozulmasına neden olmaz. Bunların birçoğu estetiktir, diğerleri çakışma özniteliğini karantinaya almak yerine standart "*pre-resiliency*" yinelenen öznitelik hatalarının atılmasına neden olur ve diğeri de bazı hataların ekstra el ile düzeltme gerektirmesine neden olur.

**Temel davranış:**

1. Belirli öznitelik yapılandırmalarına sahip nesneler, yinelenen öznitelik(ler) karantinaya alınmasıyerine dışa aktarma hataları almaya devam eder.  
   Örnek:
   
    a. Yeni kullanıcı **AD Joe\@contoso.com** ve ProxyAddress **smtp bir\@** UPN ile oluşturulur:Joe contoso.com
   
    b. ProxyAddress **SMTP:Joe\@contoso.com**olduğu varolan bir Grup ile bu nesnenin özellikleri çakışıyor.
   
    c. Dışa aktarma üzerine, çakışma özniteliklerinin karantinaya alınması yerine **proxyaddress çakışması** hatası atılır. İşlem, esneklik özelliği etkinleştirilmeden önce olduğu gibi, sonraki her eşitleme döngüsü nde yeniden denendi.
2. Aynı SMTP adresine sahip şirket içinde iki Grup oluşturulursa, ilk denemede standart yinelenen **ProxyAddress** hatasıyla birlikte bir grup oluşturulamaz. Ancak, yinelenen değer bir sonraki eşitleme döngüsünde düzgün bir şekilde karantinaya alınır.

**Ofis Portal Raporu**:

1. UPN çakışma kümesindeki iki nesne için ayrıntılı hata iletisi aynıdır. Bu, her ikisinin de UPN'lerinin değiştirilmediğini/ karantinaya alındığını, aslında sadece bir tanesinin herhangi bir verisini değiştirdiğini gösterir.
2. UPN çakışması için ayrıntılı hata iletisi, UPN'sini değiştirilen/karantinaya alan bir kullanıcı için yanlış displayName'yi gösterir. Örnek:
   
    a. **Kullanıcı A** **ilk UPN =\@Kullanıcı contoso.com**ile senkronize eder.
   
    b. **Kullanıcı B** **upn\@= Kullanıcı contoso.com**ile sonraki senkronize edilmeye çalışılır.
   
    c. **Kullanıcı B'leri** **UPN,\@User1234 contoso.onmicrosoft.com** olarak değiştirildi ve Kullanıcı **\@contoso.com** **DirSyncProvisioningErrors'a**eklendi.
   
    d. **Kullanıcı B** için hata iletisi, **Kullanıcı A'nın** zaten Kullanıcı **\@contoso.com** UPN olarak olduğunu, ancak **Kullanıcı B'nin** kendi displayName'sini gösterdiğini belirtmelidir.

**Kimlik eşitleme hata raporu:**

*Bu sorunun nasıl çözüleceği* yle ilgili adımlar için bağlantı yanlıştır:  
    ![Etkin Kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "Etkin Kullanıcılar")  

Bunu işaret [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency)etmeli.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
* [Office 365'te dizin eşitleme hatalarını belirleme](https://support.office.com/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

