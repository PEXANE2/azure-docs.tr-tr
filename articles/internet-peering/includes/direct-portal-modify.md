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
ms.openlocfilehash: d9ff01cf0180dae7f75d9753ba889d0caddad937
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775348"
---
Bu bölümde, doğrudan eşleme için aşağıdaki değişiklik işlemlerinin nasıl gerçekleştirileceği açıklanmaktadır:

### <a name="add-direct-peering-connections"></a>Doğrudan eşleme bağlantıları Ekle
1. Üstteki **+ bağlantı ekle** düğmesine tıklayın ve yeni bir eşleme bağlantısı yapılandırın.
    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-direct-modify-addconnection.png)
1. **Doğrudan eşleme bağlantısı** formunu doldurun ve **Kaydet**' e tıklayın. Bir eşleme bağlantısını yapılandırmaya yönelik yardım için yukarıdaki "doğrudan eşleme oluşturma ve sağlama" bölümündeki adımları gözden geçirin.
    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-direct-modify-savenewconnection.png)

### <a name="remove-direct-peering-connections"></a>Doğrudan eşleme bağlantılarını kaldır

Bir bağlantının kaldırılması portalda Şu anda desteklenmiyor. [Microsoft eşlemesiyle](mailto:peeringexperience@microsoft.com)iletişim kurun.

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Etkin bağlantılarda bant genişliğini yükseltme veya düşürme
1. Değiştirmek istediğiniz eşleme bağlantısına tıklayın ve ardından **..** . > **Bağlantıyı Düzenle** düğmesine tıklayın.
    > [!div class="mx-imgBorder"]
    > ![eşleme bağlantısı](../media/setup-direct-modify-editconnection.png) Düzenle
1. Bant genişliğini aşağıda gösterildiği gibi değiştirin ve ardından **Kaydet**' e tıklayın.
    > [!div class="mx-imgBorder"]
    > ![eşleme bağlantısı bant genişliğini değiştirin](../media/setup-direct-modify-editconnectionsettings.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılara IPv4/IPv6 oturumu ekleyin.
1. Değiştirmek istediğiniz eşleme bağlantısına tıklayın ve ardından yukarıda gösterildiği gibi **.** .. > **Bağlantıyı Düzenle** düğmesine tıklayın.
1. **Oturum IPv4 ön eki** veya **oturum IPv6 öneki** bilgilerini ekleyin ve **Kaydet**' e tıklayın.

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırın.
    Removing a **Session IPv4 prefix** or **Session IPv6 prefix** info is not currently supported on portal. Contact [Microsoft peering](mailto:peeringexperience@microsoft.com).
