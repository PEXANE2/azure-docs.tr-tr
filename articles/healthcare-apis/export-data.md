---
title: FHıR için Azure API 'sindeki $export komutunu çağırarak dışarı aktarmayı yürütme
description: Bu makalede $export kullanarak FHıR verilerinin nasıl dışarı aktarılacağı açıklanır
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 1/21/2021
ms.author: cavoeg
ms.openlocfilehash: 8ad5ee78a525b3798bbf613168ff74a9e21fe99b
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98920266"
---
# <a name="how-to-export-fhir-data"></a>FHıR verilerini dışa aktarma


Toplu dışa aktarma özelliği, verilerin fhır [belirtimine](https://hl7.org/fhir/uv/bulkdata/export/index.html)göre fhır sunucusundan dışarı aktarılmasını sağlar. 

$Export kullanmadan önce, FHıR için Azure API 'sinin onu kullanmak üzere yapılandırıldığından emin olmak isteyeceksiniz. Dışarı aktarma ayarlarını yapılandırmak ve Azure depolama hesabı oluşturmak için [verileri dışarı aktarma sayfasını](configure-export-data.md)inceleyin.

## <a name="using-export-command"></a>$export komutu kullanma

Dışarı aktarma işlemi için Azure API 'sını yapılandırdıktan sonra, verileri hizmetten dışarı aktarmak için $export komutunu kullanabilirsiniz. Veriler, dışarı aktarmayı yapılandırırken belirttiğiniz depolama hesabında depolanır. FHıR sunucusunda $export komutu çağırmayı öğrenmek için [HL7 fhır $Export belirtiminde](https://hl7.org/Fhir/uv/bulkdata/export/index.html)belgeleri okuyun. 

FHıR Için Azure API aşağıdaki düzeylerde $export destekler:
* [Sistem](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---system-level-export): `GET https://<<FHIR service base URL>>/$export>>`
* [Hasta](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---all-patients): `GET https://<<FHIR service base URL>>/Patient/$export>>`
* [Hastalar grubu *](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---group-of-patients) -fhır için Azure API 'si tüm ilgili kaynakları dışa aktarır ancak grubun özelliklerini dışarı aktarmaz: `GET https://<<FHIR service base URL>>/Group/[ID]/$export>>`



> [!Note] 
> `Patient/$export` ve `Group/[ID]/$export` kaynak birden fazla kaynağın bir bölmesinde ise veya birden çok grupta yer alıyorsa yinelenen kaynakları dışarı aktarabilirsiniz.

Ayrıca, aktarım sırasında konum üst bilgisi tarafından döndürülen URL aracılığıyla dışa aktarma durumunun denetlenmesi, gerçek dışa aktarma işinin iptal edilmesi ile birlikte desteklenir.

## <a name="settings-and-parameters"></a>Ayarlar ve parametreler

### <a name="headers"></a>Üst Bilgiler
$Export işleri için ayarlanması gereken iki gerekli üst bilgi parametresi vardır. Değerler geçerli [$Export belirtimi](https://hl7.org/Fhir/uv/bulkdata/export/index.html#headers)tarafından tanımlanır.
* **Accept** -Application/fhir + JSON
* **Tercih** -Yanıtla-zaman uyumsuz

### <a name="query-parameters"></a>Sorgu parametreleri
FHıR için Azure API 'SI aşağıdaki sorgu parametrelerini destekler. Bu parametrelerin tümü isteğe bağlıdır:

|Sorgu parametresi        | FHIR belirtimi tarafından tanımlandı mı?    |  Description|
|------------------------|---|------------|
| \_outputFormat | Yes | Şu anda, FHıR belirtimine uyum sağlamak için üç değeri desteklemektedir: Application/fhır + ndjson, Application/ndjson veya yalnızca ndjson. Tüm dışarı aktarma işleri döndürülecek `ndjson` ve geçirilen değerin kod davranışı üzerinde hiçbir etkisi olmayacaktır. |
| \_getirildikten | Yes | Yalnızca belirtilen süreden bu yana değiştirilen kaynakları dışarı aktarmanız sağlar |
| \_türüyle | Yes | Hangi kaynak türlerinin ekleneceğini belirtmenize izin verir. Örneğin, \_ tür = hasta yalnızca hasta kaynaklarını döndürür|
| \_tür filtresi | Yes | Daha ayrıntılı filtreleme istemek için, \_ tür parametresiyle birlikte TypeFilter kullanabilirsiniz \_ . _TypeFilter parametresinin değeri, sonuçları daha fazla kısıtlayan FHıR sorgularının virgülle ayrılmış listesidir |
| \_kapsayıcı | Hayır |  Verilerin verilmesi gereken yapılandırılmış depolama hesabı içindeki kapsayıcıyı belirtir. Bir kapsayıcı belirtilmişse, veriler bu kapsayıcıya ada sahip yeni bir klasörde verilir. Kapsayıcı belirtilmemişse, zaman damgası ve iş KIMLIĞI kullanılarak yeni bir kapsayıcıya aktaralınacaktır. |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, $export komutu kullanarak FHıR kaynaklarını dışarı aktarmayı öğrendiniz. Ardından, önceden tanımlanmış verileri dışarı aktarmayı öğrenin:
 
>[!div class="nextstepaction"]
>[Belirtilen verileri dışarı aktarma](de-identified-export.md)
