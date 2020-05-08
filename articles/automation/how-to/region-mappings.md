---
title: Azure Otomasyonu ve Log Analytics çalışma alanı eşlemeleri
description: Bu makalede bir Otomasyon hesabı ile Log Analytics çalışma alanı arasında izin verilen eşlemeler açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/23/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6b00b25dc4eaea5bc8a3f5fbd42389aff501f14a
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82901017"
---
# <a name="workspace-mappings"></a>Çalışma alanı eşlemeleri

Azure Otomasyonu 'nda şu çözümleri etkinleştirebilirsiniz: "Güncelleştirme Yönetimi," "Değişiklik İzleme ve envanter" ve "VM 'Leri kapalı saatlerde Başlat/Durdur." Ancak bunu yaptığınızda, aboneliğinizdeki bir Log Analytics çalışma alanını ve otomasyon hesabını bağlamak için yalnızca belirli bölgelerin desteklendiğini unutmayın. Bu eşleme yalnızca Automation hesabı ve Log Analytics çalışma alanı için geçerlidir. Log Analytics çalışma alanı ve Otomasyon hesabı aynı abonelikte olmalıdır, ancak aynı bölgeye dağıtılmış farklı kaynak gruplarında yer alabilir.

Daha fazla bilgi için bkz. [Log Analytics çalışma alanı ve Otomasyon hesabı](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

## <a name="supported-mappings"></a>Desteklenen eşlemeler

Aşağıdaki tabloda desteklenen eşlemeler gösterilmektedir:

|**Log Analytics çalışma alanı bölgesi**|**Azure Otomasyonu bölgesi**|
|---|---|
|**ABD**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Kanada**||
|Canadaorta|Canadaorta|
|**Asya Pasifik**||
|AustraliaSoutheast|AustraliaSoutheast|
|Güneydoğu|Güneydoğu|
|Merkezileştirme Hindistan|Merkezileştirme Hindistan|
|JapanEast|JapanEast|
|**Avrupa**||
|UKSouth|UKSouth|
|WestEurope|WestEurope|
|**US Gov**||
|USGovVirginia|USGovVirginia|

Log Analytics çalışma alanları için <sup>1</sup> EastUS eşlemesi, Otomasyon hesaplarında tam bir bölgeden bölgeye eşleme değildir, ancak doğru eşleme olur.

<sup>2</sup> kapasite depoları nedeniyle, yeni kaynaklar oluştururken bölge kullanılamaz. Bu, Otomasyon hesaplarını ve Log Analytics çalışma alanlarını içerir. Ancak, bölgede önceden var olan bağlı kaynakların çalışmaya devam etmesi gerekir.

## <a name="unlink-a-workspace"></a>Çalışma alanının bağlantısını kaldır

Artık Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmenize karar verirseniz, Hesabınızın bağlantısını doğrudan Azure portal kaldırabilirsiniz. Devam etmeden önce, ilk olarak "Güncelleştirme Yönetimi," "Değişiklik İzleme ve envanterini" ve "bunları kullanıyorsanız VM 'Leri saatlerde Başlat/Durdur" ı kaldırmanız gerekir. Bunları kaldırmazsanız, bağlantı kaldırma işlemini tamamlayamazsınız. Kaldırmak için gereken adımları anlamak üzere, etkinleştirdiğinize yönelik her çözüm için makaleyi gözden geçirin.

Bunları kaldırdıktan sonra, Otomasyon Hesabınızın bağlantısını kaldırmak için aşağıdaki adımları uygulayabilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler, Otomasyon varlıkları oluşturmuş olabilir ve çalışma alanının bağlantısı kaldırılmadan önce kaldırılması gerekebilir.

1. Azure portal Otomasyon hesabınızı açın. **Otomasyon hesabı** sayfasında, **Ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. **Çalışma alanının bağlantısını kaldır** sayfasında, **çalışma alanının bağlantısını kaldır**' ı seçin. Devam etmek isteyip istemediğinizi doğrulayan bir istem alırsınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızdaki hesabın bağlantısını kaldırmayı denediğinde, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

4. İsteğe bağlı olarak "Güncelleştirme Yönetimi" kullandıysanız, onu kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

    * Zamanlamayı güncelleştir: her birinin, oluşturduğunuz bir güncelleştirme dağıtımıyla eşleşen bir adı vardır.
    * Çözüm için oluşturulan karma çalışan grupları: her birinin şuna benzer bir adı vardır `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`.

5. "VM 'Leri çalışma saatleri dışında Başlat/Durdur" kullandıysanız, onu kaldırdıktan sonra gerekli olmayan aşağıdaki öğeleri de kaldırabilirsiniz.

    * VM runbook zamanlamalarını başlatma ve durdurma
    * VM runbook 'larını başlatma ve durdurma
    * Değişkenler

Alternatif olarak, çalışma alanınızın içindeki Otomasyon hesabınızdan çalışma alanınızın bağlantısını kaldırabilirsiniz.

1. Çalışma alanında **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin. 
2. **Otomasyon hesabı** sayfasında **Hesap bağlantısını kaldır**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* "Güncelleştirme Yönetimi" ve "Değişiklik İzleme ve envanterini" kullanmaya nasıl başlayacağınızı öğrenin:

    * Bir [sanal makineden](../automation-onboard-solutions-from-vm.md).
    * [Otomasyon hesabınızdan](../automation-onboard-solutions-from-automation-account.md).
    * [Birden çok makineye göz](../automation-onboard-solutions-from-browse.md)atarken.
    * Bir [runbook](../automation-onboard-solutions.md)'tan.

* "VM 'Leri çalışma saatleri dışında Başlat/Durdur" kullanmaya nasıl başlayacağınızı öğrenin:

    * [Çalışma saatleri dışında VM 'Leri Başlat/Durdur genel bakış](../automation-solution-vm-management.md).
