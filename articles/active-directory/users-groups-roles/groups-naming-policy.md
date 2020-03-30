---
title: Azure Etkin Dizini'nde grup adlandırma ilkesini uygulayın | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde Office 365 grupları için adlandırma ilkesi nasıl ayarlanır?
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 141e83e21db18f21468113fd9927c2bdd2ed176d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79497883"
---
# <a name="enforce-a-naming-policy-on-office-365-groups-in-azure-active-directory"></a>Azure Etkin Dizini'nde Office 365 grupları yla ilgili bir adlandırma ilkesi uygulama

Kullanıcılarınız tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kuralları uygulamak için Azure Etkin Dizini'nde (Azure AD) kiracılarınız için bir grup adlandırma ilkesi ayarlayın. Örneğin, bir grubun işlevini, üyeliği, coğrafi bölgeyi veya grubu kimin oluşturduğunu iletmek için adlandırma ilkesini kullanabilirsiniz. Adres defterindeki grupları kategorilere ayırmaya yardımcı olmak için adlandırma ilkesini de kullanabilirsiniz. İlkeyi, belirli sözcüklerin grup adlarında ve diğer adlarda kullanılmasını engellemek için kullanabilirsiniz.

> [!IMPORTANT]
> Office 365 grupları için Azure AD adlandırma ilkesini kullanmak, bir veya daha fazla Office 365 grubuna üye olan her benzersiz kullanıcı için bir Azure Active Directory Premium P1 lisansına veya Azure AD Temel EDU lisansına sahip olmanızı ancak atamamanızı gerektirir.

Adlandırma ilkesi, iş yükleri arasında oluşturulan gruplar (örneğin, Outlook, Microsoft Teams, SharePoint, Exchange veya Planner) oluşturmak veya düzenlemek için uygulanır. Hem grup adı hem de grup takma adlarına uygulanır. Adlandırma ilkenizi Azure AD'de ayarlarsanız ve varolan bir Exchange grubu adlandırma ilkeniz varsa, Azure AD adlandırma ilkesi kuruluşunuzda uygulanır.

Grup adlandırma ilkesi yapılandırıldığında, ilke son kullanıcılar tarafından oluşturulan yeni Office 365 gruplarına uygulanır. Adlandırma ilkesi, Genel Yönetici veya Kullanıcı Yöneticisi gibi belirli dizin rolleri için geçerli değildir (grup adlandırma ilkesinden muaf tutulan rollerin tam listesi için lütfen aşağıya bakın). Varolan Office 365 grupları için, ilke yapılandırma sırasında hemen uygulanmaz. Grup sahibi bu gruplar için grup adını değiştirince, adlandırma ilkesi uygulanır.

## <a name="naming-policy-features"></a>İlke özelliklerini adlandırma

Gruplar için adlandırma ilkesini iki farklı şekilde uygulayabilirsiniz:

- **Önek-sonek adlandırma ilkesi** Gruplarınızda bir adlandırma kuralını zorlamak için otomatik olarak eklenen önekleri veya sonekler tanımlayabilirsiniz (örneğin, grup\_\_adı\_"GRP JAPAN\_\_ My Group Engineering", GRP JAPAN önektir ve \_Mühendislik sonektir). 

- **Özel engellenen sözcükler** Kullanıcılar tarafından oluşturulan gruplar halinde engellenmesi için kuruluşunuza özgü bir dizi engellenmiş sözcük yükleyebilirsiniz (örneğin, "CEO, Bordro, İk").

### <a name="prefix-suffix-naming-policy"></a>Önek-sonek adlandırma ilkesi

Adlandırma kuralının genel yapısı 'Önek[GroupName]Soneki'dir. Birden çok önek ve sonek tanımlayabiliyor olsada, ayarda [GroupName] yalnızca bir örneğine sahip olabilirsiniz. Önekler veya sonekler, grubu oluşturan kullanıcıya göre değiştirilen \[\] Bölüm gibi sabit dizeleri veya kullanıcı öznitelikleri olabilir. Önek ve sonek dizeleri biraraya gelen karakter toplam sayısı 53 karakterdir. 

