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
ms.openlocfilehash: e3e87e59f5b3c95051d9ee53e4b8d87afe9d9ba8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774568"
---
Bu bölümde, Doğrudan eşleme için aşağıdaki değişiklik işlemleri nasıl gerçekleştirilireaçıklanmaktadır:

### <a name="add-exchange-peering-connections"></a>Exchange eşleme bağlantıları ekleme

1. Üstteki **+ Bağlantı Ekle** düğmesini tıklatın ve yeni bir eşleme bağlantısı yapılandırın.
    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-exchange-modify-addconnection.png)
1. **Exchange peering Bağlantı** formunu doldurun ve **Kaydet'i**tıklatın. Bir eşleme bağlantısı yapılandırma ile ilgili yardım için yukarıdaki "Doğrudan bir bakanlık oluşturma ve sağlama" bölümünün altındaki adımları gözden geçirin.
    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-exchange-modify-savenewconnection.png)

### <a name="remove-exchange-peering-connections"></a>Exchange eşleme bağlantılarını kaldırma

1. Silmek istediğiniz bir eşleme bağlantısını tıklatın ve **...** sonra, ...  >  **Bağlantı düğmesini silin.**
    > [!div class="mx-imgBorder"]
    > ![Eşleme Bağlantısı Silme](../media/setup-exchange-modify-deleteconnection.png)
1. Vurgulanan kutularda gösterildiği gibi Kaynak Kimliğini Sil kutusunu **onayla** kutusuna girin ve **Sil'i**tıklatın.
    > [!div class="mx-imgBorder"]
    > ![Peering Connection DeleteConfirm](../media/setup-exchange-modify-deleteconnectionconfirm.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumu ekleme

1. Değiştirmek istediğiniz bir eşleme bağlantısını tıklatın ve sonra, ... **...**  >  **Bağlantı düğmesini edin.**
    > [!div class="mx-imgBorder"]
    > ![Eşleme Bağlantısı Editi](../media/setup-exchange-modify-editconnection.png)
1. **IPv4 adresi** veya **IPv6 adres** bilgilerini ekleyin ve **Kaydet'i**tıklatın.
    > [!div class="mx-imgBorder"]
    > ![Eşleme Bağlantısı Değiştirme](../media/setup-exchange-modify-editconnectionsettings.png)

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırma

Varolan bir bağlantıdan bir IPv4/IPv6 oturumunun kaldırılması şu anda portalda desteklenmez. [Microsoft'a bakan kişi.](mailto:peeringexperience@microsoft.com)