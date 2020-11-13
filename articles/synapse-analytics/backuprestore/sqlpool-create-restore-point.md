---
title: Adanmış bir SQL havuzu için Kullanıcı tanımlı geri yükleme noktası oluşturma
description: Azure SYNAPSE Analytics 'te adanmış SQL havuzu için Kullanıcı tanımlı geri yükleme noktası oluşturmak üzere Azure portal nasıl kullanacağınızı öğrenin.
services: synapse-analytics
author: joannapea
manager: igorstan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 10/29/2020
ms.author: joanpo
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 21fd20100095040fda9f72b00e17147ff560fbca
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94579547"
---
# <a name="user-defined-restore-points"></a>Kullanıcı tanımlı geri yükleme noktaları

Bu makalede, Azure portal kullanarak Azure SYNAPSE Analytics 'te adanmış bir SQL havuzu için yeni bir Kullanıcı tanımlı geri yükleme noktası oluşturmayı öğreneceksiniz.

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Azure portal aracılığıyla Kullanıcı tanımlı geri yükleme noktaları oluşturma

Kullanıcı tanımlı geri yükleme noktaları, Azure portal aracılığıyla da oluşturulabilir.

1. [Azure Portal](https://portal.azure.com/) hesabınızda oturum açın.

2. İçin geri yükleme noktası oluşturmak istediğiniz adanmış SQL havuzuna gidin.

3. Sol bölmeden **genel bakış** ' ı seçin, **+ Yeni geri yükleme noktası** ' nı seçin. Yeni geri yükleme noktası düğmesi etkinleştirilmemişse, adanmış SQL havuzunun duraklatılmadığından emin olun.

    ![Yeni geri yükleme noktası](../media/sql-pools/create-sqlpool-restore-point-01.png)

4. Kullanıcı tanımlı geri yükleme noktanız için bir ad belirtin ve **Uygula** ' ya tıklayın. Kullanıcı tanımlı geri yükleme noktalarının varsayılan saklama süresi yedi gündür.

    ![Geri yükleme noktasının adı](../media/sql-pools/create-sqlpool-restore-point-02.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Mevcut ayrılmış bir SQL havuzunu geri yükleme](restore-sql-pool.md)

