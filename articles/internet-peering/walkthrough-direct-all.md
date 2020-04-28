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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75775517"
---
# <a name="direct-peering-walkthrough"></a>Doğrudan eşleme kılavuzu

Bu bölümde, bir doğrudan eşlemeyi ayarlamak ve yönetmek için izlemeniz gereken adımlar açıklanmaktadır.

## <a name="create-a-direct-peering"></a>Doğrudan eşleme oluşturma
> [!div class="mx-imgBorder"]
> ![Doğrudan eşleme iş akışı ve bağlantı durumları](./media/direct-peering.png)

Doğrudan eşleme sağlamak için aşağıdaki adımlar izlenmelidir:
1. Doğrudan eşleme gereksinimlerini anlamak için Microsoft [eşleme ilkesini](https://peering.azurewebsites.net/peering) gözden geçirin.
1. Eşleme isteği göndermek için [doğrudan eşleme oluşturma veya değiştirme](howto-direct-powershell.md) bölümündeki yönergeleri izleyin.
1. Bir eşleme isteği gönderdikten sonra, Microsoft, LOA (yetkilendirme harfi) veya diğer bilgileri sağlamak için kayıtlı e-posta adresinizi kullanarak iletişim kuracaktır.
1. Eşleme isteği onaylandıktan sonra, bağlantı durumu ProvisioningStarted olarak değişir.
1. Şunları yapmanız gerekir:
    1. LOA 'ya göre tüm kablolarını bağlama
    1. (isteğe bağlı) 169.254.0.0/16 kullanarak bağlantı testi gerçekleştirme
    1. BGP oturumunu yapılandırın ve ardından bize bildirin.
1. Microsoft, tüm ilkeyle izin verme ve uçtan uca doğrulama ile BGP oturumu sağlar.
1. İşlem başarılı olursa, eşleme bağlantı durumunun etkin olduğunu belirten bir bildirim alırsınız.
1. Daha sonra trafiğe yeni eşleme üzerinden izin verilir.

Bağlantı durumlarının standart [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) oturum durumlarıyla karıştırılmadığını unutmayın.

## <a name="convert-a-legacy-direct-peering-to-azure-resource"></a>Eski bir Doğrudan eşlemeyi Azure kaynağına dönüştürme
Eski bir doğrudan eşlemeyi Azure kaynağına dönüştürmek için aşağıdaki adımlar izlenmelidir:
1. [Eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-powershell.md) bölümündeki yönergeleri izleyin
1. Dönüştürme isteğini gönderdikten sonra, Microsoft isteği gözden geçirir ve gerekirse sizinle iletişim kuracaktır.
1. Onaylandığında, bir bağlantı durumuyla doğrudan eşinizi etkin olarak görürsünüz.

## <a name="deprovision-direct-peering"></a>Doğrudan eşleme sağlamayı kaldırma
Doğrudan eşleme sağlamak için [Microsoft eşleme](mailto:peering@microsoft.com) ekibine başvurun.

Bir doğrudan eşleme, sağlamayı kaldırma için ayarlandığında, bağlantı durumunu **Pendingremove** olarak görürsünüz.

> [!NOTE]
> ConnectionState, ProvisioningStarted veya ProvisioningCompleted olduğunda doğrudan eşlemeyi silmek için PowerShell cmdlet 'ini çalıştırırsanız işlem başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft ile eşleme ayarlama önkoşulları](prerequisites.md)hakkında bilgi edinin.
