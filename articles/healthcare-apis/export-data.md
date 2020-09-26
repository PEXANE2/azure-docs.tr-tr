---
title: FHıR için Azure API 'sindeki $export komutunu çağırarak dışarı aktarmayı yürütme
description: Bu makalede, belirtilen dışarı aktarmanın nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/26/2020
ms.author: matjazl
ms.openlocfilehash: ecc2134d1a528ee22710cb447f996e0c5e31a8de
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91308189"
---
# <a name="how-to-export-fhir-data"></a>FHıR verilerini dışa aktarma

$Export kullanmadan önce, FHıR için Azure API 'sinin onu kullanmak üzere yapılandırıldığından emin olmak isteyeceksiniz. Dışarı aktarma ayarlarını yapılandırmak ve Azure depolama hesabı oluşturmak için [verileri dışarı aktarma sayfasını](configure-export-data.md)inceleyin.

## <a name="using-export-command"></a>$export komutu kullanma

Dışarı aktarma işlemi için Azure API 'sını yapılandırdıktan sonra, verileri hizmetten dışarı aktarmak için $export komutunu kullanabilirsiniz. Veriler, dışarı aktarmayı yapılandırırken belirttiğiniz depolama hesabında depolanır. FHıR sunucusunda $export komutu çağırmayı öğrenmek için [$Export belirtiminde](https://hl7.org/Fhir/uv/bulkdata/export/index.html)belgeleri okuyun. 

FHıR için Azure API 'sindeki $export komutu, verilerin verilmesi gereken yapılandırılmış depolama hesabı içindeki kapsayıcıyı belirten isteğe bağlı bir _ \_ kapsayıcı_ parametresi alır.

`https://<<FHIR service base URL>>/$export?_container=<<container_name>>`

## <a name="supported-scenarios"></a>Desteklenen senaryolar

FHıR için Azure API, sistem, hasta ve grup düzeyinde $export destekler. Grup dışarı aktarma için tüm ilgili kaynakları dışarı aktardık, ancak grubun özelliklerini dışarı aktarmıyoruz.

> [!Note] 
> $export, kaynak birden fazla kaynağın bir bölmesinde veya birden çok grupta yer alıyorsa yinelenen kaynakları dışarı aktarır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, $export komutu kullanarak FHıR kaynaklarını dışarı aktarmayı öğrendiniz. Ardından, desteklenen özelliklerimizi gözden geçirebilirsiniz
 
>[!div class="nextstepaction"]
>[Desteklenen özellikler](fhir-features-supported.md)
