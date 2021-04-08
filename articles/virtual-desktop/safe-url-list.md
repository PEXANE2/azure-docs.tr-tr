---
title: Windows sanal masaüstü gerekli URL listesi-Azure
description: Windows sanal masaüstü dağıtımınızın istendiği gibi çalıştığından emin olmak için engellemesini kaldırmanız gereken URL 'lerin bir listesi.
author: Heidilohr
ms.topic: conceptual
ms.date: 12/04/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 872a67d8d9f41c0c809df54304352b2a5f58e011
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98251769"
---
# <a name="required-url-list"></a>Gerekli URL listesi

Windows sanal masaüstü 'Nü dağıtmak ve kullanmak için, sanal makinelerinizin (VM 'Ler) dilediğiniz zaman erişebilmeleri için belirli URL 'Lerin engellemesini kaldırmanız gerekir. Bu makalede, Windows sanal masaüstü 'Nün düzgün çalışması için, engellemesini kaldırmanız gereken URL 'Ler listelenir. 

>[!IMPORTANT]
>Windows sanal masaüstü, bu makalede listelenen URL 'Leri engelleyen dağıtımları desteklemez.

## <a name="virtual-machines"></a>Sanal makineler

Windows sanal masaüstü için oluşturduğunuz Azure sanal makineleri, Azure ticari bulutundaki aşağıdaki URL 'Lere erişebilmelidir:

