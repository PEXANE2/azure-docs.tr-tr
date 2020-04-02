---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 57368331e83111836e13b6c3796cacad9816e318
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521278"
---
Azure portalından depolama hesabı erişim anahtarlarınızı veya bağlantı dizenizi görüntülemek ve kopyalamak için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Depolama hesabınızı bulun.
3. **Ayarlar** altında **Erişim anahtarları**'nı seçin. Hesap erişim anahtarlarınız ve her bir anahtar için tam bağlantı dizesi görüntülenir.
4. **key1** bölümünde **Anahtar** değerini bulun ve **Kopyala** düğmesine tıklayarak hesap anahtarını kopyalayın.
5. Alternatif olarak, tüm bağlantı dizesini kopyalayabilirsiniz. **key1** bölümünde **Bağlantı dizesi** değerini bulun ve **Kopyala** düğmesine tıklayarak bağlantı dizesini kopyalayın.

    ![Azure portalında erişim anahtarlarını nasıl görüntüleyinen ekran görüntüsü](media/storage-view-keys-include/portal-connection-string.png)

Azure Depolama'ya erişmek için her iki anahtarı da kullanabilirsiniz, ancak genel olarak ilk anahtarı kullanmak ve anahtarları döndürürken ikinci anahtarın kullanımını ayırmak iyi bir uygulamadır.

Bir hesabın erişim anahtarlarını görüntülemek veya okumak için, kullanıcının hizmet yöneticisi olması veya **Microsoft.Storage/storageAccounts/listkeys/action**içeren bir RBAC rolü atanması gerekir. Bu eylemi içeren bazı yerleşik RBAC rolleri **Sahibi,** **Katılımcısı**ve **Depolama Hesabı Anahtar Operatörü Hizmeti Rolü** rolleridir. Hizmet Yöneticisi rolü hakkında daha fazla bilgi için [Bkz. Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure REKLAM rolleri.](../articles/role-based-access-control/rbac-and-directory-admin-roles.md) Azure Depolama için yerleşik roller hakkında ayrıntılı bilgi için Azure [RBAC için Azure yerleşik rollerdeki](../articles/role-based-access-control/built-in-roles.md#storage) **Depolama** bölümüne bakın.
