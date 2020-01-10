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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774568"
---
Bu bölümde, doğrudan eşleme için aşağıdaki değişiklik işlemlerinin nasıl gerçekleştirileceği açıklanmaktadır:

### <a name="add-exchange-peering-connections"></a>Exchange eşleme bağlantıları Ekle

1. Üstteki **+ bağlantı ekle** düğmesine tıklayın ve yeni bir eşleme bağlantısı yapılandırın.
    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-exchange-modify-addconnection.png)
1. **Exchange eşleme bağlantısı** formunu doldurun ve **Kaydet**' e tıklayın. Bir eşleme bağlantısını yapılandırmaya yönelik yardım için yukarıdaki "doğrudan eşleme oluşturma ve sağlama" bölümündeki adımları gözden geçirin.
    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-exchange-modify-savenewconnection.png)

### <a name="remove-exchange-peering-connections"></a>Exchange eşleme bağlantılarını kaldır

1. Silmek istediğiniz eşleme bağlantısına tıklayın ve ardından **..** . > **Bağlantıyı Sil** düğmesine tıklayın.
    > [!div class="mx-imgBorder"]
    > ![eşleme bağlantısı silme](../media/setup-exchange-modify-deleteconnection.png)
1. Vurgulanan kutularda gösterildiği gibi **onaylama silme** kutusuna kaynak kimliğini girin ve **Sil**' e tıklayın.
    > [!div class="mx-imgBorder"]
    > ![eşleme bağlantısı Deleteonaylayın](../media/setup-exchange-modify-deleteconnectionconfirm.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılara IPv4/IPv6 oturumu ekleme

1. Değiştirmek istediğiniz eşleme bağlantısına tıklayın ve ardından **..** . > **Bağlantıyı Düzenle** düğmesine tıklayın.
    > [!div class="mx-imgBorder"]
    > ![eşleme bağlantısı](../media/setup-exchange-modify-editconnection.png) Düzenle
1. **IPv4 adresi** veya **IPv6 adresi** bilgisi ekleyin ve **Kaydet**' e tıklayın.
    > [!div class="mx-imgBorder"]
    > ![eşleme bağlantısı değiştirme](../media/setup-exchange-modify-editconnectionsettings.png)

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumunu kaldır

Mevcut bir bağlantıdan IPv4/IPv6 oturumunun kaldırılması, portalda Şu anda desteklenmiyor. [Microsoft eşlemesiyle](mailto:peeringexperience@microsoft.com)iletişim kurun.