Önekler ve sonekler, grup adı ve grup diğer adlarında desteklenen özel karakterler içerebilir. Grup diğer adı olarak desteklenmeyen önek veya sonekteki tüm karakterler grup adına yine de uygulanır, ancak grup diğer adı kaldırılır. Bu kısıtlama nedeniyle, grup adına uygulanan önek ve sonekler grup takma adı uygulananlardan farklı olabilir. 

#### <a name="fixed-strings"></a>Sabit dizeleri

Genel adres listesindeve grup iş yüklerinin sol gezinti bağlantılarındaki grupları tarayarak ve ayırt etmeyi kolaylaştırmak için dizeleri kullanabilirsiniz. Ortak öneklerden bazıları 'Grp\_Adı' ,\#'Adı',\_'Adı' gibi anahtar kelimelerdir.

#### <a name="user-attributes"></a>Kullanıcı öznitelikleri

Sizin ve kullanıcılarınızın grubun oluşturulduğu bölümü, ofisi veya coğrafi bölgeyi belirlemenize yardımcı olabilecek öznitelikleri kullanabilirsiniz. Örneğin, adlandırma ilkenizi `PrefixSuffixNamingRequirement = "GRP [GroupName] [Department]"`,ve `User’s department = Engineering`, olarak tanımlarsanız, zorlanan bir grup adı "GRP My Group Engineering" olabilir. Desteklenen Azure AD \[öznitelikleri \[\]Bölüm,\] \[Şirket,\] \[Ofis,\] \[\] \[\]StateOrProvince , CountryOrRegion , Başlık . Desteklenmeyen kullanıcı öznitelikleri sabit dizeleri olarak kabul edilir; örneğin, "\[posta\]Kodu ". Uzantı öznitelikleri ve özel öznitelikleri desteklenmez.

Kuruluşunuzdaki tüm kullanıcılar için doldurulmuş değerlere sahip ve uzun değerlere sahip öznitelikleri kullanmamanızı öneririz.

### <a name="custom-blocked-words"></a>Özel engellenen sözcükler

Engellenen sözcük listesi, grup adlarında ve diğer adlarda engellenecek virgülle ayrılmış tümcecikler listesidir. Alt dize aramaları yapılmaz. Bir hatatetiklemek için grup adı ile özel engellenen sözcüklerden biri veya birkaçı arasında tam bir eşleşme gerekir. 'Lass' engellenmiş bir sözcük olsa bile, kullanıcıların 'Sınıf' gibi yaygın sözcükleri kullanabilmesi için alt dize araması yapılmaz.

Engellenen sözcük listesi kuralları:

- Engellenen sözcükler büyük/küçük harf duyarlı değildir.
- Bir kullanıcı bir grup adının parçası olarak engellenen bir sözcük girdiğinde, engellenen sözcüğüiçeren bir hata iletisi görür.
- Engellenen sözcüklerde karakter kısıtlaması yoktur.
- Engellenen sözcükler listesinde yapılandırılabilen 5000 tümcecikten oluşan bir üst sınır vardır. 

### <a name="roles-and-permissions"></a>Roller ve izinler

Adlandırma ilkesini yapılandırmak için, folikülasyon rollerinden biri gereklidir:
- Genel yönetici
- Grup yöneticisi
- Kullanıcı yöneticisi

Seçili yöneticiler, engellenen sözcükleri kullanarak ve kendi adlandırma kurallarıyla gruplar oluşturabilmeleri için tüm grup iş yüklerinde ve uç noktalarında bu ilkelerden muaf tutulabilir. Grup adlandırma ilkesinden muaf tutulan yönetici rollerinin listesi aşağıda veda edilmiştir.

- Genel yönetici
- Ortak Tier 1 Desteği
- Ortak Tier 2 Desteği
- Kullanıcı yöneticisi
- Dizin yazarları

