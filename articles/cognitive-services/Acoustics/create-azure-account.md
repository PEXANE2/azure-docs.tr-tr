---
title: Project Acoustics Azure Toplu Hesap Kurulumu
titlesuffix: Azure Cognitive Services
description: Bu nasıl yapılulu, Project Acoustics Unity ve Unreal motor tümleştirmeleriyle kullanılmak üzere bir Azure Toplu İş hesabı oluşturmayı açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 8f0f726d9d23f20698d3510ad674331ad74fb703
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68855089"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Project Acoustics Azure Toplu Hesap Kurulumu
Bu nasıl yapılulu, Project Acoustics Unity ve Unreal motor tümleştirmeleriyle kullanılmak üzere bir Azure Toplu İş hesabı oluşturmayı açıklar.

## <a name="get-an-azure-subscription"></a>Azure aboneliği edinme
Toplu İş ve Depolama hesapları ayarlamadan önce [Azure Aboneliği](https://azure.microsoft.com/free/) gereklidir. İlk kez kaydoluyorsanız, Azure birkaç zaman sınırlı ücretsiz kaynak ve 200 TL kredi sağlar.

## <a name="create-azure-batch-and-storage-accounts"></a>Azure Toplu İş ve depolama hesapları oluşturma
Ardından, Azure Toplu İş ve ilişkili Azure Depolama hesaplarınızı ayarlamak için [aşağıdaki yönergeleri](https://docs.microsoft.com/azure/batch/batch-account-create-portal) izleyin.

Hem Toplu İşlem hem de Depolama hesapları için varsayılan seçenekleri seçin:
  
  ![Varsayılan ayarları gösteren Azure Toplu yeni hesap seçeneklerinin ekran görüntüsü](media/new-batch-account-create.png)

  ![Varsayılan ayarları gösteren Azure Depolama yeni hesap seçeneklerinin ekran görüntüsü](media/batch-storage-account-create.png)

Azure'un hesapları dağıtması birkaç dakika sürer. Portalın sağ üst köşesinde bir tamamlama bildirimi arayın.
  
  ![Azure hesaplarının ekran görüntüsü dağıtılmış bildirim](media/batch-accounts-deploy-notification.png)

Hesaplarınız artık panonuzda görünür olmalıdır.
  
  ![Toplu İşlem ve Depolama hesabını gösteren Azure portal panosunun ekran görüntüsü](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Azure kimlik bilgileriyle akustik fırında UI'yi ayarlama
Panodaki Toplu Hesap bağlantısına tıklayın ve kimlik bilgilerinize erişmek için Toplu Hesap sayfasındaki **Anahtarlar** bağlantısına tıklayın.
  
  ![Vurgulanan Keys sayfasıyla birlikte Azure Toplu İş hesabının ekran görüntüsü](media/batch-access-keys.png)

  ![Erişim anahtarları ile Azure Toplu hesap anahtarları sayfasının ekran görüntüsü](media/batch-keys-info.png)

Azure Depolama hesabı kimlik bilgilerinize erişmek için sayfadaki **Depolama Hesabı** bağlantısını tıklayın.
  
  ![Erişim anahtarları ile Azure Depolama hesap anahtarları sayfasının ekran görüntüsü](media/storage-keys-info.png)

[Unity bake eklentisi](unity-baking.md) veya [Unreal fırın eklentisi](unreal-baking.md)bu kimlik bilgilerini girin.

## <a name="node-types-and-region-support"></a>Düğüm türleri ve bölge desteği
Project Acoustics, tüm Azure bölgelerinde desteklenmeyen Fsv2 ve H serisi bilgi işlem optimize edilmiş Azure VM düğümlerini gerektirir. Toplu Iş hesabınız için doğru yeri seçtiğinizden emin olmak için lütfen [bu tabloyu](https://azure.microsoft.com/global-infrastructure/services) kontrol edin.
![Bölgeye Göre Azure Sanal Makineleri gösteren ekran görüntüsü](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Kotanızı yükseltme
Azure Toplu İş hesapları, hesap oluşturmada 20 işlem çekirdeği sınırıyla birlikte verilir. Bu limiti daha hızlı pişirme süreleri için artırmak isteyebiliriz, çünkü akustik iş yükünüzü sahnenizdeki sonda noktalarının sayısına kadar birçok düğüme paralel hale getirebilirsiniz. Azure Toplu İşlem portalı sayfanızdaki **Kota** bağlantısına tıklayıp Kota Artırımı İsteği'ne tıklayarak kota **artışı**talep edebilirsiniz:

![Azure Kota sayfasının ekran görüntüsü](media/azure-quotas.png)

## <a name="next-steps"></a>Sonraki adımlar
* Project Acoustics eklentisini [Unity](unity-integration.md) veya [Unreal](unreal-integration.md) projenize entegre edin

