---
title: 'Hızlı başlangıç: MySQL için Azure veritabanı esnek sunucu oluşturma-Azure portal'
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı esnek sunucusu oluşturma işlemi adım adım açıklanmaktadır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 10/22/2020
ms.openlocfilehash: 074b799a4f0e83c47aac0b2b3fca5386bd45429f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100521977"
---
# <a name="quickstart-use-the-azure-portal-to-create-an-azure-database-for-mysql-flexible-server"></a>Hızlı başlangıç: MySQL için Azure veritabanı esnek sunucusu oluşturmak için Azure portal kullanma

MySQL için Azure veritabanı esnek sunucu, bulutta yüksek oranda kullanılabilir MySQL sunucularını çalıştırmak, yönetmek ve ölçeklendirmek için kullanabileceğiniz bir yönetilen hizmettir. Bu hızlı başlangıçta Azure portal kullanarak esnek sunucu oluşturma gösterilmektedir.

> [!IMPORTANT] 
> MySQL için Azure veritabanı esnek sunucu şu anda genel önizlemededir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com/) gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mysql-flexible-server"></a>MySQL için Azure veritabanı esnek sunucusu oluşturma

Tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-storage.md)ile esnek bir sunucu oluşturursunuz. Sunucu, [Azure kaynak grubu](../../azure-resource-manager/management/overview.md) içinde oluşturulur.

Esnek sunucu oluşturmak için aşağıdaki adımları uygulayın:

1. Portalda **MySQL sunucuları Için Azure veritabanı** 'nı arayın ve seçin:
    
    > :::image type="content" source="./media/quickstart-create-server-portal/find-mysql-portal.png" alt-text="MySQL sunucuları için Azure veritabanı aramasını gösteren ekran görüntüsü.":::

2. **Add (Ekle)** seçeneğini belirleyin. 

3. **MySQL Için Azure veritabanı dağıtımı seçeneğini belirleyin** sayfasında dağıtım seçeneği olarak **esnek sunucu** ' yı seçin:
     
    > :::image type="content" source="./media/quickstart-create-server-portal/deployment-option.png" alt-text="Esnek sunucu seçeneğini gösteren ekran görüntüsü.":::    

4. **Temel bilgiler** sekmesinde, aşağıdaki bilgileri girin: 

    > :::image type="content" source="./media/quickstart-create-server-portal/create-form.png" alt-text="Esnek sunucu sayfasının temel bilgiler sekmesini gösteren ekran görüntüsü."::: 
                                    
    |**Ayar**|**Önerilen değer**|**Açıklama**|
    |---|---|---|
    Abonelik|Aboneliğinizin adı|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden çok aboneliğiniz varsa, kaynak için faturalandırılması istediğiniz aboneliği seçin.|
    Kaynak grubu|**myresourcegroup**| Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.|
    Sunucu adı |**mydemoserver**|Esnek sunucunuzu tanımlayan benzersiz bir ad. Etki alanı adı, `mysql.database.azure.com` sağladığınız sunucu adına eklenir. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ile 63 arasında karakter içermelidir.|
    Yönetici kullanıcı adı |**mydemouser**| Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınız. Yönetici Kullanıcı adı **azure_superuser**, **yönetici**, **yönetici**, **kök**, **Konuk** veya **ortak** olamaz.|
    Parola |Parolanız| Sunucu yönetici hesabı için yeni bir parola. 8 ile 128 arasında karakter içermelidir. Ayrıca, şu kategorilerden üçünden karakterler içermelidir: Ingilizce büyük harfler, Ingilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #,%, vb.).|
    Region|Kullanıcılarınıza en yakın bölge| Kullanıcılarınız için en yakın olan konum.|
    Sürüm|**5.7**| MySQL ana sürümü.|
    İşlem + depolama | **Burstable**, **Standard_B1ms**, **10 GiB**, **7 gün** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Sunucuyu Yapılandır**' ı seçin. **Burstable**, **Standard_B1ms**, **10 GiB** ve **7 gün** , **işlem katmanı**, **işlem boyutu**, **depolama boyutu** ve yedekleme **saklama süresi** için varsayılan değerlerdir. Bu değerleri olduğu gibi bırakabilir veya düzenleyebilirsiniz. İşlem ve depolama seçimini kaydetmek için, yapılandırmaya devam etmek için **Kaydet** ' i seçin. Aşağıdaki ekran görüntüsünde işlem ve depolama seçenekleri gösterilmektedir.|
    
    > :::image type="content" source="./media/quickstart-create-server-portal/compute-storage.png" alt-text="İşlem ve depolama seçeneklerini gösteren ekran görüntüsü.":::

