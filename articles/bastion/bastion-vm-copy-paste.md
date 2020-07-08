---
title: 'Sanal bir makineye Kopyala ve buradan Yapıştır: Azure savunma'
description: Bu makalede, savunma kullanarak bir Azure VM 'ye kopyalamayı ve buradan yapıştırmayı öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: how-to
ms.date: 05/04/2020
ms.author: charwen
ms.openlocfilehash: 4e818617b70e874153e026bcf40b6dfee8a8e04e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84744247"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion"></a>Sanal makineye Kopyala ve Yapıştır: Azure savunma

Bu makale, Azure savunma kullanırken sanal makinelere ve sanal makinelere metin kopyalayıp yapıştırmanıza yardımcı olur. Bir VM ile çalışmadan önce, [bir savunma ana bilgisayarı oluşturma](bastion-create-host-portal.md)adımlarını izlediğinizden emin olun. Ardından, [RDP](bastion-connect-vm-rdp.md) veya [SSH](bastion-connect-vm-ssh.md)kullanarak birlikte çalışmak istediğiniz sanal makineye bağlanın.

Gelişmiş Pano API erişimini destekleyen tarayıcılarda, yerel cihazınızdaki uygulamalar arasında kopyalama ve yapıştırma yaptığınız şekilde yerel cihazınız ile uzak oturum arasında metin kopyalayabilir ve yapıştırabilirsiniz. Diğer tarayıcılarda, savunma panosu erişim aracı paleti ' ni kullanabilirsiniz.

>[!NOTE]
>Şu anda yalnızca metin kopyalama/yapıştırma destekleniyor.
>

   ![Panoya izin ver](./media/bastion-vm-manage/allow.png)

Yalnızca metin kopyalama/yapıştırma desteklenir. Doğrudan kopyalama ve yapıştırma için, tarayıcınız, savunma oturumu başlatıldığında Pano erişimi isteyebilir. Web sayfasının panoya erişmesine **Izin verin** .

## <a name="copy-to-a-remote-session"></a><a name="to"></a>Uzak oturuma Kopyala

[Azure Portal ](https://portal.azure.com)kullanarak sanal makineye bağlandıktan sonra aşağıdaki adımları izleyin:

1. Yerel cihazdan metin/içerik ' i yerel panoya kopyalayın.
1. Uzak oturum sırasında, iki oku seçerek savunma panosu erişim aracı paletini başlatın. Oklar oturumun sol ortasında bulunur.

   ![Araç paleti](./media/bastion-vm-manage/left.png)

   ![pano](./media/bastion-vm-manage/clipboard.png)
1. Genellikle, kopyalanmış metin, bir savunma Kopyala Yapıştır paletinde otomatik olarak gösterilir. Metniniz yoksa, metni paletteki metin alanına yapıştırın.
1. Metin metin alanından olduktan sonra Uzak oturuma yapıştırabilirsiniz.

   ![masına](./media/bastion-vm-manage/local.png)

## <a name="copy-from-a-remote-session"></a><a name="from"></a>Uzak oturumdan Kopyala

[Azure Portal ](https://portal.azure.com)kullanarak sanal makineye bağlandıktan sonra aşağıdaki adımları izleyin:

1. Uzak oturumdaki metin/içeriği uzak Pano 'ya (CTRL-C kullanarak) kopyalayın.

   ![Araç paleti](./media/bastion-vm-manage/remote.png)
1. Uzak oturum sırasında, iki oku seçerek savunma panosu erişim aracı paletini başlatın. Oklar oturumun sol ortasında bulunur.

   ![pano](./media/bastion-vm-manage/clipboard2.png)
1. Genellikle, kopyalanmış metin, bir savunma Kopyala Yapıştır paletinde otomatik olarak gösterilir. Metniniz yoksa, metni paletteki metin alanına yapıştırın.
1. Metin metin alanından olduktan sonra yerel cihaza yapıştırabilirsiniz.

   ![masına](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.
