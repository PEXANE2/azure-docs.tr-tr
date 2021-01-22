---
title: Yaygın Azure oto yönetimi ekleme hatalarını giderme
description: Ortak oto yönetimi ekleme hataları ve bunların sorunlarını giderme
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: alsin
ms.openlocfilehash: 13388bf289c1d10a2e7da04a4dd5d26be109535e
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697868"
---
# <a name="troubleshoot-common-automanage-onboarding-errors"></a>Ortak oto yönetimi ekleme hatalarını giderme
Oto yönetimi, bir makineyi hizmete eklemek için başarısız olabilir. Bu belgede dağıtım hatalarının nasıl giderileceği, dağıtımların başarısız olmasının bazı yaygın nedenlerini nasıl paylaştığı ve hafifletme konusunda olası bir sonraki adım açıklanmaktadır.

## <a name="troubleshooting-deployment-failures"></a>Dağıtım hatalarıyla ilgili sorunları giderme
Bir makineyi tekrar yönetmeye eklemek Azure Resource Manager dağıtımının oluşturulmasını sağlar. Ekleme başarısız olursa, neden başarısız olduğuna ilişkin daha fazla ayrıntı için dağıtıma danışmanız yararlı olabilir. Hata ayrıntısı açılır penceresinde, aşağıda yer alarak dağıtımların bağlantıları vardır.

:::image type="content" source="media\automanage-common-errors\failure-flyout.png" alt-text="Hata ayrıntısı açılır öğesini oto Yönet.":::

### <a name="check-the-deployments-for-the-resource-group-containing-the-failed-vm"></a>Başarısız VM 'yi içeren kaynak grubunun dağıtımlarını denetleyin
Hata açılır öğesi, kaynak grubu içinde, başarısız olan makineyi içeren ve dağıtımları filtrelemek için kullanabileceğiniz bir önek adı olan dağıtımlar için bir bağlantı içerir. Bağlantıya tıklamak sizi dağıtım dikey penceresine götürür. bu noktada, makinenize yönelik dağıtımları görmek için dağıtımları filtreleyebilirsiniz. Birden çok bölgeye dağıtıyorsanız, doğru bölgede dağıtıma tıklatığınızdan emin olun.

### <a name="check-the-deployments-for-the-subscription-containing-the-failed-vm"></a>Başarısız VM 'yi içeren aboneliğin dağıtımlarını denetleyin
Kaynak grubu dağıtımında bir hata görmüyorsanız, sonraki adımınız, aboneliğinizdeki, ekleme başarısız olan VM 'yi içeren dağıtımlara bakmak olacaktır. Hata açılır penceresinde **abonelik dağıtımları** bağlantısı ' na tıklayın ve yeniden **Manage-defaultresourcegroup** filtresi kullanarak dağıtımları filtreleyin. Dağıtımları filtrelemek için başarısızlık dikey penceresinden kaynak grubu adını kullanın. Dağıtım adı bir bölge adı ile son olarak düzeltilmelidir. Birden çok bölgeye dağıtıyorsanız, doğru bölgede dağıtıma tıklatığınızdan emin olun.

### <a name="check-deployments-in-a-subscription-linked-to-a-log-analytics-workspace"></a>Log Analytics çalışma alanına bağlı abonelikte dağıtımları denetleme
Kaynak grubunda veya başarısız sanal makinenizin bulunduğu abonelikte başarısız bir dağıtım görmüyorsanız ve başarısız VM 'niz farklı bir abonelikteki bir Log Analytics çalışma alanına bağlıysa, Log Analytics çalışma alanınıza bağlı aboneliğe gidin ve başarısız dağıtımları denetleyin.

## <a name="common-deployment-errors"></a>Sık karşılaşılan dağıtım hataları

Hata |  Risk azaltma
:-----|:-------------|
Hesap yetersiz izin hatası | Bu durum, yeni bir oto Yönet hesabı içeren bir aboneliği yeni bir kiracıya yakın zamanda taşıdıysanız meydana gelebilir. Bu sorunu çözmeye yönelik adımlar [burada](./repair-automanage-account.md)yer alır.
Bölge eşleme gereksinimleriyle eşleşmeyen çalışma alanı bölgesi | Otomatik Yönet, makinenizi ekleyemedi, ancak makinenin Şu anda bağlı olduğu Log Analytics çalışma alanı desteklenen bir Otomasyon bölgesine eşlenmemiş. Mevcut Log Analytics çalışma alanınızın ve otomasyon hesabınızın [desteklenen bir bölge eşlemesinde](https://docs.microsoft.com/azure/automation/how-to/region-mappings)bulunduğundan emin olun.
"Atama başarısız oldu; kullanılabilir ek bilgi yok " | Lütfen Microsoft Azure desteğiyle bir servis talebi açın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure oto yönetimi hakkında daha fazla bilgi edinin](./automanage-virtual-machines.md)

> [!div class="nextstepaction"]
> [Azure portal sanal makineler için oto yönetimini etkinleştir](quick-create-virtual-machines-portal.md)

