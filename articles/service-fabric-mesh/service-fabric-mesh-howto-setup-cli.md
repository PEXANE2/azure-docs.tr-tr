---
title: Azure Hizmet Kumaş Kafesi CLI'yi ayarlama
description: Hizmet Kumaş Kafesi Komut Hattı Arabirimi (CLI), kaynakları yerel olarak ve Azure Service Fabric Mesh'te dağıtmak ve yönetmek için gereklidir. Bunu şu şekilde ayarlayabilirsiniz.
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.openlocfilehash: 0fb65ceeabca9331130083f8ec5b3fe8acce13b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259194"
---
# <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama
Hizmet Kumaş Kafesi Komut Hattı Arabirimi (CLI), kaynakları yerel olarak ve Azure Service Fabric Mesh'te dağıtmak ve yönetmek için gereklidir. Bunu şu şekilde ayarlayabilirsiniz.

Kullanılabilecek üç tip CLI vardır ve bunlar aşağıdaki tabloda özetlenmiştir.

| CLI Modülü | Hedef Ortam |  Açıklama | 
|---|---|---|
| az örgü | Azure Servis Kumaş Örgü | Uygulamalarınızı dağıtmanızı ve kaynaklarıNızı Azure Hizmet Kumaş Kafesi ortamına göre yönetmeniziçin birincil CLI. 
| sfctl | Yerel kümeler | Service Fabric kaynaklarının yerel kümelere karşı dağıtımına ve test edilmesine olanak tanıyan Hizmet Kumaşı CLI.  
| Maven CLI | Azure Hizmet Kumaş Kafesi & yerel kümeler | Java `az mesh` geliştiricilerinin `sfctl` yerel ve Azure geliştirme deneyimi için tanıdık bir komut satırı deneyimi kullanmasına olanak tanıyan bir sarmalayıcı.  

Önizleme için, Azure Fabric Mesh CLI Azure CLI’nin uzantısı olarak yazılır. Azure Cloud Shell’de veya Azure CLI’nin yerel kurulumunda bunu yükleyebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-azure-service-fabric-mesh-cli"></a>Azure Hizmet Kumaş Kafesi CLI'yi yükleyin
1. Azure CLI sürümünü 2.0.67 veya sonraki sürümyüklemeniz gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’nin en son sürümünü yüklemek veya en son sürümüne yükseltmek için bkz. [Azure CLI yükleme][azure-cli-install].

2. Aşağıdaki komutu kullanarak Azure Hizmet Kumaş ıslımı CLI uzantı modüllerini yükleyin. 

    ```azurecli-interactive
    az extension add --name mesh
    ```

3. Aşağıdaki komutu kullanarak varolan azure hizmet kumaş örgü cli modüllerini güncelleştirin.

    ```azurecli-interactive
    az extension update --name mesh
    ```

## <a name="install-the-service-fabric-cli-sfctl"></a>Servis Kumaşı CLI (sfctl) yükleyin 

[Servis Kumaş CLI kurulum](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli)yönergeleri izleyin. **Sfctl** modülü, yerel makinenizde Servis Kumaşı kümelerine karşı kaynak modeline dayalı uygulamaların dağıtımı için kullanılabilir. 

## <a name="install-the-maven-cli"></a>Maven CLI'yi yükleyin 

Maven CLI'yi kullanabilmek için makinenize aşağıdakilerin yüklenmesi gerekir: 

* [Java](https://www.azul.com/downloads/zulu/)
* [Maven](https://maven.apache.org/download.cgi)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* Azure Mesh CLI (az mesh) - Azure Hizmet Kumaş Örgü hedeflemek için 
* SFCTL (sfctl) - Yerel kümeleri hedeflemek için 

Hizmet Kumaşı için Maven CLI hala önizlemede. 

Maven Java uygulamanızdaki Maven eklentisini kullanmak için pom.xml dosyanıza aşağıdaki parçacıkları ekleyin:

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

Ayrıntılı kullanım hakkında bilgi edinmek için [Maven CLI başvuru](service-fabric-mesh-reference-maven.md) bölümünü okuyun.

## <a name="next-steps"></a>Sonraki adımlar

[Windows dağıtım ortamınızı](service-fabric-mesh-howto-setup-developer-environment-sdk.md) da ayarlayabilirsiniz.

[Sık sorulan soruların ve sorunların](service-fabric-mesh-faq.md) yanıtlarını bulun.

[azure-cli-install]: /cli/azure/install-azure-cli
