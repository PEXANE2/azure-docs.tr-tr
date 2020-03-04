---
title: 'Hızlı başlangıç: Azure portal kullanarak bir örnek oluşturma'
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti 'nin bir örneğini oluşturmak için Azure portal kullanın.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: quickstart
ms.date: 01/08/2020
ms.openlocfilehash: efbf7d9db91b8c23151ca4337bd4d58f814a7096
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78254970"
---
# <a name="quickstart-create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure Veritabanı Geçiş Hizmeti örneği oluşturma

Bu hızlı başlangıçta, Azure veritabanı geçiş hizmeti 'nin bir örneğini oluşturmak için Azure portal kullanırsınız.  Hizmeti oluşturduktan sonra, şirket içi SQL Server verileri Azure SQL veritabanı 'na geçirmek için kullanabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Web tarayıcınızı açın, [Microsoft Azure portalına gidin](https://portal.azure.com/) ve kimlik bilgilerinizi girerek portalda oturum açın.

Varsayılan görünüm hizmet panonuzu içerir.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

İlk Veritabanı Geçiş Hizmeti örneğinizi oluşturmadan önce Microsoft.DataMigration kaynak sağlayıcısını kaydedin.

1. Azure portalında, **Tüm hizmetler**’i seçin ve sonra **Abonelikler**’i seçin.

2. Azure veritabanı geçiş hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **kaynak sağlayıcıları**' nı seçin.

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-an-instance-of-the-service"></a>Hizmetin bir örneğini oluşturma

1. Azure veritabanı geçiş hizmeti 'nin bir örneğini oluşturmak için +**kaynak oluştur** ' u seçin.

2. "Geçiş" için marketi arayın, **Azure Veritabanı Geçiş Hizmeti**’ni seçin ve **Azure Veritabanı Geçiş Hizmeti**  ekranında, **Oluştur**’u seçin.

3. **Geçiş Hizmeti Oluştur** ekranında:

    - Azure veritabanı geçiş hizmeti örneğinizi tanımlamak için hatırlayabileceğiniz ve benzersiz bir **hizmet adı** seçin.
    - Örneği oluşturmak istediğiniz Azure **Aboneliğini** seçin.
    - Yeni bir **Kaynak Grubu** seçin ya da yeni bir tane oluşturun.
    - Kaynak veya hedef sunucunuza en yakın **Konum**’u seçin.
    - Var olan bir **sanal ağı** seçin veya bir tane oluşturun.

        Sanal ağ, kaynak veritabanı ve hedef ortama erişimi olan Azure veritabanı geçiş hizmeti sağlar.

        Azure portal sanal ağ oluşturma hakkında daha fazla bilgi için [Azure Portal kullanarak sanal ağ oluşturma](https://aka.ms/vnet)makalesine bakın.

    - **Fiyatlandırma katmanı** için Temel: 1 Sanal Çekirdek seçeneğini belirleyin.

        ![Geçiş hizmetini oluşturma](media/quickstart-create-data-migration-service-portal/dms-create-service1.png)

4. **Oluştur**’u seçin.

    Birkaç dakika sonra Azure veritabanı geçiş hizmeti örneğiniz oluşturulup kullanıma hazırlanmaya hazırlandıktan sonra. Azure veritabanı geçiş hizmeti, aşağıdaki görüntüde gösterildiği gibi görüntülenir:

    ![Geçiş hizmeti oluşturuldu](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Azure kaynak grubunu](../azure-resource-manager/management/overview.md) silerek bu Hızlı Başlangıç kılavuzunda oluşturduğunuz kaynakları temizleyebilirsiniz. Kaynak grubunu silmek için, oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğine gidin. **Kaynak grubu** adını seçin ve **Kaynak grubunu sil** seçeneğini belirleyin. Bu eylem kaynak grubundaki tüm varlıkları ve kaynak grubunu siler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi SQL Server’dan Azure SQL Veritabanı’na geçiş](tutorial-sql-server-to-azure-sql.md)