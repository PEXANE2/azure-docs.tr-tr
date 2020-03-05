---
title: Öğretici-Azure 'da bir Jenkins sunucusu oluşturma
description: Bu öğreticide Jenkins 'i Azure Linux sanal makinesine Jenkins çözüm şablonundan yüklersiniz ve örnek bir Java uygulaması oluşturacaksınız.
keywords: jenkins, azure, devops, portal, sanal makine, çözüm şablonu
ms.topic: tutorial
ms.date: 03/03/2020
ms.openlocfilehash: 3fa8dcbcc0edcc987fa9c4e76fbdb9d7a2f3396c
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274903"
---
# <a name="tutorial-create-a-jenkins-server-on-an-azure-linux-vm"></a>Öğretici: Azure Linux VM 'de bir Jenkins sunucusu oluşturma 

Bu öğreticide, Azure ile çalışmak üzere yapılandırılmış araçlar ve eklentiler ile bir Ubuntu Linux VM 'ye [Jenkins](https://jenkins.io) 'nin nasıl yükleneceği gösterilmektedir. İşlemi tamamladığınızda, Azure‘da çalışan bir Jenkins sunucusuna sahip ve [GitHub](https://github.com)’dan örnek bir Java uygulaması oluşturmuş olursunuz.

> [!div class="checklist"]
> * Azure 'da bir Jenkins sunucusu yükleyip yapılandırma
> * Bir SSH tüneli kullanarak Jenkins konsoluna erişin
> * Freestyle projesi oluşturma
> * Kodu derleyin ve örnek uygulamayı paketleyin

[!INCLUDE [jenkins-install-solution-template-include](../../includes/jenkins-install-solution-template-steps.md)]