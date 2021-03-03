---
title: SSH üzerinden Azure Percept DK 'ye bağlanma
description: PuTTY ile Azure Percept DK 'de nasıl SSH kullanacağınızı öğrenin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/03/2021
ms.custom: template-how-to
ms.openlocfilehash: 8d150228be2cf6deff3bc2fd0a0599cca70d24ac
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663857"
---
# <a name="connect-to-your-azure-percept-dk-over-ssh"></a>SSH üzerinden Azure Percept DK 'ye bağlanma

[Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)aracılığıyla Azure Percept dk 'NIZE bir SSH bağlantısı kurmak için aşağıdaki adımları izleyin.

## <a name="prerequisites"></a>Önkoşullar

- Wi-Fi özelliğine sahip bir Windows, Linux veya OS X tabanlı ana bilgisayar
- SSH istemcisi
    - Ana bilgisayarınız Windows çalıştırıyorsa, [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) ETKIN bir SSH istemcudur ve bu kılavuz boyunca kullanılacaktır.
    - Ana bilgisayarınız Linux veya OS X çalıştırıyorsa, SSH Hizmetleri bu işletim sistemlerine dahil edilir ve ayrı bir istemci uygulaması olmadan çalıştırılabilir. SSH hizmetlerinin nasıl çalıştırılacağı hakkında daha fazla bilgi için işletim sistemi ürün belgelerinize bakın.
- Azure Percept DK

## <a name="initiate-the-ssh-connection"></a>SSH bağlantısını başlatma

1. Azure Percept DK (geliştirme seti) üzerinde güç

1. Geliştirme seti, Ethernet veya Wi-Fi üzerinden bir ağa zaten bağlıysa, sonraki adıma atlayın. Aksi takdirde, başka bir Wi-Fi ağa bağlanma gibi, ana bilgisayarınızı doğrudan Dev Kit Wi-Fi erişim noktasına bağlayın:
    - **ağ adı**: SCZ-xxxx (burada "xxxx", dev kıt 'in Mac ağ adresinin son dört rakamı)
    - **parola**: Dev Kit Ile gelen hoş geldiniz kartında bulunabilir

    > [!WARNING]
    > Azure Percept DK Wi-Fi erişim noktasına bağlıyken, ana bilgisayarınız Internet bağlantısını geçici olarak kaybedecektir. Etkin video konferansı çağrıları, web akışı veya diğer ağ tabanlı deneyimler, Azure Percept DK oluşturma deneyiminin 3. adımı tamamlanana kadar kesintiye uğracaktır.

1. PuTTY’yi açın. Aşağıdaki girin ve devkit 'te SSH için **Aç** ' a tıklayın:

    1. Ana bilgisayar adı: 10.1.1.1
    1. Bağlantı noktası: 22
    1. Bağlantı türü: SSH

    > [!NOTE]
    > **Ana bilgisayar adı** , cihazınızın IP adresidir. Geliştirme seti, Dev Kit 'in Wi-Fi erişim noktasına bağlıysa, IP adresi 10.1.1.1 olur. Geliştirme seti, Ethernet üzerinden bağlandıysa, cihazın Ethernet yönlendiricisinden veya hub 'ından alabileceğiniz yerel IP adresini kullanın. Cihazınız Wi-Fi üzerinden bağlandıysa, [Azure PERCEPT dk taslak deneyimi](./quickstart-percept-dk-set-up.md)SıRASıNDA toplanan IP adresini kullanmanız gerekir.

    :::image type="content" source="./media/how-to-ssh-into-percept-dk/ssh-putty.png" alt-text="Görüntüyle.":::

1. PuTTY terminalinde oturum açın. OOBE sırasında bir SSH Kullanıcı adı ve parola ayarlarsanız, istendiğinde bu oturum açma kimlik bilgilerini girin. Aksi takdirde, aşağıdakileri girin:  

    1. farklı oturum aç: kök
    1. Parola: p@ssw0rd

    :::image type="content" source="./media/how-to-ssh-into-percept-dk/putty-terminal.png" alt-text="PuTTY Terminal penceresi.":::  

## <a name="next-steps"></a>Sonraki adımlar

SSH üzerinden Azure Percept DK 'e başarıyla bağlandıktan sonra, sorun giderme, USB güncelleştirmeleri ve DiagTool veya SoftAP aracını çalıştırma gibi çeşitli görevleri gerçekleştirebilirsiniz.


