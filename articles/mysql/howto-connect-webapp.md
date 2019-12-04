---
title: Azure App Service bağlanma-MySQL için Azure veritabanı
description: Mevcut bir Azure App Service MySQL için Azure veritabanı 'na doğru şekilde bağlama yönergeleri
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 63ef9ac55fcfaebfd58ae1ccdb34107d41900be5
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770552"
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Mevcut bir Azure App Service MySQL sunucusu için Azure veritabanı 'na bağlama
Bu konu, mevcut bir Azure App Service MySQL Server için Azure veritabanınıza nasıl bağlayabileceğinizi açıklamaktadır.

## <a name="before-you-begin"></a>Başlamadan önce
[Azure Portal](https://portal.azure.com)’ında oturum açın. MySQL için Azure veritabanı sunucusu oluşturun. Ayrıntılar için [Portal 'Dan MySQL Için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md) veya [CLI kullanarak MySQL için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-cli.md)bölümüne bakın.

Şu anda bir Azure App Service erişimi bir MySQL için Azure veritabanı 'na etkinleştirmeye yönelik iki çözüm vardır. Her iki çözüm de sunucu düzeyinde güvenlik duvarı kurallarını ayarlamayı içerir.

## <a name="solution-1---allow-azure-services"></a>Çözüm 1-Azure hizmetlerine Izin ver
MySQL için Azure veritabanı, verilerinizi korumak için bir güvenlik duvarı kullanarak erişim güvenliği sağlar. Bir Azure App Service MySQL sunucusu için Azure veritabanı 'na bağlanırken, App Service giden IP 'Lerinin doğası gereği dinamik olduğunu aklınızda bulundurun. "Azure hizmetlerine erişime Izin ver" seçeneği belirlendiğinde App Service 'in MySQL sunucusuna bağlanmasına izin vermeyecektir.

1. MySQL sunucusu dikey penceresinde, ayarlar başlığı altında **bağlantı güvenliği** ' ne tıklayarak MySQL Için Azure veritabanı bağlantı güvenliği dikey penceresini açın.

   ![Azure portal-bağlantı güvenliği ' ne tıklayın](./media/howto-connect-webapp/1-connection-security.png)

2. **Azure hizmetlerine erişime Izin ver**' in **üzerinde** seçim yapın, sonra **kaydedin**.
   ![Azure portal-Azure erişimine Izin ver](./media/howto-connect-webapp/allow-azure.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>Çözüm 2-giden IP 'Lere açıkça izin vermek için bir güvenlik duvarı kuralı oluşturma
Azure App Service giden tüm IP 'lerini açık olarak ekleyebilirsiniz.

1. App Service Özellikler dikey penceresinde **gıden IP adresinizi**görüntüleyin.

   ![Azure portal-giden IP 'Leri görüntüleme](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. MySQL bağlantısı güvenliği dikey penceresinde, giden IP 'Leri tek tek ekleyin.

   ![Azure portal-açık IP 'Leri ekleme](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Güvenlik Duvarı kurallarınızı **kaydetmeyi** unutmayın.

Azure App Service, zaman içinde IP adreslerini sabit tutmaya çalışırsa, IP adreslerinin değiştirebildiği durumlar vardır. Örneğin, bu durum, uygulama geri dönüştürme veya bir ölçeklendirme işlemi gerçekleştiğinde ya da kapasiteyi artırmak için Azure bölgesel veri merkezlerinde yeni bilgisayarlar eklendiğinde ortaya çıkabilir. IP adresleri değiştiğinde, uygulama artık MySQL sunucusuna bağlanmayacak olayda kapalı kalma süresine yol açabilir. Yukarıdaki çözümlerden birini seçerken bunu göz önünde bulundurun.

## <a name="ssl-configuration"></a>SSL yapılandırması
MySQL için Azure veritabanı varsayılan olarak SSL 'yi etkinleştirdi. Uygulamanız veritabanına bağlanmak için SSL kullanmıyorsanız, MySQL sunucusunda SSL 'yi devre dışı bırakmanız gerekir. SSL 'yi yapılandırma hakkında ayrıntılı bilgi için bkz. [MySQL Için Azure veritabanı Ile SSL kullanma](howto-configure-ssl.md).

### <a name="django-pymysql"></a>Docgo (PyMySQL)
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
Bağlantı dizeleri hakkında daha fazla bilgi için, [bağlantı dizelerine](howto-connection-string.md)bakın.
