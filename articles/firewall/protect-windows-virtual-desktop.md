---
title: Azure Güvenlik Duvarı 'nı kullanarak Windows sanal masaüstünü koruma
description: Azure Güvenlik Duvarı 'nı kullanarak Windows sanal masaüstü dağıtımlarını koruma hakkında bilgi edinin
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: victorh
ms.openlocfilehash: c5d7281d50c151722303b48b2b28a597eec72d79
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254180"
---
# <a name="use-azure-firewall-to-protect-window-virtual-desktop-deployments"></a>Azure Güvenlik Duvarı 'nı kullanarak Windows sanal masaüstü dağıtımlarını koruma

Windows sanal masaüstü (WVD), Azure üzerinde çalışan bir masaüstü ve uygulama sanallaştırma hizmetidir. Bir son kullanıcı Windows sanal masaüstü ortamına bağlandığında, oturumları bir konak havuzu tarafından çalıştırılır. Konak havuzu, oturum ana bilgisayarları olarak Windows sanal masaüstüne kaydeden Azure sanal makineleri koleksiyonudur. Bu sanal makineler sanal ağınızda çalışır ve sanal ağ güvenlik denetimlerine tabidir. WVD Service 'e giden Internet erişiminin düzgün çalışması ve ayrıca son kullanıcılar için giden Internet erişimine ihtiyacı olabilir. Azure Güvenlik Duvarı, ortamınızı kilitlemenize ve giden trafiği filtrelemenize yardımcı olabilir.

