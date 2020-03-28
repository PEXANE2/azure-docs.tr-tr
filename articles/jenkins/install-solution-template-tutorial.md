---
title: Öğretici - Azure'da Jenkins sunucusu oluşturma
description: Bu eğitimde, Jenkins çözüm şablonundan bir Azure Linux sanal makinesine Jenkins yükler ve örnek bir Java uygulaması oluşturursunuz.
keywords: jenkins, azure, devops, portal, sanal makine, çözüm şablonu
ms.topic: tutorial
ms.date: 03/03/2020
ms.openlocfilehash: 3fa8dcbcc0edcc987fa9c4e76fbdb9d7a2f3396c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78274903"
---
# <a name="tutorial-create-a-jenkins-server-on-an-azure-linux-vm"></a>Öğretici: Azure Linux VM'de Jenkins sunucusu oluşturma 

Bu öğretici, [Jenkins'in](https://jenkins.io) Azure ile çalışacak şekilde yapılandırılan araçlar ve eklentilerle bir Ubuntu Linux VM'ye nasıl yükleyeceğimi gösterir. İşlemi tamamladığınızda, Azure‘da çalışan bir Jenkins sunucusuna sahip ve [GitHub](https://github.com)’dan örnek bir Java uygulaması oluşturmuş olursunuz.

> [!div class="checklist"]
> * Azure'da bir Jenkins sunucusu yükleme ve yapılandırma
> * SSH tüneli ni kullanarak Jenkins konsoluna erişin
> * Serbest stil projesi oluşturma
> * Kodu derle ve örnek uygulamayı paketle

[!INCLUDE [jenkins-install-solution-template-include](../../includes/jenkins-install-solution-template-steps.md)]