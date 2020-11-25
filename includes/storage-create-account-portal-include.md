---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/25/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: f8959904482bda8dc26e4df626377027e1ab4bd1
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96005562"
---
Azure portalında genel amaçlı v2 bir depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalı menüsünde **Tüm hizmetler**'i seçin. Kaynak listesinde **Depolama Hesapları** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Depolama hesapları**' nı seçin.
1. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
1. **Temel bilgiler** sekmesinde, depolama hesabının oluşturulacağı aboneliği seçin.
1. **Kaynak grubu** alanında, istediğiniz kaynak grubunu seçin veya yeni bir kaynak grubu oluşturun.  Azure Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../articles/azure-resource-manager/management/overview.md).
1. Ardından, depolama hesabınız için bir ad girin. Seçtiğiniz ad Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içerebilir.
1. Depolama hesabınız için bir konum seçin veya varsayılan konumu kullanın.
1. Bir performans katmanı seçin. Varsayılan katman *standarttır*.
1. **Hesap türü** alanını *Storage v2 (genel amaçlı v2)* olarak ayarlayın.
1. Depolama hesabının nasıl çoğaltılacağı belirtin. Varsayılan çoğaltma seçeneği *Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS)*' dir. Kullanılabilir çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. [Azure depolama artıklığı](../articles/storage/common/storage-redundancy.md).
1. Depolama hesabındaki bloblara yönelik erişim katmanını belirtin. Varsayılan katman çok *sıcak*. Blob erişim katmanları hakkında daha fazla bilgi için bkz. [Bloblar Için sık erişimli, seyrek erişimli ve arşiv erişim katmanları](../articles/storage/blobs/storage-blob-storage-tiers.md).
1. Azure Data Lake Storage kullanmak için, **Gelişmiş** sekmesini seçin ve ardından **hiyerarşik ad alanını** **etkin** olarak ayarlayın. Daha fazla bilgi için bkz. [Azure Data Lake Storage 2. giriş](../articles/storage/blobs/data-lake-storage-introduction.md)
1. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.
1. **Oluştur**’u seçin.

Aşağıdaki görüntüde yeni bir depolama hesabı için **temel bilgiler** sekmesindeki ayarlar gösterilmektedir:

:::image type="content" source="media/storage-create-account-portal-include/account-create-portal.png" alt-text="Azure portalında depolama hesabının nasıl oluşturulduğunu gösteren ekran görüntüsü":::