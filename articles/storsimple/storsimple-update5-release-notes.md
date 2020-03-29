---
title: StorSimple 8000 Serisi Güncelleme 5 sürüm notları
description: StorSimple 8000 Serisi Güncelleştirme5'in yeni özelliklerini, sorunlarını ve geçici çözümlerini açıklar.
author: alkohli
ms.assetid: ''
ms.service: storsimple
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: 4fed65e12a166c046a0c63d7c4849f86805bbe9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275166"
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>StorSimple 8000 Serisi Güncelleme 5 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları yeni özellikleri açıklar ve StorSimple 8000 Serisi Güncelleştirme 5 için kritik açık sorunları tanımlar. Ayrıca, bu sürümde yer alan StorSimple yazılım güncelleştirmelerinin bir listesini de içerirler.

Güncelleme 5, Güncelleme 0.1 ile Güncelleme 4 arasında çalışan herhangi bir StorSimple cihazına uygulanabilir. Güncelleştirme 5 ile ilişkili aygıt sürümü 6.3.9600.17845'tir.

Güncelleştirmeyi StorSimple çözümünüze dağıtmadan önce sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 5 zorunlu bir güncelleştirmedir ve hemen yüklenmesi gerekir. Daha fazla bilgi için Güncelleştirme 5'i nasıl [uygulayacağı](storsimple-8000-install-update-5.md)hakkında bilgi alabiliyorum.
> * Update 5 aygıt yazılımı, disk firmware, işletim sistemi güvenliği ve diğer işletim sistemi güncelleştirmeleri vardır. Bu güncelleştirmeyi yüklemek yaklaşık 4 saat sürer. Disk firmware güncelleştirmesi rahatsız edici bir güncelleştirmedir ve cihazınız için bir kapalı kalma süresi yle sonuçlanır. Cihazınızı güncel tutmak için Update 5'i uygulamanızı öneririz.
> * Yeni sürümler için güncelleştirmeleri aşamalı olarak kullanıma sunduğumuz için güncelleştirmeleri hemen göremeyebilirsiniz. Birkaç gün bekleyin ve bu güncelleştirmeler yakında kullanıma sunulacağı için güncelleştirmeleri yeniden tarayıp tarar.

## <a name="whats-new-in-update-5"></a>Güncelleme 5'teki yenilikler

Güncelleştirme 5'te aşağıdaki önemli geliştirmeler ve hata düzeltmeleri yapılmıştır.

* **StorSimple Device Manager hizmetiyle kimlik doğrulaması yapmak için Azure Active Directory 'nin (AAD) kullanımı** – Update 5'ten itibaren, Azure Etkin Dizin iyelik StorSimple Device Manager hizmetiyle kimlik doğrulaması yapmak için kullanılır. Eski kimlik doğrulama mekanizması Aralık 2017'ye kadar amortismana hazır olacak. Tüm kullanıcıların güvenlik duvarı kurallarına yeni kimlik doğrulama URL'lerini eklemesi gerekir. Daha fazla bilgi için, [StorSimple aygıtınızın ağ gereksinimlerinde listelenen kimlik doğrulama URL'lerine](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal)gidin.

    Kimlik doğrulama URL'si güvenlik duvarı kurallarına dahil edilmezse, kullanıcılar StorSimple aygıtlarının hizmetle birlikte kimlik doğrulaması yapamadığını belirten kritik bir uyarı görür. Kullanıcılar bu uyarıyı görürse, yeni kimlik doğrulama URL'sini eklemeleri gerekir. Daha fazla bilgi için [StorSimple ağ uyarılarına](storsimple-8000-manage-alerts.md#networking-alerts)gidin.

* **StorSimple Snapshot Manager'ın yeni sürümü** - StorSimple Snapshot Manager'ın yeni sürümü Güncelleme 5 ile birlikte yayımlanır ve Güncelleme 4 veya sonraki tüm StorSimple aygıtlarıyla uyumludur. Bu sürüme güncelleştirmenizi öneririz. StorSimple Snapshot Manager'ın önceki sürümü, Güncelleştirme 3 veya daha önce çalışan StorSimple aygıtları için kullanılır. [StorSimple Snapshot Manager'ın uygun sürümünü indirin](https://www.microsoft.com/en-us/download/details.aspx?id=44220) ve [StorSimple Snapshot Manager'ı dağıtmaya](storsimple-snapshot-manager-deployment.md)bakın.


## <a name="issues-fixed-in-update-5"></a>Güncelleştirme 5'te düzeltilen sorunlar

Aşağıdaki tablo, Güncelleştirme 5'te düzeltilen sorunların bir özetini sağlar.

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Windows PowerShell remoting |Önceki sürümde, Windows PowerShell üzerinden StorSimple Cloud Appliance'a uzaktan bağlantı kurmaya çalışan bir kullanıcı bir hata alır. Bu sorun kök neden oldu ve bu sürümde düzeltildi. |Hayır |Evet |
| 2 |Bant genişliği şablonları |Önceki sürümde, aygıt için yapılandırıldığından daha düşük bant genişliği ile sonuçlanan bant genişliği şablonları ile ilgili bir sorun vardı. Bu sorun bu sürümde çözülür. |Evet |Evet |
| 3 |Yük devretme |Önceki sürümde, çok sayıda birime sahip bir aygıt Güncelleştirme 4'ü çalıştıran başka bir aygıtta başarısız olduğunda, erişim denetimi kayıtlarını uygulamaya çalışırken işlem başarısız olur. Bu sorun bu sürümde giderilmiştir. |Evet |Evet |



## <a name="known-issues-in-update-5-from-previous-releases"></a>Önceki sürümlerden Güncelleştirme 5'te bilinen sorunlar

Güncelleştirme 5'te bilinen yeni sorunlar yok. Önceki sürümlerden Güncelleştirme 5'e taşınan sorunların listesi için, [Güncelleştirme 3 sürüm notlarına](storsimple-update3-release-notes.md#known-issues-in-update-3)gidin.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Güncelleme 5'te seri eklenmiş SCSI (SAS) denetleyici ve firmware güncellemeleri

Bu sürümde SAS denetleyicisi ve LSI sürücüsü ve firmware güncelleştirmeleri vardır. Bu güncelleştirmelerin nasıl yüklenirhakkında daha fazla bilgi için StorSimple cihazınıza [Update 5 yükle'ye](storsimple-8000-install-update-5.md) bakın.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>StorSimple Cloud Appliance güncellemeleri Güncelleme 5'te

Bu güncelleştirme StorSimple Cloud Appliance 'a (sanal aygıt olarak da bilinir) uygulanamaz. Yeni bulut cihazlarının Güncelleme 5 görüntüsü kullanılarak oluşturulması gerekir. StorSimple Cloud Appliance'ın nasıl oluşturulabildiğini öğrenmek için [Bir StorSimple Cloud Appliance'ı dağıt'a gidin ve yönetin.](storsimple-8000-cloud-appliance-u2.md)

## <a name="next-step"></a>Sonraki adım

StorSimple cihazınıza Update 5'i nasıl [yükleyin](storsimple-8000-install-update-5.md) öğrenin.

