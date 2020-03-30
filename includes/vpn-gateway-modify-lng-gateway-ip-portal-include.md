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
ms.openlocfilehash: 52084b065ef65a69a6691b6646d1e199f011910d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188206"
---
### <a name="to-modify-the-local-network-gateway-ip-address---no-gateway-connection"></a><a name="gwipnoconnection"></a>Yerel ağ ağ geçidi IP adresini değiştirmek için - ağ geçidi bağlantısı yok

Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın. Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. Yerel Ağ Ağ Geçidi kaynağında, **Ayarlar** bölümünde **Yapılandırma'yı**tıklatın.
2. IP **adres** kutusunda IP adresini değiştirin.
3. Ayarları kaydetmek için **Kaydet**’e tıklayın.

### <a name="to-modify-the-local-network-gateway-ip-address---existing-gateway-connection"></a><a name="gwipwithconnection"></a>Yerel ağ ağ geçidi IP adresini değiştirmek için - varolan ağ geçidi bağlantısı

Bağlantısı olan yerel bir ağ ağ ağ geçidini değiştirmek için önce bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 
#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel Ağ Ağ Geçidi kaynağında, **Ayarlar** **bölümünde, Bağlantılar'ı**tıklatın.
2. Bağlantı için satırdaki **...'u** tıklatın ve sonra **Sil'i**tıklatın.
3. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.

#### <a name="2-modify-the-ip-address"></a>2. IP adresini değiştirin.

Aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. IP **adres** kutusunda IP adresini değiştirin.
2. Ayarları kaydetmek için **Kaydet**’e tıklayın.

#### <a name="3-recreate-the-connection"></a>3. Bağlantıyı yeniden oluşturun.

1. VNet'iniz için Sanal Ağ Ağ Ağ Geçidi'ne gidin. (Yerel Ağ Ağ Ağ Geçidi değil.)
2. Sanal Ağ Ağ Ağ Geçidi'nde, **Ayarlar** **bölümünde, Bağlantılar'ı**tıklatın.
3. **Ekle bağlantı** bıçağını açmak için + **Ekle'yi** tıklatın.
4. Bağlantınızı yeniden oluşturun.
5. Bağlantıyı oluşturmak için **Tamam'ı** tıklatın.