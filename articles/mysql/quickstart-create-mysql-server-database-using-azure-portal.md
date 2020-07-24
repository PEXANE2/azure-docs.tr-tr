---
title: 'Hızlı başlangıç: sunucu oluşturma-Azure portal-MySQL için Azure veritabanı'
description: Bu makalede, Azure portalını kullanarak örnek bir MySQL için Azure Veritabanı sunucusunu yaklaşık beş dakika içinde hızlıca oluşturma adımları verilmektedir.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 7/15/2020
ms.openlocfilehash: 61da81dd96078e0577e00be37fe27211a5b62154
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084440"
---
# <a name="quickstart-create-an-azure-database-for-mysql-server-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal MySQL için Azure veritabanı sunucusu oluşturma

MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmak, yönetmek ve ölçeklendirmek için kullanılan, yönetilen bir hizmettir. Bu Hızlı Başlangıçta, Azure portalını kullanarak yaklaşık beş dakikada nasıl MySQL için Azure Veritabanı sunucusu oluşturacağınız gösterilir.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Web tarayıcınızı açın ve [Azure portalına](https://portal.azure.com/) gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/management/overview.md) içinde oluşturulur.

MySQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** (+) seçeneğini belirleyin.

2. **Veritabanları**  >  **MySQL için Azure veritabanı**' nı seçin. Ayrıca, arama kutusuna **MySQL** girerek hizmeti bulabilirsiniz.

  
>[!div class="mx-imgBorder"]
> ![MySQL için Azure Veritabanı seçeneği](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Yeni sunucu ayrıntıları formunu aşağıdaki bilgilerle doldurun:
    
>[!div class="mx-imgBorder"]
> ![Sunucu oluşturma formu](./media/quickstart-create-mysql-server-database-using-azure-portal/4-create-form.png)

**Ayar** | **Önerilen değer** | **Alan açıklaması** 
---|---|---
Abonelik | Aboneliğiniz | Sunucunuz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
Kaynak grubu | *myresourcegroup* | Yeni veya mevcut bir kaynak grubu adı girin. Kaynak grubu, tek bir projeye ait olan bağımlılıklarınızı düzenlemek için kullanılabilir.
Sunucu adı | Benzersiz sunucu adı | MySQL için Azure veritabanı sunucunuzu tanımlayan benzersiz bir ad girin. Örneğin, ' mysqldbserver '. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
Veri kaynağı |*Hiçbiri* | Sıfırdan yeni bir sunucu oluşturmak için *hiçbiri* ' ni seçin. (Mevcut bir MySQL için Azure veritabanı sunucusunun coğrafi yedeğinden bir sunucu oluşturuyorsanız *yedeklemeyi* seçersiniz).
Sunucu yöneticisi oturumu açma | myadmin | Sunucu yöneticiniz için bir Kullanıcı adı girin. Yönetici Kullanıcı adı olarak **azure_superuser**, **yönetici**, **yönetici**, **kök**, **Konuk**veya **genel** kullanamazsınız.
Parola | *Tercih ettiğiniz* | Sunucu yönetici hesabı için yeni bir parola girin. Parola, büyük harf veya küçük harf, sayı ve alfasayısal olmayan karakter (!, $, #,%, vb.) ile birlikte 8 ile 128 karakter uzunluğunda olmalıdır.
Parolayı onayla | *Tercih ettiğiniz*| Yönetici hesabı parolasını onaylayın.
Konum | *Kullanıcılarınıza en yakın bölge*| Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
Sürüm | *En son ana sürüm*| En son ana sürüm (başka bir sürüm gerektiren belirli gereksinimleriniz yoksa).
İşlem + depolama | **Genel Amaçlı**, **5. Nesil**, **2 sanal çekirdek**, **5 GB**, **7 gün**, **Coğrafi Olarak Yedekli** |Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Sunucuyu Yapılandır**' ı seçin. Ardından uygun fiyatlandırma katmanını seçin. daha fazla bilgi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/mysql/)bakın. Coğrafi olarak yedekli depolamada sunucu yedeklemelerinizi etkinleştirmek için **yedek artıklığı seçeneklerinde** **coğrafi olarak yedekli** ' ı seçin. **Tamam**’ı seçin.

   > [!NOTE]
   > Hafif işlem ve g/ç iş yükünüz için yeterli ise temel fiyatlandırma katmanını kullanmayı düşünün. Temel fiyatlandırma katmanında oluşturulan sunucuların daha sonra Genel Amaçlı veya bellek için Iyileştirilmiş olarak ölçeklenmeyeceğini unutmayın. 

4. Sunucuyu sağlamak için **gözden geçir + oluştur** ' u seçin. Hazırlama işlemi 20 dakika kadar sürebilir.
   
5. Araç çubuğunda **Bildirimler**’i (zil simgesi) seçip dağıtım işlemini izleyin.
   
Varsayılan olarak, sunucunuz altında aşağıdaki veritabanları oluşturulur: **information_schema**, **mysql**, **performance_schema** ve **sys**.

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma
Varsayılan olarak, oluşturulan sunucu bir güvenlik duvarıyla korunur ve genel erişime açık değildir. IP 'nize erişim sağlamak için, Azure portal sunucu kaynağına gidin ve sunucu kaynağınız için sol taraftaki menüden **bağlantı güvenliği** ' ni seçin. Kaynağı nasıl bulacağınızı bilmiyorsanız bkz. [kaynak açma](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resources-portal#open-resources).

>[!div class="mx-imgBorder"]
> ![Bağlantı güvenliği - Güvenlik duvarı kuralları](./media/quickstart-create-mysql-server-database-using-azure-portal/add-current-ip-firewall.png)
   
Şimdi **geçerli ISTEMCI IP adresi ekle** ' yi seçin ve ardından **Kaydet**' i seçin. Bu IP 'lerden sunucunuza bağlanmak için ek IP 'Ler ekleyebilir veya bir IP aralığı sağlayabilirsiniz. Daha fazla bilgi için bkz. [MySQL Için Azure veritabanı sunucusunda güvenlik duvarı kurallarını yönetme](./concepts-firewall-rules.md)

> [!NOTE]
> Ağınızın bağlantı sorunlarını önlemek için MySQL için Azure veritabanı tarafından kullanılan bağlantı noktası 3306 üzerinden giden trafiğe izin verdiğinden emin olun.  

## <a name="connect-to-azure-database-for-mysql-server-using-mysql-command-line-client"></a>MySQL komut satırı istemcisini kullanarak MySQL için Azure veritabanı sunucusuna bağlanma
Yerel ortamınızdan sunucusuna bağlanmak için [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) veya [MySQL çalışma ekranı](./connect-workbench.md) seçeneğini belirleyebilirsiniz. Bu hızlı başlangıçta, sunucuya bağlanmak için [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) **mysql.exe** çalıştıracağız.

1. Sol üst taraftaki vurgulanan simgeye tıklayarak portalda Azure Cloud Shell başlatın. Aşağıdaki görüntüde gösterildiği gibi, **genel bakış** bölümünden yeni oluşturduğunuz sunucu için sunucu adı, Sunucu Yöneticisi oturum açma adı, parola ve aboneliğinizi bir yere göz atın.

    >[!NOTE]
    >Cloud Shell 'i ilk kez çalıştırırsanız, bir kaynak grubu, depolama hesabı oluşturmak için bir istem görürsünüz. Bu bir kerelik bir adımdır ve tüm oturumlar için otomatik olarak eklenir. 

   >[!div class="mx-imgBorder"]
   > ![Portal tam görünüm Cloud Shell](./media/quickstart-create-mysql-server-database-using-azure-portal/use-in-cloud-shell.png)
2. Bu komutu Azure Cloud Shell terminalde çalıştırın. Değerleri gerçek sunucu adı ve Yönetici Kullanıcı oturum açma adınızla değiştirin. Yönetici Kullanıcı adı, <servername> MySQL Için Azure veritabanı için aşağıda gösterildiği gibi ' @ ' i gerektirir  

  ```azurecli-interactive
  mysql --host=mydemoserver.mysql.database.azure.com --user=myadmin@mydemoserver -p 
  ```

  Deneyim Cloud Shell terminalinde şöyle görünür
  ```
  Requesting a Cloud Shell.Succeeded.
  Connecting terminal...

  Welcome to Azure Cloud Shell

  Type "az" to use Azure CLI
  Type "help" to learn about Cloud Shell

  sunitha@Azure:~$mysql -h mydemoserver.mysql.database.azure.com -u admin@wpapp-mysqldbserver -p
  Enter password:
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 64796
  Server version: 5.6.42.0 Source distribution

  Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql>
  ```
3. Aynı Azure Cloud Shell terminalinde, bir veritabanı **konuğunu** oluşturun 
  ```
  mysql> CREATE DATABASE guest;
  Query OK, 1 row affected (0.27 sec)
  ```
4. Veritabanı **konuğunu** değiştirme
  ```
  mysql> USE guest;
  Database changed 
  ```
5. Yazın ```quit``` ve ardından Enter tuşunu seçerek MySQL 'ten çıkın.   

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kaynak grubunda MySQL için Azure veritabanı sunucusunu başarıyla oluşturdunuz.  Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu silerek veya MySQL sunucusunu silmeniz durumunda bunları silebilirsiniz. Kaynak grubunu silmek için şu adımları izleyin:
1. Azure portal, **kaynak gruplarını**arayıp seçin. 
2. Kaynak grubu listesinde, kaynak grubunuzun adını seçin.
3. Kaynak grubunuzun Genel Bakış sayfasında **kaynak grubunu sil**' i seçin.
4. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

Sunucuyu silmek için aşağıda gösterildiği gibi sunucunuzun **genel bakış** sayfasında **Sil** düğmesine tıklayabilirsiniz:
> [!div class="mx-imgBorder"]
> ![Kaynaklarınızı silme](media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
>MySQL ile Windows [üzerinde BIR php uygulaması derleme](../app-service/app-service-web-tutorial-php-mysql.md) 
> MySQL ile Linux ['TA php uygulaması derleme](../app-service/containers/tutorial-php-mysql-app.md) 
> [MySQL Ile Java tabanlı Spring uygulaması derleme](https://docs.microsoft.com/azure/developer/java/spring-framework/spring-app-service-e2e?tabs=bash)
