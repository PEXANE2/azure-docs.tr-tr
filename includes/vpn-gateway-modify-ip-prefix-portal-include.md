---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1199819d274590cc81d0234680f8765f9cc36c0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188207"
---
### <a name="to-modify-local-network-gateway-ip-address-prefixes---no-gateway-connection"></a><a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

#### <a name="to-add-additional-address-prefixes"></a>Başka adres ön ekleri eklemek için:

1. Yerel Ağ Ağ Geçidi kaynağında, **Ayarlar** bölümünde **Yapılandırma'yı**tıklatın.
2. *Ek adres aralığı* ekle kutusuna IP adresi alanını ekleyin.
3. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.

#### <a name="to-remove-address-prefixes"></a>Adres ön eklerini kaldırmak için:

1. Yerel Ağ Ağ Geçidi kaynağında, **Ayarlar** bölümünde **Yapılandırma'yı**tıklatın.
2. Kaldırmak istediğiniz önek içeren satırdaki **'...'** seçeneğini tıklatın.
3. **Kaldır**’a tıklayın.
4. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.

### <a name="to-modify-local-network-gateway-ip-address-prefixes---existing-gateway-connection"></a><a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Ağ geçidi bağlantınız varsa ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.

#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel Ağ Ağ Geçidi kaynağında, **Ayarlar** **bölümünde, Bağlantılar'ı**tıklatın.
2. Her bağlantı için satırdaki **...'u** tıklatın ve sonra **Sil'i**tıklatın.
3. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.

#### <a name="2-modify-the-address-prefixes"></a>2. Adres öneklerini değiştirin.

Başka adres ön ekleri eklemek için:

1. Yerel Ağ Ağ Geçidi kaynağında, **Ayarlar** bölümünde **Yapılandırma'yı**tıklatın.
2. IP adresi alanını ekleyin.
3. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.

Adres ön eklerini kaldırmak için:

1. Yerel Ağ Ağ Geçidi kaynağında, **Ayarlar** bölümünde **Yapılandırma'yı**tıklatın.
2. Kaldırmak istediğiniz önek içeren satırdaki **...** seçeneğini tıklatın.
3. **Kaldır**’a tıklayın.
4. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.

#### <a name="3-recreate-the-connection"></a>3. Bağlantıyı yeniden oluşturun.

1. VNet'iniz için Sanal Ağ Ağ Ağ Geçidi'ne gidin. (Yerel Ağ Ağ Ağ Geçidi değil.)
2. Sanal Ağ Ağ Ağ Geçidi'nde, **Ayarlar** **bölümünde, Bağlantılar'ı**tıklatın.
3. **Ekle bağlantı** bıçağını açmak için + **Ekle'yi** tıklatın.
4. Bağlantınızı yeniden oluşturun.
5. Bağlantıyı oluşturmak için **Tamam'ı** tıklatın.