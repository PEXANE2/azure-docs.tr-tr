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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680918"
---
Bu bölümde, Doğrudan bakışlandırma için aşağıdaki değişiklik işlemleri nasıl gerçekleştirilir açıklanmaktadır.

### <a name="add-exchange-peering-connections"></a>Exchange eşleme bağlantıları ekleme

1. + **Bağlantı Ekle** düğmesini seçin ve yeni bir karşıla bağlantı yapınız.
    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-exchange-modify-addconnection.png)
1. **Exchange peering Connection** formunu doldurun ve **Kaydet'i**seçin. Bir eşleme bağlantısı yapılandırma konusunda yardım için, "Doğrudan bir bakış oluşturma ve sağlama" bölümündeki adımları gözden geçirin.
    > [!div class="mx-imgBorder"]
    > ![Exchange Peering Connection formu](../media/setup-exchange-modify-savenewconnection.png)

### <a name="remove-exchange-peering-connections"></a>Exchange eşleme bağlantılarını kaldırma

1. Silmek istediğiniz bir eşleme bağlantısı seçin ve sonra **...**  >  **Bağlantıyı sil.**
    > [!div class="mx-imgBorder"]
    > ![Bağlantı düğmesini sil](../media/setup-exchange-modify-deleteconnection.png)
1. Sil'i Sil kutusunu **onayla** kutusuna kaynak kimliğini girin ve **Sil'i**seçin.
    > [!div class="mx-imgBorder"]
    > ![Onayı silme](../media/setup-exchange-modify-deleteconnectionconfirm.png)

### <a name="add-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4 veya IPv6 oturumu ekleme

1. Değiştirmek istediğiniz bir eşleme bağlantısı seçin ve sonra **...**  >  **Bağlantıyı edin.**
    > [!div class="mx-imgBorder"]
    > ![Bağlantıyı ede düğmesini edin](../media/setup-exchange-modify-editconnection.png)
1. **IPv4 adresi** veya **IPv6 adres** bilgilerini ekleyin ve **Kaydet'i**seçin.
    > [!div class="mx-imgBorder"]
    > ![Eşleme bağlantısı modifikasyonları](../media/setup-exchange-modify-editconnectionsettings.png)

### <a name="remove-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılarda Bir IPv4 veya IPv6 oturumunu kaldırma

Varolan bir bağlantıdan bir IPv4 veya IPv6 oturumunun kaldırılması şu anda portalda desteklenmez. Daha fazla bilgi için [Microsoft'a başvurun.](mailto:peeringexperience@microsoft.com)