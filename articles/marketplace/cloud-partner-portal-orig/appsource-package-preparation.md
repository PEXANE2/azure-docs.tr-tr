---
title: AppSource Paket Hazırlama | Azure Marketi
description: AppSource paketlerinin nasıl hazırlanacağı ve inşa edilebildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 76f8cbd6cb16b585a7dbda7b2ffa5eeeeb1b68d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280618"
---
# <a name="appsource-package-preparation"></a>AppSource Paket Hazırlama

Bir solution.zip dosyasına ek olarak, bir **AppSource Paketi**gerekir. Bu, çözümünüzü müşterilerin CRM ortamına dağıtma işlemini otomatikleştirmek için gereken tüm varlıkları içeren bir .zip dosyasıdır. **AppSource Paketi** aşağıdaki bileşenlere sahiptir

* Paket Dağıtıcı için Paket
* Kullandığınız varlıkları ile **Content_Types.xml** dosyası
* App'e özel verilerinizle xml dosyası
* Yönetici Merkezi'nde kaydınızla birlikte görüntülenecek 32x32 logosu
* Lisans koşulları, gizlilik politikası

Aşağıdaki adımlar AppSource Paketinizi oluşturmanıza yardımcı olacaktır.

## <a name="a-create-a-package-for-the-package-deployer"></a>a. Paket Dağıtıcısı için Paket Oluşturma

Paket Dağıtıcı paketi AppSource Paketinin bir parçasıdır.

Paket Dağıtıcısı için bir Paket oluşturmak için [https://msdn.microsoft.com/library/dn688182.aspx](https://msdn.microsoft.com/library/dn688182.aspx)aşağıdaki yönergeleri kullanın: . Tamamlandığında, paketiniz aşağıdaki varlıklardan oluşacaktır:

1. Paket klasörü: tüm çözümleri, yapılandırma verilerini, düz dosyaları ve paketinizin içeriğini içerir. _Not: Aşağıdaki örnekte paket klasörün "PkgFolder" olarak adlandırılacağını varsayıyoruz._
2. dll: Derleme, paketiniz için özel kodu içerir. _Not: Aşağıdaki örnekte bu dosyanın adı "MicrosoftSample.dll" olarak adlandırılacaktır._

Şimdi, "**Content_Types.xml**" adlı bir dosya oluşturmanız gerekir, bu dosya paketinizin bir parçası olan tüm varlık uzantılarını listeler. Burada dosya için örnek kodudur.

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

Son adım, aşağıdakileri tek bir dosyaya sığdırmaktır. Buna **paket.zip**deyin. Bu içerecektir

1. PkgFolder (klasörün içindeki her şey dahil)
2. Dll
3. **Content_Types.xml**

package.zip oluşturmak için adımlar:

1. Paket klasörünüzü, **Content_Types.xml** dosyanızı ve PackageName.dll'yi tek bir dizine koyun.

![CRMScreenShot2](media/CRMScreenShot2.png)

1. Klasördeki tüm öğeleri seçin, sağ tıklayın ve Sıkıştırılmış (zip) klasörüne Gönder'i seçin

![CRMScreenShot3](media/CRMScreenShot3.png)

1. Adı package.zip olarak değiştirin

![CRMScreenShot4](media/CRMScreenShot4.png)

## <a name="b-create-an-appsource-package"></a>b. AppSource Paketi Oluşturma

AppSource Paketi birkaç ek dosya gerektirir.

1. jpg (32x32 çözünürlük)
2. html (HTML biçimli dosya)
3. **Content_Types.xml** (yukarıdakiyle aynı)
4. xml

Burada input.xml için örnek kodudur. Aşağıdaki tablodaki tanımlara bakın.

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
 
**Nerede:**

|Alan|Ayrıntılar|
|---|---|
|Sağlayıcı Adı|Kimden gelen çözüm. Bir Microsoft ekibi varsa, bu Microsoft olmalıdır.|
|Paket Dosyası |Paket Dağıtıcı varlıkları bir içerik\_types.xml dosyası ile birlikte sıkıştırılmış. Bu zip dosyası paket dağıtıcı montaj ve paket dağıtıcı varlıkları ile klasörü içermelidir. Yani, package.zip|
|ÇözümAnchorName |Görüntü adı ve çözüm varlıklarının açıklaması için kullanılan paket dağıtımcısındaki çözüm zip dosyasının adı.|
| Startdate| Bu, çözüm paketinin kullanıma sunulalı olacağı tarihtir. Biçimi MM/DD/YYYY|
|EndDate|Bu, çözüm paketinin kullanıma sunulmayı durduracağı tarihtir. Biçimi MM/DD/YYYY |
|Desteklenen Ülkeler |Bu, bu paketi görmesi gereken ülkelerin/bölgelerin virgülle sınırlandırılabilen bir listesidir. Tüm geçerli ülke kodlarının listesi için çevrimiçi hizmetlerle iletişime geçin. O zaman, bu yazının listesi oldu: AE, AL,, AO, AR, AT, AU, AZ, BA, BB, BD, BE, BG, BH, BM, BN, BO, BR, BY, CA, CH, CI, CL, CM, CO, CR, CV, CW, CY, CZ, DE, DK, DO, DZ, EC, EE, EG ,ES,F,FR,TR,GT,G,GH,GR,GT,HK,HN,İk,HU,ID,IE,IL,IN,IQ,IS,IT,JM,JO, JP, KE,KG,KN,KR,KW,KY,KZ,LB, LK,LT,LU,LV,LY,MA,MC,MD,ME,MK,MN,MX,MY ,NG,NI,NL,NO,NZ,OM,PA,PE, PH, PK, PL,PR, PS, PT, PY, QA, RO, RS,RU, RW, SA, SE, SG,SI, SK,SN,SV, TH, TM,TN,TR,TT,TW,UA,US,UY,UZ,VE,VVV,ZA,ZW |
|LearnMoreLink | Bu paket için daha fazla bilgi sayfasına URL. |
|Yerel|Tercih edilen çözüm UX'de desteklemek istediğiniz her UX Dili için bu düğümün bir örneği. Bu düğüm, her diliçin yerel durumu, logoyu ve terimleri açıklayan çocuklar içerir|
|Yerel: PackageLocale.Code|Bu düğüm için dil LCID. Örnek: ABD İngilizcesi 1033|
|Yerel: PackageLocale.IsDefault|Bunun varsayılan dil olduğunu gösterir. Müşteri tarafından seçilen UX Dili kullanılamıyorsa, bu dil geri dönüş dili olarak kullanılır.|
|Yerel: Logo|Bu eğer logo bu paket için kullanmak istediğiniz. Simge için boyut 32x32'dir. İzin verilen formatlar PNG ve JPG'dir|
|Yerel:Terimler: PackageTerm.File|Bu, lisans terimlerinizi içeren HTML dokümanının dosya adıdır.|

Burada logo gösterecektir:

![CRMScreenShot5](media/CRMScreenShot5.png)

Son adım, aşağıdakileri tek bir dosyaya sığdırmaktır.

1. zip (daha önce oluşturulmuş)
2. **Content_Types.xml**
3. xml
4. png
5. html

![CRMScreenShot6](media/CRMScreenShot6.png)

Dosyayı uygulamanız için uygun bir şeye yeniden adlandırın. Şirket adınızı ve uygulama adınızı eklemenizi tercih ediyoruz. Örneğin: **_Microsoft_SamplePackage.zip**.
