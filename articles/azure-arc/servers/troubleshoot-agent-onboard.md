---
title: Sunucu Aracısı bağlantı sorunları için Azure Arc sorunlarını giderme
description: Bu makalede, hizmete bağlanmaya çalışırken sunucular için Azure Arc (Önizleme) ile birlikte gelen bağlı makine aracısında sorunları gidermeye ve çözmeye nasıl çözüm yapılacağı açıklanır.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 07/10/2020
ms.topic: conceptual
ms.openlocfilehash: 37f99ade366a73cb96caf55a562a92476223eb6b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86262205"
---
# <a name="troubleshoot-the-connected-machine-agent-connection-issues"></a>Bağlı makine Aracısı bağlantı sorunlarını giderme

Bu makalede, Windows veya Linux için Azure Arc Server (Önizleme) bağlı makine aracısını yapılandırmaya çalışırken oluşabilecek sorunları giderme ve çözme hakkında bilgi sağlanır. Hizmetle bağlantı yapılandırılırken hem etkileşimli hem de ölçekli yükleme yöntemleri dahildir. Genel bilgi için bkz. [sunucu Için yay genel bakış](./overview.md).

## <a name="agent-verbose-log"></a>Aracı ayrıntılı günlüğü

Bu makalenin ilerleyen kısımlarında açıklanan sorun giderme adımlarını takip etmeden önce, ihtiyacınız olan en düşük bilgiler ayrıntılı günlüğdeğer olacaktır. Verbose (-v) bağımsız değişkeni kullanıldığında **azcmagent** aracı komutlarının çıktısını içerir. Günlük dosyaları `%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log` Windows ve Linux için üzerine yazılır `/var/opt/azcmagent/log/azcmagent.log` .

### <a name="windows"></a>Windows

Aşağıda, etkileşimli bir yükleme gerçekleştirirken Windows için bağlı makine Aracısı ile ayrıntılı günlüğe yazmayı etkinleştirme komutuna bir örnek verilmiştir.

```
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Aşağıda, bir hizmet sorumlusu kullanarak ölçekli bir yükleme gerçekleştirirken Windows için bağlı makine Aracısı ile ayrıntılı günlüğe yazmayı etkinleştirme komutuna bir örnek verilmiştir.

```
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect `
  --service-principal-id "{serviceprincipalAppID}" `
  --service-principal-secret "{serviceprincipalPassword}" `
  --resource-group "{ResourceGroupName}" `
  --tenant-id "{tenantID}" `
  --location "{resourceLocation}" `
  --subscription-id "{subscriptionID}"
  --verbose
```

### <a name="linux"></a>Linux

Aşağıda, etkileşimli bir yükleme gerçekleştirirken Linux için bağlı makine Aracısı ile ayrıntılı günlüğe yazmayı etkinleştirme komutuna bir örnek verilmiştir.

```
azcmagent connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Aşağıda, bir hizmet sorumlusu kullanarak ölçekli bir yükleme gerçekleştirirken Linux için bağlı makine Aracısı ile ayrıntılı günlüğe yazmayı etkinleştirme komutuna bir örnek verilmiştir.

```
azcmagent connect \
  --service-principal-id "{serviceprincipalAppID}" \
  --service-principal-secret "{serviceprincipalPassword}" \
  --resource-group "{ResourceGroupName}" \
  --tenant-id "{tenantID}" \
  --location "{resourceLocation}" \
  --subscription-id "{subscriptionID}"
  --verbose
```

## <a name="agent-connection-issues-to-service"></a>Hizmete yönelik aracı bağlantısı sorunları

Aşağıdaki tabloda, bilinen hatalardan bazıları ve bunların nasıl giderileceği ve çözüleceği ile ilgili öneriler listelenmektedir.

|İleti |Hata |Olası neden |Çözüm |
|--------|------|---------------|---------|
|Yetkilendirme belirteci cihaz akışı alınamadı |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp 40.126.9.7:443: connect: network is unreachable.` |`login.windows.net`Uç noktaya ulaşılamıyor | Uç nokta bağlantısını doğrulayın. |
|Yetkilendirme belirteci cihaz akışı alınamadı |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp 40.126.9.7:443: connect: network is Forbidden`. |Proxy veya güvenlik duvarı `login.windows.net` uç noktaya erişimi engelliyor. | Uç nokta bağlantısını doğrulayın ve bir güvenlik duvarı veya proxy sunucusu tarafından engellenmiyor. |
|SPN 'den yetkilendirme belirteci alınamadı |`Failed to execute the refresh request. Error = 'Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/token?api-version=1.0: Forbidden'` |Proxy veya güvenlik duvarı `login.windows.net` uç noktaya erişimi engelliyor. |Uç nokta bağlantısını doğrulayın ve bir güvenlik duvarı veya proxy sunucusu tarafından engellenmiyor. |
|SPN 'den yetkilendirme belirteci alınamadı |`Invalid client secret is provided` |Yanlış veya geçersiz hizmet sorumlusu gizli anahtarı. |Hizmet sorumlusu gizliliğini doğrulayın. |
| SPN 'den yetkilendirme belirteci alınamadı |`Application with identifier 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' was not found in the directory 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant` |Yanlış hizmet sorumlusu ve/veya kiracı KIMLIĞI. |Hizmet sorumlusu ve/veya kiracı KIMLIĞINI doğrulayın.|
|ARM kaynak yanıtını al |`The client 'username@domain.com' with object id 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' does not have authorization to perform action 'Microsoft.HybridCompute/machines/read' over scope '/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01' or the scope is invalid. If access was recently granted, please refresh your credentials."}}" Status Code=403` |Yanlış kimlik bilgileri ve/veya izinler |**Azure bağlı makine ekleme** rolünün bir üyesi olduğunu veya hizmet sorumlunun olduğunu doğrulayın. |
|ARM kaynağı AzcmagentConnect başarısız oldu |`The subscription is not registered to use namespace 'Microsoft.HybridCompute'` |Azure kaynak sağlayıcıları kayıtlı değil. |[Kaynak sağlayıcılarını](./agent-overview.md#register-azure-resource-providers)kaydedin. |
|ARM kaynağı AzcmagentConnect başarısız oldu |`Get https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01?api-version=2019-03-18-preview:  Forbidden` |Proxy sunucusu veya güvenlik duvarı `management.azure.com` uç noktaya erişimi engelliyor. |Uç nokta bağlantısını doğrulayın ve bir güvenlik duvarı veya proxy sunucusu tarafından engellenmiyor. |

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu burada görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* [Microsoft Q&A](https://docs.microsoft.com/answers/topics/azure-arc.html)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabıyla bağlanın. Azure desteği, Azure Community 'yi yanıtlar, destek ve uzmanlar için bağlar.

* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin.