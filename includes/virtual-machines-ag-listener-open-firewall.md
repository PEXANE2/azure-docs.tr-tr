---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: e128f3c67a41322d9c25a8d6941e937729760bf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188317"
---
Bu adımda, sonda bağlantı noktasını yük dengeli bitiş noktası (daha önce belirtildiği gibi 59999) açmak için bir güvenlik duvarı kuralı ve kullanılabilirlik grubu dinleyici bağlantı noktasını açmak için başka bir kural oluşturursunuz. Kullanılabilirlik grubu yinelemelerini içeren VM'lerde yük dengeli bitiş noktası oluşturduğunuzdan, sonda bağlantı noktasını ve ilgili VM'lerde dinleyici bağlantı noktasını açmanız gerekir.

1. Yinelemeleri barındıran VM'lerde **Gelişmiş Güvenlik ile Windows Güvenlik Duvarı'nı başlatın.**

2. **Gelen Kurallar'ı**sağ tıklatın ve sonra **Yeni Kural'ı**tıklatın.

3. Kural **Türü** sayfasında **Bağlantı Noktası'nı**seçin ve sonra **İleri'yi**tıklatın.

4. Protokol **ve Bağlantı Noktaları** sayfasında, **Belirli yerel bağlantı noktaları** kutusunda **TCP,** **59999** yazın'ı seçin ve sonra **İleri'yi**tıklatın.

5. **Eylem** sayfasında, bağlantıyı seçili **olarak İzin** ver'i tutun ve sonra **İleri'yi**tıklatın.

6. **Profil** sayfasında varsayılan ayarları kabul edin ve sonra **İleri'yi**tıklatın.

7. **Ad** sayfasında, **Ad** metin kutusunda, Her Zaman Dinleyici **Sondası Bağlantı Noktası'nda**olduğu gibi bir kural adı belirtin ve ardından **Bitir'i**tıklatın.

8. Kullanılabilirlik grubu dinleyici bağlantı noktası için önceki adımları yineleyin (komut dosyasının $EndpointPort parametresinde daha önce belirtildiği gibi) ve ardından **Her Zaman Dinleyici Bağlantı Noktası'nda**gibi uygun bir kural adı belirtin.

