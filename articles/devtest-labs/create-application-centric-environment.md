---
title: Uygulama merkezli ortam oluşturma-Azure
description: Bu makalede, Cloud Shell Colony ve Microsoft Azure ile uygulama merkezli bir ortam oluşturma gösterilmektedir.
ms.topic: how-to
ms.date: 11/26/2020
ms.openlocfilehash: 88244f268d5ed038e9bb7082d3d5cc1179e5ec4e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94918260"
---
# <a name="create-an-application-centric-environment"></a>Uygulama merkezli ortam oluşturma

[Quali 'Nın CloudShell Colony](https://azuremarketplace.microsoft.com/marketplace/apps/quali_systems.cloudshell_colony?tab=Overview) , Azure ve Kubernetes dahil olmak üzere bulut teknolojilerinde karmaşık uygulama merkezli ortamlar Için ölçekte altyapı Otomasyonu sunmaya yönelik bir SaaS platformudur. CloudShell Colony, geliştirici ekiplerinin değer akışı boyunca, tüm üretime kadar karmaşık uygulamalar dağıtmaya yardımcı olmak için Azure DevTest Labs tamamlar.

Bu makalede, CloudShell Colony ve Microsoft Azure ile uygulama merkezli bir ortam oluşturma gösterilmektedir.

## <a name="set-up-the-environment-with-cloudshell-colony-and-microsoft-azure"></a>CloudShell Colony ve Microsoft Azure ortam ayarlama

1. Ücretsiz [deneme sürümü için kaydolun.](https://azuremarketplace.microsoft.com/marketplace/apps/quali_systems.cloudshell_colony?tab=Overview)

    :::image type="content" source="./media/create-application-centric-environment/free-trial.png" alt-text="Ücretsiz deneme için kaydolun":::    
1. Azure hesabınızı bağlayın ([buradaki adımları görüntüleyin](https://colonysupport.quali.com/hc/articles/360008222234)).

    :::image type="content" source="./media/create-application-centric-environment/welcome.png" alt-text="Colony 'a hoş geldiniz":::     
1. Kullanıcıları alanınıza davet edin.
1. Bir YAML dosyası kullanarak ilk şema oluşturun ([buradaki adımları görüntüleyin](https://colonysupport.quali.com/hc/articles/360001680807-Steps-to-Developing-a-Blueprint)).
    1. GitHub veya Bitbucket 'daki şema deponuzu Colony 'a bağlayın.
    1. Temel olarak bir Colony örnek şema kullanın ve uygun şekilde değiştirin.

        :::image type="content" source="./media/create-application-centric-environment/performance-stress-tests.png" alt-text="Stres testleri":::    
    1. Şema dosyanızı başkalarının kullanması için yayımlayın.
1. Colony kullanarak uygulama ortamınızı bir korumalı alana başlatın.

    :::image type="content" source="./media/create-application-centric-environment/blueprints.png" alt-text="Colony kullanarak uygulama ortamınızı bir korumalı alana başlatın":::    

> [!NOTE]
> Ayrıca, Azure DevOps 'daki bir CI/CD iş akışının parçası olarak şema 'i tümleştirebilirsiniz ([buradaki adımları burada görebilirsiniz](https://colonysupport.quali.com/hc/articles/360008464234)).

:::image type="content" source="./media/create-application-centric-environment/devops-pipeline.png" alt-text="Azure DevOps işlem hattınızla bağlantı kurmak":::    

## <a name="next-steps"></a>Sonraki adımlar

[Colony tanıtımı isteme](https://info.quali.com/cloudshell-colony-demo-request)
