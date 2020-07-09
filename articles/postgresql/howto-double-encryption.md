---
title: Altyapı Çift şifreleme-Azure portal-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanınız için altyapı çift şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: 6612fe38adcd3c8002dd4a11122b5bb2e797a4dd
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102183"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için altyapı Çift şifreleme

PostgreSQL için Azure veritabanınız için altyapı ikili şifrelemesini ayarlama ve yönetme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---portal"></a>Altyapı çift şifrelemesi ile PostgreSQL için Azure veritabanı sunucusu oluşturma-Portal

Azure portal altyapı çift şifrelemesi ile bir MySQL için Azure veritabanı sunucusu oluşturmak için aşağıdaki adımları izleyin:

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** (+) seçeneğini belirleyin.

2. **Veritabanları**  >  **PostgreSQL için Azure veritabanı**' nı seçin. Hizmeti bulmak için arama kutusuna PostgreSQL ' i de girebilirsiniz. **Tek sunuculu** dağıtım seçeneği etkinleştirildi.

   ![Menüdeki "PostgreSQL için Azure veritabanı"](./media/quickstart-create-database-portal/1-create-database.png)

3. Sunucunun temel bilgilerini sağlayın. Parametreyi ayarlamak için **ek ayarlar** ' ı seçin ve **altyapı Çift şifreleme** onay kutusunu etkin yapın.

    ![PostgreSQL için Azure veritabanı seçimleri](./media/howto-infrastructure-double-encryption/infrastructure-encryption-selected.png)

4. Sunucuyu sağlamak için **gözden geçir + oluştur** ' u seçin.

    ![PostgreSQL için Azure veritabanı Özeti](./media/howto-infrastructure-double-encryption/infrastructure-encryption-summary.png)

5. Sunucu oluşturulduktan sonra, **veri şifreleme** sunucusu dikey penceresinde durumu denetleyerek altyapıyı çift şifrelemeyi doğrulayabilirsiniz.

    ![MySQL için Azure veritabanı doğrulaması](./media/howto-infrastructure-double-encryption/infrastructure-encryption-validation.png)

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---cli"></a>Altyapı çift şifrelemesi ile PostgreSQL için Azure veritabanı sunucusu oluşturma-CLı

CLı 'dan altyapı çift şifrelemesi ile bir MySQL için Azure veritabanı sunucusu oluşturmak için aşağıdaki adımları izleyin:

Bu örnek, konumunda adlı bir kaynak grubu oluşturur `myresourcegroup` `westus` .

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
Aşağıdaki örnek, `mydemoserver` `myresourcegroup` Sunucu Yöneticisi oturum açma bilgileriyle kaynak grubunuzda adlandırılmış Batı ABD bir PostgreSQL 11 sunucusu oluşturur `myadmin` . Bu, **2 sanal çekirdeğe**sahip bir **Gen 4** **genel amaçlı** sunucusudur. Bu, oluşturulan sunucu için altyapı çift şifrelemeyi de etkinleştirdi. `<server_admin_password>` değerini kendi değerinizle değiştirin.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 11 --infrastructure-encryption >Enabled/Disabled>
```

## <a name="next-steps"></a>Sonraki adımlar

Veri şifreleme hakkında daha fazla bilgi için bkz. [PostgreSQL Için Azure veritabanı veri altyapısı Çift şifreleme](concepts-Infrastructure-double-encryption.md).

