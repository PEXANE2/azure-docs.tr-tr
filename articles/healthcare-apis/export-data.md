---
title: FHıR için Azure API 'sindeki $export komutunu çağırarak dışarı aktarmayı yürütme
description: Bu makalede $export kullanarak FHıR verilerinin nasıl dışarı aktarılacağı açıklanır
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/26/2020
ms.author: matjazl
ms.openlocfilehash: 74fe09895f49cc9f7c3cdf6b6c97c1624c3e9c0b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91839835"
---
# <a name="how-to-export-fhir-data"></a>FHıR verilerini dışa aktarma


Toplu dışa aktarma özelliği, verilerin fhır [belirtimine](https://hl7.org/fhir/uv/bulkdata/export/index.html)göre fhır sunucusundan dışarı aktarılmasını sağlar. 

$Export kullanmadan önce, FHıR için Azure API 'sinin onu kullanmak üzere yapılandırıldığından emin olmak isteyeceksiniz. Dışarı aktarma ayarlarını yapılandırmak ve Azure depolama hesabı oluşturmak için [verileri dışarı aktarma sayfasını](configure-export-data.md)inceleyin.

## <a name="using-export-command"></a>$export komutu kullanma

Dışarı aktarma işlemi için Azure API 'sını yapılandırdıktan sonra, verileri hizmetten dışarı aktarmak için $export komutunu kullanabilirsiniz. Veriler, dışarı aktarmayı yapılandırırken belirttiğiniz depolama hesabında depolanır. FHıR sunucusunda $export komutu çağırmayı öğrenmek için [$Export belirtiminde](https://hl7.org/Fhir/uv/bulkdata/export/index.html)belgeleri okuyun. 

FHıR için Azure API 'sindeki $export komutu, verilerin verilmesi gereken yapılandırılmış depolama hesabı içindeki kapsayıcıyı belirten isteğe bağlı bir _ \_ kapsayıcı_ parametresi alır. Bir kapsayıcı belirtilmişse, veriler bu kapsayıcıya ada sahip yeni bir klasörde verilir. Kapsayıcı belirtilmemişse, rastgele oluşturulmuş bir ada sahip yeni bir kapsayıcıya aktarılacaktır. 

`https://<<FHIR service base URL>>/$export?_container=<<container_name>>`

## <a name="supported-scenarios"></a>Desteklenen senaryolar

FHıR için Azure API, sistem, hasta ve grup düzeyinde $export destekler. Grup dışarı aktarma için tüm ilgili kaynakları dışarı aktardık, ancak grubun özelliklerini dışarı aktarmıyoruz.

> [!Note] 
> $export, kaynak birden fazla kaynağın bir bölmesinde veya birden çok grupta yer alıyorsa yinelenen kaynakları dışarı aktarır.

Ayrıca, aktarım sırasında konum üst bilgisi tarafından döndürülen URL aracılığıyla dışa aktarma durumunun denetlenmesi, gerçek dışa aktarma işinin iptal edilmesi ile birlikte desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, $export komutu kullanarak FHıR kaynaklarını dışarı aktarmayı öğrendiniz. Daha sonra, desteklenen özelliklerimizi gözden geçirebilirsiniz:
 
>[!div class="nextstepaction"]
>[Desteklenen özellikler](fhir-features-supported.md)
