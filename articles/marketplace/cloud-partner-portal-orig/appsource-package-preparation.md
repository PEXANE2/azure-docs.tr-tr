---
title: AppSource paket hazırlığı | Azure Marketi
description: AppSource paketlerini hazırlama ve oluşturma bölümünde Explanaion.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 76f8cbd6cb16b585a7dbda7b2ffa5eeeeb1b68d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80280618"
---
# <a name="appsource-package-preparation"></a>AppSource paket hazırlığı

Bir çözüm. zip dosyasına ek olarak bir **Appsource paketi**gerekir. Bu, çözümünüzü müşterilerin CRM ortamına dağıtma işlemini otomatik hale getirmek için gereken tüm varlıkları içeren bir. zip dosyasıdır. **Appsource paketi** aşağıdaki bileşenlere sahiptir

* Paket Dağıtıcı paketi
* Kullandığınız varlıklarla birlikte **Content_Types. xml** dosyası
* Uygulamaya özgü verileri olan XML dosyası
* Yönetim merkezinde listeleyle birlikte görüntülenecek 32x32 logo
* Lisans koşulları, Gizlilik ilkesi

Aşağıdaki adımlar, AppSource paketinizi oluşturmanıza yardımcı olur.

## <a name="a-create-a-package-for-the-package-deployer"></a>a. Paket Dağıtıcı için bir paket oluşturma

Paket Dağıtıcı paketi, AppSource paketinin bir parçasıdır.

