---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 35f506235f698fbcf42308e6f0b0f400e925df29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68361550"
---
### <a name="bastion-tier"></a>Burç katmanı

Burç ana bilgisayar, uygulama ve veritabanı örneklerine erişmek için atlama sunucusu olarak kullanabileceğiniz isteğe bağlı bir bileşendir. Tavsiye, güvenli erişim için şirket içi ağınızla bir ExpressRoute bağlantısı veya siteiçi VPN kurmak olsa da, burç ana bilgisayarı VM'nin kendisine atanmış bir genel IP adresi olabilir. Ayrıca, gelen trafik için yalnızca SSH (bağlantı noktası 22, Linux) veya RDP (port 3389, Windows Server) açılmalıdır. Yüksek kullanılabilirlik için, bir burç ana bilgisayarını iki kullanılabilirlik bölgesinde veya tek bir kullanılabilirlik kümesine dağıtın.

Ayrıca, sanal ağdaki diğer VM'lere, burç ana aracınızdan kimlik bilgilerini ileterek erişmenizi sağlayan SSH aracısının iletilmesini de etkinleştirebilirsiniz. Veya diğer örneklere erişmek için SSH tünellerini kullanın.

Aracı iletme örneği aşağıda verilmiştir:

```
ssh -A -t user@BASTION_SERVER_IP ssh -A root@TARGET_SERVER_IP`
```

Bu komut burç bağlanır ve sonra `ssh` hemen yeniden çalışır, böylece hedef örnek bir terminal olsun. Kümeniz farklı yapılandırılmışsa, hedef örnekte kök dışında bir kullanıcı belirtmeniz gerekebilir. Bağımsız `-A` değişken aracı bağlantısını iletir, böylece yerel makinenizdeki özel anahtarınız otomatik olarak kullanılır. Aracı yönlendirmenin bir zincir olduğunu unutmayın, bu nedenle ikinci `ssh` komut, hedef örnekten başlatılan sonraki SSH bağlantılarını da yerel özel anahtarınızı kullanabilmek için içerir. `-A`