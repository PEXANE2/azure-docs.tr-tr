---
title: Azure AD Connect - AD FS yönetimi ve özelleştirme | Microsoft Dokümanlar
description: Azure AD Connect ile AD FS yönetimi ve Azure AD Connect ve PowerShell ile kullanıcı AD FS oturum açma deneyiminin özelleştirilmesi.
keywords: AD FS, ADFS, AD FS yönetimi, AAD Connect, Connect, oturum açma, AD FS özelleştirme, onarım güven, O365, federasyon, güvenen taraf
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcbeedddc65a916f869a778616779917a9571181
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331973"
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Azure AD Connect'i kullanarak Active Directory Federation Hizmetlerini yönetme ve özelleştirme
Bu makalede, Azure Etkin Dizin (Azure AD) Bağlantısı kullanarak Active Directory Federation Services'ı (AD FS) nasıl yönetilip özelleştirileştirileştirilen açıklanmıştır. Ayrıca, bir AD FS çiftliğinin tam yapılandırması için yapmanız gereken diğer yaygın AD FS görevlerini de içerir.

| Konu başlığı | Neleri kapsıyor |
|:--- |:--- |
| **AD FS'yi yönetme** | |
| [Güveni onarın](#repairthetrust) |Office 365 ile federasyon güveninin onarılması. |
| [Alternatif giriş kimliği kullanarak Azure AD ile fetorat](#alternateid) | Alternatif oturum açma kimliğini kullanarak federasyonu yapılandırma  |
| [AD FS sunucusu ekleme](#addadfsserver) |Ek bir AD FS sunucusu ile bir AD FS çiftliği nasıl genişletilir. |
| [AD FS Web Application Proxy sunucusu ekleme](#addwapserver) |Nasıl ek bir Web Application Proxy (WAP) sunucusu ile bir AD FS çiftlik genişletmek için. |
| [Federe etki alanı ekleme](#addfeddomain) |Federe etki alanı nasıl eklenir. |
| [TLS/SSL sertifikasını güncelleştirin](how-to-connect-fed-ssl-update.md)| BIR AD FS çiftliği için TLS/SSL sertifikası nasıl güncellenir? |
| **AD FS'yi özelleştir** | |
| [Özel bir şirket logosu veya çizimi ekleme](#customlogo) |Şirket logosu ve çizimi olan bir AD FS oturum açma sayfası nasıl özelleştirilir? |
| [Oturum açma açıklaması ekleme](#addsignindescription) |Oturum açma sayfası açıklaması nasıl eklenir. |
| [AD FS talep kurallarını değiştirme](#modclaims) |Çeşitli federasyon senaryoları için AD FS taleplerini değiştirme. |

## <a name="manage-ad-fs"></a>AD FS'yi yönetme
Azure AD Connect sihirbazını kullanarak en az kullanıcı müdahalesiyle Azure AD Connect'te AD FS ile ilgili çeşitli görevleri gerçekleştirebilirsiniz. Sihirbazı çalıştırarak Azure AD Connect'i yüklemeyi tamamladıktan sonra, ek görevler gerçekleştirmek için sihirbazı yeniden çalıştırabilirsiniz.

## <a name="repair-the-trust"></a><a name="repairthetrust"></a>Güveni onarın 
AD FS ve Azure AD güveninin geçerli durumunu denetlemek ve güveni onarmak için uygun eylemleri yapmak için Azure AD Connect'i kullanabilirsiniz. Azure AD ve AD FS güveninizi onarmak için aşağıdaki adımları izleyin.

1. Ek görevler listesinden **Onarım AAD ve ADFS Güven'i** seçin.
   ![Onarım AAD ve ADFS Güven](./media/how-to-connect-fed-management/RepairADTrust1.PNG)

2. Azure **AD'ye Bağlan** sayfasında, Azure AD için genel yönetici kimlik bilgilerinizi sağlayın ve **İleri'yi**tıklatın.
   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/RepairADTrust2.PNG)

3. Uzaktan **erişim kimlik bilgileri** sayfasına etki alanı yöneticisinin kimlik bilgilerini girin.

   ![Uzaktan erişim kimlik bilgileri](./media/how-to-connect-fed-management/RepairADTrust3.PNG)

    **İleri'yi**tıklattıktan sonra Azure AD Connect sertifika durumunu denetler ve herhangi bir sorunu gösterir.

    ![Sertifikaların durumu](./media/how-to-connect-fed-management/RepairADTrust4.PNG)

    **Yapılandırmaya Hazır** sayfası, güveni onarmak için gerçekleştirilecek eylemlerin listesini gösterir.

    ![Yapılandırma için hazır](./media/how-to-connect-fed-management/RepairADTrust5.PNG)

4. Güveni onarmak için **Yükle'yi** tıklatın.

> [!NOTE]
> Azure AD Connect yalnızca kendi imzası olan sertifikaları onarabilir veya bunlara göre hareket edebilir. Azure AD Connect üçüncü taraf sertifikalarını onaramıyor.

## <a name="federate-with-azure-ad-using-alternateid"></a><a name="alternateid"></a>AlternateID kullanarak Azure AD ile feodeto 
Şirket içi Kullanıcı Adı(UPN) ve bulut Kullanıcı Soyadı'nın aynı tutulması önerilir. Şirket içi UPN,routable olmayan bir etki alanı kullanıyorsa (ör. Contoso.local) veya yerel uygulama bağımlılıkları nedeniyle değiştirilemez, alternatif giriş kimliği ayarlamanızı öneririz. Alternatif giriş kimliği, kullanıcıların posta gibi UPN'leri dışındaki bir öznitelikle oturum açabilecekleri bir oturum açma deneyimi yapılandırmanıza olanak tanır. Azure AD Connect'te Kullanıcı Ana Adı seçeneği, Active Directory'deki kullanıcıPrincipalName özniteliğine varsayılan olarak gelir. Kullanıcı Ana Adı için başka bir öznitelik seçerseniz ve AD FS kullanarak federating iseniz, sonra Azure AD Connect alternatif giriş kimliği için AD FS yapılandıracaktır. Kullanıcı Anaadı adı için farklı bir öznitelik seçme örneği aşağıda gösterilmiştir:

![Alternatif kimlik özniteliği seçimi](./media/how-to-connect-fed-management/attributeselection.png)

AD FS için alternatif giriş kimliğini yapılandırmak iki ana adımdan oluşur:
1. **Doğru verme talebi kümesini yapılandırın:** Azure AD relying taraf güvenindeki verme talep kuralları, seçilen UserPrincipalName özniteliğini kullanıcının alternatif kimliği olarak kullanacak şekilde değiştirilir.
2. **AD FS yapılandırmasında alternatif giriş kimliğini etkinleştirin**: AD FS yapılandırması, AD FS'nin alternatif kimliği kullanarak uygun ormanlarda kullanıcıları alabilmeleri için güncelleştirilir. Bu yapılandırma, Windows Server 2012 R2'de (KB2919355 ile) veya sonraki ad FS için desteklenir. AD FS sunucuları 2012 R2 ise, Azure AD Connect gerekli KB'nin varlığını denetler. KB algılanmazsa, aşağıda gösterildiği gibi yapılandırma tamamlandıktan sonra bir uyarı görüntülenir:

    ![2012R2'de eksik KB uyarısı](./media/how-to-connect-fed-management/kbwarning.png)

    KB eksik durumunda yapılandırma düzeltmek için, gerekli [KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590) yükleyin ve sonra [Onarım AAD ve AD FS Trust](#repairthetrust)kullanarak güven onarmak.

> [!NOTE]
> AlternatID ve el ile yapılandırma adımları hakkında daha fazla bilgi [için, Alternatif Giriş Kimliğini Yapılandırma'yı](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id) okuyun

## <a name="add-an-ad-fs-server"></a><a name="addadfsserver"></a>AD FS sunucusu ekleme 

> [!NOTE]
> Bir AD FS sunucusu eklemek için Azure AD Connect PFX sertifikasıgerektirir. Bu nedenle, bu işlemi yalnızca Azure AD Connect'i kullanarak AD FS çalışmasını yapılandırırsanız gerçekleştirebilirsiniz.

1. **Ek bir Federasyon Sunucusu Dağıt'ı**seçin ve **İleri'yi**tıklatın.

   ![Ek federasyon sunucusu](./media/how-to-connect-fed-management/AddNewADFSServer1.PNG)

2. Azure **AD'ye Bağlan** sayfasında, Azure AD için genel yönetici kimlik bilgilerinizi girin ve **İleri'yi**tıklatın.

   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/AddNewADFSServer2.PNG)

3. Etki alanı yöneticisi kimlik bilgilerini sağlayın.

   ![Etki alanı yöneticisi kimlik bilgileri](./media/how-to-connect-fed-management/AddNewADFSServer3.PNG)

4. Azure AD Connect, yeni AD FS çiftliğinizi Azure AD Connect ile yapılandırırken sağladığınız PFX dosyasının parolasını ister. PFX dosyasının parolasını sağlamak için Parola Yı **Gir'i** tıklatın.

   ![Sertifika parolası](./media/how-to-connect-fed-management/AddNewADFSServer4.PNG)

    ![TLS/SSL sertifikasını belirtin](./media/how-to-connect-fed-management/AddNewADFSServer5.PNG)

5. AD **FS Sunucuları** sayfasında, AD FS ekine eklenecek sunucu adını veya IP adresini girin.

   ![AD FS sunucuları](./media/how-to-connect-fed-management/AddNewADFSServer6.PNG)

6. **İleri'yi**tıklatın ve son Yapılandırılan sayfayı gözden **geçirin.** Azure AD Connect sunucuları AD FS ekine eklemeyi bitirdikten sonra, bağlantının doğrulanması seçeneği sunulur.

   ![Yapılandırma için hazır](./media/how-to-connect-fed-management/AddNewADFSServer7.PNG)

    ![Kurulum tamamlandı](./media/how-to-connect-fed-management/AddNewADFSServer8.PNG)

## <a name="add-an-ad-fs-wap-server"></a><a name="addwapserver"></a>AD FS WAP sunucusu ekleme 

> [!NOTE]
> Bir WAP sunucusu eklemek için Azure AD Connect PFX sertifikasıgerektirir. Bu nedenle, bu işlemi yalnızca Azure AD Connect'i kullanarak AD FS çalışmasını yapılandırırsanız gerçekleştirebilirsiniz.

1. Kullanılabilir görevler listesinden **Web Uygulama Proxy'sini dağıt'ı** seçin.

   ![Web Uygulama Proxy'yi Dağıt](./media/how-to-connect-fed-management/WapServer1.PNG)

2. Azure global yönetici kimlik bilgilerini sağlayın.

   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/wapserver2.PNG)

3. **SSL sertifika** sını belirt sayfasında, AD FS çiftliğini Azure AD Connect ile yapılandırırken sağladığınız PFX dosyasının parolasını sağlayın.
   ![Sertifika parolası](./media/how-to-connect-fed-management/WapServer3.PNG)

    ![TLS/SSL sertifikasını belirtin](./media/how-to-connect-fed-management/WapServer4.PNG)

4. WAP sunucusu olarak eklenecek sunucuyu ekleyin. WAP sunucusu etki alanına katılmayabileceğinden, sihirbaz eklenen sunucuya yönetim kimlik bilgilerini ister.

   ![Yönetim sunucu kimlik bilgileri](./media/how-to-connect-fed-management/WapServer5.PNG)

5. Proxy **güven kimlik bilgileri** sayfasında, proxy güvenini yapılandırmak ve AD FS çiftliğindeki birincil sunucuya erişmek için yönetim kimlik bilgileri sağlayın.

   ![Proxy güven kimlik bilgileri](./media/how-to-connect-fed-management/WapServer6.PNG)

6. **Yapılandırmaya Hazır** sayfasında sihirbaz, gerçekleştirilecek eylemlerin listesini gösterir.

   ![Yapılandırma için hazır](./media/how-to-connect-fed-management/WapServer7.PNG)

7. Yapılandırmayı tamamlamak için **Yükle'yi** tıklatın. Yapılandırma tamamlandıktan sonra sihirbaz, sunuculara bağlantıyı doğrulama seçeneği sunar. Bağlantıyı denetlemek için **Doğrula'yı** tıklatın.

   ![Kurulum tamamlandı](./media/how-to-connect-fed-management/WapServer8.PNG)

## <a name="add-a-federated-domain"></a><a name="addfeddomain"></a>Federe etki alanı ekleme 

Azure AD Connect'i kullanarak Azure AD ile birlikte federe olacak bir etki alanı eklemek kolaydır. Azure AD Connect, federasyon için etki alanını ekler ve Azure AD ile beslenen birden çok etki alanınız olduğunda vereni doğru yansıtacak şekilde talep kurallarını değiştirir.

1. Federe etki alanı eklemek için, **ek bir Azure AD etki alanı ekleme**görevini seçin.

   ![Ek Azure AD etki alanı](./media/how-to-connect-fed-management/AdditionalDomain1.PNG)

2. Sihirbazın bir sonraki sayfasında, Azure AD için genel yönetici kimlik bilgilerini sağlayın.

   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/AdditionalDomain2.PNG)

3. Uzaktan **erişim kimlik bilgileri** sayfasında, etki alanı yöneticisi kimlik bilgilerini sağlayın.

   ![Uzaktan erişim kimlik bilgileri](./media/how-to-connect-fed-management/additionaldomain3.PNG)

4. Bir sonraki sayfada sihirbaz, şirket içi dizininizi fecofize edebileceğiniz Azure AD etki alanlarının bir listesini sağlar. Listeden etki alanını seçin.

   ![Azure AD etki alanı](./media/how-to-connect-fed-management/AdditionalDomain4.PNG)

    Etki alanını seçtikten sonra sihirbaz, sihirbazın gerçekleştireceği diğer eylemler ve yapılandırmanın etkisi hakkında size uygun bilgiler sağlar. Bazı durumlarda, Azure AD'de henüz doğrulanmamış bir etki alanı seçerseniz, sihirbaz etki alanını doğrulamanıza yardımcı olacak bilgiler sağlar. Bkz. Daha fazla ayrıntı için [azure etkin dizinine özel alan adınızı ekleyin.](../active-directory-domains-add-azure-portal.md)

5. **İleri**'ye tıklayın. **Yapılandırmaya Hazır** sayfası, Azure AD Connect'in gerçekleştireceği eylemlerin listesini gösterir. Yapılandırmayı tamamlamak için **Yükle'yi** tıklatın.

   ![Yapılandırma için hazır](./media/how-to-connect-fed-management/AdditionalDomain5.PNG)

> [!NOTE]
> Eklenen federe etki alanından kullanıcılar, Azure AD'ye giriş yapabilmeden önce eşitlenmelidir.

## <a name="ad-fs-customization"></a>AD FS özelleştirme
Aşağıdaki bölümlerde, AD FS oturum açma sayfanızı özelleştirirken gerçekleştirmek zorunda kaldığınız ortak görevlerhakkında ayrıntılı bilgi verilmiştir.

## <a name="add-a-custom-company-logo-or-illustration"></a><a name="customlogo"></a>Özel bir şirket logosu veya çizimi ekleme 
**Oturum açma** sayfasında görüntülenen şirketin logosunu değiştirmek için aşağıdaki Windows PowerShell cmdlet ve sözdizimini kullanın.

> [!NOTE]
> Logo için önerilen boyutlar 260 \@ x 35 96 dpi ve dosya boyutu 10 KB'den büyük değildir.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> *TargetName* parametresi gereklidir. AD FS ile yayımlanan varsayılan tema Varsayılan olarak adlandırılır.

## <a name="add-a-sign-in-description"></a><a name="addsignindescription"></a>Oturum açma açıklaması ekleme 
**Oturum açma sayfasına**oturum açma sayfası açıklaması eklemek için aşağıdaki Windows PowerShell cmdlet ve sözdizimini kullanın.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <a name="modify-ad-fs-claim-rules"></a><a name="modclaims"></a>AD FS talep kurallarını değiştirme 
AD FS, özel talep kuralları oluşturmak için kullanabileceğiniz zengin bir talep dilini destekler. Daha fazla bilgi [için, Talep Kuralı Dilinin Rolü bölümüne](https://technet.microsoft.com/library/dd807118.aspx)bakın.

Aşağıdaki bölümlerde, Azure AD ve AD FS federasyonuyla ilgili bazı senaryolar için özel kurallar nasıl yazabileceğiniz açıklanmaktadır.

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Öznitelikte mevcut olan bir değere koşullu değişmez kimlik
Azure AD Connect, nesneler Azure AD ile eşitlendiğinde kaynak bağlantı noktası olarak kullanılacak bir öznitelik belirtmenize olanak tanır. Özel öznitelikteki değer boş değilse, değişmez bir kimlik talebi vermek isteyebilirsiniz.

Örneğin, kaynak çapa için öznitelik olarak **ms-ds-tutarlılığı** seçebilir ve özniteliğinin buna karşı bir değeri olması durumunda **immutableID'yi** **ms-ds-tutarlılığı** olarak verebilir. Öznitelik karşısında bir değer yoksa, değişmez kimlik olarak **objectGuid'i** sorun. Aşağıdaki bölümde açıklandığı gibi özel talep kuralları kümesini oluşturabilirsiniz.

**Kural 1: Sorgu öznitelikleri**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Bu kuralda, Active Directory'den kullanıcı için **ms-ds tutarlılığı** ve **objectGuid** değerlerini sorguluyorsunuz. AD FS dağıtımınızda mağaza adını uygun bir mağaza adı ile değiştirin. Ayrıca, **objectGuid** ve **ms-ds-consistencyguid**için tanımlandığı gibi, taleplerini federasyonunuz için uygun bir talep türüyle değiştirin.

Ayrıca, **ekle** ve **sorunu**kullanarak, varlık için giden bir sorun eklemekten kaçınırsınız ve değerleri ara değerler olarak kullanabilirsiniz. Değişmez kimlik olarak hangi değeri kullanacağınızı belirledikten sonra talebi daha sonraki bir kuralda yayınlayacaktır.

**Kural 2: Kullanıcı için ms-ds-consistencyguid olup olmadığını kontrol edin**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Bu kural, kullanıcı için **ms-ds tutarlılığı** yoksa **kullanıma uygun** olarak ayarlanan **idflag** adlı geçici bir bayrak tanımlar. Bunun arkasındaki mantık, AD FS boş iddialara izin vermiyor olmasıdır. Bu nedenle, `http://contoso.com/ws/2016/02/identity/claims/objectguid` talep `http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid` eklediğinizde ve Kural 1'de, yalnızca değer kullanıcı için doldurulursa **bir msdsconsistencyguid** talebiyle sonuçlanırsınız. Doldurulmazsa, AD FS boş bir değere sahip olacağını görür ve hemen düşürür. Tüm **nesnelerobjectGuid'e**sahip olacak, böylece kural 1 yürütüldükten sonra bu iddia her zaman orada olacaktır.

**Kural 3: Varsa değişmez kimlik olarak ms-ds-tutarlılık sorunu**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID", Value = c.Value);

Bu örtülü bir **Varvar** denetimidir. Talep değeri varsa, o zaman değişmez kimlik olarak sorun. Önceki **örneknamedentifier** iddiasını kullanır. Bunu ortamınızdaki değişmez kimlik için uygun talep türüyle değiştirmeniz gerekir.

**Kural 4: ms-ds-tutarlılıkGuid yoksa, objectGuid'i değişmez kimlik olarak sorun**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID", Value = c2.Value);

Bu kuralda, yalnızca geçici bayrak **iflag**kontrol ediyoruz. Talebin değerine göre verilip verilemeyeceğine siz karar verirsiniz.

> [!NOTE]
> Bu kuralların sırası önemlidir.

### <a name="sso-with-a-subdomain-upn"></a>Bir alt etki alanı UPN ile SSO

[Yeni bir federe etki alanı ekle'de](how-to-connect-fed-management.md#addfeddomain)açıklandığı gibi, Azure AD Connect'i kullanarak federe edilecek birden fazla etki alanı ekleyebilirsiniz. Azure AD Connect sürüm 1.1.553.0 ve en son verenID için doğru talep kuralını otomatik olarak oluşturur. Azure AD Connect sürüm 1.1.553.0 veya en son sürümünü kullanamıyorsanız, [Azure AD RPT Talep Kuralları](https://aka.ms/aadrptclaimrules) aracının Azure AD güvendiğiniz taraf güveni için doğru talep kuralları oluşturmak ve ayarlamak için kullanılması önerilir.

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı oturum açma seçenekleri](plan-connect-user-signin.md)hakkında daha fazla bilgi edinin.