Paket Dağıtıcı için bir paket oluşturmak üzere aşağıdaki yönergeleri kullanın: [https://msdn.microsoft.com/library/dn688182.aspx](https://msdn.microsoft.com/library/dn688182.aspx). İşlem tamamlandığında, paketiniz aşağıdaki varlıklardan oluşur:

1. Paket klasörü: tüm çözümleri, yapılandırma verilerini, düz dosyaları ve paketinizin içeriğini içerir. _Note: aşağıdaki örnekte, paket klasörü "PkgFolder" olarak adlandırıldığını varsayacağız_
2. DLL: derleme, paketinizin özel kodunu içerir. _Note: aşağıdaki örnekte, bu dosyanın "MicrosoftSample. dll" olarak adlandırıldığını varsayacağız._

Şimdi, "**Content_Types. xml**" adlı bir dosya oluşturmanız gerekir. bu dosya, paketinizin bir parçası olan tüm varlık genişletmelerini listelenecek. Dosya için örnek kod aşağıda verilmiştir.

    <?xml version="1.0" encoding="utf-8"?>
        <Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
        <Default Extension="xml" ContentType="application/octet-stream" />
        <Default Extension="xaml" ContentType="application/octet-stream" />
        <Default Extension="dll" ContentType="application/octet-stream" />
        <Default Extension="zip" ContentType="application/octet-stream" />
        <Default Extension="jpg" ContentType="application/octet-stream" />
        <Default Extension="gif" ContentType="application/octet-stream" />
        <Default Extension="png" ContentType="application/octet-stream" />
        <Default Extension="htm" ContentType="application/octet-stream" />
        <Default Extension="html" ContentType="application/octet-stream" />
        <Default Extension="db" ContentType="application/octet-stream" />
        <Default Extension="css" ContentType="application/octet-stream" />
    </Types>

Son adım, aşağıdakilerden biri olarak bir dosyada yer alan bir dosyadır. It **Package. zip**' i çağırın. Şunu içerir

1. PkgFolder (klasörün içindeki her şey dahil)
2. dosyasını
3. **Content_Types. xml**

Package. zip oluşturma adımları:

1. Paket klasörünüzü, **Content_Types. xml** dosyanızı ve PackageName. dll dosyasını tek bir dizine yerleştirin.

![CRMScreenShot2](media/CRMScreenShot2.png)

1. Klasördeki tüm öğeleri seçin, sağ tıklayın ve sıkıştırılmış (zip) klasöre Gönder ' i seçin.

![CRMScreenShot3](media/CRMScreenShot3.png)

1. Adı Package. zip olarak değiştirin

![CRMScreenShot4](media/CRMScreenShot4.png)

## <a name="b-create-an-appsource-package"></a>b. AppSource paketi oluşturma

AppSource paketi için birkaç ek dosya gereklidir.

1. jpg (32x32 çözünürlük)
2. HTML (HTML biçimli dosya)
3. **Content_Types. xml** (yukarıdaki gibi)
4. xml

Input. xml için örnek kod aşağıda verilmiştir. Aşağıdaki tabloda bulunan tanımlara bakın.

    <PvsPackageData>
        <ProviderName>Microsoft</ProviderName>
        <PackageFile>package.zip</PackageFile>
        <SolutionAnchorName>SampleSolution.zip</SolutionAnchorName>
        <StartDate>01/01/2016</StartDate>
        <EndDate>01/01/2021</EndDate>
        <SupportedCountries>US,CA</SupportedCountries>
        <LearnMoreLink>https://www.microsoft.com</LearnMoreLink>
        <Locales>
        <PackageLocale Code="1033" IsDefault="true">
        <Logo>logo32x32.png</Logo>
        <Terms>
        <PackageTerm File="TermsOfUse.html" />
        </Terms>
        </PackageLocale>
        </Locales>
    </PvsPackageData>
 
**Olmadığı**

|Alan|Ayrıntılar|
|---|---|
|Adı|Çözüm kimin geldiği. Bir Microsoft ekibi ise Microsoft 'un olması gerekir.|
|PackageFile |Paket Dağıtıcı varlıkları bir içerik\_türleri. xml dosyası ile birlikte sıkıştırıldı. Bu ZIP dosyası paket dağıtıcı derlemesini ve paket dağıtıcı varlıklarını içeren klasörü içermelidir. Diğer bir deyişle, Package. zip|
|SolutionAnchorName |Çözüm varlıklarının görünen adı ve açıklaması için kullanılan paket dağıtıcı içindeki çözüm ZIP dosyasının adı.|
| Başlangıç| Bu, çözüm paketinin kullanılabilir olacağı tarihtir. Biçim AA/GG/YYYY şeklindedir|
|EndDate|Bu, çözüm paketinin kullanılabilir olacağını durdurulacağı tarihtir. Biçim AA/GG/YYYY şeklindedir |
|Supportedülkeleriyle |Bu, bu paketi görmeniz gereken ülkelerin/bölgelerin virgülle ayrılmış listesidir. Geçerli tüm ülke kodlarının listesi için çevrimiçi hizmetler başvurun. Bu noktada, listenin bu yazma tarihi: AE, AL, har, AO, AR, AT, AU, AZ, BA, BB, BD,,, BG, BH, BM, mılyar TL, BO, BR, BY, CA, CH, CI, CL, CM, CO, CR, CV, CW, CY, CZ, DE, DK, DO, DZ, EC, EE, EG, ES, FI, FR, GB, GE, GH, GR, GT, HK, HN, HR, HU, d, IE, IL , IN, ıQ,,, ıT, JM, JO, JP, KE, KG, KN, KR, KW, KY, KZ, LB, LK, LT, LU, LV, LY, MA, MC, MD, ME, MK, MN, MO, MT, MU, MX, MY, NG, Nı, BOOK, Mı, NL, NO, NZ, OM, PA, PE, PH, PK, PL, PR, PS, PT, ASA, QA, RO, RS, RU, , TR, TT, TW, UA, US, UY, UZ, VE, VI, VN, ZA, ZW |
|LearnMoreLink | Bu paketin daha fazla bilgi sayfasının URL 'SI. |
|Yerel ayarlar|Tercih edilen çözüm UX ' de desteklemek istediğiniz her UX dili için bu düğümün bir örneği. Bu düğüm, her dil için yerel ayarı, logoyu ve koşulları tanımlayan alt öğeleri içerir|
|Yerel Ayarlar: PackageLocale. Code|Bu düğüm için dilin LCıD 'SI. Örnek: ABD Ingilizcesi 1033|
|Yerel Ayarlar: PackageLocale. IsDefault|Varsayılan dil olduğunu gösterir. Bu, müşteri tarafından seçilen UX dili kullanılamıyorsa geri dönüş dili olarak kullanılır.|
|Yerel Ayarlar: logo|Bu paket için kullanmak istediğiniz logo varsa bunu yapın. Simgenin boyutu 32x32 ' dir. PNG ve JPG olarak izin verilen biçimler|
|Yerel Ayarlar: terimler: PackageTerm. File|Bu, lisans koşullarınızı içeren HTML belgesi dosya adıdır.|

Logonun görüntüleneceği yer şöyledir:

![CRMScreenShot5](media/CRMScreenShot5.png)

Son adım, aşağıdakilerden biri olarak bir dosyada yer alan bir dosyadır.

1. zip (daha önce oluşturulmuş)
2. **Content_Types. xml**
3. xml
4. png
5. html

![CRMScreenShot6](media/CRMScreenShot6.png)

Dosyayı uygulamanız için uygun bir adla yeniden adlandırın. Şirket adınızı ve Uygulama adınızı dahil etmeniz önerilir. Örneğin: **_Microsoft_SamplePackage. zip**.
