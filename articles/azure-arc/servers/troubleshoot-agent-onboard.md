---
title: Azure Arc etkin sunucular (Önizleme) Aracısı bağlantı sorunlarını giderme
description: Bu makalede, hizmete bağlanmaya çalışırken Azure Arc etkin sunucuları (Önizleme) ile birlikte gelen bağlı makine aracısında sorunları gidermeye ve gidermeye nasıl çözüm yapılacağı açıklanır.
ms.date: 07/20/2020
ms.topic: conceptual
ms.openlocfilehash: aeb370fb6cd4eacf20c1acf29e84c03e5e322d39
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88213535"
---
# <a name="troubleshoot-the-connected-machine-agent-connection-issues"></a>Bağlı makine Aracısı bağlantı sorunlarını giderme

Bu makalede, Azure Arc etkin sunucuları (Önizleme) Windows veya Linux için bağlı makine Aracısı 'nı yapılandırmaya çalışırken oluşabilecek sorunların giderilmesi ve çözümlenmesi hakkında bilgi sağlanır. Hizmetle bağlantı yapılandırılırken hem etkileşimli hem de ölçekli yükleme yöntemleri dahildir. Genel bilgiler için bkz. [Arc etkin sunucularına genel bakış](./overview.md).

## <a name="agent-verbose-log"></a>Aracı ayrıntılı günlüğü

Bu makalenin ilerleyen kısımlarında açıklanan sorun giderme adımlarını takip etmeden önce, ihtiyacınız olan en düşük bilgiler ayrıntılı günlüğdeğer olacaktır. Verbose (-v) bağımsız değişkeni kullanıldığında **azcmagent** aracı komutlarının çıktısını içerir. Günlük dosyaları `%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log` Windows ve Linux için üzerine yazılır `/var/opt/azcmagent/log/azcmagent.log` .

### <a name="windows"></a>Windows

Aşağıda, etkileşimli bir yükleme gerçekleştirirken Windows için bağlı makine Aracısı ile ayrıntılı günlüğe yazmayı etkinleştirme komutuna bir örnek verilmiştir.

```console
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Aşağıda, bir hizmet sorumlusu kullanarak ölçekli bir yükleme gerçekleştirirken Windows için bağlı makine Aracısı ile ayrıntılı günlüğe yazmayı etkinleştirme komutuna bir örnek verilmiştir.

```console
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

>[!NOTE]
>**Azcmagent**çalıştırmak için Linux makinelerde *kök* erişim izinlerine sahip olmanız gerekir.

```bash
azcmagent connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Aşağıda, bir hizmet sorumlusu kullanarak ölçekli bir yükleme gerçekleştirirken Linux için bağlı makine Aracısı ile ayrıntılı günlüğe yazmayı etkinleştirme komutuna bir örnek verilmiştir.

```bash
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
|Yetkilendirme belirteci cihaz akışı alınamadı  |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp lookup login.windows.net: no such host`. | Grup ilkesi nesne *Bilgisayar Yapılandırması \ Yönetim Şablonları \ sistem \ Kullanıcı profilleri \ sistem yeniden başlatıldığında belirtilen gün sayısından daha eski Kullanıcı profillerini Sil* etkin. | GPO 'nun etkinleştirildiğini ve etkilenen makinenin hedeflendiğini doğrulayın. Daha fazla ayrıntı için bkz. dipnot <sup>[1](#footnote1)</sup> . |
|SPN 'den yetkilendirme belirteci alınamadı |`Failed to execute the refresh request. Error = 'Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/token?api-version=1.0: Forbidden'` |Proxy veya güvenlik duvarı `login.windows.net` uç noktaya erişimi engelliyor. |Uç nokta bağlantısını doğrulayın ve bir güvenlik duvarı veya proxy sunucusu tarafından engellenmiyor. |
|SPN 'den yetkilendirme belirteci alınamadı |`Invalid client secret is provided` |Yanlış veya geçersiz hizmet sorumlusu gizli anahtarı. |Hizmet sorumlusu gizliliğini doğrulayın. |
| SPN 'den yetkilendirme belirteci alınamadı |`Application with identifier 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' was not found in the directory 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant` |Yanlış hizmet sorumlusu ve/veya kiracı KIMLIĞI. |Hizmet sorumlusu ve/veya kiracı KIMLIĞINI doğrulayın.|
|ARM kaynak yanıtını al |`The client 'username@domain.com' with object id 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' does not have authorization to perform action 'Microsoft.HybridCompute/machines/read' over scope '/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01' or the scope is invalid. If access was recently granted, please refresh your credentials."}}" Status Code=403` |Yanlış kimlik bilgileri ve/veya izinler |**Azure bağlı makine ekleme** rolünün bir üyesi olduğunu veya hizmet sorumlunun olduğunu doğrulayın. |
|ARM kaynağı AzcmagentConnect başarısız oldu |`The subscription is not registered to use namespace 'Microsoft.HybridCompute'` |Azure kaynak sağlayıcıları kayıtlı değil. |[Kaynak sağlayıcılarını](./agent-overview.md#register-azure-resource-providers)kaydedin. |
|ARM kaynağı AzcmagentConnect başarısız oldu |`Get https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01?api-version=2019-03-18-preview:  Forbidden` |Proxy sunucusu veya güvenlik duvarı `management.azure.com` uç noktaya erişimi engelliyor. |Uç nokta bağlantısını doğrulayın ve bir güvenlik duvarı veya proxy sunucusu tarafından engellenmiyor. |

<a name="footnote1"></a><sup>1</sup> Bu GPO etkin ve bağlı makine aracısına sahip makineler için geçerliyse, *hımds* hizmeti için belirtilen yerleşik hesapla ilişkili kullanıcı profilini siler. Sonuç olarak, yerel sertifika deposunda önbelleğe alınan hizmetle iletişim kurmak için kullanılan kimlik doğrulama sertifikasını 30 gün boyunca de siler. 30 günlük sınırından önce, sertifikayı yenilemek için bir girişimde bulunuldu. Bu sorunu çözmek için, [makinenin kaydını silme](manage-agent.md#unregister-machine) adımlarını izleyin ve ardından çalıştıran hizmete yeniden kaydedin `azcmagent connect` .

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu burada görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* [Microsoft Q&A](/answers/topics/azure-arc.html)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabıyla bağlanın. Azure desteği, Azure Community 'yi yanıtlar, destek ve uzmanlar için bağlar.

* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin.
