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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188207"
---
### <a name="to-modify-local-network-gateway-ip-address-prefixes---no-gateway-connection"></a><a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

#### <a name="to-add-additional-address-prefixes"></a>Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. IP adresi alanını *ek adres aralığı Ekle* kutusuna ekleyin.
3. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

#### <a name="to-remove-address-prefixes"></a>Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. Kaldırmak istediğiniz öneki içeren satırdaki **'... '** düğmesine tıklayın.
3. **Kaldır**’a tıklayın.
4. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

### <a name="to-modify-local-network-gateway-ip-address-prefixes---existing-gateway-connection"></a><a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Ağ geçidi bağlantınız varsa ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.

#### <a name="1-remove-the-connection"></a>1. bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **Bağlantılar**' a tıklayın.
2. Her bağlantı için satırdaki **...** öğesine tıklayın ve ardından **Sil**' e tıklayın.
3. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

#### <a name="2-modify-the-address-prefixes"></a>2. adres öneklerini değiştirin.

Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. IP adresi alanını ekleyin.
3. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağında, **Ayarlar** bölümünde **yapılandırma**' ya tıklayın.
2. Kaldırmak istediğiniz öneki içeren satırdaki **...** öğesine tıklayın.
3. **Kaldır**’a tıklayın.
4. Ayarlarınızı kaydetmek için **Kaydet** ' e tıklayın.

#### <a name="3-recreate-the-connection"></a>3. bağlantıyı yeniden oluşturun.

1. VNet 'iniz için sanal ağ geçidine gidin. (Yerel ağ geçidi değil.)
2. Sanal ağ geçidinde, **Ayarlar** bölümünde **Bağlantılar**' a tıklayın.
3. **+ Ekle** ' ye tıklayarak **bağlantı ekle** dikey penceresini açın.
4. Bağlantınızı yeniden oluşturun.
5. Bağlantıyı oluşturmak için **Tamam** ' ı tıklatın.