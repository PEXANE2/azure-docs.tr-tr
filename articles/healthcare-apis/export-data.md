---
title: FHıR için Azure API 'sindeki $export komutunu çağırarak dışarı aktarmayı yürütme
description: Bu makalede, belirtilen dışarı aktarmanın nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/26/2020
ms.author: matjazl
ms.openlocfilehash: 83509b5f452ab7cf88774561c12d7aa2cf3b46cf
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482326"
---
# <a name="how-to-export-fhir-data"></a>FHıR verilerini dışa aktarma

Dışarı aktarma ayarlarını yapılandırmak ve Azure depolama hesabı oluşturmak için lütfen [buraya](configure-export-data.md)bakın.

## <a name="exporting-fhir-resources-using-export-command"></a>$export komutu kullanarak FHıR kaynaklarını dışarı aktarma

Dışa aktarma için FHıR için Azure API yapılandırıldıktan sonra, verileri hizmetten dışarı aktarma yapılandırılırken belirlediğimiz depolama hesabına aktarmak için $export komutunu kullanmaya devam edebilirsiniz. FHıR sunucusunda $export komutunun nasıl çağıralınacağını öğrenmek için lütfen adresinde $export belirtimindeki belgeleri okuyun [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html) . 

FHıR için Azure API 'sindeki $export komutu, verilerin verilmesi gereken yapılandırılmış depolama hesabı içinde kapsayıcıyı belirtmek için kullanılabilen isteğe bağlı bir _ \_ conatiner_ parametresi alır.

`https://<<FHIR service base URL>>/$export?_container=<<container_name>>`

> [!IMPORTANT]
> Şu anda FHıR için Azure API 'sinin, ' de $export belirtiminde tanımlandığı şekilde yalnızca sistem düzeyinde dışarı aktarmayı desteklediğini unutmayın [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html) . Ayrıca, yalnızca sorgu parametresi şu anda _ \_ desteklendiğinden_ .

## <a name="exporting-de-identified-data-preview"></a>Belirtilen verileri dışarı aktarma (Önizleme)

$Export komutu, FHıR sunucusundan de belirtilen verileri dışarı aktarmak için de kullanılabilir. Anonimleştirme [Için Fhır araçlarından](https://github.com/microsoft/FHIR-Tools-for-Anonymization)anonimleştirme altyapısını kullanır ve sorgu parametrelerinde anonimleştirme yapılandırma ayrıntılarını alır. Kendi anonimleştirme yapılandırma dosyanızı oluşturabilir veya HIPAA güvenli Harbveya yöntemi için bir başlangıç noktası olarak [örnek yapılandırma dosyasını](https://github.com/microsoft/FHIR-Tools-for-Anonymization#sample-configuration-file-for-hipaa-safe-harbor-method) kullanabilirsiniz. 

 `https://<<FHIR service base URL>>/$export?_container=<<container_name>>&_anonymizationConfig=<<config file name>>&_anonymizationConfigEtag=<<ETag on storage>>`

|Sorgu parametresi            | Örnek |Optionitesi| Description|
|---------------------------|---------|-----------|------------|
| _\_anonymizationConfig_   |Üzerinde DemoConfig.js|Geçersiz şekilde tanımlanan dışarı aktarma için gereklidir |Yapılandırma dosyasının adı. Yapılandırma dosyası biçimini [buradan](https://github.com/microsoft/FHIR-Tools-for-Anonymization#configuration-file-format)görebilirsiniz. Bu dosya, dışarı aktarma konumu olarak yapılandırılmış aynı Azure depolama hesabı içinde, **anonimleştirme** adlı bir kapsayıcı içinde tutulmalıdır. |
| _\_anonymizationConfigEtag_|"0x8D8494A069489EC"|Geçersiz şekilde tanımlanan dışarı aktarma için isteğe bağlı|Bu, yapılandırma dosyasının ETag ' i. Blob özelliğinden Azure Depolama Gezgini kullanarak ETag 'i alabilirsiniz|

> [!IMPORTANT]
> Hem ham dışa aktarmanın hem de dışa aktarma yapılandırmasının bir parçası olarak belirtilen Azure depolama hesabına dışarı aktarma yazmaları olduğunu unutmayın. Farklı bir şekilde tanımlanan yapılandırmaya karşılık gelen farklı kapsayıcılar kullanmanız ve kapsayıcı düzeyinde Kullanıcı erişimini yönetmeniz önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, geçersiz bir şekilde tanımlanan veriler de dahil olmak üzere $export komutunu kullanarak FHıR kaynaklarını dışarı aktarmayı öğrendiniz. Ardından, dışa aktarma verilerinizi yapılandırabilirsiniz:
 
>[!div class="nextstepaction"]
>[dışarı aktarma verilerini yapılandırma](configure-export-data.md)
