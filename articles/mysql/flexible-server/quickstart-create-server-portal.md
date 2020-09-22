---
title: 'Hızlı başlangıç: MySQL için Azure VERITABANı esnek sunucu oluşturma-Azure portal'
description: Bu makalede, bir MySQL için Azure veritabanı esnek sunucusunu birkaç dakika içinde hızlıca oluşturmak için Azure portal kullanma adımları açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 9/21/2020
ms.openlocfilehash: 4d4d65f9ad04ca6bf99375647684a75e8662bb4d
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948114"
---
# <a name="quickstart-use-the-azure-portal-to-create-an-azure-database-for-mysql-flexible-server"></a>Hızlı başlangıç: MySQL için Azure veritabanı esnek sunucusu oluşturmak için Azure portal kullanma

MySQL için Azure veritabanı esnek sunucu, bulutta yüksek oranda kullanılabilir MySQL sunucularını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız bir yönetilen hizmettir. Bu hızlı başlangıçta, Azure portal kullanarak nasıl esnek bir sunucu oluşturacağınız gösterilmektedir.

> [!IMPORTANT] 
> MySQL için Azure veritabanı esnek sunucu şu anda genel önizlemede

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Web tarayıcınızı açın ve [Azure portalına](https://portal.azure.com/) gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mysql-flexible-server"></a>MySQL için Azure veritabanı esnek sunucusu oluşturma

Tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-storage.md)ile esnek bir sunucu oluşturursunuz. Sunucu, [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) içinde oluşturulur.

Esnek sunucu oluşturmak için aşağıdaki adımları izleyin:

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** (+) seçeneğini belirleyin.

2. **Veritabanları**  >  **MySQL için Azure veritabanı**' nı seçin. Ayrıca, arama kutusuna **MySQL** girerek hizmeti bulabilirsiniz.

    >[!div class="mx-imgBorder"]
    > :::image type="content" source="./media/quickstart-create-server-portal/navigate-to-mysql.png" alt-text="MySQL için Azure Veritabanı seçeneği":::

3. Dağıtım seçeneği olarak **esnek sunucu** ' yı seçin.
     
    >[!div class="mx-imgBorder"]
    > :::image type="content" source="./media/quickstart-create-server-portal/deployment-option.png" alt-text="Dağıtım seçeneğini belirleyin":::    

4. **Temel** bilgiler formunu aşağıdaki bilgilerle doldurun: 

    >[!div class="mx-imgBorder"]
    > :::image type="content" source="./media/quickstart-create-server-portal/create-form.png" alt-text="Sunucu oluşturma formu"::: 
                                    
    |**Ayar**|**Önerilen değer**|**Açıklama**|
    |---|---|---|
    Abonelik|Aboneliğinizin adı|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden çok aboneliğiniz varsa, kaynağın faturalandırılması için istediğiniz aboneliği seçin.|
    Kaynak grubu|*myresourcegroup*| Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.|
    Sunucu adı |*mydemoserver*|Esnek sunucunuzu tanımlayan benzersiz bir ad. Girdiğiniz sunucu adına *mysql.database.azure.com* etki alanı adı eklenir. Sunucunuz yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. En az 3, en çok 63 karakterden oluşmalıdır.|
    Yönetici kullanıcı adı |*mydemouser*| Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınız. Yönetici oturum açma adı **azure_superuser**, **yönetici**, **yönetici**, **kök**, **Konuk**veya **ortak**olamaz.|
    Parola |Parolanız| Sunucu yönetici hesabı için yeni bir parola. 8 ile 128 arasında karakter içermelidir. Parolanız şu kategorilerden üçünde yer alan karakterlerden oluşmalıdır: İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0 - 9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).|
    Region|Kullanıcılarınıza en yakın bölge| Kullanıcılarınız için en yakın olan konum.|
    Sürüm|5.7| MySQL ana sürümü.|
    İşlem + depolama | **Burstable**, **Standard_B1ms**, **10 GiB**, **7 gün** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Sunucuyu Yapılandır**' ı seçin. *Burstable*, *Standard_B1ms*, *10 GiB*ve *7 gün* ,     **Işlem katmanı**, **işlem boyutu**, **depolama**ve **yedekleme saklama dönemi**için varsayılan değerlerdir. Bu kaydırıcıları olduğu gibi bırakabilir veya düzenleyebilirsiniz. Bu işlem ve depolama seçimini kaydetmek için, yapılandırmalara devam etmek için **Kaydet** ' i seçin. Aşağıdaki ekran görüntüsünde işlem ve depolama seçenekleri gösterilmektedir.|
    
    >[!div class="mx-imgBorder"]
    > :::image type="content" source="./media/quickstart-create-server-portal/compute-storage.png" alt-text="İşlem + depolama":::

