---
title: Adanmış bir SQL havuzu için Kullanıcı tanımlı geri yükleme noktası oluşturma
description: Adanmış SQL havuzu için geri yükleme noktası oluşturma.
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
ms.openlocfilehash: c0835fb48d00fe5277732f34fd6b0de1448f6a78
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94332163"
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