5. Ağ seçeneklerini yapılandırın.

    **Ağ** sekmesinde, sunucunuza nasıl ulaşılabildiğini seçebilirsiniz. MySQL için Azure veritabanı esnek sunucusu, sunucunuza bağlanmak için iki yol sunar: 
   - Genel erişim (izin verilen IP adresleri)
   - Özel erişim (Sanal Ağ Tümleştirmesi) 
   
   Genel erişimi kullandığınızda, sunucunuza erişim bir güvenlik duvarı kuralına eklediğiniz izin verilen IP adresleriyle sınırlıdır. Bu yöntem, belirli bir IP adresi veya aralığı için güvenlik duvarını açmak üzere bir kural oluşturmadığınız müddetçe, dış uygulamaların ve araçların sunucuya ve sunucu üzerindeki veritabanlarına bağlanmasını engeller. Özel erişim (VNet tümleştirmesi) kullandığınızda, sunucunuza erişim sanal ağınızla sınırlıdır. Bu hızlı başlangıçta, sunucuya bağlanmak için genel erişimin nasıl etkinleştirileceğini öğreneceksiniz. [Kavramlar makalesindeki bağlantı yöntemleri hakkında daha fazla bilgi edinin.](./concepts-networking.md)

    > [!NOTE]
    > Sunucuyu oluşturduktan sonra bağlantı yöntemini değiştiremezsiniz. Örneğin, sunucuyu oluştururken **genel erişim (izin VERILEN IP adresleri)** seçeneğini belirlerseniz, sunucu oluşturulduktan sonra **özel erişim (VNET tümleştirmesi)** olarak değiştiremezsiniz. Sanal ağ tümleştirme aracılığıyla sunucunuza güvenli erişim sağlamaya yardımcı olmak için sunucunuzu özel erişimle oluşturmanız önemle tavsiye ederiz. [Kavramlar makalesinde özel erişim hakkında daha fazla bilgi edinin.](./concepts-networking.md)

    > :::image type="content" source="./media/quickstart-create-server-portal/networking.png" alt-text="Ağ sekmesini gösteren ekran görüntüsü.":::  

6. Esnek sunucu yapılandırmanızı gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

7. Sunucuyu sağlamak için **Oluştur**’u seçin. Sağlama birkaç dakika sürebilir.

8. Dağıtım işlemini izlemek için araç çubuğunda (çan düğmesi) **Bildirimler** ' i seçin. Dağıtım tamamlandıktan sonra, Azure portal panonuzda esnek sunucu için bir kutucuk oluşturan **panoya sabitle**' yi seçebilirsiniz. Bu kutucuk, sunucunun **genel bakış** sayfasına yönelik bir kısayoldur. **Kaynağa Git**' i seçtiğinizde, sunucunun **genel bakış** sayfası açılır.

Varsayılan olarak bu veritabanları, sunucunuz altında oluşturulur: information_schema, MySQL, performance_schema ve sys.

> [!NOTE]
> Bağlantı sorunlarından kaçınmak için, ağınızın, MySQL esnek sunucusu için Azure veritabanı tarafından kullanılan bağlantı noktası 3306 üzerinden giden trafiğe izin verdiğinden emin olun.  

## <a name="connect-to-the-server-by-using-mysqlexe"></a>mysql.exe kullanarak sunucuya bağlanma

Esnek sunucunuzu özel erişim (VNet tümleştirmesi) kullanarak oluşturduysanız, sunucunuza aynı sanal ağ içindeki bir kaynaktan sunucunuza bağlanmanız gerekir. Bir sanal makine oluşturabilir ve bunu esnek sunucunuz ile oluşturulan sanal ağa ekleyebilirsiniz. Daha fazla bilgi edinmek için [özel erişim belgelerini](how-to-manage-virtual-network-portal.md) yapılandırma konusuna bakın.

