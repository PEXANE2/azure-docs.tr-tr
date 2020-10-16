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
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90709056"
---
# <a name="delete-replications"></a>Çoğaltmaları silme

Birim çoğaltmasını silerek kaynak ve hedef birimler arasındaki çoğaltma bağlantısını sonlandırabilirsiniz. Silme işlemini kaynak ya da hedef birimden gerçekleştirebilirsiniz. Silme işlemi yalnızca çoğaltma için Yetkilendirmeyi kaldırır; kaynağı veya hedef birimi kaldırmaz. 

## <a name="steps"></a>Adımlar

1. Birim çoğaltmasını silmek için kaynaktan veya hedef birimden **çoğaltma** ' yı seçin.  

2. **Sil**'e tıklayın.    

3. **Evet** yazarak silmeyi onaylayın ve **Sil**' i tıklatın.   

    ![Çoğaltmayı Sil](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="next-steps"></a>Sonraki adımlar  

* [Bölgeler arası çoğaltma](cross-region-replication-introduction.md)
* [Bölgeler arası çoğaltmayı kullanma gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md)
* [Çoğaltma ilişkisinin uygunluk durumunu görüntüleme](cross-region-replication-display-health-status.md)
* [Bölgeler arası çoğaltma sorunlarını giderme](troubleshoot-cross-region-replication.md)

