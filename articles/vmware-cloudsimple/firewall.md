---
title: CloudSimple tarafından Azure VMware Çözümü - Güvenlik duvarı tabloları ve kuralları ayarlama
description: Alt ağlar ve VN'lerde trafiği kısıtlamak için Özel Bulut güvenlik duvarı tablolarının ve kurallarının nasıl ayarlanır şekilde ayarlanır.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 986f4b0da7254ebac3725a704f32af785c72fbcc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244673"
---
# <a name="set-up-firewall-tables-and-rules-for-private-clouds"></a>Özel Bulutlar için güvenlik duvarı tabloları ve kuralları ayarlama

Güvenlik duvarı tabloları ve ilişkili kurallar, belirli alt ağlara ve VLAN'lara uygulanacak trafik kısıtlamaları belirtmenize olanak sağlar.

* Bir alt ağ tek bir güvenlik duvarı tablosuyla ilişkilendirilebilir.
* Güvenlik duvarı tablosu birden çok alt ağla ilişkilendirilebilir.

## <a name="add-a-new-firewall-table"></a>Yeni bir güvenlik duvarı tablosu ekleme

1. [CloudSimple portalına erişin](access-cloudsimple-portal.md) ve yan menüde **Ağ'ı** seçin.
2. **Güvenlik Duvarı Tabloları'nı**seçin.
3. **Güvenlik duvarı tablosu oluştur'u**seçin.

    ![VLAN/alt ağ sayfası](media/firewall-tables-page.png)

4. Tablo için bir ad girin.
5. Tablo için varsayılan kural listelenir. Ek bir kural oluşturmak için **Yeni Kural Oluştur'u** tıklatın. Ayrıntılar için aşağıdaki yordama bakın.
6. Güvenlik duvarı tablosunu kaydetmek için **Bitti'yi** tıklatın.

> [!IMPORTANT]
> Özel Bulut başına en fazla iki Güvenlik Duvarı masası oluşturabilirsiniz.

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Güvenlik duvarı kuralları, güvenlik duvarının belirli trafik türlerini nasıl ele alsüreceğini belirler. Seçili bir güvenlik duvarı tablosunun **Kurallar** sekmesi, ilişkili tüm kuralları listeler.

![Güvenlik duvarı kuralları tablosu](media/firewall-rules-tab.png)

## <a name="create-a-firewall-rule"></a>Güvenlik duvarı kuralı oluşturma

1. Aşağıdakiyollardan birinde bir güvenlik duvarı kuralı oluşturmak için ayarları görüntüleyin:
    * Güvenlik duvarı tablosu oluştururken **Kuralı Ekle'yi** tıklatın.
    * Ağ > Güvenlik Duvarı **Tabloları** sayfasında belirli bir güvenlik duvarı tablosunu seçin ve yeni güvenlik duvarı kuralı **oluştur'u**tıklatın.
2. Kuralı aşağıdaki gibi ayarlayın:
    * **Adı**. Kurala bir isim ver.
    * **Öncelik**. Kurala öncelik atayın. Önce daha düşük sayılara sahip kurallar yürütülür.
    * **Trafik türü.** Kuralın Özel Bulut, Internet veya VPN trafiği (durum belirtemesi) veya genel bir IP adresi (durum belirteç) için olup olmadığını seçin.
    * **Protokol.** Kuralın kapsadığı protokolü (TCP, UDP veya herhangi bir protokol) seçin.
    * **Yön**. Kuralın gelen veya giden trafik için olup olmadığını seçin. Gelen ve giden trafik için ayrı kurallar tanımlamanız gerekir.
    * **Eylem**. Kural eşleşiyorsa (izin verin veya reddedin) yapılacak eylemi seçin.
    * **Kaynak**. Kuralın kapsadığı kaynakları (CIDR bloğu, dahili veya herhangi bir kaynak) belirtin.
    * **Kaynak bağlantı noktası aralığı.** Kurala tabi bağlantı noktalarının aralığını belirtin.
    * **Yön**. Gelen veya giden'i seçin.
    * **Hedef**. Kuralın kapsadığı hedefleri (CIDR bloğu, dahili veya herhangi bir kaynak) belirtin.
    * **Kaynak bağlantı noktası aralığı.** Kurala tabi bağlantı noktalarının aralığını belirtin.

    ![Güvenlik duvarı tablosu kuralı ekle](media/firewall-rule-create.png)

3. Kuralı kaydetmek ve güvenlik duvarı tablosuiçin kurallar listesine eklemek için **Bitti'yi** tıklatın.

> [!IMPORTANT]
> Her Güvenlik Duvarı tablosunda en fazla 10 gelen kuralı ve 20 giden kural olabilir. Bu limitler [desteğe başvurarak](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)artırılabilir.

## <a name="attach-vlanssubnets"></a><a name="attach-vlans-subnet"></a>V'leri/alt ağlarını takın

Güvenlik duvarı tablosunu tanımladıktan sonra, tablodaki kurallara tabi olan alt ağları belirtebilirsiniz.

1. **Ağ** > **Güvenlik Duvarı Tabloları** sayfasında bir güvenlik duvarı tablosu seçin.
2. Ekli **VLAN'lar/Altnet** sekmesini açın.
3. **VLAN/Subnet'e Ekle'yi**tıklatın.
4. Özel Bulut ve VLAN'ı seçin. İlişkili alt ağ adı ve CIDR bloğu gösterilir.
5. **Gönder'i**tıklatın.
