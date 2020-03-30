---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/17/2019
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: 1f567b3d083853f9bb342bfad462e8545caa6480
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188804"
---
## <a name="download-the-source-code"></a>Kaynak kodunu indirin

Uzaktan İzleme kaynak kodu depoları, docker görüntülerini çalıştırmak için gereken kaynak kodu ve Docker yapılandırma dosyalarını içerir.

Deponun yerel bir sürümünü klonlamak ve oluşturmak için, yerel makinenizdeki uygun bir klasöre gitmek için komut satırı ortamınızı kullanın. Ardından ,.NET deposunu klonlamak için aşağıdaki komut kümelerinden birini çalıştırın:

.NET microservice uygulamalarının en son sürümünü indirmek için aşağıdakileri çalıştırın:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

> [!NOTE]
> Bu komutlar, mikro hizmetleri yerel olarak çalıştırmak için kullandığınız komutlara ek olarak tüm mikro hizmetlerin kaynak kodunu karşıdan yükler. Docker'daki mikro hizmetleri çalıştırmak için kaynak koduna ihtiyacınız olmasa da, daha sonra çözüm hızlandırıcısını değiştirmeyi ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız kaynak kodu yararlıdır.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro hizmetlerin yerel olarak nasıl çalıştırılacakları gösterilse de, bunlar bulutta çalışan Azure hizmetlerine bağlıdır. Azure hizmetlerini dağıtmak için aşağıdaki komut dosyasını kullanın. Aşağıdaki komut dosyası örnekleri, Windows makinesinde .NET deposunu kullandığınızı varsayar. Başka bir ortamda çalışıyorsanız, yolları, dosya uzantılarını ve yol ayırıcılarını uygun şekilde ayarlayın.

### <a name="create-new-azure-resources"></a>Yeni Azure kaynakları oluşturun

Gerekli Azure kaynaklarını henüz oluşturmadıysanız aşağıdaki adımları izleyin:

1. Komut satırı ortamınızda, deponun klonlanmış kopyasındaki **\services\scripts\local\launch** klasörüne gidin.

1. **Pcs** CLI aracını yüklemek ve Azure hesabınızda oturum açabilmek için aşağıdaki komutları çalıştırın:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. **start.cmd** komut dosyasını çalıştırın. Komut dosyası sizi aşağıdaki bilgiler için ister:
   * Çözüm adı.
   * Kullanılacak Azure aboneliği.
   * Kullanılacak Azure veri merkezinin konumu.

     Komut dosyası, Azure'da çözüm adınız ile kaynak grubu oluşturur. Bu kaynak grubu, çözüm hızlandırıcının kullandığı Azure kaynaklarını içerir. Karşılık gelen kaynaklara artık ihtiyacınız olmadığında bu kaynak grubunu silebilirsiniz.

     Komut dosyası ayrıca yerel makinenize bir önek **PCS** içeren bir ortam değişkenleri kümesi de ekler. Bu ortam değişkenleri, Bir Azure Anahtar Kasası kaynağından okuyabilmek için Uzaktan İzleme'nin ayrıntılarını sağlar. Bu Key Vault kaynağı, Uzaktan İzleme'nin yapılandırma değerlerini okuyacağı yerdir.

     > [!TIP]
     > Komut dosyası tamamlandığında, aynı zamanda ** \<ev klasörü\>\\.pcs\\\<çözüm adı\>.env**denilen bir dosyaya ortam değişkenleri kaydeder. Bunları gelecekteki çözüm hızlandırıcı dağıtımları için kullanabilirsiniz. Yerel makinenizde ayarlanan tüm ortam **değişkenlerinin, docker-compose'i**çalıştırdığınızda **yerel\\\\\\.env** dosyasındaki hizmetler komut dosyalarındaki değerleri geçersiz kıldığında dikkat edin.

1. Komut satırı ortamınızdan çıkın.

### <a name="use-existing-azure-resources"></a>Varolan Azure kaynaklarını kullanma

Gerekli Azure kaynaklarını zaten oluşturduysanız, yerel makinenizde karşılık gelen ortam değişkenlerini oluşturun.
Ortam değişkenlerini aşağıdakiler için ayarlayın:
* **PCS_KEYVAULT_NAME** - Azure Anahtar Kasası kaynağının adı
* **PCS_AAD_APPID** - AAD başvuru kimliği
* **PCS_AAD_APPSECRET** - AAD uygulama gizli

Yapılandırma değerleri bu Azure Anahtar Kasası kaynağından okunur. Bu ortam değişkenleri, dağıtımdan ** \<ev\>\\klasörünüze kaydedilebilir .pcs\\\<çözüm adı\>.env** dosyası. Yerel makinenizde ayarlanan ortam **değişkenlerinin, docker-compose'i**çalıştırdığınızda **yerel\\\\\\.env** dosyasındaki hizmetler komut dosyalarındaki değerleri geçersiz kıldığında dikkat edin.

Mikro hizmetin gerektirdiği yapılandırmanın bir kısmı, ilk dağıtımda oluşturulan **Key Vault** örneğinde depolanır. Keyvault'taki karşılık gelen değişkenler gerektiği gibi değiştirilmelidir.