---
title: Bağlı Log Analytics çalışma alanı için desteklenen bölgeler
description: Bu makalede bir Otomasyon hesabı ile Log Analytics çalışma alanı arasındaki desteklenen bölge eşlemeleri açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/23/2020
ms.topic: conceptual
manager: carmonm
ms.custom: references_regions
ms.openlocfilehash: d594d5a9dd263817023d2b5987c03aa50828191c
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84191855"
---
# <a name="supported-regions-for-linked-log-analytics-workspace"></a>Bağlı Log Analytics çalışma alanı için desteklenen bölgeler

Azure Otomasyonu 'nda, sanal makinelerinizin Güncelleştirme Yönetimi, Değişiklik İzleme ve envanterini ve VM'leri çalışma saatleri dışında başlat/durdur özelliklerini etkinleştirebilirsiniz. Ancak, aboneliğinizdeki bir Log Analytics çalışma alanını ve otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Bölge eşlemeleri yalnızca Otomasyon hesabı ve Log Analytics çalışma alanı için geçerlidir. Log Analytics çalışma alanı ve Otomasyon hesabı aynı abonelikte olmalıdır, ancak aynı bölgeye dağıtılmış farklı kaynak gruplarında yer alabilir. Daha fazla bilgi için bkz. [Log Analytics çalışma alanı ve Otomasyon hesabı](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

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

Artık Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmenize karar verirseniz, Hesabınızın bağlantısını doğrudan Azure portal kaldırabilirsiniz. Devam etmeden önce, ilk olarak Güncelleştirme Yönetimi, Değişiklik İzleme ve envanteri ve bunları kullanıyorsanız VM'leri çalışma saatleri dışında başlat/durdur [kaldırmanız](move-account.md#remove-features) gerekir. Bunları kaldırmazsanız, bağlantı kaldırma işlemini tamamlayamazsınız. 

Kaldırılan özellikler ile otomasyon Hesabınızın bağlantısını kaldırmak için aşağıdaki adımları izleyebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı özellikler, çalışma alanının bağlantısı kaldırılmadan önce kaldırılması gereken Otomasyon varlıklarını oluşturmuş olabilir.

1. Azure portal Otomasyon hesabınızı açın. Otomasyon hesabı sayfasında, **Ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. Çalışma alanının bağlantısını Kaldır sayfasında, **çalışma alanının bağlantısını kaldır**' ı seçin. Devam etmek isteyip istemediğinizi doğrulayan bir istem alırsınız.

3. Azure Otomasyonu hesabın Log Analytics çalışma alanınızdan bağlantısını kaldırılırken, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

4. Güncelleştirme Yönetimi kullandıysanız, isteğe bağlı olarak, artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz:

    * Zamanlamayı güncelleştir: her birinin, oluşturduğunuz bir güncelleştirme dağıtımıyla eşleşen bir adı vardır.
    * Özelliği için oluşturulan karma çalışan grupları: her birinin şuna benzer bir adı vardır `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8` .

5. VM'leri çalışma saatleri dışında başlat/durdur kullandıysanız, isteğe bağlı olarak artık gerekli olmayan aşağıdaki öğeleri kaldırabilirsiniz:

    * VM runbook zamanlamalarını başlatma ve durdurma
    * VM runbook 'larını başlatma ve durdurma
    * Değişkenler

Alternatif olarak, çalışma alanınızın içindeki Otomasyon hesabınızdan çalışma alanınızın bağlantısını kaldırabilirsiniz.

1. Çalışma alanında **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin. 
2. Otomasyon hesabı sayfasında **Hesap bağlantısını kaldır**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Güncelleştirme yönetimi genel bakışta](../automation-update-management.md)güncelleştirme yönetimi hakkında bilgi edinin.
* [Değişiklik izleme ve envanterde genel bakış](../change-tracking.md)hakkında bilgi edinin değişiklik izleme.
* [VM'leri çalışma saatleri dışında Başlat/Durdur genel bakışta](../automation-solution-vm-management.md)VM'leri çalışma saatleri dışında Başlat/Durdur hakkında bilgi edinin.
