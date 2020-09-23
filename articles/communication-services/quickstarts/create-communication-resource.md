---
title: Hızlı başlangıç-Azure Iletişim hizmetlerinde kaynak oluşturma ve yönetme
titleSuffix: An Azure Communication Services quickstart
description: Bu hızlı başlangıçta, ilk Azure Iletişim Hizmetleri kaynağınızı oluşturmayı ve yönetmeyi öğreneceksiniz.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
zone_pivot_groups: acs-plat-azp-net
ms.openlocfilehash: 3ba3d8d4b345c9db2e41c647147021eb9616288e
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948300"
---
# <a name="quickstart-create-and-manage-communication-services-resources"></a>Hızlı başlangıç: Iletişim Hizmetleri kaynaklarını oluşturma ve yönetme

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

İlk Iletişim Hizmetleri kaynağınızı sağlayarak Azure Iletişim Hizmetleri ile çalışmaya başlayın. İletişim Hizmetleri kaynakları, Azure portal veya .NET Management istemci kitaplığı ile sağlanabilir. Yönetim istemci kitaplığı, [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview), Azure 'un dağıtım ve yönetim hizmeti ile kaynak ve arabirimlerinizi oluşturmanıza, yapılandırmanıza, güncelleştirmenize ve silmesine izin verir. İstemci kitaplıklarında bulunan tüm işlevler Azure portal kullanılabilir. 

> [!WARNING]
> Iletişim Hizmetleri kullanılabilirliğinin genel önizleme sırasında ABD coğrafya ile sınırlı olduğunu unutmayın. Ayrıca, genel önizleme sırasında iletişim kaynaklarının farklı bir aboneliğe aktarılamayacağını unutmayın.

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/create-resource-azp.md)]
::: zone-end

::: zone pivot="platform-net"
[!INCLUDE [.NET](./includes/create-resource-net.md)]
::: zone-end

## <a name="access-your-connection-strings-and-service-endpoints"></a>Bağlantı Dizeleriniz ve hizmet uç noktalarınıza erişin

Bağlantı dizeleri Iletişim Hizmetleri istemci kitaplıklarının Azure 'a bağlanmasına ve kimlik doğrulamasından geçmesini sağlar. Iletişim Hizmetleri bağlantı Dizeleriniz ve hizmet uç noktalarınıza Azure portal veya program aracılığıyla Azure Resource Manager API 'lerle erişebilirsiniz. 

Iletişim Hizmetleri kaynağınız üzerinde gezindikten sonra, gezinti menüsünden **anahtarlar** ' ı seçin ve iletişim hizmeti istemci kitaplıklarının kullanımı için **bağlantı dizesini** veya **uç nokta** değerlerini kopyalayın. Birincil ve ikincil anahtarlara erişiminizin olduğunu unutmayın. Bu, Iletişim Hizmetleri kaynaklarınıza üçüncü tarafa veya hazırlama ortamına geçici erişim sağlamak istediğiniz senaryolarda yararlı olabilir.

:::image type="content" source="./media/key.png" alt-text="Iletişim Hizmetleri anahtar sayfasının ekran görüntüsü.":::

## <a name="store-your-connection-string"></a>Bağlantı dizenizi depolayın

İletişim Hizmetleri istemci kitaplıkları Iletişim hizmetlerine yapılan istekleri yetkilendirmek için bağlantı dizelerini kullanır. Bağlantı dizenizi depolamak için çeşitli seçenekleriniz vardır:

* Masaüstünde veya cihazda çalışan bir uygulama, bağlantı dizesini bir **app.config** veya **web.config** dosyasında saklayabilir. Bağlantı dizesini bu dosyalardaki **appSettings** bölümüne ekleyin.
* Azure App Service çalışan bir uygulama, bağlantı dizesini [App Service uygulama ayarlarında](https://docs.microsoft.com/azure/app-service/configure-common)saklayabilir. Bağlantı dizesini Portal içindeki uygulama ayarları sekmesinin **bağlantı dizeleri** bölümüne ekleyin.
* Bağlantı dizenizi [Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)' de saklayabilirsiniz.
* Uygulamanızı yerel olarak çalıştırıyorsanız, Bağlantı dizenizi bir ortam değişkeninde depolamak isteyebilirsiniz.

### <a name="store-your-connection-string-in-an-environment-variable"></a>Bağlantı dizenizi bir ortam değişkeninde depolayın

Bir ortam değişkenini yapılandırmak için, bir konsol penceresi açın ve aşağıdaki sekmelerden işletim sisteminizi seçin. `<yourconnectionstring>`Gerçek bağlantı dizeniz ile değiştirin.

#### <a name="windows"></a>[Windows](#tab/windows)

Bir konsol penceresi açın ve aşağıdaki komutu girin:

```console
setx COMMUNICATION_SERVICES_CONNECTION_STRING "<yourconnectionstring>"
```

Ortam değişkenini ekledikten sonra, konsol penceresi de dahil olmak üzere ortam değişkenini okumak için gereken tüm çalışan programları yeniden başlatmanız gerekebilir. Örneğin, düzenleyiciniz olarak Visual Studio kullanıyorsanız, örneği çalıştırmadan önce Visual Studio 'Yu yeniden başlatın.

#### <a name="macos"></a>[macOS](#tab/unix)

**. Zshrc**'nizi düzenleyin ve ortam değişkenini ekleyin:

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.zshrc` çalıştırın. IDE 'niz açıkken ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekebilir.

#### <a name="linux"></a>[Linux](#tab/linux)

**. Bash_profile**düzenleyin ve ortam değişkenini ekleyin:

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bash_profile` çalıştırın. IDE 'niz açıkken ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekebilir.

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

Kaynak silme işleminin ardından kaynağınız için atanan telefon numaranız varsa, telefon numaraları otomatik olarak kaynağından aynı anda serbest bırakılır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrenirsiniz:

> [!div class="checklist"]
> * Iletişim Hizmetleri kaynağı oluşturma
> * Kaynak Coğrafya ve etiketlerini yapılandırma
> * Bu kaynağın anahtarlarına erişin
> * Kaynağı Sil

> [!div class="nextstepaction"]
> [İlk Kullanıcı erişim belirteçlerinizi oluşturma](access-tokens.md)