|Adres|Giden TCP bağlantı noktası|Amaç|Hizmet etiketi|
|---|---|---|---|
|*. wvd.microsoft.com|443|Hizmet trafiği|WindowsVirtualDesktop|
|gcs.prod.monitoring.core.windows.net|443|Aracı trafiği|AzureCloud|
|production.diagnostics.monitoring.core.windows.net|443|Aracı trafiği|AzureCloud|
|* xt.blob.core.windows.net|443|Aracı trafiği|AzureCloud|
|* eh.servicebus.windows.net|443|Aracı trafiği|AzureCloud|
|* xt.table.core.windows.net|443|Aracı trafiği|AzureCloud|
|* xt.queue.core.windows.net|443|Aracı trafiği|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Market|AzureCloud|
|kms.core.windows.net|1688|Windows etkinleştirme|İnternet|
|mrsglobalsteus2prod.blob.core.windows.net|443|Aracı ve SXS yığın güncelleştirmeleri|AzureCloud|
|wvdportalstorageblob.blob.core.windows.net|443|Azure portal desteği|AzureCloud|
| 169.254.169.254 | 80 | [Azure örnek meta veri hizmeti uç noktası](../virtual-machines/windows/instance-metadata-service.md) | Yok |
| 168.63.129.16 | 80 | [Oturum konak durumu izleme](../virtual-network/network-security-groups-overview.md#azure-platform-considerations) | Yok |

>[!IMPORTANT]
>Windows sanal masaüstü artık FQDN etiketini destekliyor. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı 'Nı kullanarak Windows sanal masaüstü dağıtımlarını koruma](../firewall/protect-windows-virtual-desktop.md).
>
>Hizmet sorunlarını engellemek için URL 'Ler yerine FQDN etiketleri veya hizmet etiketleri kullanmanızı öneririz. Listelenen URL 'Ler ve Etiketler yalnızca Windows sanal masaüstü siteleri ve kaynaklarına karşılık gelir. Azure Active Directory gibi diğer hizmetlere yönelik URL 'Ler dahil değildir.

Windows sanal masaüstü için oluşturduğunuz Azure sanal makineleri, Azure Kamu Bulutu 'nda aşağıdaki URL 'Lere erişebilmelidir:

|Adres|Giden TCP bağlantı noktası|Amaç|Hizmet etiketi|
|---|---|---|---|
|*. wvd.microsoft.us|443|Hizmet trafiği|WindowsVirtualDesktop|
|gcs.monitoring.core.usgovcloudapi.net|443|Aracı trafiği|AzureCloud|
|monitoring.core.usgovcloudapi.net|443|Aracı trafiği|AzureCloud|
|fairfax.warmpath.usgovcloudapi.net|443|Aracı trafiği|AzureCloud|
|* xt.blob.core.usgovcloudapi.net|443|Aracı trafiği|AzureCloud|
|*. servicebus.usgovcloudapi.net|443|Aracı trafiği|AzureCloud|
|* xt.table.core.usgovcloudapi.net|443|Aracı trafiği|AzureCloud|
|Kms.core.usgovcloudapi.net|1688|Windows etkinleştirme|İnternet|
|mrsglobalstugviffx.blob.core.usgovcloudapi.net|443|Aracı ve SXS yığın güncelleştirmeleri|AzureCloud|
|wvdportalstorageblob.blob.core.usgovcloudapi.net|443|Azure portal desteği|AzureCloud|
| 169.254.169.254 | 80 | [Azure örnek meta veri hizmeti uç noktası](../virtual-machines/windows/instance-metadata-service.md) | Yok |
| 168.63.129.16 | 80 | [Oturum konak durumu izleme](../virtual-network/network-security-groups-overview.md#azure-platform-considerations) | Yok |

Aşağıdaki tabloda, Azure sanal makinelerinizin erişebileceği isteğe bağlı URL 'Ler listelenmektedir:

|Adres|Giden TCP bağlantı noktası|Amaç|Azure gov|
|---|---|---|---|
|*.microsoftonline.com|443|Microsoft Online Services kimlik doğrulaması|login.microsoftonline.us|
|*. events.data.microsoft.com|443|Telemetri Hizmeti|Yok|
|www.msftconnecttest.com|443|İşletim sisteminin Internet 'e bağlı olup olmadığını algılar|Yok|
|*. prod.do.dsp.mp.microsoft.com|443|Windows Update|Yok|
|login.windows.net|443|Microsoft Online Services 'da oturum açın Microsoft 365|login.microsoftonline.us|
|*. sfx.ms|443|OneDrive istemci yazılımı güncelleştirmeleri|oneclient.sfx.ms|
|*. digicert.com|443|Sertifika iptal denetimi|Yok|
|*. azure-dns.com|443|Azure DNS çözümleme|Yok|
|*. azure-dns.net|443|Azure DNS çözümleme|Yok|

>[!NOTE]
>Windows sanal masaüstü 'nde, ağ trafiğine izin vermek için engelbir IP adresi aralığı listesi yoktur. Şu anda yalnızca belirli URL 'Lerin engellenmesini destekliyoruz.
>
>Yeni nesil güvenlik duvarı (NGFW) kullanıyorsanız, bağlanabildiğinizden emin olmak için Azure IP 'Leri için özel olarak oluşturulmuş dinamik bir liste kullanmanız gerekir.
>
>Gerekli Azure Active Directory ilgili URL 'ler de dahil olmak üzere, Office ile ilgili güvenli URL 'Lerin bir listesi için bkz. [office 365 URL 'leri ve IP adresi aralıkları](/office365/enterprise/urls-and-ip-address-ranges).
>
>Hizmet trafiğiyle ilgili URL 'Ler için (*) joker karakterini kullanmanız gerekir. Aracıyla ilgili trafik için * kullanmayı tercih ediyorsanız, joker karakterleri olmayan URL 'Leri bulma hakkında bilgi edinebilirsiniz:
>
>1. Sanal makinelerinizi Windows sanal masaüstü ana bilgisayar havuzuna kaydedin.
>2. **Olay Görüntüleyicisi**'ni açın, sonra **Windows günlükleri**  >  **uygulaması**  >  **WVD-Agent** ' a gidin ve olay kimliği 3701 ' i arayın.
>3. Olay KIMLIĞI 3701 altında bulduğunuz URL 'Lerin engelini kaldırın. Olay KIMLIĞI 3701 altındaki URL 'Ler bölgeye özeldir. Sanal makinelerinizi dağıtmak istediğiniz her bölge için ilgili URL 'lerle engellemeyi kaldırma işlemini tekrarlamanız gerekir.

## <a name="remote-desktop-clients"></a>Uzak Masaüstü istemcileri

Kullandığınız herhangi bir uzak masaüstü istemcisinin aşağıdaki URL 'Lere erişimi olmalıdır:

|Adres|Giden TCP bağlantı noktası|Amaç|İstemci (ler)|Azure gov|
|---|---|---|---|---|
|*. wvd.microsoft.com|443|Hizmet trafiği|Tümü|*. wvd.microsoft.us|
|*.servicebus.windows.net|443|Sorun giderme verileri|Tümü|*. servicebus.usgovcloudapi.net|
|go.microsoft.com|443|Microsoft FWLinks|Tümü|Yok|
|aka.ms|443|Microsoft URL kısaltalayıcı|Tümü|Yok|
|docs.microsoft.com|443|Belgeler|Tümü|Yok|
|privacy.microsoft.com|443|Gizlilik bildirimi|Tümü|Yok|
|query.prod.cms.rt.microsoft.com|443|İstemci güncelleştirmeleri|Windows Masaüstü|Yok|

>[!IMPORTANT]
>Bu URL 'Leri açmak, güvenilir bir istemci deneyimi için gereklidir. Bu URL 'lere erişimin engellenmesi desteklenmez ve hizmet işlevlerini etkiler.
>
>Bu URL 'Ler yalnızca istemci sitelerine ve kaynaklarına karşılık gelir. Bu liste Azure Active Directory gibi diğer hizmetlere yönelik URL 'Ler içermez. Azure Active Directory URL 'Ler, [Office 365 URL 'lerinde ve IP adresi ARALıKLARıNDA](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online)kimlik 56 ' de bulunabilir.