---
title: Hesap yönetimi-Azure VMware çözümleri (AVS) portalı
description: Azure VMware Solutions (AVS) portalındaki hesapların nasıl yönetileceğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1fd7cb1c55fb664828448cef0b67ea9b16323bdf
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025376"
---
# <a name="manage-accounts-on-the-azure-vmware-solutions-avs-portal"></a>Azure VMware Solutions (AVS) portalındaki hesapları yönetme

AVS hizmetinizi oluştururken, AVS 'de bir hesap oluşturur. Hesap, hizmetin bulunduğu Azure aboneliğiniz ile ilişkilendirilir. Abonelikte sahip ve katkıda bulunan rollerine sahip tüm kullanıcıların AVS portalına erişimi vardır. AVS hizmetiyle ilişkili Azure abonelik KIMLIĞI ve kiracı KIMLIĞI hesaplar sayfasında bulunur.

AVS portalındaki hesapları yönetmek için [portala erişin](access-cloudsimple-portal.md) ve yan menüdeki **Hesap** ' ı seçin.

Şirketinizin AVS yapılandırması hakkındaki bilgileri görüntülemek için **Özet** ' i seçin. Bulut yapılandırmanızın geçerli kapasitesi, AVS özel bulutları, toplam depolama, vSphere kümesi yapılandırması, düğüm sayısı ve işlem çekirdekleri dahil olmak üzere gösterilir. Geçerli yapılandırma tüm ihtiyaçlarınızı karşılamıyorsa ek düğümler satın almak için bir bağlantı eklenmiştir.

## <a name="email-alerts"></a>E-posta uyarıları

AVS özel bulut yapılandırmasında değişiklikler hakkında bilgilendirmek istediğiniz kişilerin e-posta adreslerini ekleyebilirsiniz.

1. **Ek e-posta uyarıları** alanında **Yeni Ekle**' ye tıklayın.
2. E-posta adresini girin.
3. Return tuşuna basın.  

Bir girişi kaldırmak için **X**' e tıklayın.

## <a name="avs-operator-access"></a>AVS İşletmen erişimi

Operatör erişim ayarı, AVS 'nin bir destek mühendisine oturum açmaya izin vererek sorun gidermeye yardımcı olmaya olanak tanır. Ayar varsayılan olarak etkindir. Müşteri hesabınızda oturum açarken destek mühendisi tarafından gerçekleştirilen tüm eylemler, **etkinlik** > **Denetim** sayfasında gözden geçirmeniz için kaydedilir ve kullanılabilir.

Erişimi açmak veya kapatmak için **AVS işleci erişimi etkin** ' i tıklatın.
