---
title: 'Sanal makineye kopyalayıp yapıştırMa: Azure Kalesi'
description: Bu makalede, Bastion'u kullanarak azure vm'ye kopyalama ve yapıştırmayı öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: 0af0fdfa1b0bcbb06d9424390e7ca8c8f406bcb3
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619315"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion"></a>Sanal makineye kopyalayıp yapıştır: Azure Kalesi

Bu makale, Azure Bastion kullanırken sanal makinelere metin kopyalayıp yapıştırmanıza yardımcı olur. Bir VM ile çalışmadan önce, [bir Bastion ana bilgisayar oluşturmak](bastion-create-host-portal.md)için adımları izlediğinden emin olun. Ardından, [RDP](bastion-connect-vm-rdp.md) veya [SSH](bastion-connect-vm-ssh.md)kullanarak çalışmak istediğiniz VM'ye bağlanın.

Gelişmiş Pano API erişimini destekleyen tarayıcılar için, yerel aygıtınızdaki uygulamaları kopyalayıp yapıştırdığınız şekilde yerel aygıtınız ile uzak oturum arasındaki metni kopyalayıp yapıştırabilirsiniz. Diğer tarayıcılar için Bastion pano erişim aracı paletini kullanabilirsiniz.

   ![Panoya izin ver](./media/bastion-vm-manage/allow.png)

Yalnızca metin kopyalama/yapıştırılar desteklenir. Doğrudan kopyalama ve yapıştırması için tarayıcınız, Bastion oturumu başolarak lanse edilirken pano erişimiiçin sizi isteyebilir. Web sayfasının panoya erişmesine **izin verin.**

## <a name="copy-to-a-remote-session"></a><a name="to"></a>Uzak bir oturuma kopyalama

[Azure portalını ](https://portal.azure.com)kullanarak sanal makineye bağlandıktan sonra aşağıdaki adımları tamamlayın:

1. Yerel aygıttaki metni/içeriği yerel panoya kopyalayın.
1. Uzak oturum sırasında, iki ok seçerek Bastion pano erişim aracı paletini başlatın. Oklar oturumun sol merkezinde yer alır.

   ![takım paleti](./media/bastion-vm-manage/left.png)

   ![pano](./media/bastion-vm-manage/clipboard.png)
1. Genellikle, kopyalanan metin otomatik olarak Bastion kopyala yapıştır paletinde gösterir. Metniniz orada değilse, metni paletteki metin alanına yapıştırın.
1. Metin metin alanına girince, metin den uzak oturuma yapıştırabilirsiniz.

   ![Yapıştır](./media/bastion-vm-manage/local.png)

## <a name="copy-from-a-remote-session"></a><a name="from"></a>Uzak bir oturumdan kopyalama

[Azure portalını ](https://portal.azure.com)kullanarak sanal makineye bağlandıktan sonra aşağıdaki adımları tamamlayın:

1. Uzak oturumdaki metni/içeriği uzak panoya (Ctrl-C kullanarak) kopyalayın.

   ![takım paleti](./media/bastion-vm-manage/remote.png)
1. Uzak oturum sırasında, iki ok seçerek Bastion pano erişim aracı paletini başlatın. Oklar oturumun sol merkezinde yer alır.

   ![pano](./media/bastion-vm-manage/clipboard2.png)
1. Genellikle, kopyalanan metin otomatik olarak Bastion kopyala yapıştır paletinde gösterir. Metniniz orada değilse, metni paletteki metin alanına yapıştırın.
1. Metin metin alanına girince, yerel aygıta yapıştırabilirsiniz.

   ![Yapıştır](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Sonraki adımlar

[Bastion SSS'yi](bastion-faq.md)okuyun.