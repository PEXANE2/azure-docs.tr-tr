---
title: Azure Uygulama Hizmetine Bağlanın - MySQL için Azure Veritabanı
description: MySQL için mevcut bir Azure Uygulama Hizmetinin Azure Veritabanına düzgün bir şekilde bağlanma sına yönelik yönergeler
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: ba75daa47ca1f77cd5828d13877238a64deeeb41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062456"
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>MySQL sunucusu için mevcut bir Azure Uygulama Hizmetini Azure Veritabanına bağlayın
Bu konu, varolan bir Azure Uygulama Hizmeti'ni MySQL sunucusu için Azure Veritabanınıza nasıl bağlayabilirsiniz açıklar.

## <a name="before-you-begin"></a>Başlamadan önce
[Azure portalında](https://portal.azure.com)oturum açın. MySQL sunucusu için bir Azure Veritabanı oluşturun. Ayrıntılar için, [Portal'dan MySQL sunucusu için Azure Veritabanı nasıl oluşturulur](quickstart-create-mysql-server-database-using-azure-portal.md) veya [CLI kullanarak MySQL sunucusu için Azure Veritabanı nasıl oluşturulur'a](quickstart-create-mysql-server-database-using-azure-cli.md)bakın.

Şu anda, Bir Azure Uygulama Hizmeti'nden MySQL için bir Azure Veritabanına erişimi etkinleştirmek için iki çözüm vardır. Her iki çözüm de sunucu düzeyinde güvenlik duvarı kurallarıayarlamayı içerir.

## <a name="solution-1---allow-azure-services"></a>Çözüm 1 - Azure hizmetlerine izin ver
MySQL için Azure Veritabanı, verilerinizi korumak için bir güvenlik duvarı kullanarak erişim güvenliği sağlar. Bir Azure Uygulama Hizmeti'nden MySQL sunucusu için Azure Veritabanı'na bağlanırken, Uygulama Hizmeti'nin giden IP'lerinin dinamik olduğunu unutmayın. "Azure hizmetlerine erişime izin ver" seçeneğini seçmek, uygulama hizmetinin MySQL sunucusuna bağlanmasını sağlar.

1. MySQL sunucu çubuğunda, Ayarlar başlığı altında, MySQL için Azure Veritabanı için Bağlantı Güvenliği bıyısını açmak için **Bağlantı Güvenliği'ni** tıklatın.

   ![Azure portalı - Bağlantı Güvenliği'ni tıklatın](./media/howto-connect-webapp/1-connection-security.png)

2. **Azure hizmetlerine erişime izin ver'de A.B.'yi**seçin ve ardından **Kaydet'** i seçin. **ON**
   ![Azure portalı - Azure erişimine izin ver](./media/howto-connect-webapp/allow-azure.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>Çözüm 2 - Giden IP'lere açıkça izin vermek için bir güvenlik duvarı kuralı oluşturma
Azure Uygulama Hizmetinizin giden tüm IP'lerini açıkça ekleyebilirsiniz.

1. App Service Properties bıyeninde, **GIDEN IP ADRESİ'nizi**görüntüleyin.

   ![Azure portalı - Giden IP'leri görüntüle](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. MySQL Bağlantısı güvenlik bıçağında giden IP'leri tek tek ekleyin.

   ![Azure portalı - Açık IP'ler ekleme](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Güvenlik duvarı kurallarınızı **kaydetmeyi** unutmayın.

Azure Uygulaması hizmeti IP adreslerini zaman içinde sabit tutmaya çalışmasa da, IP adreslerinin değişebileceği durumlar vardır. Örneğin, bu durum uygulama geri dönüşümleri veya ölçek işlemi gerçekleştiğinde veya kapasiteyi artırmak için Azure bölgesel veri merkezlerine yeni bilgisayarlar eklendiğinde oluşabilir. IP adresleri değiştiğinde, uygulama MySQL sunucusuna artık bağlanamaması durumunda kapalı kalma süresi yaşayabilir. Önceki çözümlerden birini seçerken bu hususu göz önünde bulundurun.

## <a name="ssl-configuration"></a>SSL yapılandırması
MySQL için Azure Veritabanı varsayılan olarak SSL özelliklidir. Uygulamanız veritabanına bağlanmak için SSL kullanmıyorsa, MySQL sunucusunda SSL'yi devre dışı bmalısınız. SSL'nin nasıl yapılandırılabildiğini öğrenmek [için MySQL için Azure Veritabanı ile SSL kullanma bilgisine](howto-configure-ssl.md)bakın.

### <a name="django-pymysql"></a>Django (PyMySQL)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'quickstartdb',
        'USER': 'myadmin@mydemoserver',
        'PASSWORD': 'yourpassword',
        'HOST': 'mydemoserver.mysql.database.azure.com',
        'PORT': '3306',
        'OPTIONS': {
            'ssl': {'ssl-ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Bağlantı dizeleri hakkında daha fazla bilgi için [Bağlantı Dizeleri'ne](howto-connection-string.md)bakın.
