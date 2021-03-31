---
title: 'Hızlı başlangıç: sunucu oluşturma-Azure portal-MySQL için Azure veritabanı'
description: Bu makalede, Azure portalını kullanarak örnek bir MySQL için Azure Veritabanı sunucusunu yaklaşık beş dakika içinde oluşturma adımları gösterilmektedir.
author: savjani
ms.author: pariks
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/04/2020
ms.openlocfilehash: b8f3a8e06bb27051417205c4dc3141948960bb00
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98567798"
---
# <a name="quickstart-create-an-azure-database-for-mysql-server-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak MySQL için Azure veritabanı sunucusu oluşturma

MySQL için Azure veritabanı, bulutta yüksek oranda kullanılabilir MySQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız yönetilen bir hizmettir. Bu hızlı başlangıçta, Azure portal kullanarak MySQL için Azure veritabanı tek sunucu oluşturma gösterilmektedir. Ayrıca sunucuya nasıl bağlanagösterdiğini gösterir.

## <a name="prerequisites"></a>Önkoşullar
Bir Azure aboneliği gereklidir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-an-azure-database-for-mysql-single-server"></a>MySQL için Azure veritabanı tek sunucu oluşturma
1. Bir MySQL tek sunucu veritabanı oluşturmak için [Azure Portal](https://portal.azure.com/) gidin. **MySQL Için Azure veritabanı**'nı arayın ve seçin:

   >[!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/find-azure-mysql-in-portal.png" alt-text="MySQL için Azure veritabanı 'nı bulun":::

1. **Add (Ekle)** seçeneğini belirleyin.

2. **MySQL Için Azure veritabanı dağıtım seçeneğini seçin** sayfasında **tek sunucu**' yı seçin:
   >[!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/choose-singleserver.png" alt-text="Tek sunucu seçeneğini gösteren ekran görüntüsü.":::

3. Yeni bir tek sunucu için temel ayarları girin:

   >[!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/4-create-form.png" alt-text="MySQL sunucusu oluştur sayfasını gösteren ekran görüntüsü.":::

   **Ayar** | **Önerilen değer** | **Açıklama**
   ---|---|---
   Abonelik | Aboneliğiniz | İstediğiniz Azure aboneliğini seçin.
   Kaynak grubu | **myresourcegroup** | Aboneliğinizden yeni bir kaynak grubu veya mevcut bir kaynak grubu girin.
   Sunucu adı | **mydemoserver** | Benzersiz bir ad girin. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ile 63 arasında karakter içermelidir.
   Veri kaynağı |**Hiçbiri** | Sıfırdan yeni bir sunucu oluşturmak için **Yok**'u seçin. Yalnızca var olan bir sunucunun coğrafi yedeklemesinden geri yükleme yapıyorsanız **yedekleme** ' yi seçin.
   Konum |İstediğiniz konum | Listeden bir konum seçin.
   Sürüm | En son ana sürüm| En son ana sürümü kullanın. [Desteklenen tüm sürümleri](concepts-supported-versions.md)görün.
   İşlem + depolama | Varsayılanları Kullan| Varsayılan fiyatlandırma katmanı, **4 sanal çekirdek** ve **100 GB** depolama alanı ile **genel amaçlı** . Yedekleme saklama, **coğrafi olarak yedekli** yedekleme seçeneğiyle **7 güne** ayarlanır.<br/>[Fiyatlandırma](https://azure.microsoft.com/pricing/details/mysql/) sayfasını gözden geçirin ve gerekirse Varsayılanları güncelleştirin.
   Yönetici kullanıcı adı | **mydemoadmin** | Sunucu Yöneticisi Kullanıcı adınızı girin. Yönetici Kullanıcı adı için **azure_superuser**, **yönetici**, **yönetici**, **kök**, **Konuk** veya **ortak** kullanamazsınız.
   Parola | Parola | Sunucu Yöneticisi kullanıcısı için yeni bir parola. Parola 8 ile 128 karakter uzunluğunda olmalı ve büyük veya küçük harfler, sayılar ve alfasayısal olmayan karakterler (!, $, #,%, vb.) birleşimini içermelidir.
  

   > [!NOTE]
   > Hafif işlem ve g/ç iş yükünüz için yeterli ise temel fiyatlandırma katmanını kullanmayı düşünün. Temel fiyatlandırma katmanında oluşturulan sunucuların daha sonra Genel Amaçlı veya bellek için Iyileştirilmiş olarak ölçeklenemeyeceğini unutmayın.

4. Sunucuyu sağlamak için **gözden geçir + oluştur** ' u seçin.

5. Portal sayfasında **Dağıtımınız tamamlandı** iletisinin görüntülenmesini bekleyin. Yeni oluşturulan sunucu sayfasına gitmek için **Kaynağa Git** ' i seçin:

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/deployment-complete.png" alt-text="Dağıtımınızın tamamlandığını gösteren ekran görüntüsü.":::

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/mysql-doc-feedback)

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

Varsayılan olarak, yeni sunucu bir güvenlik duvarıyla korunur. Bağlanmak için aşağıdaki adımları tamamlayarak IP 'nize erişim sağlamanız gerekir:

1. Sunucu Kaynağınızın sol bölmesindeki **bağlantı güvenliği** ' ne gidin. Kaynağınızın nasıl bulunacağını bilmiyorsanız bkz. [kaynak açma](../azure-resource-manager/management/manage-resources-portal.md#open-resources).

   >[!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/add-current-ip-firewall.png" alt-text="Bağlantı güvenliği > güvenlik duvarı kuralları sayfasını gösteren ekran görüntüsü.":::

2. **Geçerli ISTEMCI IP adresi ekle**' yi seçin ve ardından **Kaydet**' i seçin.

   > [!NOTE]
   > Bağlantı sorunlarından kaçınmak için ağınızın, MySQL için Azure veritabanı tarafından kullanılan 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verdiğinden emin olun. 

Bu IP 'lerden sunucunuza bağlanmak için daha fazla IP ekleyebilirsiniz veya bir IP aralığı sağlayabilirsiniz. Daha fazla bilgi için bkz. [MySQL Için Azure veritabanı sunucusunda güvenlik duvarı kurallarını yönetme](./concepts-firewall-rules.md).


[Sorun mu yaşıyorsunuz? Bize bilgi verin](https://aka.ms/mysql-doc-feedback)

## <a name="connect-to-the-server-by-using-mysqlexe"></a>mysql.exe kullanarak sunucuya bağlanma
Yerel ortamınızdan sunucusuna bağlanmak için [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) ya da [MySQL çalışma ekranı](./connect-workbench.md) kullanabilirsiniz. Bu hızlı başlangıçta, sunucuya bağlanmak için [Azure Cloud Shell](../cloud-shell/overview.md) mysql.exe kullanacağız.


1. Aşağıdaki ekran görüntüsünde gösterildiği gibi, araç çubuğundaki ilk düğmeyi seçerek portalda Azure Cloud Shell açın. Ekran görüntüsünde gösterildiği gibi, **genel bakış** bölümünde sunucu adı, Sunucu Yöneticisi adı ve yeni sunucunuzun aboneliğini göz önünde edin.

    > [!NOTE]
    > Cloud Shell ilk kez açarsanız, bir kaynak grubu ve depolama hesabı oluşturmanız istenir. Bu bir kerelik bir adımdır. Tüm oturumlar için otomatik olarak eklenecektir.

   >[!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/use-in-cloud-shell.png" alt-text="Azure portal Cloud Shell gösteren ekran görüntüsü.":::
2. Azure Cloud Shell terminalinde aşağıdaki komutu çalıştırın. Burada gösterilen değerleri gerçek sunucu adı ve yönetici kullanıcı adıyla değiştirin. MySQL için Azure veritabanı 'nda, `@\<servername>` burada gösterildiği gibi yönetici kullanıcı adına eklemeniz gerekir: 

      ```azurecli-interactive
      mysql --host=mydemoserver.mysql.database.azure.com --user=myadmin@mydemoserver -p
      ```

      Cloud Shell terminalinde şöyle görünür:

      ```
      Requesting a Cloud Shell.Succeeded.
      Connecting terminal...

      Welcome to Azure Cloud Shell

      Type "az" to use Azure CLI
      Type "help" to learn about Cloud Shell

      user@Azure:~$mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
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
3. Aynı Azure Cloud Shell terminalinde, adlı bir veritabanı oluşturun `guest` :
      ```
      mysql> CREATE DATABASE guest;
      Query OK, 1 row affected (0.27 sec)
      ```
4. `guest`Veritabanına geç:
      ```
      mysql> USE guest;
      Database changed
      ```
5. Yazın `quit` ve ardından **ENTER** ' u seçerek MySQL 'ten çıkın.

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/mysql-doc-feedback)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık bir kaynak grubunda MySQL için Azure veritabanı sunucusu oluşturdunuz.  Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu silerek bunları silebilir veya yalnızca MySQL sunucusunu silebilirsiniz. Kaynak grubunu silmek için şu adımları izleyin:
1. Azure portal, **kaynak gruplarını** arayıp seçin.
2. Kaynak grupları listesinde, kaynak grubunuzun adını seçin.
3. Kaynak grubunuzun **genel bakış** sayfasında **kaynak grubunu sil**' i seçin.
4. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

Sunucuyu silmek için, aşağıda gösterildiği gibi sunucunuzun **genel bakış** sayfasında **Sil** ' i seçebilirsiniz:
> [!div class="mx-imgBorder"]
> :::image type="content" source="media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png" alt-text="Sunucu genel bakış sayfasında Sil düğmesini gösteren ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
>[MySQL ile Windows üzerinde bir PHP uygulaması derleme](../app-service/tutorial-php-mysql-app.md) <br/>

> [!div class="nextstepaction"]
>[MySQL ile Linux 'ta PHP uygulaması derleme](../app-service/tutorial-php-mysql-app.md?pivots=platform-linux%3fpivots%3dplatform-linux)<br/><br/>

[Aradığınızı bulamıyor musunuz? Bize bilgi verin.](https://aka.ms/mysql-doc-feedback)
