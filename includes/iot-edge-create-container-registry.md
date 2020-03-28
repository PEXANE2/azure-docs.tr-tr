---
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/30/2019
ms.author: kgremban
ms.openlocfilehash: f63510771e4bd71a3ab6cf048bc5fb5296042a4d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75564705"
---
## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu öğreticide, bir modül oluşturmak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için Azure IoT Araçları uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.

Konteyner görüntülerinizi tutmak için Docker uyumlu herhangi bir kayıt defteri kullanabilirsiniz. İki popüler Docker kayıt defteri hizmeti [Azure Konteyner Kayıt Defteri](https://docs.microsoft.com/azure/container-registry/) ve Docker [Hub'dır.](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) Bu öğreticide Azure Container Registry kullanılır.

Zaten bir kapsayıcı kayıt defteriniz yoksa, Azure'da yeni bir tane oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

2. Kapsayıcı kayıt defterinizi oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer |
   | ----- | ----- |
   | Kayıt defteri adı | Benzersiz bir ad girin. |
   | Abonelik | Açılan listeden bir abonelik seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. |
   | Yönetici kullanıcı | **Etkinleştir**'i seçin. |
   | SKU | **Temel**'i seçin. |

3. **Oluştur'u**seçin.

4. Konteyner kayıt defteriniz oluşturulduktan sonra, ona göz atın ve sol bölmeden **Ayarlar'ın**altında bulunan menüden **Erişim tuşlarını** seçin.

5. **Giriş sunucusu,** **Kullanıcı Adı**ve **Şifre** değerlerini kopyalayın ve uygun bir yere kaydedin. Bu değerleri, kapsayıcı kayıt defterine erişim sağlamak için bu öğretici boyunca kullanırsınız.

   ![Konteyner kayıt defteri için giriş sunucusunu, kullanıcı adını ve parolayı kopyalama](./media/iot-edge-create-container-registry/registry-access-key.png)