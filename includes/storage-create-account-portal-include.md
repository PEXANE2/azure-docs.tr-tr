---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ea8ed75bf91850abb95ebe983923989375c0fcf5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76759866"
---
Azure portalında genel amaçlı v2 bir depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalı menüsünde **Tüm hizmetler**’i seçin. Kaynak listesinde **Depolama Hesapları** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Depolama Hesapları**’nı seçin.
2. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
3. Depolama hesabının oluşturulacağı aboneliği seçin.
4. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Aşağıdaki görüntüde gösterildiği gibi yeni kaynak grubunuz için bir ad girin.

    ![Portalda kaynak grubu oluşturmayı gösteren ekran görüntüsü](./media/storage-create-account-portal-include/create-resource-group-for-storage.png)

5. Ardından, depolama hesabınız için bir ad girin. Seçtiğiniz ad Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 - 24 karakter uzunluğunda olmalıdır ve yalnızca rakam ve küçük harf içerebilir.
6. Depolama hesabınız için bir konum seçin veya varsayılan konumu kullanın.
7. Bu alanları varsayılan değerlerine ayarlanmış olarak bırakın:

   |Alan  |Değer  |
   |---------|---------|
   |Dağıtım modeli     |Resource Manager         |
   |Performans     |Standart         |
   |Hesap türü     |StorageV2 (genel amaçlı v2)         |
   |Çoğaltma     |Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)         |
   |Erişim katmanı     |Sık Erişimli         |

8. [Azure Veri Gölü Depolama'yı](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmayı planlıyorsanız, **Gelişmiş** sekmesini seçin ve ardından **Hiyerarşik ad alanını** **Etkin**olarak ayarlayın.
9. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.
10. **Oluştur'u**seçin.

Depolama hesaplarının türleri ve diğer depolama hesabı ayarları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview). Kaynak grupları hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)bilgisine bakın. 
