---
title: Altyapı Çift şifreleme-Azure portal-MySQL için Azure veritabanı
description: MySQL için Azure veritabanınız için altyapı çift şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 4623791e19ec83446b2978105286b71e6ff1eb1c
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86034906"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için altyapı çift şifrelemesi

MySQL için Azure veritabanınız için altyapı ikili şifrelemesini ayarlama ve yönetme hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.

## <a name="create-an-azure-database-for-mysql-server-with-infrastructure-double-encryption---portal"></a>Altyapı çift şifrelemesi ile MySQL için Azure veritabanı sunucusu oluşturma-Portal

Azure portal altyapı çift şifrelemesi ile bir MySQL için Azure veritabanı sunucusu oluşturmak için aşağıdaki adımları izleyin:

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** (+) seçeneğini belirleyin.

2. **Veritabanları**  >  **MySQL için Azure veritabanı**' nı seçin. Ayrıca, arama kutusuna **MySQL** girerek hizmeti bulabilirsiniz.

   ![MySQL için Azure Veritabanı seçeneği](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Sunucunun temel bilgilerini sağlayın. Parametreyi ayarlamak için **ek ayarlar** ' ı seçin ve **altyapı Çift şifreleme** onay kutusunu etkin yapın.

    ![MySQL için Azure veritabanı seçimleri](./media/howto-double-encryption/infrastructure-encryption-selected.png)

4. Sunucuyu sağlamak için **gözden geçir + oluştur** ' u seçin.

    ![MySQL için Azure veritabanı Özeti](./media/howto-double-encryption/infrastructure-encryption-summary.png)

5. Sunucu oluşturulduktan sonra, **veri şifreleme** sunucusu dikey penceresinde durumu denetleyerek altyapıyı çift şifrelemeyi doğrulayabilirsiniz.

    ![MySQL için Azure veritabanı doğrulaması](./media/howto-double-encryption/infrastructure-encryption-validation.png)

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
