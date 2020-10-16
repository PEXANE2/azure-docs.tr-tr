---
title: Altyapı Çift şifreleme-Azure portal-MySQL için Azure veritabanı
description: MySQL için Azure veritabanınız için altyapı çift şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: eafad5edf9dcac5745986d09060baf7e4278762d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90903969"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için altyapı çift şifrelemesi

MySQL için Azure veritabanınız için altyapı ikili şifrelemesini ayarlama ve yönetme hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.

## <a name="create-an-azure-database-for-mysql-server-with-infrastructure-double-encryption---portal"></a>Altyapı çift şifrelemesi ile MySQL için Azure veritabanı sunucusu oluşturma-Portal

Azure portal altyapı çift şifrelemesi ile bir MySQL için Azure veritabanı sunucusu oluşturmak için aşağıdaki adımları izleyin:

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** (+) seçeneğini belirleyin.

2. **Veritabanları**  >  **MySQL için Azure veritabanı**' nı seçin. Ayrıca, arama kutusuna **MySQL** girerek hizmeti bulabilirsiniz.

   :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png" alt-text="MySQL için Azure Veritabanı seçeneği":::

3. Sunucunun temel bilgilerini sağlayın. Parametreyi ayarlamak için **ek ayarlar** ' ı seçin ve **altyapı Çift şifreleme** onay kutusunu etkin yapın.

    :::image type="content" source="./media/howto-double-encryption/infrastructure-encryption-selected.png" alt-text="MySQL için Azure Veritabanı seçeneği":::

4. Sunucuyu sağlamak için **gözden geçir + oluştur** ' u seçin.

    :::image type="content" source="./media/howto-double-encryption/infrastructure-encryption-summary.png" alt-text="MySQL için Azure Veritabanı seçeneği":::

5. Sunucu oluşturulduktan sonra, **veri şifreleme** sunucusu dikey penceresinde durumu denetleyerek altyapıyı çift şifrelemeyi doğrulayabilirsiniz.

    :::image type="content" source="./media/howto-double-encryption/infrastructure-encryption-validation.png" alt-text="MySQL için Azure Veritabanı seçeneği":::

## <a name="create-an-azure-database-for-mysql-server-with-infrastructure-double-encryption---cli"></a>Altyapı çift şifrelemesi ile MySQL için Azure veritabanı sunucusu oluşturma-CLı

CLı 'dan altyapı çift şifrelemesi ile bir MySQL için Azure veritabanı sunucusu oluşturmak için aşağıdaki adımları izleyin:

Bu örnek, konumunda adlı bir kaynak grubu oluşturur `myresourcegroup` `westus` .

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
Aşağıdaki örnekte, Batı ABD bölgesinde `myadmin` sunucu yöneticisi oturum açma adıyla `myresourcegroup` kaynak grubunuzda `mydemoserver` adlı bir MySQL 5.7 sunucusu oluşturulur. Bu, **2 sanal çekirdeğe**sahip bir **Gen 4** **genel amaçlı** sunucusudur. Bu, oluşturulan sunucu için altyapı çift şifrelemeyi de etkinleştirdi. `<server_admin_password>` değerini kendi değerinizle değiştirin.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 5.7 --infrastructure-encryption <Enabled/Disabled>
```

## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi edinmek için bkz. [MySQL Için Azure veritabanı veri altyapısı Çift şifreleme](concepts-Infrastructure-double-encryption.md).
