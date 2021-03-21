---
title: Azure Event Grid-sorun giderme kılavuzu
description: Bu makale, hata kodlarının, hata iletilerinin, açıklamaların ve önerilen eylemlerin bir listesini sağlar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 79533918ccc6995f459b39f058de9e01091c0958
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94593000"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Azure Event Grid hatalarında sorun giderme
Bu sorun giderme kılavuzu, aşağıdaki bilgileri sağlar: 

- Azure Event Grid hata kodları
- Hata iletileri
- Hataların açıklamaları
- Bu hataları aldığınızda yapmanız gereken önerilen eylemler. 

## <a name="error-code-400"></a>Hata kodu: 400
| Hata kodu | Hata iletisi | Description | Öneri |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode. BadRequest<br/>400 | Konu adı 3 ila 50 karakter uzunluğunda olmalıdır. | Özel konu adı uzunluğu 3 ila 50 karakter uzunluğunda olmalıdır. Konu adında yalnızca alfasayısal harfler, rakamlar ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır: <ul><li>MICROSOFT</li><li>EventGrid</li><li>Sistemin</li></ul> | Konu adı gereksinimlerine uygun olan farklı bir konu adı seçin. |
| HttpStatusCode. BadRequest<br/>400 | Etki alanı adı 3 ila 50 karakter uzunluğunda olmalıdır. | Etki alanı adının uzunluğu 3 ila 50 karakter uzunluğunda olmalıdır. Etki alanı adında yalnızca alfasayısal harfler, rakamlar ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır:<ul><li>MICROSOFT</li><li>EventGrid</li><li>Sistemin</li> | Etki alanı adı gereksinimlerine uygun farklı bir etki alanı adı seçin. |
| HttpStatusCode. BadRequest<br/>400 | Geçersiz süre sonu zamanı. | Olay aboneliğinin sona erme saati, olay aboneliğinin ne zaman devre dışı olacağını belirler. Bu değer, gelecekte geçerli bir DateTime değeri olmalıdır.| Olay aboneliği zaman aşımı süresinin geçerli bir tarih saat biçiminde ve gelecekte olacak şekilde ayarlandığından emin olun. |

## <a name="error-code-409"></a>Hata kodu: 409
| Hata kodu | Hata iletisi | Description | Önerilen eylem |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode. Conflict <br/>409 | Belirtilen ada sahip bir konu zaten var. Farklı bir konu adı seçin.   | Özel konu adı, doğru bir yayımlama işlemi sağlamak için tek bir Azure bölgesinde benzersiz olmalıdır. Aynı ad, farklı Azure bölgelerinde kullanılabilir. | Konu için farklı bir ad seçin. |
| HttpStatusCode. Conflict <br/> 409 | Belirtilen etki alanı zaten var. Farklı bir etki alanı adı seçin. | Doğru bir yayımlama işlemi sağlamak için etki alanı adı tek bir Azure bölgesinde benzersiz olmalıdır. Aynı ad, farklı Azure bölgelerinde kullanılabilir. | Etki alanı için farklı bir ad seçin. |
| HttpStatusCode. Conflict<br/>409 | Kota sınırına ulaşıldı. Bu limitlerin hakkında daha fazla bilgi için bkz. [Azure Event Grid sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits).  | Her Azure aboneliğinin kullanabileceği Azure Event Grid kaynak sayısı sınırlıdır. Bu kotanın bazıları veya tümü aşılmıştı ve daha fazla kaynak oluşturulamadı. |    Geçerli kaynak kullanımınızı denetleyin ve gerekmeyen kaynakları silin. Kotayı artırmanız gerekiyorsa, [aeg@microsoft.com](mailto:aeg@microsoft.com) tam olarak gereken kaynak sayısıyla bir e-posta gönderin. |

## <a name="error-code-403"></a>Hata kodu: 403

| Hata kodu | Hata iletisi | Description | Önerilen eylem |
| ---------- | ------------- | ----------- | ------------------ |
| HttpStatusCode. yasak <br/>403 | IPAddress filtreleme kuralları nedeniyle {IPAddress} istemcisinin {konu/etki alanı} öğesine yayımlanması reddedildi. | Konu veya etki alanında IP güvenlik duvarı kuralları yapılandırılmış ve erişim yalnızca yapılandırılan IP adresleriyle kısıtlıdır. | IP adresini IP güvenlik duvarı kurallarına ekleyin, bkz. [IP güvenlik duvarını yapılandırma](configure-firewall.md) |
| HttpStatusCode. yasak <br/> 403 | Özel uç noktasından istek geldiği ve kaynak için eşleşen özel uç nokta bağlantısı bulunmadığından, istemci tarafından {topic/etki alanı} üzerinde yayımlama reddedildi. | Konunun veya etki alanının özel uç noktaları ve yayımlama isteği, yapılandırılmamış veya onaylanmamış özel bir uç noktadan geldi. | Konu/etki alanı için özel bir uç nokta yapılandırın. [Özel uç noktaları yapılandırma](configure-private-endpoints.md) |

Ayrıca, Web kancasının bir Azure Application Gateway veya Web uygulaması güvenlik duvarının arkasında olup olmadığını kontrol edin. Varsa, aşağıdaki güvenlik duvarı kurallarını devre dışı bırakın ve bir HTTP POST yeniden yapın:

- 920300 (istek bir Accept üst bilgisini içermiyor)
- 942430 (kısıtlanan SQL karakter anomali algılama (args): özel karakter sayısı aşıldı (12))
- 920230 (birden çok URL kodlaması algılandı)
- 942130 (SQL ekleme saldırısı: SQL tautology algılandı.)
- 931130 (olası uzaktan dosya ekleme (RFı) saldırısı = kapalı etki alanı başvurusu/bağlantı)



## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa sorununuzu [Stack Overflow forumuna](https://stackoverflow.com/questions/tagged/azure-eventgrid) gönderin veya bir [destek bileti](https://azure.microsoft.com/support/options/)açın. 
