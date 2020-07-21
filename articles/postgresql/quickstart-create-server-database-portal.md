---
title: 'Hızlı başlangıç: sunucu oluşturma-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu'
description: Azure portal Kullanıcı arabirimini kullanarak PostgreSQL için Azure veritabanı oluşturma ve yönetmeye yönelik hızlı başlangıç kılavuzu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/27/2020
ms.openlocfilehash: 9741e00cdcc4907c547ebfcc6f8e7df7a3b32891
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86529701"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-server-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında PostgreSQL için Azure Veritabanı sunucusu oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız, yönetilen bir hizmettir. Bu Hızlı Başlangıçta, Azure portalı kullanarak yaklaşık beş dakika içinde PostgreSQL için Azure Veritabanı sunucusunun nasıl oluşturulduğu gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Web tarayıcınızı açın ve [portala](https://portal.azure.com/) gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

PostgreSQL için Azure Veritabanı sunucusu, yapılandırılmış bir dizi [işlem ve depolama kaynağı](./concepts-pricing-tiers.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/management/overview.md) içinde oluşturulur.

PostgreSQL için Azure Veritabanı sunucusu oluşturmak için şu adımları uygulayın:
1. Portalın sol üst köşesinde bulunan **kaynak oluştur** (+) seçeneğini belirleyin.

2. **Veritabanları**  >  **PostgreSQL için Azure veritabanı**' nı seçin.

   > [!div class="mx-imgBorder"]
   > ![Menüdeki "PostgreSQL için Azure veritabanı"](./media/quickstart-create-database-portal/1-create-database.png)

3. **Tek sunuculu** dağıtım seçeneğini belirleyin.

   > [!div class="mx-imgBorder"]
   > ![PostgreSQL için Azure veritabanı-tek sunuculu dağıtım seçeneği seçin](./media/quickstart-create-database-portal/select-deployment-option.png)

4. **Temel** bilgiler formunu aşağıdaki bilgilerle doldurun:

   > [!div class="mx-imgBorder"]
   > ![Sunucu oluşturma](./media/quickstart-create-database-portal/create-basics.png)

   Ayar|Önerilen Değer|Açıklama
   ---|---|---
   Abonelik|Aboneliğinizin adı|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden fazla aboneliğiniz varsa kaynağın faturalandırıldığı aboneliği seçin.
   Kaynak grubu|*myresourcegroup*| Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
   Sunucu adı |*mydemoserver*|PostgreSQL için Azure Veritabanı sunucunuzu tanıtan benzersiz bir ad. Girdiğiniz sunucu adına *postgres.database.azure.com* etki alanı adı eklenir. Sunucunuz yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. En az 3, en çok 63 karakterden oluşmalıdır.
   Veri kaynağı | *Hiçbiri* | Sıfırdan yeni bir sunucu oluşturmak için *hiçbiri* ' ni seçin. (Mevcut bir PostgreSQL için Azure Veritabanı sunucusunun bir coğrafi yedeğinden bir sunucu oluşturuyorsanız, *Yedek* seçeneğini belirleyin).
   Yönetici Kullanıcı adı |*myadmin*| Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınız. Yönetici oturum açma adı **azure_superuser**, **azure_pg_admin**, **yönetici**, **yönetici**, **kök**, **Konuk**veya **ortak**olamaz. **PG_** ile başlayamaz.
   Parola |Parolanız| Sunucu yönetici hesabı için yeni bir parola. 8 ile 128 arasında karakter içermelidir. Parolanız şu kategorilerden üçünde yer alan karakterlerden oluşmalıdır: İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0 - 9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).
   Konum|Kullanıcılarınıza en yakın bölge| Kullanıcılarınız için en yakın olan konum.
   Sürüm|En son ana sürüm| Belirli gereksinimleriniz olmadığı sürece, en son PostgreSQL ana sürümü.
   İşlem + depolama | **Genel Amaçlı**, **5. Nesil**, **2 sanal çekirdek**, **5 GB**, **7 gün**, **Coğrafi Olarak Yedekli** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Sunucuyu Yapılandır**' ı seçin. Ardından uygun fiyatlandırma katmanını seçin, daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/postgresql/server/). Coğrafi olarak yedekli depolamada sunucu yedeklemelerinizi etkinleştirmek için **yedek artıklığı seçeneklerinde** **coğrafi olarak yedekli** ' ı seçin. **Tamam**’ı seçin.

   > [!NOTE]
   > Hafif işlem ve g/ç iş yükünüz için yeterli ise temel fiyatlandırma katmanını kullanmayı düşünün. Temel fiyatlandırma katmanında oluşturulan sunucuların daha sonra Genel Amaçlı veya bellek için Iyileştirilmiş olarak ölçeklenmeyeceğini unutmayın. 
   
5. Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. Sunucuyu sağlamak için **Oluştur**’u seçin. Bu işlem birkaç dakika sürebilir.

