---
title: Azure Otomasyonu ve Log Analytics çalışma alanı eşlemeleri
description: Bu makalede, bir Otomasyon hesabı ile Log Analytics çalışma alanı arasında çözümü desteklemeye izin verilen eşlemeler açıklanmaktadır
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/23/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1bc47fe6e1f0730bcff49e138df92f85ba3ef1a8
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82114488"
---
# <a name="workspace-mappings"></a>Çalışma alanı eşlemeleri

Güncelleştirme Yönetimi, Değişiklik İzleme ve envanterini veya VM'leri çalışma saatleri dışında başlat/durdur etkinleştirirken, aboneliğinizdeki bir Log Analytics çalışma alanını ve otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Bu eşleme yalnızca Automation hesabı ve Log Analytics çalışma alanı için geçerlidir. Log Analytics çalışma alanı ve Otomasyon hesabı aynı abonelikte olmalıdır, ancak aynı bölgeye dağıtılmış farklı kaynak gruplarında yer alabilir.

Daha fazla bilgi için bkz. [Log Analytics çalışma alanı ve Otomasyon hesabı](../azure-monitor/insights/solutions.md#log-analytics- workspace-and-automation-account).

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

<sup>1</sup> EastUS Log Analytics çalışma alanları için Otomasyon hesaplarına yönelik eşleme, bölge eşlemesinin tam bir bölgesi değildir ancak doğru eşleme olur.

<sup>2</sup> kapasite depoları nedeniyle bölge, yeni kaynaklar oluşturulurken kullanılamaz. Bu, Otomasyon hesaplarını ve Log Analytics çalışma alanlarını içerir. Ancak, bölgede önceden var olan bağlı kaynakların çalışmaya devam etmesi gerekir.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldırma

Artık Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmenize karar verirseniz, Hesabınızın bağlantısını doğrudan Azure portal kaldırabilirsiniz. Devam etmeden önce, ilk olarak Güncelleştirme Yönetimi, Değişiklik İzleme ve envanteri ve bunları kullanıyorsanız VM'leri çalışma saatleri dışında başlat/durdur kaldırmanız gerekir. Bunları kaldıramadıysanız, bağlantı kaldırma işlemini tamamlayamazsınız. ' İ kaldırmak için gereken adımları anlamak üzere, her biri için etkinleştiriyorsunuz makaleyi gözden geçirin.

Bunları kaldırdıktan sonra, Otomasyon Hesabınızın bağlantısını kaldırmak için aşağıdaki adımları uygulayabilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri dahil bazı çözümler Automation varlıkları oluşturmuş olabilir ve çalışma alanının bağlantısı kaldırılmadan önce kaldırılması gerekebilir.

1. Azure portal Otomasyon hesabınızı açın. Otomasyon hesabı sayfasında, **Ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. Çalışma alanının bağlantısını Kaldır sayfasında, **çalışma alanının bağlantısını kaldır**' a tıklayın. Devam etmek isteyip istemediğiniz bir istem alırsınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızdaki hesabın bağlantısını kaldırmayı denediğinde, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

4. Güncelleştirme Yönetimi kullandıysanız, isteğe bağlı olarak, onu kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

    * Zamanlamayı Güncelleştir-her birinin, oluşturduğunuz bir güncelleştirme dağıtımıyla eşleşen bir adı vardır.
    * Çözüm için oluşturulan karma çalışan grupları-her birinin şuna benzer bir adı vardır `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`.

5. VM'leri çalışma saatleri dışında başlat/durdur kullandıysanız, onu kaldırdıktan sonra gerekli olmayan aşağıdaki öğeleri de kaldırabilirsiniz.

    * VM runbook zamanlamalarını başlatma ve durdurma
    * VM runbook 'larını başlatma ve durdurma
    * Değişkenler

Alternatif olarak, çalışma alanınızın içindeki Otomasyon hesabınızdan çalışma alanınızın bağlantısını kaldırabilirsiniz.

1. Çalışma alanında **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin. 
2. Otomasyon hesabı sayfasında **Hesap bağlantısını kaldır**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Güncelleştirme Yönetimi ve Değişiklik İzleme ve envanterin nasıl ekleneceğini öğrenin:

    * Bir [sanal makineden](../automation-onboard-solutions-from-vm.md)
    * [Otomasyon hesabınızdan](../automation-onboard-solutions-from-automation-account.md)
    * [Birden çok makineye gözatarken](../automation-onboard-solutions-from-browse.md)
    * Bir [runbook](../automation-onboard-solutions.md) 'tan

* VM'leri çalışma saatleri dışında başlat/durdur eklemeyi öğrenin:

    * [VM'leri çalışma saatleri dışında başlat/durdur genel bakış](../automation-solution-vm-management.md)
