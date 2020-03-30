---
title: 'Hızlı başlatma: Azure portalını kullanarak bir örnek oluşturma'
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'nin bir örneğini oluşturmak için Azure portalını kullanın.
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78254970"
---
# <a name="quickstart-create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure Veritabanı Geçiş Hizmeti örneği oluşturma

Bu Quickstart'ta, Azure Veritabanı Geçiş Hizmeti'nin bir örneğini oluşturmak için Azure portalını kullanırsınız.  Hizmeti oluşturduktan sonra, verileri SQL Server'dan şirket içinde Azure SQL Veritabanı'na geçirmek için kullanabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Web tarayıcınızı açın, [Microsoft Azure portalına gidin](https://portal.azure.com/) ve kimlik bilgilerinizi girerek portalda oturum açın.

Varsayılan görünüm hizmet panonuzu içerir.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

İlk Veritabanı Geçiş Hizmeti örneğinizi oluşturmadan önce Microsoft.DataMigration kaynak sağlayıcısını kaydedin.

1. Azure portalında, **Tüm hizmetler**’i seçin ve sonra **Abonelikler**’i seçin.

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-an-instance-of-the-service"></a>Hizmetin bir örneğini oluşturma

1. Azure Veritabanı Geçiş Hizmeti'nin bir örneğini oluşturmak için kaynak**oluşturun** + seçin.

2. "Geçiş" için marketi arayın, **Azure Veritabanı Geçiş Hizmeti**’ni seçin ve **Azure Veritabanı Geçiş Hizmeti ** ekranında, **Oluştur**’u seçin.

3. **Geçiş Hizmeti Oluştur** ekranında:

    - Azure Veritabanı Geçiş Hizmeti örneğini tanımlamak için unutulmaz ve benzersiz bir **Hizmet Adı** seçin.
    - Örneği oluşturmak istediğiniz Azure **Aboneliğini** seçin.
    - Yeni bir **Kaynak Grubu** seçin ya da yeni bir tane oluşturun.
    - Kaynak veya hedef sunucunuza en yakın **Konum**’u seçin.
    - Varolan bir **Sanal ağ** seçin veya bir ağ oluşturun.

        Sanal ağ, kaynak veritabanına ve hedef ortama erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

        Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/vnet)bakın.

    - **Fiyatlandırma katmanı** için Temel: 1 Sanal Çekirdek seçeneğini belirleyin.

        ![Geçiş hizmetini oluşturma](media/quickstart-create-data-migration-service-portal/dms-create-service1.png)

4. **Oluştur'u**seçin.

    Birkaç dakika sonra Azure Veritabanı Geçişi hizmetini örnek alır ve kullanıma hazır olarak oluşturulur. Azure Veritabanı Geçiş Hizmeti aşağıdaki resimde gösterildiği gibi görüntüler:

    ![Geçiş hizmeti oluşturuldu](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Azure kaynak grubunu](../azure-resource-manager/management/overview.md) silerek bu Hızlı Başlangıç kılavuzunda oluşturduğunuz kaynakları temizleyebilirsiniz. Kaynak grubunu silmek için, oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğine gidin. **Kaynak grubu** adını seçin ve **Kaynak grubunu sil** seçeneğini belirleyin. Bu eylem kaynak grubundaki tüm varlıkları ve kaynak grubunu siler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi SQL Server’dan Azure SQL Veritabanı’na geçiş](tutorial-sql-server-to-azure-sql.md)