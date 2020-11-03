---
title: Altyapı Çift şifreleme-Azure portal-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanınız için altyapı çift şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: ea486b534ac3e703849ddb3922d7c3a428dd076b
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93242237"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için altyapı Çift şifreleme

PostgreSQL için Azure veritabanınız için altyapı ikili şifrelemesini ayarlama ve yönetme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---portal"></a>Altyapı çift şifrelemesi ile PostgreSQL için Azure veritabanı sunucusu oluşturma-Portal

Azure portal altyapı çift şifrelemesi ile bir MySQL için Azure veritabanı sunucusu oluşturmak için aşağıdaki adımları izleyin:

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** (+) seçeneğini belirleyin.

2. **Veritabanları**  >  **PostgreSQL için Azure veritabanı** ' nı seçin. Hizmeti bulmak için arama kutusuna PostgreSQL ' i de girebilirsiniz. **Tek sunuculu** dağıtım seçeneği etkinleştirildi.

   :::image type="content" source="./media/quickstart-create-database-portal/1-create-database.png" alt-text="Menüdeki PostgreSQL için Azure veritabanı":::

3. Sunucunun temel bilgilerini sağlayın. Parametreyi ayarlamak için **ek ayarlar** ' ı seçin ve **altyapı Çift şifreleme** onay kutusunu etkin yapın.

    :::image type="content" source="./media/howto-infrastructure-double-encryption/infrastructure-encryption-selected.png" alt-text="PostgreSQL için Azure veritabanı seçimleri":::

4. Sunucuyu sağlamak için **gözden geçir + oluştur** ' u seçin.

    :::image type="content" source="./media/howto-infrastructure-double-encryption/infrastructure-encryption-summary.png" alt-text="PostgreSQL için Azure veritabanı Özeti":::

5. Sunucu oluşturulduktan sonra, **veri şifreleme** sunucusu dikey penceresinde durumu denetleyerek altyapıyı çift şifrelemeyi doğrulayabilirsiniz.

    :::image type="content" source="./media/howto-infrastructure-double-encryption/infrastructure-encryption-validation.png" alt-text="MySQL için Azure veritabanı doğrulaması":::

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---cli"></a>Altyapı çift şifrelemesi ile PostgreSQL için Azure veritabanı sunucusu oluşturma-CLı

CLı 'dan altyapı çift şifrelemesi ile bir MySQL için Azure veritabanı sunucusu oluşturmak için aşağıdaki adımları izleyin:

Bu örnek, konumunda adlı bir kaynak grubu oluşturur `myresourcegroup` `westus` .

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
Aşağıdaki örnek, `mydemoserver` `myresourcegroup` Sunucu Yöneticisi oturum açma bilgileriyle kaynak grubunuzda adlandırılmış Batı ABD bir PostgreSQL 11 sunucusu oluşturur `myadmin` . Bu, **2 sanal çekirdeğe** sahip bir **Gen 4** **genel amaçlı** sunucusudur. Bu, oluşturulan sunucu için altyapı çift şifrelemeyi de etkinleştirdi. `<server_admin_password>` değerini kendi değerinizle değiştirin.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 11 --infrastructure-encryption >Enabled/Disabled>
```

## <a name="next-steps"></a>Sonraki adımlar

Veri şifreleme hakkında daha fazla bilgi için bkz. [PostgreSQL Için Azure veritabanı veri altyapısı Çift şifreleme](concepts-Infrastructure-double-encryption.md).

