---
title: VM'ler için Azure İzleyici Konuk sistem durumu sorunlarını giderme (Önizleme)
description: VM'ler için Azure İzleyici sistem durumu ile ilgili sorunlar yaşıyorsanız gerçekleştirebileceğiniz sorun giderme adımlarını açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/08/2020
ms.openlocfilehash: 048230bb85d2ac599efc6bd3fb326f98038cce57
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687168"
---
# <a name="troubleshoot-azure-monitor-for-vms-guest-health-preview"></a>VM'ler için Azure İzleyici Konuk sistem durumu sorunlarını giderme (Önizleme)
Bu makalede, VM'ler için Azure İzleyici sistem durumu ile ilgili sorunlar yaşıyorsanız gerçekleştirebileceğiniz sorun giderme adımları açıklanmaktadır.

## <a name="error-message-that-no-data-is-available"></a>Kullanılabilir veri yok iletisi 

![Veri yok](media/vminsights-health-troubleshoot/no-data.png)


### <a name="verify-that-the-virtual-machine-meets-configuration-requirements"></a>Sanal makinenin yapılandırma gereksinimlerini karşıladığını doğrulayın

1. Sanal makinenin bir Azure sanal makinesi olduğunu doğrulayın. Sunucular için Azure Arc Şu anda desteklenmiyor.
2. Sanal makinenin [desteklenen bir işletim sistemi](vminsights-health-enable.md?current-limitations.md)çalıştırdığından emin olun.
3. Sanal makinenin [desteklenen bir bölgede](vminsights-health-enable.md?current-limitations.md)yüklü olduğunu doğrulayın.
4. Log Analytics çalışma alanının [desteklenen bir bölgede](vminsights-health-enable.md?current-limitations.md)yüklü olduğunu doğrulayın.

### <a name="verify-that-the-vm-is-properly-onboarded"></a>VM 'nin düzgün şekilde eklendi olduğunu doğrulama
Azure Izleyici Aracısı uzantısının ve konuk VM sistem durumu aracısının sanal makinede başarıyla sağlandığını doğrulayın. Azure portal sanal makinenin menüsünden **Uzantılar** ' ı seçin. İki aracı listeleniyorsa, bkz []() ..

![VM uzantıları](media/vminsights-health-troubleshoot/extensions.png)

### <a name="verify-the-system-assigned-identity-is-enabled-on-the-virtual-machine"></a>Sanal makinede sistem tarafından atanan kimliğin etkinleştirildiğini doğrulayın
Sistem tarafından atanan kimliğin sanal makinede etkinleştirildiğini doğrulayın. Azure portal sanal makinenin menüsünden **kimlik** ' i seçin. İki aracı listeleniyorsa, bkz []() ..

![Sistem tarafından atanan kimlik](media/vminsights-health-troubleshoot/system-identity.png)

### <a name="verify-data-collection-rule"></a>Veri toplama kuralını doğrula
Veri kaynağı olarak sistem durumu uzantısı belirten veri toplama kuralının sanal makineyle ilişkili olduğunu doğrulayın.

## <a name="error-message-for-bad-request-due-to-insufficient-permissions"></a>Yetersiz izinler nedeniyle hatalı istek için hata iletisi
Bu hata, **Microsoft. WorkloadMonitor** kaynak sağlayıcısı 'nın abonelikte kayıtlı olmadığını gösterir. Bu kaynak sağlayıcıyı kaydetme hakkındaki ayrıntılar için bkz. [Azure kaynak sağlayıcıları ve türleri](/azure/azure-resource-manager/management/resource-providers-and-types#register-resource-provider) . 

![Hatalı istek](media/vminsights-health-troubleshoot/bad-request.png)

## <a name="next-steps"></a>Sonraki adımlar

- [VM'ler için Azure İzleyici Konuk sistem durumu özelliğine genel bakış alın](vminsights-health-overview.md)
