---
title: StorBasit Aygıt Yöneticisi depolama hesaplarını, abonelikleri geçirin
description: StorSimple Device Manager hizmetiniz için abonelikleri, depolama hesaplarını nasıl geçirteceklerinizi öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: 428c336d98e278910b229e9c0d877a9ae6268c96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77169715"
---
# <a name="migrate-subscriptions-and-storage-accounts-associated-with-storsimple-device-manager-service"></a>StorSimple Device Manager hizmetiyle ilişkili abonelikleri ve depolama hesaplarını geçirme

StorSimple hizmetinizi yeni bir kayda veya yeni bir aboneliğe taşımanız gerekebilir. Bu geçiş senaryoları hesap değişiklikleri veya veri merkezi değişiklikleridir. Taşımak için ayrıntılı adımlar da dahil olmak üzere bu senaryolardan hangilerinin destekleniyi anlamak için aşağıdaki tabloyu kullanın.

## <a name="account-changes"></a>Hesap değişiklikleri

| Hareket edebilir misiniz ...| Destekleniyor| Downtime| Azure Destek süreci| Yaklaşım|
|-----|-----|-----|-----|-----|
| Başka bir kayıt için tüm abonelik (StorSimple hizmet ve depolama hesapları içerir)? | Evet       | Hayır       | **Kayıt Transferi**<br>Kullanın:<li>Yeni bir sözleşme kapsamında yeni bir Azure taahhüdü satın aldığınızda.</li><li>Tüm hesapları ve abonelikleri eski kayıttan yenisine geçirmek istiyorsunuz. Buna, eski abonelik altındaki tüm Azure hizmetleri dahildir.</li> | **Adım 1: Azure Kurumsal İşlem Desteği biletini açın.**<li>[https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport)Git.</li><li> **Kayıt Yönetimi'ni** seçin ve ardından **bir kayıttan yeni bir kayda Aktar'ı**seçin.<br>**Adım 2: İstenen bilgileri sağlama**<br>Içerir:<li>kaynak kayıt numarası</li><li> hedef kayıt numarası</li><li>transfer yürürlük tarihi|
| Varolan bir hesaptan yeni bir kayda StorSimple hizmeti?    | Evet       | Hayır       | **Hesap Transferi**<br>Kullanın:<li>Tam bir kayıt transferi istemediğiniz zaman.</li><li>Yalnızca belirli hesapları yeni bir kayda taşımak istiyorsunuz.</li>| **Adım 1: Azure Kurumsal İşlem Desteği biletini açın.**<li>[https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport)Git.</li><li>**Kayıt Yönetimi'ni** seçin ve ardından **bir EA Hesabını yeni bir kayda aktar'ı**seçin.<br>**Adım 2: İstenen bilgileri sağlama**<br>Içerir:<li>kaynak kayıt numarası</li><li> hedef kayıt numarası</li><li>transfer yürürlük tarihi|
| Bir abonelikten diğerine StorSimple hizmeti?      | Hayır        |    Evet         | Yok, manuel işlem|<li>Verileri StorSimple aygıtından geçirin.</li><li>Aygıtın fabrika sıfırlamasını gerçekleştirin, bu aygıttaki tüm yerel verileri siler.</li><li>Aygıtı yeni abonelikle storbasit aygıt yöneticisi hizmetine kaydedin.</li><li>Verileri aygıta geri geçirin.|
|Azure aboneliğinin sahipliğini başka bir dizine aktarabilir miyim? | Evet       | Hayır       | Varolan bir aboneliği Azure REKLAM dizininizde ilişkilendirme | [Azure REKLAM dizininizdeki mevcut aboneliği ilişkilendirmek için](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)bkz. Her şeyin doğru şekilde gösterilmesi 10 dakikaya kadar sürebilir.|
| StorBasit aygıt bir StorSimple Aygıt Yöneticisi hizmetinden farklı bir bölgedeki başka bir hizmete mi?      | Hayır        | Evet            | Yok, manuel işlem |Yukarıdakiyle aynı.|
| Yeni bir abonelik veya kaynak grubuna depolama hesabı?     | Evet        | Hayır             |Depolama hesabını farklı abonelik veya kaynak grubuna taşıma |Taşımadan sonra, depolama hesabı erişim anahtarları güncelleştirilirse, kullanıcının storSimple Device Manager hizmeti aracılığıyla geçirilen depolama hesabının erişim anahtarlarını el ile yapılandırması gerekir.|
| Azure Kaynak Yöneticisi depolama hesabına klasik depolama hesabı      | Evet        | Hayır             |Klasikten Azure Kaynak Yöneticisi'ne geçiş |<li>Bir depolama hesabını klasikten Azure Kaynak Yöneticisi'ne nasıl geçirilir hakkında ayrıntılı talimatlar [için, klasik bir depolama hesabını geçir'e](../virtual-machines/windows/migration-classic-resource-manager-ps.md#step-52-migrate-a-storage-account)gidin.</li><li> Depolama hesabı erişim anahtarları geçişten sonra güncelleştirilirse, kullanıcının geçirilen depolama hesabının erişim anahtarlarını StorSimple Device Manager hizmeti aracılığıyla eşitleması gerekir. Bu, StorSimple aygıtlarının normal şekilde çalışmaya devam etmesini ve birincil/yedekleme verilerini Azure'a katmanla aktarabilmesini sağlamak içindir. Erişim anahtarlarını eşitleme yle ilgili ayrıntılı talimatlar için [Döndürme iş akışına](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)gidin.</li><li> StorSimple Cloud Appliance durumunda, klasik depolama hesabı taşınır ancak altta yatan sanal makine hala klasik olarak kalırsa, cihaz düzgün çalışması gerekir. Bulut cihazının altında yatan sanal makine taşınırsa, devre dışı bırakma ve silme işlevi çalışmaz.</li><li> Azure portalında yeni bir StorSimple Bulut Cihazları oluşturmanız ve ardından eski bulut cihazlarından başarısız olmalısınız. Klasik bir depolama hesabı kullanarak yeni Azure portalında storSimple Cloud Appliance oluşturamazsınız, bir Azure Kaynak Yöneticisi depolama hesabına sahip olmaları gerekir. Daha fazla bilgi için [Bir StorSimple Cloud Appliance'ı dağıt'a gidin ve yönetin.](storsimple-8000-cloud-appliance-u2.md)</li>|

## <a name="datacenter-changes"></a>Veri merkezi değişiklikleri

| Hareket edebilir misiniz ...| Destekleniyor|Downtime| Azure Destek süreci| Yaklaşım|
|-----|-----|-----|-----|-----|
| Bir Azure veri merkezinden diğerine bir StorSimple hizmeti? | Hayır | Evet |Yok, manuel işlem  |<li>Verileri StorSimple aygıtından geçirin.</li><li>Aygıtın fabrika sıfırlamasını gerçekleştirin, bu aygıttaki tüm yerel verileri siler.</li><li>Yeni bir StorSimple Device Manager hizmetine yeni abonelikle cihazı kaydedin.</li><li>Verileri aygıta geri geçirin.|
| Bir Azure veri merkezinden diğerine bir depolama hesabı? | Hayır |Evet  |Yok, manuel işlem  | Yukarıdakiyle aynı.|

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple Device Manager hizmetini dağıtın](storsimple-8000-manage-service.md)
* [Azure portalında StorSimple 8000 serisi cihazı dağıtma](storsimple-8000-deployment-walkthrough-u2.md)
