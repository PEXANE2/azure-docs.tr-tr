---
title: Doğrudan eşleme kılavuzu
titleSuffix: Azure
description: Doğrudan eşleme kılavuzu
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d88fcfc4d3e073bf544f2ca0f4d01dbe305b45da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775517"
---
# <a name="direct-peering-walkthrough"></a>Doğrudan eşleme kılavuzu

Bu bölümde, Doğrudan bir bakış kurmak ve yönetmek için izlemeniz gereken adımlar açıklanmaktadır.

## <a name="create-a-direct-peering"></a>Doğrudan eşleme oluşturma
> [!div class="mx-imgBorder"]
> ![Doğrudan eşleme iş akışı ve bağlantı durumları](./media/direct-peering.png)

Doğrudan bir bakış sağlamak için aşağıdaki adımların izlenmesi gerekir:
1. Doğrudan bakma gereksinimlerini anlamak için Microsoft [bakış ilkesini](https://peering.azurewebsites.net/peering) gözden geçirin.
1. Bir eşleme isteği göndermek için [Doğrudan bakış oluşturma'daki](howto-direct-powershell.md) yönergeleri izleyin veya değiştirin.
1. Bir eşleme isteği gönderdikten sonra, Microsoft kayıtlı e-posta adresinizi kullanarak LOA (Yetki Mektubu) sağlamak veya başka bilgiler için iletişim kuracaktır.
1. Eşleme isteği onaylandıktan sonra, ProvisioningStarted bağlantı durumu değişiklikleri.
1. Şunları yapmanız gerekir:
    1. LOA'ya göre komple kablolama
    1. (isteğe bağlı olarak) 169.254.0.0/16 kullanarak bağlantı testi gerçekleştirin
    1. BGP oturumunu yapılandırın ve sonra bize bildirin.
1. MICROSOFT, TÜM ilkeyi REDDET ve uçuça doğrulayan BGP oturumunu karşılar.
1. Başarılı olursa, bakan bağlantı durumunun Etkin olduğuna dair bir bildirim alırsınız.
1. Trafik daha sonra yeni bir bakış yoluyla izin verilecektir.

Bağlantı durumlarının standart [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) oturum durumları ile karıştırılmamalıdır unutmayın.

## <a name="convert-a-legacy-direct-peering-to-azure-resource"></a>Eski bir Doğrudan eşlemeyi Azure kaynağına dönüştürme
Azure kaynağına bakan bir eski doğrudan dönüştürmek için aşağıdaki adımların izlenmesi gerekir:
1. Eski bir [Azure kaynağını doğrudan gözden dönüştür'teki](howto-legacy-direct-powershell.md) yönergeleri izleyin
1. Dönüşüm isteğini gönderdikten sonra, Microsoft isteği inceler ve gerekirse sizinle iletişime geçer.
1. Onaylandıktan sonra, Doğrudan'ınızın Etkin olarak bir bağlantı durumuyla bakabildiğimi görürsünüz.

## <a name="deprovision-direct-peering"></a>Deprovision Doğrudan bakma
Doğrudan bakmayı deprovision'ı devre denmek için [Microsoft'un izleme](mailto:peering@microsoft.com) ekibine başvurun.

Doğrudan bir eşleme deprovision için ayarlandığında, bağlantı durumunu **BekleyenRemove** olarak görürsünüz

> [!NOTE]
> ConnectionState ProvisioningStarted veya ProvisioningTamamlandığında Doğrudan eşlemesi silmek için PowerShell cmdlet çalıştırınİşlembaşarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft [ile eşlemeyi ayarlamak için Önkoşullar](prerequisites.md)hakkında bilgi edinin.
