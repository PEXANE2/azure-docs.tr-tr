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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188206"
---
### <a name="to-modify-the-local-network-gateway-ip-address---no-gateway-connection"></a><a name="gwipnoconnection"></a>Yerel ağ geçidi IP adresini değiştirmek için-ağ geçidi bağlantısı yok

Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın. Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. **IP adresi** kutusunda, IP adresini değiştirin.
3. Ayarları kaydetmek için **Kaydet**’e tıklayın.

### <a name="to-modify-the-local-network-gateway-ip-address---existing-gateway-connection"></a><a name="gwipwithconnection"></a>Yerel ağ geçidi IP adresini değiştirmek için-mevcut ağ geçidi bağlantısı

Bağlantısı olan bir yerel ağ geçidini değiştirmek için öncelikle bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 
#### <a name="1-remove-the-connection"></a>1. bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **Bağlantılar**' a tıklayın.
2. Bağlantı satırında **...** öğesine tıklayın ve ardından **Sil**' e tıklayın.
3. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

#### <a name="2-modify-the-ip-address"></a>2. IP adresini değiştirin.

Aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. **IP adresi** kutusunda, IP adresini değiştirin.
2. Ayarları kaydetmek için **Kaydet**’e tıklayın.

#### <a name="3-recreate-the-connection"></a>3. bağlantıyı yeniden oluşturun.

1. VNet 'iniz için sanal ağ geçidine gidin. (Yerel ağ geçidi değil.)
2. Sanal ağ geçidinde, **Ayarlar** bölümünde **Bağlantılar**' a tıklayın.
3. **+ Ekle** ' ye tıklayarak **bağlantı ekle** dikey penceresini açın.
4. Bağlantınızı yeniden oluşturun.
5. Bağlantıyı oluşturmak için **Tamam** ' ı tıklatın.