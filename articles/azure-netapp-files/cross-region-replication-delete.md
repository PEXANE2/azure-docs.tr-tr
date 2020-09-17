---
title: Azure NetApp Files çapraz bölge çoğaltması için çoğaltmaları silme | Microsoft Docs
description: Kaynak ve hedef birimler arasında artık gerekli olmayan bir çoğaltma bağlantısının nasıl silineceğini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: 0904ac36a9453e51dbb1efc50eee2b9bf3c669c7
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709056"
---
# <a name="delete-replications"></a>Çoğaltmaları Sil

Birim çoğaltmasını silerek kaynak ve hedef birimler arasındaki çoğaltma bağlantısını sonlandırabilirsiniz. Silme işlemini kaynak ya da hedef birimden gerçekleştirebilirsiniz. Silme işlemi yalnızca çoğaltma için Yetkilendirmeyi kaldırır; kaynağı veya hedef birimi kaldırmaz. 

## <a name="steps"></a>Adımlar

1. Birim çoğaltmasını silmek için kaynaktan veya hedef birimden **çoğaltma** ' yı seçin.  

2. **Sil**'e tıklayın.    

3. **Evet** yazarak silmeyi onaylayın ve **Sil**' i tıklatın.   

    ![Çoğaltmayı Sil](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="next-steps"></a>Sonraki adımlar  

* [Çapraz bölge çoğaltma](cross-region-replication-introduction.md)
* [Bölgeler arası çoğaltmayı kullanma gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md)
* [Çoğaltma ilişkisinin sistem durumunu görüntüle](cross-region-replication-display-health-status.md)
* [Bölgeler arası çoğaltma sorunlarını giderme](troubleshoot-cross-region-replication.md)

