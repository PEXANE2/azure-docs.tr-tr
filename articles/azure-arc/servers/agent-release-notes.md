---
title: Azure Arc etkin sunucular aracısındaki yenilikler
description: Bu makalede, Azure Arc etkin sunucular aracısına yönelik sürüm notları bulunur. Özetlenen birçok sorun için ek ayrıntıların bağlantıları vardır.
ms.topic: conceptual
ms.date: 09/16/2020
ms.openlocfilehash: 1918d03b5bbfaaa64b7d74c18fad4eb9a86800a0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90908164"
---
# <a name="whats-new-with-azure-arc-enabled-servers-agent"></a>Azure Arc etkin sunucular aracısındaki yenilikler

Azure Arc etkin sunucular bağlı makine Aracısı, iyileştirmeleri sürekli olarak alır. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri

## <a name="september-2020"></a>Eylül 2020

Sürüm: 1,0 (genel kullanılabilirlik)

### <a name="plan-for-change"></a>Değişiklik planı

- Önizleme aracıları (1,0 ' den eski tüm sürümler) için destek, gelecekteki bir hizmet güncelleştirmesinde kaldırılacaktır.
- Geri dönüş uç noktası için destek kaldırıldı `.azure-automation.net` . Bir ara sunucu varsa, uç noktaya izin vermeniz gerekir `*.his.arc.azure.com` .
- Bağlı makine Aracısı Azure 'da barındırılan bir sanal makinede yüklüyse, VM uzantıları Arc etkin sunucular kaynağından yüklenemez veya değiştirilemez. Bu, sanal makinenin **Microsoft. COMPUTE** ve **Microsoft. hybridcompute** kaynağından çakışan uzantı işlemlerinin gerçekleştirilmesini önmaktır. Tüm uzantı işlemleri için makineye ait **Microsoft. COMPUTE** kaynağını kullanın.
- Konuk yapılandırma işleminin adı, *GCD* 'de Linux üzerinde *gcad* ve *gcservice* ile *gcarcservice* arasında değiştirilmiştir.

### <a name="new-feature"></a>Yeni Özellik

- `azcmagent logs`Destek için bilgi toplama seçeneği eklendi.
- `azcmagent license`EULA 'yı görüntüleme seçeneği eklendi.
- `azcmagent show --json`, Kolayca ayrılabilir biçimdeki çıkış Aracısı durumuna eklenen seçenek.
- `azcmagent show`Sunucunun Azure 'da barındırılan bir sanal makinede olup olmadığını göstermek için çıkışa bayrak eklendi.
- `azcmagent disconnect --force-local-only`Azure hizmetine ulaşılamadığından yerel aracı durumunun sıfırlanmasına izin verme seçeneği eklendi.
- `azcmagent connect --cloud`Ek bulutları desteklemek için seçeneği eklendi. Bu sürümde, aracı sürümü sırasında yalnızca Azure hizmeti desteklenir.
- Aracı, Azure tarafından desteklenen dillerde yerelleştirildi.

### <a name="fixed"></a>Sabit

- Bağlantı denetimi geliştirmeleri.
- Linux üzerinde aracı yükseltilirken, ara sunucu ayarları kaybedilmekte olan sorun düzeltildi.
- Windows Server 2012 R2 çalıştıran sunucuya aracı yüklenmeye çalışıldığında oluşan sorunlar çözüldü.
- Uzantı yükleme güvenilirliği geliştirmeleri

## <a name="august-2020"></a>Ağustos 2020

Sürüm: 0,11

- Bu sürüm daha önce Ubuntu 20,04 için destek duyurmuştur. Bazı Azure VM uzantıları Ubuntu 20,04 ' ı desteklemediğinden, Ubuntu 'nın bu sürümü için destek kaldırılıyor.

- Uzantı dağıtımları için güvenilirlik iyileştirmeleri.

### <a name="known-issues"></a>Bilinen sorunlar

Linux aracısının eski bir sürümünü kullanıyorsanız ve bunu bir ara sunucu kullanacak şekilde yapılandırdıysanız, yükseltmeden sonra proxy sunucu ayarını yeniden yapılandırmanız gerekir. Bunu yapmak için öğesini çalıştırın `sudo azcmagent_proxy add http://proxyserver.local:83` .

## <a name="next-steps"></a>Sonraki adımlar

Birden çok karma makinede yay etkin sunucuları değerlendirmeden veya etkinleştirmeden önce, gereksinimleri anlamak için [bağlı makine aracısına genel bakış](agent-overview.md) ' ı ve aracı hakkındaki teknik ayrıntıları ve dağıtım yöntemlerini gözden geçirin.