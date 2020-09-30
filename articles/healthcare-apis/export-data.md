---
title: FHıR için Azure API 'sindeki $export komutunu çağırarak dışarı aktarmayı yürütme
description: Bu makalede, belirtilen dışarı aktarmanın nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/26/2020
ms.author: matjazl
ms.openlocfilehash: ee7ba96a7cc8789e1a949db80bc84c63b28f4518
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91531675"
---
# <a name="how-to-export-fhir-data"></a>FHıR verilerini dışa aktarma

Toplu dışa aktarma özelliği, verilerin FHıR belirtiminde] FHıR belirtimi] ( https://hl7.org/fhir/uv/bulkdata/export/index.html) . 

$Export kullanmadan önce, FHıR için Azure API 'sinin onu kullanmak üzere yapılandırıldığından emin olmak isteyeceksiniz. Dışarı aktarma ayarlarını yapılandırmak ve Azure depolama hesabı oluşturmak için [verileri dışarı aktarma sayfasını](configure-export-data.md)inceleyin.

## <a name="using-export-command"></a>$export komutu kullanma

Dışarı aktarma işlemi için Azure API 'sını yapılandırdıktan sonra, verileri hizmetten dışarı aktarmak için $export komutunu kullanabilirsiniz. Veriler, dışarı aktarmayı yapılandırırken belirttiğiniz depolama hesabında depolanır. FHıR sunucusunda $export komutu çağırmayı öğrenmek için [$Export belirtiminde](https://hl7.org/Fhir/uv/bulkdata/export/index.html)belgeleri okuyun. 

FHıR için Azure API 'sindeki $export komutu, verilerin verilmesi gereken yapılandırılmış depolama hesabı içindeki kapsayıcıyı belirten isteğe bağlı bir _ \_ kapsayıcı_ parametresi alır. Bir kapsayıcı belirtilmişse, veriler bu kapsayıcıya ada sahip yeni bir klasörde verilir. Hiçbir kapsayıcı belirtilmemişse, veriler yeni bir kapsayıcıya bu adla aktarılacak

`https://<<FHIR service base URL>>/$export?_container=<<container_name>>`

## <a name="supported-scenarios"></a>Desteklenen senaryolar

FHıR için Azure API, sistem, hasta ve grup düzeyinde $export destekler. Grup dışarı aktarma için tüm ilgili kaynakları dışarı aktardık, ancak grubun özelliklerini dışarı aktarmıyoruz.

> [!Note] 
> $export, kaynak birden fazla kaynağın bir bölmesinde veya birden çok grupta yer alıyorsa yinelenen kaynakları dışarı aktarır.

Ayrıca, aktarım sırasında konum üst bilgisi tarafından döndürülen URL aracılığıyla dışarı aktarma durumunu denetlemek, gerçek dışa aktarma işinin desteklenmesinin iptal edilmesi ile birlikte desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, $export komutu kullanarak FHıR kaynaklarını dışarı aktarmayı öğrendiniz. Ardından, desteklenen özelliklerimizi gözden geçirebilirsiniz
 
>[!div class="nextstepaction"]
>[Desteklenen özellikler](fhir-features-supported.md)