6. Araç çubuğunda, dağıtım sürecini izlemek için **Bildirimler** simgesini (zil) seçin. Dağıtım tamamlandıktan sonra **Kaynağa Git** ' i seçtiğinizde sunucunun **genel bakış** sayfası açılır.

Boş bir veritabanı, **Postgres** oluşturulur. Ayrıca, yönetilen hizmet işlemlerini Kullanıcı eylemlerinden ayırmak için kullanılan **azure_maintenance** veritabanı da bulacaksınız. **Azure_maintenance** veritabanına erişemezsiniz.

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma
Varsayılan olarak, oluşturulan sunucuya herkese açık olmaz ve IP 'niz için izinler vermeniz gerekir. IP 'nize erişim sağlamak için, Azure portal sunucu kaynağına gidin ve sunucu kaynağınız için sol taraftaki menüden **bağlantı güvenliği** ' ni seçin. Kaynağınızı bulma konusunda emin değilseniz, bkz. [kaynak açma](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resources-portal#open-resources).

> [!div class="mx-imgBorder"]
> ![Bağlantı güvenliği - Güvenlik duvarı kuralları](./media/quickstart-create-database-portal/add-current-ip-firewall.png)
  
Şimdi **geçerli ISTEMCI IP adresi ekle** ' yi seçin ve ardından **Kaydet**' i seçin. Bu IP 'lerden sunucunuza bağlanmak için ek IP 'Ler ekleyebilir veya bir IP aralığı sağlayabilirsiniz. Daha fazla bilgi için bkz. [güvenlik duvarı kurallarını yönetme](./concepts-firewall-rules.md)
   
> [!NOTE]
> Ağınızın bağlantı sorunlarını önlemek için PostgreSQL için Azure veritabanı tarafından kullanılan bağlantı noktası 5432 üzerinden giden trafiğe izin verdiğini denetleyin.  

## <a name="connect-to-azure-database-for-postgresql-server-using-psql"></a>Psql kullanarak PostgreSQL için Azure veritabanı sunucusuna bağlanma

[Psql](http://postgresguide.com/utilities/psql.html) veya [pgAdmin](https://www.pgadmin.org/docs/pgadmin4/latest/connecting.html) 'ı kullanarak, popüler PostgreSQL istemcileri kullanabilirsiniz. Bu hızlı başlangıçta, Azure portal içinde [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) psql kullanarak bağlanacağız.

1. Aşağıdaki görüntüde gösterildiği gibi sunucunuzun **genel bakış** bölümünden yeni oluşturduğunuz sunucu için sunucu adı, Sunucu Yöneticisi oturum açma adı, parola ve abonelik kimliğini bir yere göz atın.

2. Aşağıdaki görüntüde vurgulanan şekilde sol üst taraftaki simgeyi seçerek portalda Azure Cloud Shell başlatın.

   > [!NOTE]
   > Cloud Shell ilk kez çalıştırıyorsanız, bir kaynak grubu, depolama hesabı oluşturmak için bir istem görürsünüz. Bu bir kerelik bir adımdır ve tüm oturumlar için otomatik olarak eklenir. 

   > [!div class="mx-imgBorder"]
   > ![Azure Cloud Shell açın](media/quickstart-create-database-portal/use-in-cloud-shell.png)

3. Bu komutu Azure Cloud Shell terminalde çalıştırın. Değerleri gerçek sunucu adı ve Yönetici Kullanıcı oturum açma adınızla değiştirin. PostgreSQL için Azure veritabanı için aşağıda gösterildiği gibi, bu biçimdeki yönetici kullanıcıyla birlikte boş veritabanı **Postgres** 'i kullanın ' <admin-username> @ <servername>

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```
 
   Deneyim Cloud Shell terminalinde şöyle görünür
   
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
4. Aynı Azure Cloud Shell terminalinde, bir veritabanı **konuğunu** oluşturun
   ```bash
   postgres=> CREATE DATABASE guest;
   ```

5. Artık bağlantıları yeni oluşturulan veritabanı **konuğa** geçirmek için

   ```bash
   \c guest
   ```
6. Yazın `\q` ve ardından Enter tuşunu seçerek psql 'den çıkın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kaynak grubundaki PostgreSQL için Azure veritabanı sunucusunu başarıyla oluşturdunuz.  Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu silerek bunları silebilir veya PostgreSQL sunucusunu silmeniz yeterlidir. Kaynak grubunu silmek için şu adımları izleyin:

1. Azure portal, **kaynak gruplarını**arayıp seçin. 
2. Kaynak grubu listesinde, kaynak grubunuzun adını seçin.
3. Kaynak grubunuzun Genel Bakış sayfasında **kaynak grubunu sil**' i seçin.
4. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

Sunucuyu silmek için aşağıda gösterildiği gibi sunucunuzun **genel bakış** sayfasında **Sil** düğmesine tıklayabilirsiniz:
> [!div class="mx-imgBorder"]
> ![Kaynaklarınızı silme](media/quickstart-create-database-portal/12-delete.png)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