Ortak erişim (izin verilen IP adresleri) kullanarak esnek sunucunuzu oluşturduysanız, yerel IP adresinizi sunucunuzdaki güvenlik duvarı kuralları listesine ekleyebilirsiniz. Adım adım yönergeler için [güvenlik duvarı kuralları oluşturma veya yönetme belgelerini](how-to-manage-firewall-portal.md) inceleyin.

Yerel ortamınızdan sunucusuna bağlanmak için [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) ya da [MySQL çalışma ekranı](./connect-workbench.md) kullanabilirsiniz. 

```bash
wget --no-check-certificate https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem
mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p --ssl=true --ssl-ca=DigiCertGlobalRootCA.crt.pem
```

Esnek sunucunuzu **ortak erişim** kullanarak sağladıysanız, aşağıda gösterildiği gibi önceden yüklenmiş MySQL istemcisini kullanarak esnek sunucunuza bağlanmak için [Azure Cloud Shell](https://shell.azure.com/bash) de kullanabilirsiniz:

Esnek sunucunuza bağlanmak için Azure Cloud Shell kullanabilmeniz için, Azure Cloud Shell ağ üzerinden esnek sunucunuza erişim izni vermeniz gerekir. Bunu başarmak için, MySQL esnek sunucunuz için Azure portal **ağ** dikey penceresine gidebilir ve **güvenlik duvarı** bölümünde "Azure 'daki herhangi bir Azure hizmetinden bu sunucuya genel erişime izin ver" diyen ve ayarı sürdürmek için Kaydet ' e tıklayabilirsiniz.

> [!NOTE]
> **Azure 'daki herhangi bir Azure hizmetinden bu sunucuya genel erişime Izin ver** ' in yalnızca geliştirme veya test için kullanılması gerekir. Güvenlik duvarını, diğer müşterilerin aboneliklerinden gelen bağlantılar da dahil olmak üzere herhangi bir Azure hizmetine veya varlığına ayrılan IP adreslerinden gelen bağlantılara izin verecek şekilde yapılandırır.

Azure Cloud Shell başlatmak için **dene** ' ye tıklayın ve esnek sunucunuza bağlanmak için aşağıdaki komutları kullanın. Komutta sunucu adınızı, Kullanıcı adınızı ve parolanızı kullanın. 

```azurecli-interactive
wget --no-check-certificate https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem
mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p --ssl=true --ssl-ca=DigiCertGlobalRootCA.crt.pem
```

Daha önce komutu takip eden esnek sunucunuza bağlanırken aşağıdaki hata iletisini görürseniz, daha önce bahsedilen "Azure 'daki herhangi bir Azure hizmetinden ortak erişime Izin ver" seçeneğini kullanarak güvenlik duvarı kuralını ayarlamayı kaçırdınız veya seçenek kaydedilmez. Lütfen güvenlik duvarını ayarlamayı yeniden deneyin ve tekrar deneyin.

Hata 2002 (HY000): MySQL sunucusuna bağlanılamıyor <servername> (115)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık bir kaynak grubunda MySQL için Azure veritabanı esnek sunucusu oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu silerek bunları silebilir veya yalnızca MySQL sunucusunu silebilirsiniz. Kaynak grubunu silmek için şu adımları izleyin:

1. Azure portal, **kaynak gruplarını** arayıp seçin.
1. Kaynak grupları listesinde, kaynak grubunuzun adını seçin.
1. Kaynak grubunuzun **genel bakış** sayfasında **kaynak grubunu sil**' i seçin.
1. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

Sunucuyu silmek için, aşağıda gösterildiği gibi sunucunuzun **genel bakış** sayfasında **Sil** ' i seçebilirsiniz:

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-server-portal/delete-server.png" alt-text="Bir sunucunun nasıl silineceğini gösteren ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MySQL ile PHP (Laralevel) Web uygulaması oluşturma](tutorial-php-database-app.md)
