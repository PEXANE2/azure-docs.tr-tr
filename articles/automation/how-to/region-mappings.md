---
title: Azure Otomasyonu ve Log Analytics çalışma alanı haritalamaları
description: Bu makalede, çözümü desteklemek için Bir Otomasyon Hesabı ile Log Analytics Çalışma Alanı arasında izin verilen eşlemeler açıklanmaktadır
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 965d6b206bb64e90fe59798ce0c37ccf029117f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849522"
---
# <a name="workspace-mappings"></a>Çalışma alanı eşlemeleri

Mesai dışı çözüm sırasında Güncelleştirme Yönetimi, İzleme ve Envanter veya Start/Stop VM'leri gibi çözümleri etkinleştirirken, log analytics çalışma alanı ve Otomasyon Hesabı bağlamak için yalnızca belirli bölgeler desteklenir. Bu haritalama yalnızca Otomasyon Hesabı ve Log Analytics çalışma alanı için geçerlidir. Otomasyon Hesabınıza veya Log Analytics çalışma alanınıza rapor bildiren kaynaklar diğer bölgelerde bulunabilir.

## <a name="supported-mappings"></a>Desteklenen eşlemeler

Aşağıdaki tablo desteklenen eşlemeleri gösterir:

|**Günlük Analitik Çalışma Alanı Bölgesi**|**Azure Otomasyon Bölgesi**|
|---|---|
|**ABD**||
|DoğuUS<sup>1</sup>|DoğuUS2|
|BatıUS2|BatıUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Kanada**||
|KanadaMerkez|KanadaMerkez|
|**Asya Pasifik**||
|AvustralyaGüneydoğu|AvustralyaGüneydoğu|
|Güneydoğu Asya|Güneydoğu Asya|
|Orta Hindistan|Orta Hindistan|
|JapanDoğu|JapanDoğu|
|**Avrupa**||
|UkSouth|UkSouth|
|WestEurope|WestEurope|
|**US Gov**||
|ABD GovVirginia|ABD GovVirginia|

<sup>1</sup> Log Analytics çalışma alanlarıiçin Otomasyon Hesapları için EastUS eşleme, bölge eşleme için kesin bir bölge değil, doğru eşlemedir.

<sup>2</sup> Kapasite kısıtlamaları nedeniyle bölge yeni kaynaklar oluştururken kullanılamaz. Buna Otomasyon Hesapları ve Log Analytics çalışma alanları dahildir. Ancak, bölgede önceden var olan bağlantılı kaynaklar çalışmaya devam etmelidir.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldırma

Otomasyon hesabınızı artık bir Log Analytics çalışma alanıyla tümleştirmek istemediğinize karar verirseniz, hesabınızın bağlantısını doğrudan Azure portalından çıkarabilirsiniz. Devam etmeden önce, bunları kullanıyorsanız, önce Güncelleme Yönetimi, İzleme ve Envanteri Değiştirme veya mesai dışı çözümler sırasında Başlat/Durdur VM'lerini kaldırmanız gerekir. Bunları kaldırmazsanız, bu işlemin devam etmesini engeller. Kaldırmak için gereken adımları anlamak için içe aktardığınız belirli çözüm için makaleyi gözden geçirin.

Bu çözümleri kaldırdıktan sonra, Otomasyon hesabınızın bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler otomasyon varlıkları oluşturmuş olabilir ve çalışma alanını bağlamadan önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve Otomasyon hesabı sayfasında soldaki **İlgili Kaynaklar** bölümünün altında Bağlantılı **çalışma alanı** seçin.

2. Bağlantıyı Bırakma çalışma alanı sayfasında, **Bağlantıyı Çöz çalışma alanını**tıklatın. Devam etmek istediğinizi doğrulayan bir istem alırsınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızın hesabının bağlantısını açmaya çalışırken, **Bildirimler** altındaki ilerlemeyi menüden izleyebilirsiniz.

Güncelleştirme Yönetimi çözümlerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Zamanlamaları güncelleştir - Her birinde oluşturduğunuz güncelleştirme dağıtımlarıyla eşleşen adlar olacaktır)

* Çözüm için oluşturulan karma işçi grupları - Her `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`biri benzer şekilde adlandırılacaktır).

Mesai saatleri dışında başlat/durdur vm'lerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* VM çalışma kitabı zamanlamalarını başlatın ve durdurun
* VM runbook'larını başlatVe durdur
* Değişkenler

Alternatif olarak, Çalışma alanınızı Log Analytics çalışma alanınızdan Otomasyon Hesabınızdan da çıkarabilirsiniz. Çalışma alanınızda, **İlgili Kaynaklar**altında **Otomasyon Hesabı'nı** seçin. Otomasyon Hesabı sayfasında **Bağlantıyı Aç hesabını**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki çözümleri nasıl ekine bindireceklerini öğrenin:

Güncelleme Yönetimi ve Değişim İzleme ve Envanter:

* Sanal bir [makineden](../automation-onboard-solutions-from-vm.md)
* Otomasyon [hesabınızdan](../automation-onboard-solutions-from-automation-account.md)
* [Birden fazla makineye göz atarken](../automation-onboard-solutions-from-browse.md)
* [Runbook'tan](../automation-onboard-solutions.md)

Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma

* [Mesai saatleri dışında Başlat/Durdur VM'leri dağıtma](../automation-solution-vm-management.md)
