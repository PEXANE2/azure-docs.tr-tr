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
ms.openlocfilehash: 3894bf046ed4ee3f068e43dbc5bc5b7f2a1002b4
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681053"
---
Bu bölümde, Doğrudan bakışlandırma için aşağıdaki değişiklik işlemleri nasıl gerçekleştirilir açıklanmaktadır.

### <a name="add-direct-peering-connections"></a>Doğrudan eşleme bağlantıları ekleme
1. + **Bağlantı Ekle** düğmesini seçin ve yeni bir karşıla bağlantı yapınız.
    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-direct-modify-addconnection.png)

1. **Doğrudan Eşleme Bağlantısı** formunu doldurun ve **Kaydet'i**seçin. Bir eşleme bağlantısı yapılandırma konusunda yardım için, "Doğrudan bir bakış oluşturma ve sağlama" bölümündeki adımları gözden geçirin.
    > [!div class="mx-imgBorder"]
    > ![Doğrudan Peering Bağlantı formu](../media/setup-direct-modify-savenewconnection.png)

### <a name="remove-direct-peering-connections"></a>Doğrudan bakan bağlantıları kaldırma

Bir bağlantıyı kaldırmak şu anda Azure portalında desteklenmez. Daha fazla bilgi için [Microsoft'a başvurun.](mailto:peeringexperience@microsoft.com)

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Etkin bağlantılarda bant genişliğini yükseltme veya düşürme
1. Değiştirmek istediğiniz bir eşleme bağlantısı seçin ve sonra **...**  >  **Bağlantıyı edin.**
    > [!div class="mx-imgBorder"]
    > ![Bağlantıyı ede](../media/setup-direct-modify-editconnection.png)

1. Kaydırıcıyı hareket ettirerek bant genişliğini değiştirin ve ardından **Kaydet'i**seçin.
    > [!div class="mx-imgBorder"]
    > ![Bant genişliğini değiştirme](../media/setup-direct-modify-editconnectionsettings.png)

### <a name="add-ipv4-or-ipv6-session-information-on-active-connections"></a>Etkin bağlantılara IPv4 veya IPv6 oturum bilgileri ekleme
1. Değiştirmek istediğiniz bir eşleme bağlantısı seçin ve sonra **...**  > Bağlantıyı adım 1'de gösterildiği gibi **edin.**
1. **Oturum IPv4 öneki** veya **Oturum IPv6 öneki** bilgilerini girin ve **Kaydet'i**seçin.

### <a name="remove-ipv4-or-ipv6-session-information-on-active-connections"></a>Etkin bağlantılardaki IPv4 veya IPv6 oturum bilgilerini kaldırma
Oturum **IPv4 öneki** veya **Oturum IPv6 öneki** bilgilerini kaldırma şu anda portalda desteklenmez. Daha fazla bilgi için [Microsoft'a başvurun.](mailto:peeringexperience@microsoft.com)
