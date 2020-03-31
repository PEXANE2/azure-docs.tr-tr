---
title: Depolama güvenlik duvarı ayarlarını aşma
description: Azure HPC Önbelleğinde Bir Azure Blob depolama hedefi oluştururken depolama hesabı ağı güvenlik duvarı ayarı başarısızlığa neden olabilir. Bu makalede, bir yazılım düzeltmesi yerine gelene kadar sınırlama için geçici çözüm sağlar.
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 11/7/2019
ms.author: rohogue
ms.openlocfilehash: 6643662d498db8cbcffcb120a9ceabc46cfc04cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74174414"
---
# <a name="work-around-blob-storage-account-firewall-settings"></a>Blob depolama hesabı güvenlik duvarı ayarları geçici çözümü

Depolama hesabı güvenlik duvarlarında kullanılan belirli bir ayar Blob depolama hedef oluşturma başarısız neden olabilir. Azure HPC Önbellek ekibi bu sorun için bir yazılım düzeltmesi üzerinde çalışıyor, ancak bu makaledeki yönergeleri izleyerek bu sorunu çözebilirsiniz.

Yalnızca "seçili ağlardan" erişime izin veren güvenlik duvarı ayarı, önbelleğin Blob depolama hedefi oluşturmasını engelleyebilir. Bu yapılandırma, depolama hesabının **Güvenlik Duvarları ve sanal ağlar** ayarları sayfasındadır.

Sorun, önbellek hizmetinin müşteri ortamlarından ayrı bir gizli hizmet sanal ağı kullanmasıdır. Bu ağa depolama hesabınıza erişme yetkisi vermek mümkün değildir.

Blob depolama hedefi oluşturduğunuzda, önbellek hizmeti kapsayıcının boş olup olmadığını denetlemek için bu ağı kullanır. Güvenlik duvarı gizli ağdan erişime izin vermiyorsa, denetim başarısız olur ve depolama hedefi oluşturma başarısız olur.

Sorunu aşmak için, depolama hedefini oluştururken güvenlik duvarı ayarlarınızı geçici olarak değiştirin:

1. Depolama hesabı **Güvenlik Duvarları ve sanal ağlar** sayfasına gidin ve "Erişime izin ver" ayarını Tüm **ağlarla**değiştirin.
1. Azure HPC Önbelleğinizde Blob depolama hedefini oluşturun.
1. Depolama hedefi başarıyla oluşturulduktan sonra, hesabın güvenlik duvarı ayarını **Seçili ağlara**geri değiştirin.

Azure HPC Önbelleği, tamamlanan depolama hedefine erişmek için hizmet sanal ağını kullanmaz.

Bu geçici çözümle ilgili yardım için [Microsoft Service and Support'a başvurun.](hpc-cache-support-ticket.md)
