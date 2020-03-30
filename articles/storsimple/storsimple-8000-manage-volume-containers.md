---
title: StorSimple 8000 serisi cihazlar için birim kapları yönetme
description: Bir birim kapsayıcı eklemek, değiştirmek veya silmek için StorSimple Device Manager hizmet birim kapsayıcıları sayfasını nasıl kullanabileceğinizi açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: cbdad3c68848ce552811ee658bb29df74a6fad19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267683"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-volume-containers"></a>StorSimple birim kapsayıcılarını yönetmek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış
Bu öğretici, StorSimple birim kapsayıcıları oluşturmak ve yönetmek için StorSimple Device Manager hizmetini nasıl kullanacağımı açıklar.

Microsoft Azure StorSimple aygıtındaki bir birim kapsayıcı, depolama hesabı, şifreleme ve bant genişliği tüketim ayarlarını paylaşan bir veya daha fazla birim içerir. Aygıt, tüm birimleri için birden çok birim kapsayıcısına sahip olabilir. 

Bir birim kapsayıcı aşağıdaki öznitelikleri vardır:

* **Birimler** – Birim kapsayıcıiçinde bulunan katmanlı veya yerel olarak sabitlenmiş StorSimple birimleri. 
* **Şifreleme** – Her birim kapsayıcı için tanımlanabilen bir şifreleme anahtarı. Bu anahtar, StorSimple cihazınızdan buluta gönderilen verileri şifrelemek için kullanılır. Kullanıcı girilen anahtarla askeri sınıf AES-256 bit anahtar kullanılır. Verilerinizi güvence altına almak için bulut depolama şifrelemesini her zaman etkinleştirmenizi öneririz.
* **Depolama hesabı** – Verileri depolamak için kullanılan Azure depolama hesabı. Bir birim kapsayıcıda bulunan tüm birimler bu depolama hesabını paylaşır. Varolan bir listeden bir depolama hesabı seçebilir veya birim kapsayıcıyı oluşturduğunuzda yeni bir hesap oluşturabilir ve ardından bu hesabın erişim kimlik bilgilerini belirtebilirsiniz.
* **Bulut bant genişliği** – Aygıttan gelen veriler buluta gönderilirken aygıt tarafından tüketilen bant genişliği. Bu kapsayıcıyı oluştururken 1 Mbps ile 1.000 Mbps arasında bir değer belirterek bant genişliği denetimini zorlayabilirsiniz. Aygıtın kullanılabilir tüm bant genişliğini tüketmesini istiyorsanız, bu alanı **Sınırsız**olarak ayarlayın. Ayrıca, zamanlamaya göre bant genişliği ayırmak için bir bant genişliği şablonu oluşturabilir ve uygulayabilirsiniz.

Aşağıdaki yordamlar, aşağıdaki yaygın işlemleri tamamlamak için StorSimple **Volume kapları** bıçak nasıl kullanılacağını açıklar:

* Birim kapsayıcı ekleme
* Birim kapsayıcıyı değiştirme
* Birim kapsayıcıyı silme

## <a name="add-a-volume-container"></a>Birim kapsayıcı ekleme
Birim kapsayıcı eklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>Birim kapsayıcıyı değiştirme
Birim kapsayıcısını değiştirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Birim kapsayıcıyı silme
Bir birim kapsayıcı içinde hacimleri vardır. Yalnızca içindeki tüm birimler ilk silinirse silinebilir. Birim kapsayıcısını silmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple birimlerini yönetme](storsimple-8000-manage-volumes-u2.md)hakkında daha fazla bilgi edinin. 
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

