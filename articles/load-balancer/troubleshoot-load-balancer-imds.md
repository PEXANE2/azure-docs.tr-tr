---
title: Azure Instance Metadata Service (ıMDS) için ortak hata kodları
titleSuffix: Azure Load Balancer
description: Azure Instance Metadata Service (ıMDS) için ortak hata kodlarına ve karşılık gelen risk azaltma yöntemlerine genel bakış
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: troubleshooting
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: e932e211996a05b2740613381735a7de3492e5bf
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100519193"
---
# <a name="error-codes-common-error-codes-when-using-imds-to-retrieve-load-balancer-information"></a>Hata kodları: yük dengeleyici bilgilerini almak için ıDS kullanılırken ortak hata kodları

Bu makalede, yaygın dağıtım hataları ve Azure Instance Metadata Service (ıMDS) kullanılırken bu hataların nasıl çözümleneceği açıklanır.

## <a name="error-codes"></a>Hata kodları

| Hata kodu | Hata iletisi | Ayrıntılar ve risk azaltma |
| --- | ---------- | ----------------- |
| 400 | Gerekli "" parametresi eksik \<ParameterName> . Lütfen isteği düzeltip yeniden deneyin. | Hata kodu eksik bir parametreyi gösterir. </br> Eksik parametre ekleme hakkında daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response).
| 400 | Parametre değerine izin verilmiyor veya \<ParameterValue> "ParameterName" parametresi için "" parametre değerine izin verilmiyor. Lütfen isteği düzeltip yeniden deneyin. | Hata kodu, istek biçiminin düzgün yapılandırılmadığını gösterir. </br> Daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response) ve yeniden deneme sorunu giderme. |
| 400 | Beklenmeyen istek. Lütfen sorgu parametrelerini denetleyin ve yeniden deneyin. | Hata kodu, istek biçiminin düzgün yapılandırılmadığını gösterir. </br> Daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response) ve yeniden deneme sorunu giderme. |
| 404 | Yük dengeleyici meta verileri bulunamadı. Lütfen sanal makinenizin temel olmayan SKU yük dengeleyiciyi kullanıp kullanabileceksiniz ve daha sonra yeniden deneyin. | Hata kodu, sanal makinenizin bir yük dengeleyiciyle ilişkili olmadığını veya yük dengeleyicinin Standart yerine temel SKU olduğunu gösterir. </br> Daha fazla bilgi için bkz. hızlı başlangıç: Standart yük dengeleyici dağıtmak için [Azure Portal kullanarak VM 'leri yük dengelemeye yönelik ortak yük dengeleyici oluşturma](quickstart-load-balancer-standard-public-portal.md?tabs=option-1-create-load-balancer-standard) .|
| 404 | API bulunamadı: path = " \<UrlPath> ", method = " \<Method> " | Hata kodu yolun bir yanlış yapılandırılmasını gösterir. </br> Daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response) ve yeniden deneme sorunu giderme.|
| 405 | Http yöntemine izin verilmiyor: path = " \<UrlPath> ", method = " \<Method> " | Hata kodu, desteklenmeyen bir HTTP fiilini gösterir. </br> Daha fazla bilgi için bkz. desteklenen fiiller için [Azure Instance Metadata Service (IMDS)](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#http-verbs) . |
| 429 | Çok fazla istek | Hata kodu bir hız sınırını gösterir. </br> Hız sınırlaması hakkında daha fazla bilgi için bkz. [Azure Instance Metadata Service (IMDS)](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#rate-limiting).|
| 400 | İstek gövdesi MaxBodyLength değerinden büyük:... | Hata kodu MaxBodyLength değerinden daha büyük bir isteği gösterir. </br> Gövde uzunluğu hakkında daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response).|
| 400 | Parametre anahtar uzunluğu MaxParameterKeyLength 'den büyük:... | Hata kodu, MaxParameterKeyLength 'den büyük bir parametre anahtarı uzunluğunu gösterir. </br> Gövde uzunluğu hakkında daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response). |
| 400 | Parametre değeri uzunluğu MaxParameterValueLength değerinden büyük:... | Hata kodu, MaxParameterValueLength değerinden büyük bir parametre anahtarı uzunluğunu gösterir. </br> Değer uzunluğu hakkında daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response).|
| 400 | Parametre üst bilgisi değer uzunluğu MaxHeaderValueLength değerinden büyük:... | Hata kodu, MaxHeaderValueLength değerinden büyük bir parametre üst bilgi değeri uzunluğunu gösterir. </br> Değer uzunluğu hakkında daha fazla bilgi için bkz. [Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma](howto-load-balancer-imds.md#sample-request-and-response).|
| 404 | Load Balancer meta veri API 'SI Şu anda kullanılamıyor. Lütfen daha sonra yeniden deneyin | Hata kodu, API 'nin sağlanamayacağını gösterir. İsteğinizi daha sonra deneyin. |
| 404 | /Metadata/loadbalancer Şu anda kullanılamıyor | Hata kodu, API 'nin etkinleştirme ilerlemede olduğunu gösterir. İsteğinizi daha sonra deneyin. |
| 503 | İç hizmet kullanılamıyor. Lütfen daha sonra yeniden deneyin  | Hata kodu, API 'nin geçici olarak kullanılamadığını gösterir. İsteğinizi daha sonra deneyin. |
|  |  |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md) hakkında daha fazla bilgi

