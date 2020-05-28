---
title: include dosyası
titleSuffix: Azure
description: include dosyası
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e06b5261ca6923e158c818d236a30cf6ebff189b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81680918"
---
Bu bölümde, doğrudan eşleme için aşağıdaki değişiklik işlemlerinin nasıl gerçekleştirileceği açıklanmaktadır.

### <a name="add-exchange-peering-connections"></a>Exchange eşleme bağlantıları Ekle

1. **+ Bağlantı ekle** düğmesini seçin ve yeni bir eşleme bağlantısı yapılandırın.
    > [!div class="mx-imgBorder"]
    > ![Eşleme kaynağı görünümü](../media/setup-exchange-modify-addconnection.png)
1. **Exchange eşleme bağlantısı** formunu doldurun ve **Kaydet**' i seçin. Eşleme bağlantısı yapılandırmaya yönelik yardım için "doğrudan eşleme oluşturma ve sağlama" bölümündeki adımları gözden geçirin.
    > [!div class="mx-imgBorder"]
    > ![Exchange eşleme bağlantı formu](../media/setup-exchange-modify-savenewconnection.png)

### <a name="remove-exchange-peering-connections"></a>Exchange eşleme bağlantılarını kaldır

1. Silmek istediğiniz eşleme bağlantısını seçin ve ardından **..**  >  . seçeneğini belirleyin. **Bağlantıyı silin**.
    > [!div class="mx-imgBorder"]
    > ![Bağlantıyı Sil düğmesi](../media/setup-exchange-modify-deleteconnection.png)
1. **Onaylama silme** kutusuna kaynak kimliğini girin ve **Sil**' i seçin.
    > [!div class="mx-imgBorder"]
    > ![Silme onayı](../media/setup-exchange-modify-deleteconnectionconfirm.png)

### <a name="add-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılara IPv4 veya IPv6 oturumu ekleme

1. Değiştirmek istediğiniz eşleme bağlantısını seçin ve ardından **..**  >  . seçeneğini belirleyin. **Bağlantıyı Düzenle**.
    > [!div class="mx-imgBorder"]
    > ![Bağlantıyı Düzenle düğmesi](../media/setup-exchange-modify-editconnection.png)
1. **IPv4 adresi** veya **IPv6 adresi** bilgileri ekleyin ve **Kaydet**' i seçin.
    > [!div class="mx-imgBorder"]
    > ![Eşleme bağlantısı değişiklikleri](../media/setup-exchange-modify-editconnectionsettings.png)

### <a name="remove-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4 veya IPv6 oturumu kaldırma

Mevcut bir bağlantıdan IPv4 veya IPv6 oturumunun kaldırılması Şu anda portalda desteklenmiyor. Daha fazla bilgi için [Microsoft eşlemesi](mailto:peeringexperience@microsoft.com)'ne başvurun.