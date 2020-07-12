---
title: Azure Service Fabric ağı CLı 'sını ayarlama
description: Service Fabric ağ komut satırı arabirimi (CLı), kaynakları yerel olarak ve Azure Service Fabric ağı 'nda dağıtmak ve yönetmek için gereklidir. Nasıl ayarlanacağı aşağıda verilmiştir.
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.openlocfilehash: 3ce2dbcefa63cc55a66e712664d1436ca24d3a1d
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86248552"
---
# <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama
Service Fabric ağ komut satırı arabirimi (CLı), kaynakları yerel olarak ve Azure Service Fabric ağı 'nda dağıtmak ve yönetmek için gereklidir. Nasıl ayarlanacağı aşağıda verilmiştir.

Kullanılabilecek üç tür CLı bulunur ve bunlar aşağıdaki tabloda özetlenmiştir.

| CLı modülü | Hedef Ortam |  Açıklama | 
|---|---|---|
| az ağ | Azure Service Fabric ağı | Azure Service Fabric kafes ortamında uygulamalarınızı dağıtmanıza ve kaynakları yönetmenize olanak tanıyan birincil CLı. 
| sfctl | Yerel kümeler | Yerel kümelerdeki Service Fabric kaynaklarının dağıtımına ve test edilmesine izin veren Service Fabric CLı.  
| Maven CLı | Azure Service Fabric ağ & yerel kümeler | Ve çevresindeki bir sarmalayıcı `az mesh` , `sfctl` Java geliştiricilerinin yerel ve Azure geliştirme deneyimi için tanıdık bir komut satırı deneyimi kullanmasına olanak sağlar.  

Önizleme için, Azure Fabric Mesh CLI Azure CLI’nin uzantısı olarak yazılır. Azure Cloud Shell’de veya Azure CLI’nin yerel kurulumunda bunu yükleyebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-azure-service-fabric-mesh-cli"></a>Azure Service Fabric Kafesclı 'yi yükler
1. Azure CLı sürüm 2.0.67 veya üstünü yüklemelisiniz. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’nin en son sürümünü yüklemek veya en son sürümüne yükseltmek için bkz. [Azure CLI yükleme][azure-cli-install].

2. Aşağıdaki komutu kullanarak Azure Service Fabric kafes CLı uzantısı modülünü yükler. 

    ```azurecli-interactive
    az extension add --name mesh
    ```

3. Aşağıdaki komutu kullanarak mevcut bir Azure Service Fabric kafesi CLı modülünü güncelleştirin.

    ```azurecli-interactive
    az extension update --name mesh
    ```

## <a name="install-the-service-fabric-cli-sfctl"></a>Service Fabric CLı 'yi (sfctl) yükler 

[SERVICE fabrıc CLI 'Yi ayarlama](../service-fabric/service-fabric-cli.md)yönergelerini izleyin. **Sfctl** modülü, kaynak modeline bağlı olarak, yerel makinenizde Service Fabric kümelerine göre uygulama dağıtımı için kullanılabilir. 

## <a name="install-the-maven-cli"></a>Maven CLı 'yı yükler 

Maven CLı 'yı kullanabilmeniz için, makinenizde aşağıdakiler yüklü olmalıdır: 

* [Java](https://www.azul.com/downloads/zulu/)
* [Maven](https://maven.apache.org/download.cgi)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* Azure ağ CLı (az ağ)-Azure Service Fabric ağı hedeflemek Için 
* SFCTL (sfctl)-yerel kümeleri hedeflemek Için 

Service Fabric için Maven CLı hala önizlemededir. 

Maven Java uygulamanızda Maven eklentisini kullanmak için, pom.xml dosyanıza aşağıdaki kod parçacığını ekleyin:

```XML
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-sfmesh-maven-plugin</artifactId>
          <version>0.1.0</version>
          <configuration>
            ...
          </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Ayrıntılı kullanım hakkında bilgi edinmek için [Maven CLI başvurusu](service-fabric-mesh-reference-maven.md) bölümünü okuyun.

## <a name="next-steps"></a>Sonraki adımlar

[Windows dağıtım ortamınızı](service-fabric-mesh-howto-setup-developer-environment-sdk.md) da ayarlayabilirsiniz.

[Sık sorulan soruların ve sorunların](service-fabric-mesh-faq.md) yanıtlarını bulun.

[azure-cli-install]: /cli/azure/install-azure-cli