## <a name="configure-naming-policy-in-azure-portal"></a>Azure portalında adlandırma ilkesini yapılandırma

1. Grup yöneticisi hesabıyla [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Gruplar'ı**seçin, ardından Adlandırma ilkesi sayfasını açmak için **Adlandırma ilkesini** seçin.

    ![yönetici merkezinde Adlandırma ilkesi sayfasını açma](./media/groups-naming-policy/policy.png)

### <a name="view-or-edit-the-prefix-suffix-naming-policy"></a>Önek-sonek adlandırma ilkesini görüntüleme veya düzeltme

1. Adlandırma **ilkesi** sayfasında **Grup adlandırma ilkesini**seçin.
1. Adlandırma ilkesinin bir parçası olarak uygulamak istediğiniz öznitelikleri veya dizeleri seçerek geçerli önek veya sonek adlandırma ilkelerini tek tek görüntüleyebilir veya düzenleme yapabilirsiniz.
1. Bir önek veya sonek listeden kaldırmak için önek veya sonek seçin ve sonra **Sil'i**seçin. Aynı anda birden çok öğe silinebilir.
1. Kaydet'i seçerek değişikliklerinizi yeni ilkenin yürürlüğe girmesi için **kaydedin.**

### <a name="edit-custom-blocked-words"></a>Özel engellenen sözcükleri edin

1. Adlandırma **ilkesi** sayfasında **Engellenen sözcükleri**seçin.

    ![adlandırma ilkesi için engellenen sözcükler listesini edin ve yükleyin](./media/groups-naming-policy/blockedwords.png)

1. **İndir'i**seçerek özel engellenen sözcüklerin geçerli listesini görüntüleyin veya düzenleme.
1. Dosya simgesini seçerek özel engellenen sözcüklerin yeni listesini yükleyin.
1. Kaydet'i seçerek değişikliklerinizi yeni ilkenin yürürlüğe girmesi için **kaydedin.**

## <a name="install-powershell-cmdlets"></a>PowerShell cmdlet'lerini yükleme

PowerShell komutlarını çalıştırmadan önce Windows PowerShell Graph için Azure Active Directory PowerShell Modülünün eski sürümlerini kaldırın ve [Graph için Azure Active Directory PowerShell - Genel Önizleme Sürümünü 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) yükleyin.

1. Windows PowerShell uygulamasını yönetici olarak açın.
2. Eski AzureADPreview sürümlerini kaldırın.
  
   ``` PowerShell
   Uninstall-Module AzureADPreview
   ```

3. En son AzureADPreview sürümünü yükleyin.
  
   ``` PowerShell
   Install-Module AzureADPreview
   ```

   Güvenilmeyen bir depoya erişmek istenirse, **Y**girin. Yeni modülün yüklenmesi birkaç dakika sürebilir.

## <a name="configure-naming-policy-in-powershell"></a>PowerShell'de adlandırma ilkesini yapılandırma

1. Bilgisayarınızda bir Windows PowerShell penceresi açın. Yüksek ayrıcalıklar olmadan açabilirsiniz.

1. Ortamı cmdlet'leri çalıştırmaya hazır hale getirmek için aşağıdaki komutları çalıştırın.
  
   ``` PowerShell
   Import-Module AzureADPreview
   Connect-AzureAD
   ```

   Açılan **Hesabınızda oturum açın** ekranında hizmetinizle bağlantı kurmak için yönetici hesabınızın adını ve parolasını girin **Oturum aç**'ı seçin.

1. Bu kiracının grup ayarlarını oluşturmak için [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md) adımlarını izleyin.

### <a name="view-the-current-settings"></a>Geçerli ayarları görüntüleme

1. Geçerli ayarları görüntülemek için geçerli adlandırma ilkesini getirin.
  
   ``` PowerShell
   $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
   ```
  
1. Geçerli grup ayarlarını görüntüleyin.
  
   ``` PowerShell
   $Setting.Values
   ```
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Adlandırma ilkesini ve özel engellenen sözcükleri ayarlama

1. Azure AD PowerShell'de grup adı ön ve son eklerini ayarlayın. Özelliğin düzgün çalışması için [GroupName] ayarına eklenmelidir.
  
   ``` PowerShell
   $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
   ```
  
1. Sınırlamak istediğiniz özel engellenen sözcükleri belirleyin. Aşağıdaki örnekte kendi özel sözcüklerinizi ekleme adımları gösterilmektedir.
  
   ``` PowerShell
   $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
   ```
  
1. Aşağıdaki örnekte olduğu gibi, yeni ilkeğin yürürlüğe girmesi için ayarları kaydedin.
  
   ``` PowerShell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
  
İşte bu kadar. Adlandırma ilkenizi ayarladınız ve engellenen sözcüklerinizi eklediniz.

## <a name="export-or-import-custom-blocked-words"></a>Özel engellenen sözcükleri dışa aktarma veya alma

Daha fazla bilgi için, [grup ayarlarını yapılandırmak için Azure Active Directory cmdlets makalesine](groups-settings-cmdlets.md)bakın.

Aşağıda, birden çok engellenen sözcük dışa aktarmak için bir PowerShell komut dosyası örneği verilmiştir:

``` PowerShell
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
```

Aşağıda, birden çok engellenen sözcük almak için örnek bir PowerShell komut dosyası verilmiştir:

``` PowerShell
$BadWords = Get-Content "C:\work\currentblockedwordslist.txt"
$BadWords = [string]::join(",", $BadWords)
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}
if ($Settings.Count -eq 0)
    {$Template = Get-AzureADDirectorySettingTemplate | Where-Object {$_.DisplayName -eq "Group.Unified"}
    $Settings = $Template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $Settings
    $Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}}
