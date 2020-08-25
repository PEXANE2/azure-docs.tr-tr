---
title: 'Hızlı başlangıç: sunucu oluşturma-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu'
description: Bu hızlı başlangıç kılavuzunda, Azure portal kullanarak bir PostgreSQL için Azure veritabanı sunucusu oluşturacaksınız ve yönetebilirsiniz.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/27/2020
ms.openlocfilehash: 9b3e1916301d6346defd6ebeab2f3784882f2d5f
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88798865"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-server-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak PostgreSQL için Azure veritabanı sunucusu oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız, yönetilen bir hizmettir. Bu hızlı başlangıçta, Azure portal kullanarak yaklaşık beş dakika içinde PostgreSQL için tek bir Azure veritabanı sunucusu oluşturma işlemlerinin nasıl yapılacağı gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Web tarayıcınızı açın ve [portala](https://portal.azure.com/) gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

PostgreSQL için Azure Veritabanı sunucusu, yapılandırılmış bir dizi [işlem ve depolama kaynağı](./concepts-pricing-tiers.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/management/overview.md) içinde oluşturulur.

PostgreSQL için Azure veritabanı sunucusu oluşturmak için:

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** ' u seçin.

2. **Veritabanları**  >  **PostgreSQL için Azure veritabanı**' nı seçin.

   > [!div class="mx-imgBorder"]
   > ![Menüdeki "PostgreSQL için Azure veritabanı" gösteren ekran görüntüsü.](./media/quickstart-create-database-portal/1-create-database.png)

3. **Tek sunuculu** dağıtım seçeneğini belirleyin.

   > [!div class="mx-imgBorder"]
   > ![PostgreSQL için Azure veritabanı için tek sunuculu dağıtım seçeneğini seçme ekran görüntüsü.](./media/quickstart-create-database-portal/select-deployment-option.png)