[![Windows sanal masaüstü mimarisi](media/protect-windows-virtual-desktop/windows-virtual-desktop-architecture-diagram.png)](media/protect-windows-virtual-desktop/windows-virtual-desktop-architecture-diagram.png#lightbox)

Azure Güvenlik Duvarı 'nı kullanarak WVD Host havuzunuza ek koruma sağlamak için bu makaledeki yönergeleri izleyin.

## <a name="prerequisites"></a>Ön koşullar


 - Dağıtılan bir WVD ortamı ve konak Havuzu.

   Daha fazla bilgi için bkz. [öğretici: Azure Marketi 'ni kullanarak bir konak havuzu oluşturma](../virtual-desktop/create-host-pools-azure-marketplace.md) ve [Azure Resource Manager şablonuyla bir konak havuzu oluşturma](../virtual-desktop/create-host-pools-arm-template.md).

WVD ortamları hakkında daha fazla bilgi edinmek için bkz. [Windows sanal masaüstü ortamı](../virtual-desktop/environment-setup.md).

## <a name="host-pool-outbound-access-to-windows-virtual-desktop"></a>Windows sanal masaüstüne giden ana bilgisayar havuzu erişimi

Windows sanal masaüstü için oluşturduğunuz Azure sanal makineleri, düzgün şekilde çalışmak için birkaç tam etki alanı adına (FQDN) erişime sahip olmalıdır. Azure Güvenlik Duvarı, bu yapılandırmayı basitleştirmek için bir Windows sanal masaüstü FQDN etiketi sağlar. Giden WVD platform trafiğine izin vermek için aşağıdaki adımları kullanın:

- Azure Güvenlik Duvarı 'nı dağıtın ve tüm trafiği Azure Güvenlik Duvarı aracılığıyla yönlendirmek için WVD konak havuzu alt ağı Kullanıcı tanımlı yol (UDR) yapılandırın. Varsayılan yol artık güvenlik duvarını işaret ediyor.
- Bir uygulama kuralı koleksiyonu oluşturun ve *Windowsvirtualdesktop* FQDN etiketini etkinleştirmek için bir kural ekleyin. Kaynak IP adresi aralığı konak havuzunun sanal ağı, protokol **https**ve hedef Ise **windowsvirtualdesktop**' dır.

- WVD Host havuzunuzun gerekli depolama ve hizmet veri yolu hesaplarının kümesi dağıtıma özgüdür, bu nedenle WindowsVirtualDesktop FQDN etiketinde henüz yakalanmamıştır. Bunu aşağıdaki yollarla ele alabilirsiniz:

   - Ana bilgisayar havuzunuzdaki https erişimine * xt.blob.core.windows.net, * eh.servicebus.windows.net ve * xt.table.core.windows.net olarak izin verin. Bu joker karakter FQDN 'leri gereken erişimi etkinleştirir, ancak daha az kısıtlayıcıdır.
   - Gerekli FQDN 'leri tam olarak listelemek için aşağıdaki Log Analytics sorgusunu kullanın ve ardından Güvenlik Duvarı uygulama kurallarınızı açık olarak bunlara izin verin:
   ```
   AzureDiagnostics
   | where Category == "AzureFirewallApplicationRule"
   | search "Deny"
   | search "gsm*eh.servicebus.windows.net" or "gsm*xt.blob.core.windows.net" or "gsm*xt.table.core.windows.net"
   | parse msg_s with Protocol " request from " SourceIP ":" SourcePort:int " to " FQDN ":" *
   | project TimeGenerated,Protocol,FQDN
   ```

- Ağ kuralı koleksiyonu oluşturma aşağıdaki kuralları ekleyin:

   - DNS 'ye izin ver – TCP ve UDP bağlantı noktaları 53 için özel IP adresi ekleme işleminden gelen trafiğe izin ver.
   - KMS 'ye izin ver – WVD sanal makinelerinizin Windows etkinleştirme hizmeti TCP bağlantı noktası 1688 ' e giden trafiğe izin verin Hedef IP adresleri hakkında daha fazla bilgi için bkz. [Zorlamalı tünel senaryosunda Windows etkinleştirme başarısız oluyor](../virtual-machines/troubleshooting/custom-routes-enable-kms-activation.md#solution).

> [!NOTE]
> Bazı dağıtımlar DNS kurallarına ihtiyaç duymayabilir, örneğin Azure Active Directory etki alanı denetleyicileri DNS sorgularını 168.63.129.16 adresinde Azure DNS.

## <a name="host-pool-outbound-access-to-the-internet"></a>Internet 'e giden ana havuz erişimi

Kuruluşunuzun ihtiyaçlarına bağlı olarak son kullanıcılarınız için güvenli giden Internet erişimini etkinleştirmek isteyebilirsiniz. İzin verilen hedeflerin listesinin iyi tanımlanmış olduğu durumlarda (örneğin, [Office 365 erişimi](https://docs.microsoft.com/Office365/Enterprise/office-365-ip-web-service)), gerekli erişimi yapılandırmak Için Azure Güvenlik Duvarı uygulamasını ve ağ kurallarını kullanabilirsiniz. Bu, en iyi performansı elde etmek için son kullanıcı trafiğini doğrudan Internet 'e yönlendirir.

Mevcut bir şirket içi güvenli Web ağ geçidini kullanarak giden Kullanıcı Internet trafiğini filtrelemek istiyorsanız, açık bir ara sunucu yapılandırmasıyla WVD konak havuzunda çalışan Web tarayıcılarını veya diğer uygulamaları yapılandırabilirsiniz. Örneğin, [Ara sunucu ayarlarını yapılandırmak Için Microsoft Edge komut satırı seçeneklerini kullanma](https://docs.microsoft.com/deployedge/edge-learnmore-cmdline-options-proxy-settings)konusuna bakın. Bu proxy ayarları yalnızca son kullanıcı Internet erişiminizi etkiler ve doğrudan Azure Güvenlik Duvarı aracılığıyla WVD platform giden trafiğine izin verir.

## <a name="additional-considerations"></a>Diğer konular

Gereksinimlerinize bağlı olarak ek güvenlik duvarı kuralları yapılandırmanız gerekebilir:

- NTP sunucusu erişimi

   Varsayılan olarak, Windows çalıştıran sanal makineler, zaman eşitlemeye yönelik UDP bağlantı noktası 123 üzerinden time.windows.com 'e bağlanır. Bu erişime izin vermek için veya ortamınızda kullandığınız bir zaman sunucusu için bir ağ kuralı oluşturun.


## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü hakkında daha fazla bilgi edinin: [Windows sanal masaüstü nedir?](../virtual-desktop/overview.md)