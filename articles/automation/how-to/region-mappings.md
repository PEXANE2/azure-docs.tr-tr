---
title: Azure Otomasyonu ve Log Analytics çalışma alanı haritalamaları
description: Bu makalede, çözümü desteklemek için bir Otomasyon hesabı ile Log Analytics Çalışma Alanı arasında izin verilen eşlemeler açıklanmaktadır
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7f6508648be1e857a29f46b57e8309a7ec797291
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681845"
---
# <a name="workspace-mappings"></a>Çalışma alanı eşlemeleri

Kapalı saatlerde Güncelleştirme Yönetimi, İzleme ve Envanter veya Start/Stop VM'leri gibi çözümleri etkinleştirirken, log analytics çalışma alanını ve Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Bu eşleme yalnızca Otomasyon hesabı ve Log Analytics çalışma alanı için geçerlidir. Otomasyon hesabınıza veya Log Analytics çalışma alanına rapor bildiren kaynaklar diğer bölgelerde bulunabilir.

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

<sup>1</sup> Log Analytics çalışma alanlarıiçin Otomasyon hesaplarına EastUS eşleme, bölge eşleme için kesin bir bölge değil, doğru eşlemedir.

<sup>2</sup> Kapasite kısıtlamaları nedeniyle bölge yeni kaynaklar oluştururken kullanılamaz. Buna Otomasyon hesapları ve Log Analytics çalışma alanları dahildir. Ancak, bölgede önceden var olan bağlantılı kaynaklar çalışmaya devam etmelidir.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldırma

Otomasyon hesabınızı artık bir Log Analytics çalışma alanıyla tümleştirmek istemediğinize karar verirseniz, hesabınızın bağlantısını doğrudan Azure portalından çıkarabilirsiniz. Devam etmeden önce, öncelikle Güncelleştirme Yönetimi, İzleme ve Envanteri Değiştirme ve Bunları kullanıyorsanız mesai saatleri dışında VM'leri Başlatma/Durdur una kaldırmanız gerekir. Bunları kaldırmazsanız, bağlantı kaldırma işlemini tamamlayamazsınız. Kaldırmak için gereken adımları anlamak için içe aktardığınız belirli çözüm için makaleyi gözden geçirin.

Bu çözümleri kaldırdıktan sonra, Otomasyon hesabınızın bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler otomasyon varlıkları oluşturmuş olabilir ve çalışma alanını bağlamadan önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın. Otomasyon hesap sayfasında, **İlgili Kaynaklar**altında Bağlantılı **çalışma alanını** seçin.

2. Bağlantıyı Bırakma çalışma alanı sayfasında, **Bağlantıyı Çöz çalışma alanını**tıklatın. Devam etmek istiyorsanız doğrulamak için bir istem alırsınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızın hesabının bağlantısını açmaya çalışırken, **Bildirimler** altındaki ilerlemeyi menüden izleyebilirsiniz.

4. Güncelleştirme Yönetimi çözümlerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

    * Zamanlamaları güncelleştir - Her birinin oluşturduğunuz bir güncelleştirme dağıtımıyla eşleşen bir adı vardır.
    * Çözüm için oluşturulan karma işçi grupları - `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`Her biri benzer bir adı vardır.

5. Mesai saatleri dışında Başlat/Durdur VM'lerini kullandıysanız, çözümü kaldırdıktan sonra gerekli olmayan aşağıdaki öğeleri isteğe bağlı olarak kaldırabilirsiniz.

    * VM çalışma kitabı zamanlamalarını başlatın ve durdurun
    * VM runbook'larını başlatVe durdur
    * Değişkenler

Alternatif olarak, çalışma alanınızdaki Otomasyon hesabınızdan çalışma alanınızı boşaltabilirsiniz. 

1. Çalışma alanında, **İlgili Kaynaklar**altında **Otomasyon Hesabı'nı** seçin. 
2. Otomasyon Hesabı sayfasında **Bağlantıyı Aç hesabını**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Gemide Güncelleştirme Yönetimi ve İzleme ve Envanter çözümlerini nasıl değiştireceğinizi öğrenin:

    * Sanal bir [makineden](../automation-onboard-solutions-from-vm.md)
    * Otomasyon [hesabınızdan](../automation-onboard-solutions-from-automation-account.md)
    * [Birden fazla makineye göz atarken](../automation-onboard-solutions-from-browse.md)
    * [Runbook'tan](../automation-onboard-solutions.md)

* Mesai saatleri dışında başlat/durdur un adedinde nasıl atılağınızda bilgi edinin:

    * [Kapalı saatlerde Başlat/Durdur VM'leri dağıtma](../automation-solution-vm-management.md)
