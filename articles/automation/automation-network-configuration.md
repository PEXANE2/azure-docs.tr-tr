---
title: Azure Otomasyonu ağ yapılandırması ayrıntıları
description: Bu makalede, Azure Otomasyonu durum yapılandırması, Azure Otomasyonu karma Runbook Worker, Güncelleştirme Yönetimi ve Değişiklik İzleme ve envanter için gereken ağ bilgilerinin ayrıntıları sağlanmaktadır.
ms.author: magoedte
ms.topic: conceptual
ms.date: 01/26/2021
ms.openlocfilehash: 0add7eed6abbe6c137d423ee4a7ef5f0f60072e3
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98900327"
---
# <a name="azure-automation-network-configuration-details"></a>Azure Otomasyonu ağ yapılandırması ayrıntıları

Bu sayfa, [karma runbook worker ve durum yapılandırması](#hybrid-runbook-worker-and-state-configuration)için gereken ağ ayrıntılarını ve [güncelleştirme yönetimi Ile değişiklik izleme ve envanterini](#update-management-and-change-tracking-and-inventory)sağlar.

## <a name="hybrid-runbook-worker-and-state-configuration"></a>Karma runbook worker ve durum yapılandırması

Karma Runbook Worker için aşağıdaki bağlantı noktası ve URL 'Ler gereklidir ve [Automation durum yapılandırması](automation-dsc-overview.md) 'Nın Azure Otomasyonu ile iletişim kurması gerekir.

* Bağlantı noktası: yalnızca 443 giden internet erişimi için gereklidir
* Genel URL: `*.azure-automation.net`
* US Gov Virginia genel URL 'SI: `*.azure-automation.us`
* Aracı hizmeti: `https://<workspaceId>.agentsvc.azure-automation.net`

### <a name="network-planning-for-hybrid-runbook-worker"></a>Karma Runbook Worker için ağ planlaması

Azure Otomasyonu 'na bağlanıp kaydolmak üzere bir sistem veya Kullanıcı karma Runbook Worker için, bu bölümde açıklanan bağlantı noktası numarasına ve URL 'Lere erişimi olmalıdır. Çalışan Ayrıca Log Analytics aracısının Azure Izleyici Log Analytics çalışma alanına bağlanması için [gereken bağlantı noktalarına ve URL 'lere](../azure-monitor/platform/agent-windows.md) erişebilmelidir.

Belirli bir bölge için tanımlanan bir Otomasyon hesabınız varsa, karma Runbook Worker iletişimini ilgili bölge veri merkezi ile kısıtlayabilirsiniz. Azure Otomasyonu tarafından gerekli DNS kayıtları için [kullanılan DNS kayıtlarını](how-to/automation-region-dns-records.md) gözden geçirin.

### <a name="configuration-of-private-networks-for-state-configuration"></a>Durum yapılandırması için özel ağların yapılandırması

Düğümleriniz özel bir ağda bulunuyorsa, yukarıda tanımlanan bağlantı noktası ve URL 'Ler gereklidir. Bu kaynaklar yönetilen düğüm için ağ bağlantısı sağlar ve DSC 'nin Azure Otomasyonu ile iletişim kurmasına izin verir.

[Waitfor * kaynakları](/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)gibi düğümler arasında ILETIŞIM kuran DSC kaynakları kullanıyorsanız, düğümler arasında trafiğe de izin vermeniz gerekir. Bu ağ gereksinimlerini anlamak için her DSC kaynağına yönelik belgelere bakın.

TLS 1,2 için istemci gereksinimlerini anlamak üzere bkz. [Azure Otomasyonu Için tls 1,2 zorlaması](automation-managing-data.md#tls-12-enforcement-for-azure-automation).

## <a name="update-management-and-change-tracking-and-inventory"></a>Güncelleştirme Yönetimi ve Değişiklik İzleme ve envanter

Bu tablodaki adresler hem Güncelleştirme Yönetimi hem de Değişiklik İzleme ve envanter için gereklidir. Tabloyu izleyen paragraf, her ikisi için de geçerlidir.

Bu adreslere yönelik iletişim **443 numaralı bağlantı noktasını** kullanır.

|Azure Genel  |Azure Kamu  |
|---------|---------|
|\*.ods.opinsights.azure.com    | \*. ods.opinsights.azure.us         |
|\*.oms.opinsights.azure.com     | \*. oms.opinsights.azure.us        |
|\*.blob.core.windows.net | \*. blob.core.usgovcloudapi.net|
|\*.azure-automation.net | \*. azure-automation.us|

Ağ grubu güvenlik kuralları oluşturduğunuzda veya Otomasyon hizmetine ve Log Analytics çalışma alanına giden trafiğe izin vermek üzere Azure Güvenlik Duvarı 'nı yapılandırdığınızda, **guestandhybridmanagement** ve **AzureMonitor** [hizmet etiketlerini](../virtual-network/service-tags-overview.md#available-service-tags) kullanın. Bu, ağ güvenliği kurallarınızın devam eden yönetimini basitleştirir. Azure VM 'lerinizin Otomasyon hizmetine güvenli bir şekilde ve özel olarak bağlanmak için [Azure özel bağlantı kullanımı](./how-to/private-link-security.md)' nı gözden geçirin. Şirket içi güvenlik duvarı yapılandırmalarının bir parçası olarak dahil edilecek geçerli hizmet etiketi ve Aralık bilgilerini almak için bkz. [INDIRILEBILIR JSON dosyaları](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files).

## <a name="next-steps"></a>Sonraki adımlar

* [Otomasyon güncelleştirme yönetimi genel bakış](update-management\overview.md)hakkında bilgi edinin.
* [Karma Runbook Worker](automation-hybrid-runbook-worker.md)hakkında bilgi edinin.
* [Değişiklik izleme ve envanter](change-tracking\overview.md)hakkında bilgi edinin.
* [Otomasyon durumu yapılandırması](automation-dsc-overview.md)hakkında bilgi edinin.
