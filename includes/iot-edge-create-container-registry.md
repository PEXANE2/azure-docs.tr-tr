---
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/30/2019
ms.author: kgremban
ms.openlocfilehash: f63510771e4bd71a3ab6cf048bc5fb5296042a4d
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75564705"
---
## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu öğreticide, Azure IoT araçları uzantısını kullanarak bir modül oluşturup dosyalardan bir **kapsayıcı görüntüsü** oluşturursunuz. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.

Herhangi bir Docker ile uyumlu kayıt defteri, kapsayıcı görüntülerinizi tutmak için kullanabilirsiniz. İki popüler Docker kayıt defteri Hizmetleri [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu öğreticide Azure Container Registry kullanılır.

Kapsayıcı kayıt defteri zaten yoksa, azure'da yeni bir tane oluşturmak için aşağıdaki adımları izleyin:

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

3. **Oluştur**’u seçin.

4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu dosyaya gidin ve sol bölmeden **Ayarlar**' ın altında bulunan menüden **erişim tuşları** ' nı seçin.

5. **Oturum açma sunucusu**, **Kullanıcı adı**ve **parola** değerlerini kopyalayın ve uygun bir yere kaydedin. Bu değerleri, kapsayıcı kayıt defterine erişim sağlamak için bu öğreticide kullanın.

   ![Kapsayıcı kayıt defteri için oturum açma sunucusu, Kullanıcı adı ve parola kopyalama](./media/iot-edge-create-container-registry/registry-access-key.png)