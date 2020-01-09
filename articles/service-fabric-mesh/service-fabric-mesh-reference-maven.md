---
title: Azure Service Fabric kafes Maven başvurusu
description: Service Fabric ağı için Maven eklentisinin nasıl kullanılacağına ilişkin başvuruyu içerir
author: suhuruli
ms.author: suhuruli
ms.date: 11/26/2018
ms.topic: reference
ms.openlocfilehash: bcc3fb7c6c3adce0997d0960c4d98227089b048b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75459011"
---
# <a name="maven-plugin-for-service-fabric-mesh"></a>Service Fabric ağı için Maven eklentisi

## <a name="prerequisites"></a>Ön koşullar

- Java SDK
- Maven
- Ağ uzantısı ile Azure CLı
- Service Fabric CLI

## <a name="goals"></a>Hedefleri

### `azure-sfmesh:init`
- `application.yaml` dosyasına sahip bir `appresources` klasörü içeren `servicefabric` bir klasör oluşturur. 

### `azure-sfmesh:addservice`
- `servicefabric` klasörü içinde hizmet adıyla bir klasör oluşturur ve hizmetin YAML dosyasını oluşturur. 

### `azure-sfmesh:addnetwork`
- `appresources` klasöründe belirtilen ağ adına sahip bir `network` YAML oluşturur 

### `azure-sfmesh:addgateway`
- `appresources` klasöründe belirtilen ağ geçidi adına sahip bir `gateway` YAML oluşturur 

#### `azure-sfmesh:addvolume`
- `appresources` klasöründe, belirtilen birim adı ile bir `volume` YAML oluşturur.

### `azure-sfmesh:addsecret`
- `appresources` klasörde, belirtilen gizli adı taşıyan bir `secret` YAML oluşturur 

### `azure-sfmesh:addsecretvalue`
- `appresources` klasöründe, belirtilen gizli dizi ve gizli değer adı ile `secretvalue` YAML oluşturur 

### `azure-sfmesh:deploy`
- `servicefabric` klasöründeki yamls 'yi birleştirir ve geçerli klasörde JSON Azure Resource Manager şablonu oluşturur.
- Tüm kaynakları Azure Service Fabric kafes ortamına dağıtır 

### `azure-sfmesh:deploytocluster`
- Service Fabric kümeler için geçerli olan yamls 'lerden oluşturulan dağıtım Jlerini içeren bir klasör (`meshDeploy`) oluşturur
- Tüm kaynakları Service Fabric kümesine dağıtır
 

## <a name="usage"></a>Kullanım

Maven Java uygulamanızda Maven eklentisini kullanmak için, potm. xml dosyanıza aşağıdaki kod parçacığını ekleyin:

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
      </plugin>
    </plugins>
  </build>
</project>
```

## <a name="common-configuration"></a>Ortak yapılandırma

Maven eklentisi şu anda Azure için Maven eklentilerinin ortak yapılandırmasını desteklememektedir.

## <a name="how-to"></a>Nasıl Yapılır

### <a name="initialize-maven-project-for-azure-service-fabric-mesh"></a>Azure Service Fabric ağı için Maven projesini başlatma
Uygulama kaynağı YAML dosyasını oluşturmak için aşağıdaki komutu çalıştırın.

```cmd
mvn azure-sfmesh:init -DapplicationName=helloworldserver
```

- Kök klasörünüzde `servicefabric->appresources` adında bir uygulama YAML içeren bir klasör oluşturur `app_helloworldserver`

### <a name="add-resource-to-your-application"></a>Uygulamanıza kaynak ekleme

#### <a name="add-a-new-network-to-your-application"></a>Uygulamanıza yeni bir ağ ekleyin
Bir ağ kaynağı YAML oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addnetwork -DnetworkName=helloworldservicenetwork -DnetworkAddressPrefix=10.0.0.0/22
```

- `servicefabric->appresources` adlı klasörde bir ağ YAML oluşturur `network_helloworldservicenetwork`

