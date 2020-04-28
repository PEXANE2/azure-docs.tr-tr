---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: e128f3c67a41322d9c25a8d6941e937729760bf4
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188317"
---
Bu adımda, yük dengeli uç nokta 59999 (daha önce belirtildiği gibi) için araştırma bağlantı noktasını açmak üzere bir güvenlik duvarı kuralı ve kullanılabilirlik grubu dinleyicisi bağlantı noktasını açmak için başka bir kural oluşturursunuz. Kullanılabilirlik grubu çoğaltmaları içeren VM 'lerde yük dengeli uç nokta oluşturduğunuz için, ilgili VM 'lerde yoklama bağlantı noktasını ve dinleyici bağlantı noktasını açmanız gerekir.

1. Çoğaltmaları barındıran VM 'lerde, **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**'nı başlatın.

2. **Gelen kuralları**' na sağ tıklayın ve ardından **Yeni kural**' a tıklayın.

3. **Kural türü** sayfasında, **bağlantı noktası**' nı seçin ve ardından **İleri**' ye tıklayın.

4. **Protokol ve bağlantı noktaları** sayfasında **TCP**' yi seçin, **belirli yerel bağlantı noktaları** kutusuna **59999** yazın ve ardından **İleri**' ye tıklayın.

5. **Eylem** sayfasında, **bağlantıya izin ver** ' i seçili bırakın ve **İleri**' ye tıklayın.

6. **Profil** sayfasında, varsayılan ayarları kabul edin ve ardından **İleri**' ye tıklayın.

7. **Ad** sayfasında, **ad** metin kutusunda, **her zaman dinleyici araştırması bağlantı noktası**gibi bir kural adı belirtin ve ardından **son**' a tıklayın.

8. Kullanılabilirlik grubu dinleyicisi bağlantı noktası için önceki adımları tekrarlayın (örneğin, betiğin $EndpointPort parametresinde daha önce belirtildiği gibi) ve ardından **her zaman açık dinleyici bağlantı noktası**gibi uygun bir kural adı belirtin.