$Settings["CustomBlockedWordsList"] = $BadWords
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings 
```

## <a name="remove-the-naming-policy"></a>Adlandırma ilkesini kaldırma

### <a name="remove-the-naming-policy-using-azure-portal"></a>Azure portalını kullanarak adlandırma ilkesini kaldırma

1. Adlandırma **ilkesi** sayfasında, **silme ilkesini**seçin.
1. Silme işlemini onayladıktan sonra, tüm öneek-sonek adlandırma ilkesi ve özel engellenen sözcükler de dahil olmak üzere adlandırma ilkesi kaldırılır.

### <a name="remove-the-naming-policy-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak adlandırma ilkesini kaldırma

1. Azure AD PowerShell'de grup adı önekleri ve soneklerini boşaltın.
  
   ``` PowerShell
   $Setting["PrefixSuffixNamingRequirement"] =""
   ```
  
1. Özel engellenen sözcükleri boşaltın.
  
   ``` PowerShell
   $Setting["CustomBlockedWordsList"]=""
   ```
  
1. Ayarları kaydedin.
  
   ``` PowerShell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```

## <a name="experience-across-office-365-apps"></a>Office 365 uygulamalarında deneyim

Azure AD'de bir grup adlandırma ilkesi ayarladıktan sonra, bir kullanıcı Bir Office 365 uygulamasında bir grup oluşturduğunda şunları görürler:

- Kullanıcı grup adına girer gelmez adlandırma ilkenize göre (önek ve sonekler ile) adın önizlemesi
- Kullanıcı engellenen sözcükleri girerse, engellenen sözcükleri kaldırabilmek için bir hata iletisi görür.

İş yükü | Uyumluluk
----------- | -------------------------------
Azure Active Directory portalları | Azure AD portalı ve Erişim Paneli portalı, kullanıcı bir grup oluştururken veya grup düzenlerken bir grup adına yazdığında adlandırma ilkesinin zorlanmış adını gösterir. Bir kullanıcı özel engellenmiş bir sözcük girdiğinde, kullanıcının kaldırabilmesi için engellenen sözcüğüiçeren bir hata iletisi görüntülenir.
Outlook Web Erişimi (OWA) | Outlook Web Access, kullanıcı bir grup adı veya grup adı yazdığında, adlandırma ilkesinin zorlanan adını gösterir. Bir kullanıcı özel engellenmiş bir sözcük girdiğinde, kullanıcının kaldırabilmesi için kullanıcının engellenen sözcüğün yanı sıra Kullanıcı Ara Birimi'nde bir hata iletisi gösterilir.
Outlook Masaüstü | Outlook masaüstünde oluşturulan gruplar adlandırma ilkesi ayarlarıyla uyumlu. Outlook masaüstü uygulaması henüz zorlanan grup adının önizlemesini göstermez ve kullanıcı grup adını girdiğinde özel engellenen sözcük hatalarını döndürmez. Ancak, bir grup oluşturulurken veya düzenlerken adlandırma ilkesi otomatik olarak uygulanır ve grup adında veya diğer adda özel engellenmiş sözcükler varsa kullanıcılar hata iletileri görür.
Microsoft Teams | Microsoft Teams, kullanıcı bir takım adı girdiğinde grup adlandırma ilkesini gösterir. Bir kullanıcı özel engellenmiş bir sözcük girdiğinde, kullanıcının kaldırabilmesi için engellenen sözcükle birlikte bir hata iletisi gösterilir.
SharePoint  |  SharePoint, kullanıcı bir site adı veya grup e-posta adresi yazdığında, adlandırma ilkesinin zorlanan adını gösterir. Bir kullanıcı özel engellenmiş bir sözcük girdiğinde, kullanıcının kaldırabilmesi için engellenen sözcükle birlikte bir hata iletisi gösterilir.
Microsoft Stream | Microsoft Akışı, kullanıcı bir grup adı veya grup e-posta takma adı yazdığında, grup adlandırma ilkesini zorlanmış adı gösterir. Bir kullanıcı özel engellenmiş bir sözcük girdiğinde, kullanıcının kaldırabilmesi için engellenen sözcüğün yanında bir hata iletisi gösterilir.
Outlook iOS ve Android Uygulaması | Outlook uygulamalarında oluşturulan gruplar, yapılandırılan adlandırma ilkesiyle uyumlu. Outlook mobil uygulaması henüz adlandırma ilkesinin önizlemesini göstermiyor ve kullanıcı grup adını girdiğinde özel engellenmiş sözcük hatalarını döndürmez. Ancak, adlandırma ilkesi oluştur/edit'i tıklatırken otomatik olarak uygulanır ve grup adında veya diğer adda özel engellenmiş sözcükler varsa kullanıcılar hata iletilerini görür.
Gruplar mobil uygulaması | Gruplar mobil uygulamasında oluşturulan gruplar adlandırma ilkesiyle uyumlu. Gruplar mobil uygulaması adlandırma ilkesinin önizlemesini göstermez ve kullanıcı grup adını girdiğinde özel engellenmiş sözcük hatalarını döndürmez. Ancak bir grup oluşturulurken veya düzenlerken adlandırma ilkesi otomatik olarak uygulanır ve grup adında veya diğer adda özel engellenmiş sözcükler varsa kullanıcılara uygun hatalar sunulur.
Planner | Planlayıcı adlandırma ilkesiyle uyumludur. Planlayıcı, plan adını girerken adlandırma ilkesi önizlemesini gösterir. Bir kullanıcı özel engellenmiş bir sözcük girdiğinde, planı oluştururken bir hata iletisi gösterilir.
Müşteri Etkileşimi için Dynamics 365 | Müşteri Katılımı için Dynamics 365, adlandırma ilkesiyle uyumludur. Dynamics 365, kullanıcı bir grup adı veya grup e-posta takma adı yazdığında, adlandırma ilkesinin zorlanan adını gösterir. Kullanıcı özel olarak engellenen bir sözcük girdiğinde, kullanıcının kaldırabilmesi için engellenen sözcüğün yanında bir hata iletisi gösterilir.
Okul Veri Eşitleme (SDS) | SDS aracılığıyla oluşturulan gruplar adlandırma ilkesine uygundur, ancak adlandırma ilkesi otomatik olarak uygulanmaz. SDS yöneticileri, grupların oluşturulması ve ardından SDS'ye yüklenmesi gereken sınıf adlarına önekleri ve sonekleri eklemalıdır. Grup oluşturma veya başka türlü başarısız olur.
Outlook Müşteri Yöneticisi (OCM) | Outlook Müşteri Yöneticisi, Outlook Müşteri Yöneticisi'nde oluşturulan gruba otomatik olarak uygulanan adlandırma ilkesiyle uyumludur. Özel engellenen bir sözcük algılanırsa, OCM'de grup oluşturma engellenir ve kullanıcının OCM uygulamasını kullanması engellenir.
Sınıf uygulaması | Classroom uygulamasında oluşturulan gruplar adlandırma ilkesine uygundur, ancak adlandırma ilkesi otomatik olarak uygulanmaz ve bir sınıf grubu adı girerken adlandırma ilkesi önizlemesi kullanıcılara gösterilmez. Kullanıcılar, önek ve soneklerle birlikte zorunlu sınıf grup adını girmeli. Değilse, sınıf grubu oluşturmak veya düzenlediğim işlem hatalarla başarısız olur.
Power BI | Power BI çalışma alanları adlandırma ilkesiyle uyumlu.    
Yammer | Bir kullanıcı Azure Active Directory hesabıyla Yammer'da oturum açmışsa bir grup oluşturur veya bir grup adını düzeltirken, grup adı adlandırma ilkesine uyar. Bu, hem Office 365 bağlı grupları hem de diğer tüm Yammer grupları için geçerlidir.<br>Adlandırma ilkesi gerçekleşmeden önce bir Office 365 bağlı grubu oluşturulduysa, grup adı adlandırma ilkelerini otomatik olarak izlemez. Bir kullanıcı grup adını düzelttığında, önek ve sonek eklemeleri istenir.
PersonelHub  | StaffHub ekipleri adlandırma ilkesini izlemez, ancak temel Office 365 grubu izler. StaffHub takım adı önekleri ve sonekler uygulanmaz ve özel engellenen sözcükleri denetlemez. Ancak StaffHub önekleri ve sonekleri uygular ve engellenen sözcükleri temel Office 365 grubundan kaldırır.
Exchange PowerShell | Exchange PowerShell cmdlets adlandırma ilkesi ile uyumlu. Kullanıcılar, grup adı ve grup adı (mailNickname) adlandırma ilkesini izlemedikleri nde, önerilen önek ve soneklerle ve özel engellenen sözcüklerle birlikte uygun hata iletileri alır.
Azure Active Directory PowerShell cmdlets | Azure Active Directory PowerShell cmdlets adlandırma ilkesiyle uyumlu. Kullanıcılar, grup adlarında ve grup takma adlarında adlandırma kuralını izlemedikleri nde, önerilen önek ve soneklerle ve özel engellenen sözcüklerle uygun hata iletileri alırlar.
Exchange yönetici merkezi | Exchange yönetici merkezi adlandırma ilkesiyle uyumludur. Kullanıcılar, grup adı ve grup diğer adlarında adlandırma kuralını izlemedikleri takdirde, önerilen önek ve soneklerle ve özel engellenen sözcüklerle birlikte uygun hata iletileri alırlar.
Microsoft 365 yönetim merkezi | Microsoft 365 yönetici merkezi adlandırma ilkesiyle uyumludur. Bir kullanıcı grup adlarını oluşturduğunda veya düzelttığında, adlandırma ilkesi otomatik olarak uygulanır ve kullanıcılar özel engellenen sözcükleri girdiklerinde uygun hataları alır. Microsoft 365 yönetici merkezi henüz adlandırma ilkesinin önizlemesini göstermez ve kullanıcı grup adını girdiğinde özel engellenmiş sözcük hatalarını döndürmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure REKLAM grupları hakkında ek bilgiler sağlar.

- [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Office 365 grupları için son kullanma ilkesi](groups-lifecycle.md)
- [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
- [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
