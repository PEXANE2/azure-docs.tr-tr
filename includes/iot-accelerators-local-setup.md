---
title: dosya dahil etme
description: dosya dahil etme
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/17/2019
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: 1f567b3d083853f9bb342bfad462e8545caa6480
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188804"
---
## <a name="download-the-source-code"></a>Kaynak kodunu indirin

Uzaktan Izleme kaynak kodu depoları, mikro hizmetler Docker görüntülerini çalıştırmak için gereken kaynak kodu ve Docker yapılandırma dosyalarını içerir.

Deponun yerel bir sürümünü kopyalamak ve oluşturmak için, komut satırı ortamınızı kullanarak yerel makinenizde uygun bir klasöre gidin. Ardından, .NET deposunu kopyalamak için aşağıdaki komut kümelerinden birini çalıştırın:

.NET mikro hizmet uygulamalarının en son sürümünü indirmek için şunu çalıştırın:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

> [!NOTE]
> Bu komutlar, mikro hizmetleri yerel olarak çalıştırmak için kullandığınız betiklerin yanı sıra tüm mikro hizmetler için kaynak kodu indirir. Docker 'da mikro hizmetleri çalıştırmak için kaynak koda gerek duymasanız da, daha sonra çözüm hızlandırıcıyı değiştirmeyi ve değişikliklerinizi yerel olarak test etmek istiyorsanız kaynak kodu yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro hizmetlerin yerel olarak nasıl çalıştırılacağı gösterilmekle birlikte, bulutta çalışan Azure hizmetlerine bağımlıdır. Azure hizmetlerini dağıtmak için aşağıdaki betiği kullanın. Aşağıdaki betik örnekleri, bir Windows makinesinde .NET deposunu kullandığınızı varsayar. Başka bir ortamda çalışıyorsanız, yollar, dosya uzantıları ve yol ayırıcıları uygun şekilde ayarlayın.

### <a name="create-new-azure-resources"></a>Yeni Azure kaynakları oluşturma

Gerekli Azure kaynaklarını henüz oluşturmadıysanız, şu adımları izleyin:

1. Komut satırı ortamınızda, deponun kopyalanmış kopyasında **\services\scripts\local\launch** klasörüne gidin.

1. **PC** CLI aracını yüklemek ve Azure hesabınızda oturum açmak için aşağıdaki komutları çalıştırın:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. **Start. cmd** betiğini çalıştırın. Komut dosyası aşağıdaki bilgileri ister:
   * Bir çözüm adı.
   * Kullanılacak Azure aboneliği.
   * Kullanılacak Azure veri merkezinin konumu.

     Betik, Azure 'da çözüm adınızla kaynak grubu oluşturur. Bu kaynak grubu, çözüm hızlandırıcının kullandığı Azure kaynaklarını içerir. İlgili kaynaklara artık gerek kalmadığında, bu kaynak grubunu silebilirsiniz.

     Betik Ayrıca yerel makinenize önek **bilgisayarları** olan bir ortam değişkenleri kümesi de ekler. Bu ortam değişkenleri, uzaktan Izlemenin Azure Key Vault bir kaynaktan okuyabilmesini sağlayacak ayrıntıları sağlar. Bu Key Vault kaynak, uzaktan Izlemenin yapılandırma değerlerini okuacaktır.

     > [!TIP]
     > Betik tamamlandığında, ortam değişkenlerini ** \<your home folder\> \\ . PCs \\ \<solution name\> . env**adlı bir dosyaya de kaydeder. Bunları, gelecekteki Çözüm Hızlandırıcısı dağıtımları için kullanabilirsiniz. Yerel makinenizde ayarlanan tüm ortam değişkenlerinin, **Docker-Compose**çalıştırdığınızda **Hizmetler \\ betiklerinin \\ yerel \\ . env** dosyasında değerlerini geçersiz kıldığını unutmayın.

1. Komut satırı ortamınızdan çıkın.

### <a name="use-existing-azure-resources"></a>Mevcut Azure kaynaklarını kullanma

Gerekli Azure kaynaklarını zaten oluşturduysanız, yerel makinenizde karşılık gelen ortam değişkenlerini oluşturun.
Aşağıdaki için ortam değişkenlerini ayarlayın:
* **PCS_KEYVAULT_NAME** -Azure Key Vault kaynağının adı
* **PCS_AAD_APPID** -AAD uygulama kimliği
* **PCS_AAD_APPSECRET** -AAD uygulama gizli anahtarı

Yapılandırma değerleri bu Azure Key Vault kaynağından okunacaktır. Bu ortam değişkenleri, dağıtımdan ** \<your home folder\> \\ . PCs \\ \<solution name\> . env** dosyasına kaydedilebilir. Yerel makinenizde ayarlanan ortam değişkenlerinin, **Docker-Compose**çalıştırdığınızda **Hizmetler \\ KomutDosyaları \\ yerel \\ . env** dosyasındaki değerleri geçersiz kıldığını unutmayın.

Mikro hizmet tarafından gereken bazı yapılandırmalar ilk dağıtımda oluşturulmuş bir **Key Vault** örneğine depolanır. Anahtar kasasındaki karşılık gelen değişkenler gerektiği şekilde değiştirilmelidir.