#### <a name="add-a-new-service-to-your-application"></a>Uygulamanıza yeni bir hizmet ekleyin
Bir hizmet YAML oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addservice -DapplicationName=helloworldserver -DserviceName=helloworldservice -DimageName=helloworldserver:latest -DlistenerPort=8080 -DnetworkRef=helloworldservicenetwork
```

- `helloworldserver` uygulama & `helloworldservicenetwork` başvuran `service_helloworldservice` adlandırılmış `servicefabric->helloworldservice` klasörde bir hizmet YAML oluşturur
- Hizmet, 8080 numaralı bağlantı noktasını dinler
- Hizmet, bir ***helloworldserver: en son*** kapsayıcı görüntüsü olarak kullanılıyor.

#### <a name="add-a-new-gateway-resource-to-your-application"></a>Uygulamanıza yeni bir ağ geçidi kaynağı ekleyin
Bir ağ geçidi kaynağı YAML oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addgateway -DapplicationName=helloworldserver -DdestinationNetwork=helloworldservicenetwork -DgatewayName=helloworldgateway -DlistenerName=helloworldserviceListener -DserviceName=helloworldservice -DsourceNetwork=open -DtcpPort=80
```

- `servicefabric->appresources` adlı klasörde yeni bir ağ geçidi YAML oluşturur `gateway_helloworldgateway`
- Bu ağ geçidinden gelen çağrıları dinleyen hizmet dinleyicisi olarak `helloworldservicelistener` başvurur. Ayrıca hizmet olarak `helloworldservice`, ağ olarak `helloworldservicenetwork` ve uygulama olarak `helloworldserver` da başvurur. 
- 80 numaralı bağlantı noktasında istekleri dinler

#### <a name="add-a-new-volume-to-your-application"></a>Uygulamanıza yeni bir birim ekleyin
Bir birim kaynağı YAML oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addvolume -DvolumeAccountKey=key -DvolumeAccountName=name -DvolumeName=vol1 -DvolumeShareName=share
```

- `servicefabric->appresources` adlı klasörde bir birim YAML oluşturur `volume_vol1`
- Gerekli parametreler, `volumeAccountKey`ve `volumeShareName` özelliklerini yukarıda gösterildiği gibi ayarlar
- Oluşturulan bu birime nasıl başvurulacağını hakkında daha fazla bilgi için, [Azure dosyalarını kullanarak uygulamayı dağıtma](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md) bölümünü ziyaret edin.

#### <a name="add-a-new-secret-resource-to-your-application"></a>Uygulamanıza yeni bir gizli kaynak ekleyin
Gizli bir kaynak kaynağı oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addsecret -DsecretName=secret1
```

- `servicefabric->appresources` adlı klasörde bir gizli YAML oluşturur `secret_secret1`
- Oluşturulan bu gizliliğe başvurma hakkında daha fazla bilgi için aşağıdakileri ziyaret edin, [gizli dizileri yönetin](service-fabric-mesh-howto-manage-secrets.md)

#### <a name="add-a-new-secretvalue-resource-to-your-application"></a>Uygulamanıza yeni bir secretvalue kaynağı ekleyin
Bir secretvalue kaynağı YAML oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addsecretvalue -DsecretValue=someVal -DsecretValueName=secret1/v1
```

- `servicefabric->appresources` adlı klasörde bir secretvalue YAML oluşturun `secretvalue_secret1_v1`

### <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Hedef `azure-sfmesh:deploytocluster`yardımıyla, aşağıdaki komutu kullanarak uygulamayı yerel olarak çalıştırabilirsiniz:

```cmd
mvn azure-sfmesh:deploytocluster
```

Varsayılan olarak bu hedef, kaynakları yerel kümeye dağıtır. Yerel kümeye dağıtıyorsanız, yerel bir Service Fabric kümenizin çalışır duruma sahip olduğunu varsayar. Kaynaklar için yerel Service Fabric kümesi şu anda yalnızca [Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md)'da desteklenmektedir.

- Yamls 'lerden Service Fabric kümeler için geçerli olan jler oluşturur
- Daha sonra küme uç noktasına dağıtır

### <a name="deploy-application-to-azure-service-fabric-mesh"></a>Azure Service Fabric ağı 'na uygulama dağıtma

Hedef `azure-sfmesh:deploy`yardımıyla, aşağıdaki komutu çalıştırarak Service Fabric ağ ortamına dağıtabilirsiniz:

```cmd
mvn azure-sfmesh:deploy -DresourceGroup=todoapprg -Dlocation=eastus
```

- Mevcut değilse `todoapprg` adlı bir kaynak grubu oluşturur.
- YAMLs 'Leri birleştirerek JSON Azure Resource Manager şablon oluşturur. 
- JSON 'ı Azure Service Fabric kafes ortamına dağıtır.
