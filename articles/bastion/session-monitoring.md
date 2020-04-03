---
title: Azure Bastion oturum izleme ve yönetimi | Microsoft Dokümanlar
description: Bu makalede, devam eden bir oturumu nasıl seçip zorla bağlantıyı kesecek veya silebilirsiniz öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: 6bf80be4868295145fa79ae29d5322181b6131d1
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619189"
---
# <a name="session-monitoring-and-management-for-azure-bastion"></a>Azure Bastion için oturum izleme ve yönetim

Bastion hizmeti sanal ağınızda sağlanıp dağıtıldıktan sonra, bu sanal ağdaki herhangi bir VM'ye sorunsuz bir şekilde bağlanmak için bu hizmeti kullanabilirsiniz. Kullanıcılar iş yüklerine bağlandıkça, Azure Bastion uzak oturumları izlemek ve hızlı yönetim eylemleri yapmak için kullanılabilir. Azure Bastion oturum izleme, hangi kullanıcıların hangi VM'lere bağlı olduğunu görüntülemenizi sağlar. Kullanıcının bağlı olduğu IP'yi, ne kadar süredir bağlı olduklarını ve ne zaman bağlandıklarını gösterir. Oturum yönetimi deneyimi, kullanıcının devam eden oturumla bağlantısını kesmek için devam eden bir oturum seçmenize ve oturumu zorla kesmenize veya silmenize olanak tanır.

## <a name="monitor-remote-sessions"></a><a name="monitor"></a>Uzak oturumları izleme

1. Azure [portalında](https://portal.azure.com)Azure Bastion kaynağınıza gidin ve Azure Bastion sayfasından **Oturumlar'ı** seçin.

   ![Oturum](./media/session-monitoring/sessions.png)
2. **Oturumlar** sayfasında, devam eden uzak oturumları sağ tarafta görebilirsiniz.

   ![oturumu görüntüleme](./media/session-monitoring/view-session.png)
3. Güncelleştirilmiş uzak oturumlistesini görmek için **Yenile'yi** seçin. Yenile'yi seçtiğinizde, Azure Bastion en son izleme bilgilerini getirir ve portalda yeniler.

   ![refresh](./media/session-monitoring/refresh.png)

>[!IMPORTANT]
> Oturum izlemenin çalışması için Ağ Geçidi Yöneticisi'nden gelen trafik için bağlantı noktası 4443'ü etkinleştirin.
>

## <a name="delete-or-force-disconnect-an-ongoing-remote-session"></a><a name="view"></a>Devam eden bir uzak oturumu silme veya zorla kesme

Bir oturum(lar) kümesi seçebilir ve bunları zorla kesebilirsiniz. Aşağıdaki adımlar, uzak oturumları nasıl sildiğinizi gösterir:

1. Azure Bastion kaynağınıza gidin ve Azure Bastion sayfasından **Oturumlar'ı** seçin.

   ![navigate](./media/session-monitoring/navigate.png)
2. Oturumları seçtikten sonra, uzak oturumların listesini görürsünüz.

   ![liste oturumları](./media/session-monitoring/list.png)
3. Belirli bir uzak oturumu seçin, ardından oturum satırının sağ ucundaki üç elipsi seçin ve sonra **Sil'i**seçin.

   ![delete](./media/session-monitoring/delete.png)
4. Sil'i seçtiğinizde, uzak oturumun bağlantısı kesilir ve kullanıcıya uzak oturumda "Bağlantınız kesildi" iletisi gösterilir.

   ![Bağlantıyı kes](./media/session-monitoring/disconnect.png)

## <a name="next-steps"></a>Sonraki adımlar

[Bastion SSS'yi](bastion-faq.md)okuyun.