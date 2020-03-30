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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775348"
---
Bu bölümde, Doğrudan eşleme için aşağıdaki değişiklik işlemleri nasıl gerçekleştirilireaçıklanmaktadır:

### <a name="add-direct-peering-connections"></a>Doğrudan eşleme bağlantıları ekleme
1. Üstteki **+ Bağlantı Ekle** düğmesini tıklatın ve yeni bir eşleme bağlantısı yapılandırın.
    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-direct-modify-addconnection.png)
1. **Doğrudan eşleme Bağlantısı** formunu doldurun ve **Kaydet'i**tıklatın. Bir eşleme bağlantısı yapılandırma ile ilgili yardım için yukarıdaki "Doğrudan bir bakanlık oluşturma ve sağlama" bölümünün altındaki adımları gözden geçirin.
    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-direct-modify-savenewconnection.png)

### <a name="remove-direct-peering-connections"></a>Doğrudan bakan bağlantıları kaldırma

Bağlantıyı kaldırma şu anda portalda desteklenmez. [Microsoft'a bakan kişi.](mailto:peeringexperience@microsoft.com)

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Etkin bağlantılarda bant genişliğini yükseltme veya düşürme
1. Değiştirmek istediğiniz bir eşleme bağlantısını tıklatın ve sonra, ... **...**  >  **Bağlantı düğmesini edin.**
    > [!div class="mx-imgBorder"]
    > ![Eşleme Bağlantısı Editi](../media/setup-direct-modify-editconnection.png)
1. Bant genişliğini aşağıda gösterildiği gibi değiştirin ve ardından **Kaydet'i**tıklatın.
    > [!div class="mx-imgBorder"]
    > ![Eşleme Bağlantısı Bant Genişliğini Değiştir](../media/setup-direct-modify-editconnectionsettings.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumu ekleyin.
1. Değiştirmek istediğiniz bir eşleme bağlantısını tıklatın ve sonra, ... **...**  > Yukarıda gösterildiği gibi bağlantı düğmesini **edin.**
1. **Oturum IPv4 öneki** veya **Oturum IPv6 öneki** bilgisi ekleyin ve **Kaydet'i**tıklatın.

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırın.
    Removing a **Session IPv4 prefix** or **Session IPv6 prefix** info is not currently supported on portal. Contact [Microsoft peering](mailto:peeringexperience@microsoft.com).
