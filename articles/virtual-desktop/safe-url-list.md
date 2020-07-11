---
title: Windows sanal masaüstü güvenli URL listesi-Azure
description: Windows sanal masaüstü dağıtımınızın istendiği gibi çalıştığından emin olmak için engellemesini kaldırmanız gereken URL 'lerin bir listesi.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 07/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 9b1bdfc326ff217e68785d823b4af046af3241b7
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225301"
---
# <a name="safe-url-list"></a>Güvenli URL listesi

Windows sanal masaüstü dağıtımınızın düzgün çalışması için bazı URL 'Lerin engellemesini kaldırmanız gerekir. Bu makalede, hangilerinin güvenli olduğunu bilmeniz için bu URL 'Ler listelenir.

## <a name="virtual-machines"></a>Sanal makineler

Windows sanal masaüstü için oluşturduğunuz Azure sanal makinelerinin aşağıdaki URL 'Lere erişimi olmalıdır:

|Adres|Giden TCP bağlantı noktası|Amaç|Hizmet etiketi|
|---|---|---|---|
|*. wvd.microsoft.com|443|Hizmet trafiği|WindowsVirtualDesktop|
|mrsglobalsteus2prod.blob.core.windows.net|443|Aracı ve SXS yığın güncelleştirmeleri|AzureCloud|
|*.core.windows.net|443|Aracı trafiği|AzureCloud|
|*.servicebus.windows.net|443|Aracı trafiği|AzureCloud|
|prod.warmpath.msftcloudes.com|443|Aracı trafiği|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Market|AzureCloud|
|kms.core.windows.net|1688|Windows etkinleştirme|İnternet|
|wvdportalstorageblob.blob.core.windows.net|443|Azure portal desteği|AzureCloud|

>[!IMPORTANT]
>Windows sanal masaüstü artık FQDN etiketini destekliyor. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı 'Nı kullanarak Windows sanal masaüstü dağıtımlarını koruma](../firewall/protect-windows-virtual-desktop.md).
>
>Hizmet sorunlarını engellemek için URL 'Ler yerine FQDN etiketleri veya hizmet etiketleri kullanmanızı öneririz. Listelenen URL 'Ler ve Etiketler yalnızca Windows sanal masaüstü siteleri ve kaynaklarına karşılık gelir. Azure Active Directory gibi diğer hizmetlere yönelik URL 'Ler dahil değildir.

Aşağıdaki tabloda, Azure sanal makinelerinizin erişebileceği isteğe bağlı URL 'Ler listelenmektedir:

|Adres|Giden TCP bağlantı noktası|Amaç|Hizmet etiketi|
|---|---|---|---|
|*.microsoftonline.com|443|Microsoft Online Services kimlik doğrulaması|Hiçbiri|
|*. events.data.microsoft.com|443|Telemetri hizmeti|Hiçbiri|
|www.msftconnecttest.com|443|İşletim sisteminin Internet 'e bağlı olup olmadığını algılar|Hiçbiri|
|*. prod.do.dsp.mp.microsoft.com|443|Windows Update|Hiçbiri|
|login.windows.net|443|Microsoft Online Services 'da oturum açın Microsoft 365|Hiçbiri|
|*. sfx.ms|443|OneDrive istemci yazılımı güncelleştirmeleri|Hiçbiri|
|*. digicert.com|443|Sertifika iptal denetimi|Hiçbiri|

>[!NOTE]
>Windows sanal masaüstü 'nde, ağ trafiğine izin vermek için engelbir IP adresi aralığı listesi yoktur. Şu anda yalnızca belirli URL 'Lerin engellenmesini destekliyoruz.
>
>Gerekli Azure Active Directory ilgili URL 'ler de dahil olmak üzere, Office ile ilgili güvenli URL 'Lerin bir listesi için bkz. [office 365 URL 'leri ve IP adresi aralıkları](/office365/enterprise/urls-and-ip-address-ranges).
>
>Hizmet trafiğiyle ilgili URL 'Ler için (*) joker karakterini kullanmanız gerekir. Aracıyla ilgili trafik için * kullanmayı tercih ediyorsanız, joker karakterleri olmayan URL 'Leri bulma hakkında bilgi edinebilirsiniz:
>
>1. Sanal makinelerinizi Windows sanal masaüstü ana bilgisayar havuzuna kaydedin.
>2. **Olay Görüntüleyicisi**'ni açın, sonra **Windows günlükleri**  >  **uygulaması**  >  **WVD-Agent** ' a gidin ve olay kimliği 3702 ' i arayın.
>3. Olay KIMLIĞI 3702 altında bulduğunuz URL 'Leri beyaz listeye ekleyin. Olay KIMLIĞI 3702 altındaki URL 'Ler bölgeye özeldir. Sanal makinelerinizi dağıtmak istediğiniz her bölge için ilgili URL 'lerle engellemeyi kaldırma işlemini tekrarlamanız gerekir.

## <a name="remote-desktop-clients"></a>Uzak Masaüstü istemcileri

Kullandığınız herhangi bir uzak masaüstü istemcisinin aşağıdaki URL 'Lere erişimi olmalıdır:

|Adres|Giden TCP bağlantı noktası|Amaç|İstemci (ler)|
|---|---|---|---|
|*. wvd.microsoft.com|443|Hizmet trafiği|Tümü|
|*.servicebus.windows.net|443|Sorun giderme verileri|Tümü|
|go.microsoft.com|443|Microsoft FWLinks|Tümü|
|aka.ms|443|Microsoft URL kısaltalayıcı|Tümü|
|docs.microsoft.com|443|Belgeler|Tümü|
|privacy.microsoft.com|443|Gizlilik bildirimi|Tümü|
|query.prod.cms.rt.microsoft.com|443|İstemci güncelleştirmeleri|Windows Masaüstü|

>[!IMPORTANT]
>Bu URL 'Leri açmak, güvenilir bir istemci deneyimi için gereklidir. Bu URL 'lere erişimin engellenmesi desteklenmez ve hizmet işlevlerini etkiler. Bu URL 'Ler yalnızca istemci sitelerine ve kaynaklarına karşılık gelir ve Azure Active Directory gibi diğer hizmetlere yönelik URL 'Ler içermez.