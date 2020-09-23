---
title: 'Öğretici: MySQL için Azure veritabanı esnek sunucu (Önizleme) ve aynı sanal ağda Azure App Service Web uygulaması oluşturma'
description: Bir sanal ağda Web uygulamasıyla MySQL için Azure veritabanı esnek sunucusu (Önizleme) oluşturmak için hızlı başlangıç kılavuzu
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 9/21/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 9cbcbeb9d9e216a0b686cba258288db8439e0a9c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90946759"
---
# <a name="tutorial-create-an-azure-database-for-mysql---flexible-server-preview-with-app-services-web-app-in-virtual-network"></a>Öğretici: Sanal ağda uygulama Hizmetleri Web uygulamasıyla bir MySQL için Azure veritabanı (Önizleme) oluşturma

> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

Bu öğreticide, bir [sanal ağ](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)Içinde MySQL esnek sunucusu (Önizleme) Ile Azure App Service Web uygulaması oluşturma işlemlerinin nasıl yapılacağı gösterilir.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu makalede, Azure CLı sürüm 2,0 veya üstünü yerel olarak çalıştırıyor olmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

[Az Login](https://docs.microsoft.com/cli/azure/reference-index#az-login) komutunu kullanarak hesabınızda oturum açmanız gerekir. Komut çıktısındaki ilgili abonelik adına karşılık gelen **id** özelliğinin değerini not edin.

```azurecli
az login
```

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. **Abonelik kimliği özelliğini aboneliğiniz** için **az oturum açma** çıktısından abonelik kimliği yer tutucusuna koyun.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-mysql-flexible-server"></a>MySQL için Azure veritabanı esnek sunucusu oluşturma

Aşağıdaki komutu kullanarak bir sanal ağ (VNET) içinde özel bir esnek sunucu oluşturun:
```azurecli
az mysql flexible-server create --resource-group myresourcegroup --location westus2
```
Bu komut, birkaç dakika sürebilen aşağıdaki eylemleri gerçekleştirir:

- Zaten mevcut değilse kaynak grubunu oluşturun.
- Sağlanmazsa bir sunucu adı oluşturur.
- Yeni MySQL sunucunuz için yeni bir sanal ağ oluşturun. Web uygulamasını aynı sanal ağa eklemeniz gerektiğinden, sunucunuz için oluşturulan sanal ağ adını ve alt ağ adını bir yere unutmayın.
- Sağlanmazsa, sunucunuz için Yönetici Kullanıcı adı, parola oluşturur.
- **Flexibtaserverdb** adlı boş bir veritabanı oluşturur

> [!NOTE]
> Sağlanmazsa, sizin için üretilecek olan parolanızı not edin. Parolayı unutursanız, komutunu kullanarak parolayı sıfırlamanız gerekir ``` az mysql flexible-server update```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Bu bölümde App Service uygulamasında uygulama konağı oluşturup bu uygulamayı MySQL veritabanına bağlayaöğreneceksiniz. Terminalde uygulama kodunuzun depo kökünde olduğunuzdan emin olun.

Az WebApp up komutuyla bir App Service uygulaması (ana bilgisayar işlemi) oluşturun

```azurecli
az webapp up --resource-group myresourcegroup --location westus2 --plan testappserviceplan --sku B1 --name mywebapp
```

> [!NOTE]
> - --Location bağımsız değişkeni için, önceki bölümde veritabanıyla aynı konumu kullanın.
> - _ &lt; App-name>_ tüm Azure genelinde benzersiz bir adla değiştirin (sunucu uç noktası https:// \<app-name> . azurewebsites.net). <app-name> için izin verilen karakterler A-Z, 0-9 ve-. İyi bir model, şirketinizin adının ve uygulama tanımlayıcısının bir birleşimini kullanmaktır.

Bu komut, birkaç dakika sürebilen aşağıdaki eylemleri gerçekleştirir:

- Zaten mevcut değilse kaynak grubunu oluşturun. (Bu komutta veritabanını daha önce oluşturduğunuz kaynak grubunu kullanırsınız.)
- ```testappserviceplan```Mevcut değilse, temel fiyatlandırma katmanında (B1) App Service planı oluşturun. --plan ve--SKU isteğe bağlıdır.
- Yoksa App Service uygulamasını oluşturun.
- Zaten etkinleştirilmemişse, uygulama için varsayılan günlüğü etkinleştirin.
- Derleme Otomasyonu etkinken ZIP dağıtımını kullanarak depoyu karşıya yükleyin.

## <a name="add-the-web-app-to-the-virtual-network"></a>Web uygulamasını sanal ağa ekleme

Bir WebApp 'ye bölgesel sanal ağ tümleştirmesi eklemek için **az WebApp VNET-Integration** komutunu kullanın. _ &lt; VNET-Name>_ ve _ &lt; alt ağ adını_ , esnek sunucunun kullandığı sanal ağ ve alt ağ adıyla değiştirin.

```azurecli
az webapp vnet-integration add -g myresourcegroup -n  mywebapp --vnet <vnet-name> --subnet <subnet-name>
```

## <a name="configure-environment-variables-to-connect-the-database"></a>Veritabanını bağlamak için ortam değişkenlerini yapılandırma

Kod artık App Service 'e dağıtıldığına göre, bir sonraki adım, uygulamayı Azure 'daki esnek sunucuya bağlayabilmektedir. Uygulama kodu, çeşitli ortam değişkenlerinde veritabanı bilgilerini bulmayı bekler. App Service ortam değişkenlerini ayarlamak için set komutuyla "uygulama ayarları" oluşturursunuz ```az webapp config appsettings``` .

```azurecli
az webapp config appsettings set --settings DBHOST="<mysql-server-name>.mysql.database.azure.com" DBNAME="flexibleserverdb" DBUSER="<username>" DBPASS="<password>"
```

- Yeni oluşturulan esnek sunucu komutu için _ &lt; MySQL-Server-Name>_, _ &lt; UserName>_ ve _ &lt; Password>_ yerine koyun.
- _ &lt; UserName>_ ve _ &lt; Password>_ komutunun sizin için de oluşturduğu kimlik bilgileriyle değiştirin.
- Kaynak grubu ve uygulama adı,. Azure/config dosyasındaki önbelleğe alınmış değerlerden çizilir.
- Komut DBHOST, DBNAME, DBUSER ve DBPASS adlı ayarları oluşturur. Uygulama kodunuz veritabanı bilgileri için farklı bir ad kullanıyorsa, bu adları kodda belirtildiği gibi uygulama ayarları için kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aşağıdaki komutu kullanarak öğreticide oluşturduğunuz tüm kaynakları temizleyin. Bu komut, bu kaynak grubundaki tüm kaynakları siler.

```azurecli
az group delete -n myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure App Service eşleme](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain)