4. **Temel** bilgiler formunu aşağıdaki bilgilerle doldurun.

   > [!div class="mx-imgBorder"]
   > ![Tek bir sunucu oluşturmaya yönelik temel bilgiler sekmesini gösteren ekran görüntüsü.](./media/quickstart-create-database-portal/create-basics.png)

   Ayar|Önerilen değer|Açıklama
   ---|---|---
   Abonelik|Aboneliğinizin adı|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden fazla aboneliğiniz varsa kaynağın faturalandırıldığı aboneliği seçin.
   Kaynak grubu|*myresourcegroup*| Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
   Sunucu adı |*mydemoserver*|PostgreSQL için Azure Veritabanı sunucunuzu tanıtan benzersiz bir ad. *Postgres.Database.Azure.com* etki alanı adı, sağladığınız sunucu adına eklenir. Sunucunuz yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ile 63 arasında karakter içermelidir.
   Veri kaynağı | **Hiçbiri** | Sıfırdan yeni bir sunucu oluşturmak için **hiçbiri** ' ni seçin. (Mevcut bir PostgreSQL için Azure veritabanı sunucusunun coğrafi yedeklemesinden bir sunucu oluşturuyorsanız **yedekleme** ' yi seçersiniz.)
   Yönetici Kullanıcı adı |*myadmin*| Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınız. Yönetici oturum açma adı **azure_superuser**, **azure_pg_admin**, **yönetici**, **yönetici**, **kök**, **Konuk**veya **ortak**olamaz. **PG_** ile başlayamaz.
   Parola |Parolanız| Sunucu yönetici hesabı için yeni bir parola. Aşağıdaki kategorilerden üçünden 8 ila 128 karakter içermesi gerekir: Ingilizce büyük harfler, Ingilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (örneğin,!, $, #,%).
   Konum|Kullanıcılarınıza en yakın bölge| Kullanıcılarınız için en yakın olan konum.
   Sürüm|En son ana sürüm| Belirli gereksinimleriniz olmadığı sürece, en son PostgreSQL ana sürümü.
   İşlem + depolama | **Genel Amaçlı**, **5. Nesil**, **2 sanal çekirdek**, **5 GB**, **7 gün**, **Coğrafi Olarak Yedekli** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Sunucuyu Yapılandır**' ı seçin. Ardından uygun fiyatlandırma katmanını seçin. Daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/postgresql/server/). Coğrafi olarak yedekli depolamada sunucu yedeklemelerinizi etkinleştirmek için **yedek artıklığı seçeneklerinde** **coğrafi olarak yedekli** ' ı seçin. **Tamam**’ı seçin.

   > [!NOTE]
   > Hafif işlem ve g/ç iş yükünüz için yeterli ise temel fiyatlandırma katmanını kullanmayı düşünün. Temel fiyatlandırma katmanında oluşturulan sunucuların daha sonra Genel Amaçlı veya bellek için Iyileştirilmiş olarak ölçeklenemeyeceğini unutmayın. 
   
5. Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. Sunucuyu sağlamak için **Oluştur**’u seçin. Bu işlem birkaç dakika sürebilir.

6. Araç çubuğunda, dağıtım sürecini izlemek için **Bildirimler** simgesini (zil) seçin. Dağıtım tamamlandıktan sonra, sunucunun **genel bakış** sayfasını açmak Için **Kaynağa Git** ' i seçin.

Boş bir veritabanı, **Postgres**oluşturulur. Ayrıca, yönetilen hizmet işlemlerini Kullanıcı eylemlerinden ayırmak için kullanılan bir **azure_maintenance** veritabanı bulacaksınız. **Azure_maintenance** veritabanına erişemezsiniz.

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma
Varsayılan olarak, oluşturduğunuz sunucuya herkese açık bir şekilde erişilebilir. IP adresiniz için izinler vermeniz gerekir. Azure portal sunucu kaynağınız ' ne gidin ve sunucu kaynağınız için sol taraftaki menüden **bağlantı güvenliği** ' ni seçin. Kaynağınızın nasıl bulunacağını bilmiyorsanız bkz. [kaynakları açma](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resources-portal#open-resources).

> [!div class="mx-imgBorder"]
> ![Bağlantı güvenliği için güvenlik duvarı kurallarını gösteren ekran görüntüsü.](./media/quickstart-create-database-portal/add-current-ip-firewall.png)
  
**Geçerli ISTEMCI IP adresi ekle**' yi seçin ve ardından **Kaydet**' i seçin. Daha fazla IP adresi ekleyebilir veya bu IP adreslerinden sunucunuza bağlanmak için bir IP aralığı sağlayabilirsiniz. Daha fazla bilgi için bkz. [PostgreSQL Için Azure veritabanı 'Nda güvenlik duvarı kuralları](./concepts-firewall-rules.md).
   
> [!NOTE]
> Bağlantı sorunlarından kaçınmak için ağınızın 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verdiğinden emin olun. PostgreSQL için Azure veritabanı bu bağlantı noktasını kullanır.  

## <a name="connect-to-azure-database-for-postgresql-server-by-using-psql"></a>Psql kullanarak PostgreSQL için Azure veritabanı sunucusuna bağlanma

Popüler PostgreSQL istemcileri olan [psql](http://postgresguide.com/utilities/psql.html) veya [pgAdmin](https://www.pgadmin.org/docs/pgadmin4/latest/connecting.html)' i kullanabilirsiniz. Bu hızlı başlangıçta, Azure portal içindeki [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) psql kullanarak bağlanacağız.

1. Sunucunuzun **genel bakış** bölümünden yeni oluşturduğunuz sunucu için sunucu adı, Sunucu Yöneticisi oturum açma adı, parola ve abonelik kimliğini bir yere göz atın.

2. Sol üst taraftaki simgeyi seçerek portalda Azure Cloud Shell açın.

   > [!NOTE]
   > Cloud Shell ilk kez açarsanız, bir kaynak grubu ve depolama hesabı oluşturmak için bir istem görürsünüz. Bu bir kerelik bir adımdır ve tüm oturumlar için otomatik olarak eklenir. 

   > [!div class="mx-imgBorder"]
   > ![Azure Cloud Shell açma için sunucu bilgilerini ve simgesini gösteren ekran görüntüsü.](media/quickstart-create-database-portal/use-in-cloud-shell.png)

3. Azure Cloud Shell terminalinde aşağıdaki komutu çalıştırın. Değerleri gerçek sunucu adı ve Yönetici Kullanıcı oturum açma adınızla değiştirin. Yönetici kullanıcıyla boş veritabanı **Postgres** 'yi şu biçimde kullanın: `<admin-username>@<servername>` .

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```
 
   Bu deneyim Cloud Shell terminalinde nasıl görünür:
   
   ```bash
    Requesting a Cloud Shell.Succeeded.
    Connecting terminal...

    Welcome to Azure Cloud Shell
 
    Type "az" to use Azure CLI
    Type "help" to learn about Cloud Shell

    user@Azure:~$psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
    Password for user myadmin@mydemoserver.postgres.database.azure.com:
    psql (12.2 (Ubuntu 12.2-2.pgdg16.04+1), server 11.6)
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
    Type "help" for help.

    postgres=>
    ```
4. Aynı Azure Cloud Shell terminalinde, **Konuk**adlı bir veritabanı oluşturun.

   ```bash
   postgres=> CREATE DATABASE guest;
   ```

5. Bağlantıları yeni oluşturulan **Konuk** veritabanına geçirin.

   ```bash
   \c guest
   ```
6. Yazın `\q` ve ardından Enter tuşunu seçerek psql ' i kapatın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kaynak grubundaki PostgreSQL için Azure veritabanı sunucusunu başarıyla oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu veya PostgreSQL sunucusunu silerek bunları silebilirsiniz. 

Kaynak grubunu silmek için:

1. Azure portal, **kaynak gruplarını**arayıp seçin. 
2. Kaynak grubu listesinde, kaynak grubunuzun adını seçin.
3. Kaynak grubunuzun **genel bakış** sayfasında **kaynak grubunu sil**' i seçin.
4. Onay iletişim kutusunda, kaynak grubunuzun adını girip **Sil**' i seçin.

Sunucuyu silmek için, sunucunuzun **genel bakış** sayfasında **Sil** düğmesini seçin:

> [!div class="mx-imgBorder"]
> ![Sunucu silme düğmesini gösteren ekran görüntüsü.](media/quickstart-create-database-portal/12-delete.png)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı aktarma ve içeri aktarma kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
