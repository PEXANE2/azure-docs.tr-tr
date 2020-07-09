---
title: Azure Event Grid-sorun giderme kılavuzu
description: Bu makale, hata kodlarının, hata iletilerinin, açıklamaların ve önerilen eylemlerin bir listesini sağlar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: ab52cea6ab43763cf2d9dc2b57b7f369072a399e
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119047"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Azure Event Grid hatalarında sorun giderme
Bu sorun giderme kılavuzu, Azure Event Grid hata kodlarının, hata iletilerinin, açıklamalarının ve bu hataları aldığınızda gerçekleştirmeniz gereken önerilen eylemlerin bir listesini sağlar. 

## <a name="error-code-400"></a>Hata kodu: 400
| Hata kodu | Hata iletisi | Description | Öneri |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode. BadRequest<br/>400 | Konu adı 3 ila 50 karakter uzunluğunda olmalıdır. | Özel konu adı uzunluğu 3 ila 50 karakter uzunluğunda olmalıdır. Konu adında yalnızca alfasayısal harfler, rakamlar ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır: <ul><li>Microsoft</li><li>EventGrid</li><li>Sistem</li></ul> | Konu adı gereksinimlerine uygun olan farklı bir konu adı seçin. |
| HttpStatusCode. BadRequest<br/>400 | Etki alanı adı 3 ila 50 karakter uzunluğunda olmalıdır. | Etki alanı adının uzunluğu 3 ila 50 karakter uzunluğunda olmalıdır. Konu adında yalnızca alfasayısal harfler, rakamlar ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır:<ul><li>Microsoft</li><li>EventGrid</li><li>Sistem</li> | Etki alanı adı gereksinimlerine uygun farklı bir etki alanı adı seçin. |
| HttpStatusCode. BadRequest<br/>400 | Geçersiz süre sonu zamanı. | Olay aboneliğinin sona erme saati, olay aboneliğinin ne zaman devre dışı olacağını belirler. Bu değer, gelecekte geçerli bir DateTime değeri olmalıdır.| Olay aboneliği zaman aşımı süresinin geçerli bir tarih saat biçiminde ve gelecekte olacak şekilde ayarlandığından emin olun. |

## <a name="error-code-409"></a>Hata kodu: 409
| Hata kodu | Hata iletisi | Description | Önerilen eylem |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode. Conflict <br/>409 | Belirtilen ada sahip bir konu zaten var. Farklı bir konu adı seçin.   | Özel konu adı, doğru bir yayımlama işleminin sağlamak için tek bir Azure bölgesinde benzersiz olmalıdır. Aynı ad, farklı Azure bölgelerinde kullanılabilir. | Konu için farklı bir ad seçin. |
| HttpStatusCode. Conflict <br/> 409 | Belirtilen etki alanı zaten var. Farklı bir etki alanı adı seçin. | Doğru bir yayımlama işlemi sağlamak için etki alanı adının tek bir Azure bölgesinde benzersiz olması gerekir. Aynı ad, farklı Azure bölgelerinde kullanılabilir. | Etki alanı için farklı bir ad seçin. |
| HttpStatusCode. Conflict<br/>409 | Kota sınırına ulaşıldı. Bu limitlerin hakkında daha fazla bilgi için bkz. [Azure Event Grid sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits).  | Her Azure aboneliğinin kullanabileceği Azure Event Grid kaynak sayısı sınırlıdır. Bu kotanın bazıları veya tümü aşılmıştı ve daha fazla kaynak oluşturulamadı. |    Geçerli kaynak kullanımınızı denetleyin ve gerekmeyen kaynakları silin. Kotayı artırmanız gerekiyorsa, [aeg@microsoft.com](mailto:aeg@microsoft.com) tam olarak gereken kaynak sayısıyla bir e-posta gönderin. |

## <a name="error-code-403"></a>Hata kodu: 403

| Hata kodu | Hata iletisi | Description | Önerilen eylem |
| ---------- | ------------- | ----------- | ------------------ |
| HttpStatusCode. yasak <br/>403 | IPAddress filtreleme kuralları nedeniyle {IPAddress} istemcisinin {konu/etki alanı} öğesine yayımlanması reddedildi. | Konu veya etki alanında IP güvenlik duvarı kuralları yapılandırılmış ve erişim yalnızca yapılandırılan IP adresleriyle kısıtlıdır. | IP adresini IP güvenlik duvarı kurallarına ekleyin, bkz. [IP güvenlik duvarını yapılandırma](configure-firewall.md) |
| HttpStatusCode. yasak <br/> 403 | Özel uç noktasından istek geldiği ve kaynak için eşleşen özel uç nokta bağlantısı bulunmadığından, istemci tarafından {topic/etki alanı} üzerinde yayımlama reddedildi. | Konu veya etki alanı için yapılandırılmış özel uç noktalar ve yayımlama isteği, yapılandırılmamış/onaylanmamış özel bir uç noktadan geldi. | Konu/etki alanı için özel bir uç nokta yapılandırın. [Özel uç noktaları yapılandırma](configure-private-endpoints.md) |

## <a name="troubleshoot-event-subscription-validation"></a>Olay aboneliği doğrulama sorunlarını giderme

Olay aboneliği oluşturma sırasında, gibi bir hata mesajı görüyorsanız `The attempt to validate the provided endpoint https://your-endpoint-here failed. For more details, visit https://aka.ms/esvalidation` , doğrulama el sıkışmasının bir hata olduğunu gösterir. Bu hatayı çözmek için aşağıdaki noktaları doğrulayın:

- Postman veya kıvrık ya da benzer bir aracı kullanarak [örnek bir SubscriptionValidationEvent](webhook-event-delivery.md#validation-details) istek gövdesi ile Web kancası URL 'niz IÇIN BIR http gönderisi yapın.
- Web kancası, zaman uyumlu doğrulama el sıkışma mekanizması uygualıyorsa, yanıtın bir parçası olarak ValidationCode 'un döndürüldüğünden emin olun.
- Web kancası, zaman uyumsuz doğrulama el sıkışma mekanizmasını uygulamadıysanız, HTTP POST 'un 200 doğru olduğunu doğrulayın.
- Web kancası yanıt karşılığında 403 (yasak) döndürüyorsa, Web kancasının bir Azure Application Gateway veya Web uygulaması güvenlik duvarının arkasında olup olmadığını denetleyin. Bu durumda, bu güvenlik duvarı kurallarını devre dışı bırakıp bir HTTP POST işlemini yeniden gerçekleştirmeniz gerekir:

  920300 (istekte bir Accept üst bilgisi eksik, bunu çözebiliriz)

  942430 (kısıtlanan SQL karakter anomali algılama (args): özel karakter sayısı aşıldı (12))

  920230 (birden çok URL kodlaması algılandı)

  942130 (SQL ekleme saldırısı: SQL tautology algılandı.)

  931130 (olası uzaktan dosya ekleme (RFı) saldırısı = kapalı etki alanı başvurusu/bağlantı)


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa sorununuzu [Stack Overflow forumuna](https://stackoverflow.com/questions/tagged/azure-eventgrid) gönderin veya bir [destek bileti](https://azure.microsoft.com/support/options/)açın. 
