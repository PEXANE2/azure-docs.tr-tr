---
title: FHıR için Azure API veri dönüştürme
description: FHIR için Azure API 'sindeki verileri dönüştürmek için $convert-veri uç noktası ve özelleştirme-dönüştürücü şablonlarını kullanın.
services: healthcare-apis
author: ranvijaykumar
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 01/19/2021
ms.author: ranku
ms.openlocfilehash: c794af26fdfe2d3706d8d8d266d0756eff391b50
ms.sourcegitcommit: 3c8964a946e3b2343eaf8aba54dee41b89acc123
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98747708"
---
# <a name="how-to-convert-data-to-fhir"></a>Verileri FHıR 'ye dönüştürme

FHıR için Azure API 'sindeki $convert-Data özel uç noktası, farklı biçimlerdeki FHıR 'ye veri dönüştürme için tasarlanmıştır. Bu, varsayılan şablonlar olarak [Fhır dönüştürücü](https://github.com/microsoft/FHIR-Converter) projesindeki likit şablon altyapısını ve şablonları kullanır. Bu dönüştürme şablonlarını gerektiği şekilde özelleştirebilirsiniz. Şu anda HL7v2 to FHıR dönüşümünü destekler.

## <a name="use-the-convert-data-endpoint"></a>$Convert-veri uç noktasını kullanma

`https://<<FHIR service base URL>>/$convert-data`

$convert-veri, aşağıda açıklandığı gibi istek gövdesinde bir [parametre](http://hl7.org/fhir/parameters.html) kaynağı alır:

**Parametre kaynağı:**

| Parametre Adı      | Açıklama | Kabul edilen değerler |
| ----------- | ----------- | ----------- |
| inputData      | Dönüştürülecek veriler. | JSON dize veri türünün geçerli bir değeri|
| ınputdatatype   | Girişin veri türü. | ```HL7v2``` |
| templateCollectionReference | Bir şablon koleksiyonuna başvuru. Bu, **varsayılan şablonlara** veya fhır için Azure API 'sine kayıtlı özel bir şablon görüntüsüne bir başvuru olabilir. Şablonları özelleştirme, ACR 'de barındırma ve FHıR için Azure API 'sine kaydolma hakkında bilgi edinmek için aşağıya bakın.  | ```microsofthealth/fhirconverter:default```, \<RegistryServer\>/\<imageName\>@\<imageDigest\> |
| rootTemplate | Veriler dönüştürülürken kullanılacak kök şablon. | ```ADT_A01```, ```OML_O21```, ```ORU_R01```, ```VXU_V04``` |  

> [!WARNING]
> Varsayılan Şablonlar hızla kullanmaya başlamanıza yardımcı olur. Ancak, FHıR için Azure API 'YI yükselttiğinizde Bu durum güncelleştirilebiliriz. FHıR için Azure API 'nin farklı sürümlerinde tutarlı veri dönüştürme davranışına sahip olmak için, şablon kopyanızı bir Azure Container Registry barındırmalısınız, bu verileri FHıR için Azure API 'sine kaydedebilir ve daha sonra açıklandığı gibi API çağrılarında kullanın.

**Örnek istek:**

```json
{
    "resourceType": "Parameters",
    "parameter": [
        {
            "name": "inputData",
            "valueString": "MSH|^~\\&|SIMHOSP|SFAC|RAPP|RFAC|20200508131015||ADT^A01|517|T|2.3|||AL||44|ASCII\nEVN|A01|20200508131015|||C005^Whittingham^Sylvia^^^Dr^^^DRNBR^PRSNL^^^ORGDR|\nPID|1|3735064194^^^SIMULATOR MRN^MRN|3735064194^^^SIMULATOR MRN^MRN~2021051528^^^NHSNBR^NHSNMBR||Kinmonth^Joanna^Chelsea^^Ms^^CURRENT||19870624000000|F|||89 Transaction House^Handmaiden Street^Wembley^^FV75 4GJ^GBR^HOME||020 3614 5541^HOME|||||||||C^White - Other^^^||||||||\nPD1|||FAMILY PRACTICE^^12345|\nPV1|1|I|OtherWard^MainRoom^Bed 183^Simulated Hospital^^BED^Main Building^4|28b|||C005^Whittingham^Sylvia^^^Dr^^^DRNBR^PRSNL^^^ORGDR|||CAR|||||||||16094728916771313876^^^^visitid||||||||||||||||||||||ARRIVED|||20200508131015||"
        },
        {
            "name": "inputDataType",
            "valueString": "Hl7v2"
        },
        {
            "name": "templateCollectionReference",
            "valueString": "microsofthealth/fhirconverter:default"
        },
        {
            "name": "rootTemplate",
            "valueString": "ADT_A01"
        }
    ]
}
```

**Örnek yanıt:**

```json
{
  "resourceType": "Bundle",
  "type": "transaction",
  "entry": [
    {
      "fullUrl": "urn:uuid:9d697ec3-48c3-3e17-db6a-29a1765e22c6",
      "resource": {
        "resourceType": "Patient",
        "id": "9d697ec3-48c3-3e17-db6a-29a1765e22c6",
        ...
        ...
      "request": {
        "method": "PUT",
        "url": "Location/50becdb5-ff56-56c6-40a1-6d554dca80f0"
      }
    }
  ]
}
```

## <a name="customize-templates"></a>Şablonları özelleştirme

Şablonları gereksinimlerinize göre özelleştirmek için [Fhır dönüştürücü uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-health-fhir-converter) Visual Studio Code için kullanabilirsiniz. Uzantı etkileşimli bir düzen deneyimi sağlar ve Microsoft tarafından yayımlanan şablonları ve örnek verileri indirmeyi kolaylaştırır. Ayrıntılar için uzantıdaki belgelere bakın.

## <a name="host-and-use-templates"></a>Konak ve kullanım şablonları

ACR 'de kendi şablon kopyanızı barındırmanızı önemle tavsiye edilir. Şablon kopyanızı barındırmakla ilgili dört adım vardır ve bunları $convert-Data işleminde kullanın:

1. Şablonları Azure Container Registry gönderin.
1. FHıR örneği için Azure API 'niz üzerinde yönetilen kimliği etkinleştirin.
1. FHıR tarafından yönetilen kimlik için ACR 'ye Azure API 'sine erişim sağlama.
1. ACR sunucularını FHıR için Azure API 'sine kaydettirin.

### <a name="push-templates-to-azure-container-registry"></a>Şablonları Azure Container Registry gönder

Bir ACR örneği oluşturduktan sonra, özel şablonları ACR 'ye göndermek için [Fhır dönüştürücü uzantısında](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-health-fhir-converter) _fhır dönüştürücüsü: gönderme şablonları_ komutunu kullanabilirsiniz. Alternatif olarak, bu amaçla [şablon YÖNETIMI CLI aracını](https://github.com/microsoft/FHIR-Converter/blob/main/docs/TemplateManagementCLI.md) kullanabilirsiniz.

### <a name="enable-managed-identity-on-azure-api-for-fhir"></a>FHıR için Azure API 'de yönetilen kimliği etkinleştirme

Azure portal FHıR hizmeti için Azure API örneğinize gidin ve **kimlik** dikey penceresini seçin.
FHıR için Azure API 'de yönetilen kimliği etkinleştirmek üzere durumu **Açık** olarak değiştirin.

![Yönetilen kimliği etkinleştir](media/convert-data/fhir-mi-enabled.png)

### <a name="provide-access-of-the-acr-to-azure-api-for-fhir"></a>FHıR için ACR 'ye Azure API erişimi sağlama

ACR örneğinizdeki Access Control (ıAM) dikey penceresine gidin ve _rol atamaları Ekle_' yi seçin.

![ACR rol ataması](media/convert-data/fhir-acr-role-assignment.png)

FHıR hizmet örneği için Azure API 'nize AcrPull rolü verin.

![Rol Ekle](media/convert-data/fhir-acr-role-add.png)

### <a name="register-the-acr-servers-in-azure-api-for-fhir"></a>FHıR için Azure API 'ye ACR sunucularını kaydetme

FHıR için Azure API 'sinde en fazla yirmi ACR sunucusu kaydedebilirsiniz.

Gerekirse Azure PowerShell healthumapı CLı API 'lerini yükler:

```powershell
az extension add -n healthcareapis
```

Aşağıdaki örnekleri izleyerek ACR sunucularını FHıR için Azure API 'sine kaydedin:

#### <a name="register-a-single-acr-server"></a>Tek bir ACR sunucusunu kaydetme

```powershell
az healthcareapis acr add --login-servers "fhiracr2021.azurecr.io" --resource-group fhir-test --resource-name fhirtest2021
```

#### <a name="register-multiple-acr-servers"></a>Birden çok ACR sunucusunu kaydetme

```powershell
az healthcareapis acr add --login-servers "fhiracr2021.azurecr.io fhiracr2020.azurecr.io" --resource-group fhir-test --resource-name fhirtest2021
```

### <a name="verify"></a>Doğrulama

TemplateCollectionReference parametresinde şablon başvurusunu belirten $convert-Data API 'sine bir çağrı yapın.

`<RegistryServer>/<imageName>@<imageDigest>`

## <a name="known-issues-and-workarounds"></a>Bilinen sorunlar ve geçici çözümler

- Bazı varsayılan şablon dosyaları UTF-8 BOM içerir. Sonuç olarak, oluşturulan KIMLIK değerleri bir BOM karakteri içerir. Bu, FHıR sunucusuyla ilgili bir sorun oluşturabilir. Geçici çözüm, VS Code uzantısı kullanarak Microsoft şablonlarını çekmek ve _ID/_yordamdan. sıvı_, _ID/_provenance. sıvı_ ve __ID/çözüm. sıvı '_ den sonra da kendi ACR 'nize gönderim yapmak.

