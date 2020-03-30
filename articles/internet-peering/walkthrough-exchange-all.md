---
title: Exchange eşleme kılavuzu
titleSuffix: Azure
description: Exchange eşleme kılavuzu
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: bb761afa6d8953b441b6c9541c43b73031719494
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775179"
---
# <a name="exchange-peering-walkthrough"></a>Exchange eşleme kılavuzu

Bu bölümde, Exchange eşlemi ayarlamak ve yönetmek için izlemeniz gereken adımlar açıklanmaktadır.

## <a name="create-an-exchange-peering"></a>Exchange eşleme oluşturma
> [!div class="mx-imgBorder"]
> ![Exchange eşleme iş akışı ve bağlantı durumları](./media/exchange-peering.png)

Exchange'e bakmak için aşağıdaki adımlar izlenmelidir:
1. Exchange eşleme gereksinimlerini anlamak için Microsoft [bakış ilkesini](https://peering.azurewebsites.net/peering) gözden geçirin.
1. [PeeringDB'de](https://www.peeringdb.com/net/694) Microsoft'un eşleme konumunu ve bakan tesis kimliğini bulma
1. Create'deki yönergeleri kullanarak bir eşleme konumu için Exchange'i isteyin ve daha fazla ayrıntı için PowerShell makalesini [kullanarak Exchange'e bakan bir exchange'i değiştirin.](howto-exchange-powershell.md)
1. Bir eşleme isteği gönderdikten sonra, Microsoft isteği gözden geçirip gerekirse sizinle iletişim erecektir.
1. Onaylandıktan sonra, bağlantı durumu Onaylandı'da değişir
1. BGP oturumunu sonunda yapılandırın ve Microsoft'a bildirin
1. Biz DENY ALL ilkesi ile BGP oturumu hükmü ve uç-uç doğrulayın.
1. Başarılı olursa, bakan bağlantı durumunun Etkin olduğuna dair bir bildirim alırsınız.
1. Trafik daha sonra yeni bir bakış yoluyla izin verilecektir.

Bağlantı durumlarının standart [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) oturum durumları ile karıştırılmamalıdır unutmayın.

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Eski bir Exchange eşlemeyi Azure kaynağına dönüştürme
Azure kaynağına bakan eski bir Exchange'i dönüştürmek için aşağıdaki adımların izlenmesi gerekir:
1. [Azure kaynağına bakan eski bir Exchange'i dönüştür'teki](howto-legacy-exchange-powershell.md) yönergeleri izleyin
1. Dönüşüm isteğini gönderdikten sonra, Microsoft isteği inceler ve gerekirse sizinle iletişime geçer.
1. Onaylandıktan sonra, Exchange'inizin bağlantı durumuyla Etkin olarak bakacağını görürsünüz.

## <a name="deprovision-exchange-peering"></a>Deprovision Exchange akran
Exchange'in eşlemasını deprovision'e vermek için [Microsoft'a](mailto:peering@microsoft.com) başvurun.

Exchange eşlemi deprovision için ayarlandığında, bağlantı durumunu **BekleyenRemove** olarak görürsünüz

> [!NOTE]
> Bağlantı durumu ProvisioningStarted veya ProvisioningOlduğunda Exchange eşlemesini silmek için PowerShell cmdlet çalıştırıyorsanızİşlem başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft [ile eşlemeyi ayarlamak için Önkoşullar](prerequisites.md)hakkında bilgi edinin.