5. Ağ seçeneklerini yapılandırma

    Ağ sekmesinde, sunucunuza nasıl ulaşılabildiğini seçebilirsiniz. MySQL için Azure veritabanı esnek sunucu, sunucunuza *genel erişim (izin VERILEN IP adresleri)* ve *özel erişim (VNET tümleştirmesi)* aracılığıyla bağlanmak için iki seçenek sunar. *Genel erişim (izin VERILEN IP adresleri)* ile sunucunuza erişim, bir güvenlik duvarı kuralına eklenen ızın verilen IP adresleriyle sınırlıdır. Belirli bir IP adresi veya aralığı için güvenlik duvarını açmak üzere bir kural oluşturmadığınız müddetçe, dış uygulamaların ve araçların sunucuya ve sunucu üzerindeki veritabanlarına bağlanmasını engeller. *Özel erişim (VNET tümleştirmesi)* ile, sunucunuza erişim sanal ağınızla sınırlıdır. Bu hızlı başlangıçta, sunucuya bağlanmak için genel erişimin nasıl etkinleştirileceğini göstereceğiz. [Kavramlar makalesindeki](./concepts-networking.md)bağlantı yöntemleri hakkında daha fazla bilgi edinin.

    > [!NOTE]
    > Sunucu oluşturulduktan sonra bağlantı yöntemi değiştirilemez. Örneğin, oluştur sırasında *ortak erişim (izin VERILEN IP adresleri)* seçtiyseniz, oluşturma Işleminden sonra *özel erişim (VNET tümleştirmesi)* olarak değiştirilemez. VNet tümleştirmesini kullanarak sunucunuza güvenli bir şekilde erişmek için özel erişime sahip bir sunucu oluşturmanız önemle önerilir. [Kavramlar makalesinde](./concepts-networking.md)özel erişim hakkında daha fazla bilgi edinin.

    >[!div class="mx-imgBorder"]
    > :::image type="content" source="./media/quickstart-create-server-portal/networking.png" alt-text="Ağ yapılandırması":::  

    <!--:::image type="content" source="./media/quickstart-create-database-portal/6-add-client-ip.png" alt-text="Select "Add current client IP address"":::-->

6. Esnek sunucu yapılandırmanızı gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

7. Sunucuyu sağlamak için **Oluştur**’u seçin. Sağlama birkaç dakika sürebilir.

8. Araç çubuğunda **Bildirimler**’i (zil simgesi) seçip dağıtım işlemini izleyin. Dağıtım tamamlandıktan sonra **panoya sabitle**' yi seçerek, Azure Portal panonuzda bu esnek sunucu Için sunucunun **genel bakış** sayfasına kısayol olarak bir kutucuk oluşturulur. **Kaynağa git**’i seçmek sunucunun **Genel bakış** sayfasını açar.

Varsayılan olarak, sunucunuz altında aşağıdaki veritabanları oluşturulur: **information_schema**, **mysql**, **performance_schema** ve **sys**.

> [!NOTE]
> Ağınızın bağlantı sorunlarını önlemek için MySQL esnek sunucusu için Azure veritabanı tarafından kullanılan 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verdiğinden emin olun.  

## <a name="connect-to-using-mysql-command-line-client"></a>MySQL komut satırı istemcisini kullanarak bağlanma

Esnek sunucunuzu *özel erişim (VNET tümleştirmesi)* ile oluşturduysanız, sunucunuza aynı VNET içindeki bir kaynaktan sunucunuza bağlanmanız gerekir. Bir sanal makine oluşturabilir ve bunu esnek sunucunuz ile oluşturulan VNet 'e ekleyebilirsiniz.

Esnek sunucunuzu *ortak erişim (izin VERILEN IP adresleri)* ile oluşturduysanız, sunucunuzdaki güvenlik duvarı kuralları lıstesıne yerel IP adresinizi ekleyebilirsiniz.

Yerel ortamınızdan sunucusuna bağlanmak için [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) veya [MySQL çalışma ekranı](./connect-workbench.md) seçeneğini belirleyebilirsiniz. 

mysql.exe, aşağıdaki komutu kullanarak bağlanın. Değerleri gerçek sunucu adı ve parolasıyla değiştirin. 

```bash
 mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p
```
## <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kaynak grubunda MySQL için Azure veritabanı esnek sunucusunu başarıyla oluşturdunuz.  Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu silerek veya MySQL sunucusunu silmeniz durumunda bunları silebilirsiniz. Kaynak grubunu silmek için şu adımları izleyin:

1. Azure portal, **kaynak gruplarını**arayıp seçin.
1. Kaynak grubu listesinde, kaynak grubunuzun adını seçin.
1. Kaynak grubunuzun Genel Bakış sayfasında **kaynak grubunu sil**' i seçin.
1. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

Sunucuyu silmek için aşağıda gösterildiği gibi sunucunuzun **genel bakış** sayfasında **Sil** düğmesine tıklayabilirsiniz:

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-server-portal/delete-server.png" alt-text="Kaynaklarınızı silme":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MySQL ile PHP (Laralevel) Web uygulaması oluşturma](tutorial-php-database-app.md)
