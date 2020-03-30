---
title: Azure Servis Kumaş Örgü Maven referans
description: Servis Kumaş Örgü için Maven eklentisi nasıl kullanılacağı için referans içerir
author: suhuruli
ms.author: suhuruli
ms.date: 11/26/2018
ms.topic: reference
ms.openlocfilehash: bcc3fb7c6c3adce0997d0960c4d98227089b048b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75459011"
---
# <a name="maven-plugin-for-service-fabric-mesh"></a>Servis Kumaş Örgü için Maven Plugin

## <a name="prerequisites"></a>Ön koşullar

- Java SDK
- Maven
- Kafes uzantılı Azure CLI
- Service Fabric CLI

## <a name="goals"></a>Hedefler

### `azure-sfmesh:init`
- Dosyaya `servicefabric` sahip bir `appresources` klasör içeren bir klasör oluşturur. `application.yaml` 

### `azure-sfmesh:addservice`
- Hizmet adını içeren `servicefabric` klasörün içinde bir klasör oluşturur ve hizmetin YAML dosyasını oluşturur. 

### `azure-sfmesh:addnetwork`
- Klasörde `network` sağlanan ağ adı içeren bir YAML oluşturur `appresources` 

### `azure-sfmesh:addgateway`
- Klasörde `gateway` sağlanan ağ geçidi adı içeren bir YAML oluşturur `appresources` 

#### `azure-sfmesh:addvolume`
- Klasörde `volume` sağlanan birim adı içeren bir YAML oluşturur. `appresources`

### `azure-sfmesh:addsecret`
- Klasörde `secret` sağlanan gizli adı içeren bir YAML oluşturur `appresources` 

### `azure-sfmesh:addsecretvalue`
- Klasörde `secretvalue` sağlanan gizli ve gizli değer adı içeren bir YAML oluşturur `appresources` 

### `azure-sfmesh:deploy`
- `servicefabric` Klasördeki yaml'ları birleştirir ve geçerli klasörde bir Azure Kaynak Yöneticisi şablonu JSON oluşturur.
- Tüm kaynakları Azure Hizmet Kumaş Kafesi ortamına dağıtir 

### `azure-sfmesh:deploytocluster`
- Hizmet Kumaş kümeleri için geçerli olan yaml'lardan oluşturulan dağıtım JSON'larını içeren bir klasör (`meshDeploy`) oluşturur
- Tüm kaynakları Hizmet Kumaşı kümesine dağıtir
 

## <a name="usage"></a>Kullanım

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
      </plugin>
    </plugins>
  </build>
</project>
```

## <a name="common-configuration"></a>Ortak Yapılandırma

Maven eklentisi şu anda Azure için Maven Eklentilerinin yaygın yapılandırmalarını desteklemiyor.

## <a name="how-to"></a>Nasıl Yapılır

### <a name="initialize-maven-project-for-azure-service-fabric-mesh"></a>Azure Hizmet Kumaş Örgüsü için Başlangıç Maven projesi
Uygulama kaynağı YAML dosyasını oluşturmak için aşağıdaki komutu çalıştırın.

```cmd
mvn azure-sfmesh:init -DapplicationName=helloworldserver
```

- Kök klasörünüzde `servicefabric->appresources` YAML adlı bir uygulama içeren bir klasör oluşturur`app_helloworldserver`

### <a name="add-resource-to-your-application"></a>Uygulamanıza kaynak ekleme

#### <a name="add-a-new-network-to-your-application"></a>Uygulamanıza yeni bir ağ ekleme
Bir ağ kaynağı yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addnetwork -DnetworkName=helloworldservicenetwork -DnetworkAddressPrefix=10.0.0.0/22
```

- Adlı klasörde `servicefabric->appresources` bir ağ YAML oluşturur`network_helloworldservicenetwork`

#### <a name="add-a-new-service-to-your-application"></a>Uygulamanıza yeni bir hizmet ekleme
Bir hizmet yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addservice -DapplicationName=helloworldserver -DserviceName=helloworldservice -DimageName=helloworldserver:latest -DlistenerPort=8080 -DnetworkRef=helloworldservicenetwork
```

- Uygulama `servicefabric->helloworldservice` nın & `service_helloworldservice` başvuruyapan `helloworldservicenetwork` klasörde bir hizmet YAML oluşturur `helloworldserver`
- Hizmet port 8080 üzerinde dinleyecek
- Hizmet ***helloworldserver kullanıyor olurdu:konteyner*** görüntü olarak en son.

#### <a name="add-a-new-gateway-resource-to-your-application"></a>Uygulamanıza yeni bir ağ geçidi kaynağı ekleme
Ağ geçidi kaynağı yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addgateway -DapplicationName=helloworldserver -DdestinationNetwork=helloworldservicenetwork -DgatewayName=helloworldgateway -DlistenerName=helloworldserviceListener -DserviceName=helloworldservice -DsourceNetwork=open -DtcpPort=80
```

- Adlı klasörde `servicefabric->appresources` yeni bir ağ geçidi YAML oluşturur`gateway_helloworldgateway`
- Bu `helloworldservicelistener` ağ geçidinden gelen aramaları dinleyen servis dinleyicisi olarak başvurur. Ayrıca hizmet `helloworldservice` olarak, `helloworldservicenetwork` ağ olarak `helloworldserver` ve uygulama olarak başvurur. 
- Bağlantı noktası 80'deki istekleri dinler

#### <a name="add-a-new-volume-to-your-application"></a>Uygulamanıza yeni bir birim ekleme
Bir birim kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addvolume -DvolumeAccountKey=key -DvolumeAccountName=name -DvolumeName=vol1 -DvolumeShareName=share
```

- Adlı klasörde `servicefabric->appresources` bir birim YAML oluşturur`volume_vol1`
- Gerekli parametreler `volumeAccountKey`için özellikleri `volumeShareName` ayarlar ve yukarıdaki gibi
- Oluşturulan bu birimde nasıl başvurulması hakkında daha fazla bilgi için aşağıdakileri ziyaret edin, [Azure Dosyaları Hacmini kullanarak Uygulamayı Dağıt](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)

#### <a name="add-a-new-secret-resource-to-your-application"></a>Uygulamanız için yeni bir gizli kaynak ekleme
Gizli bir kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addsecret -DsecretName=secret1
```

- Adlı klasörde `servicefabric->appresources` gizli bir YAML oluşturur`secret_secret1`
- Oluşturulan bu gizliye nasıl başvurulması hakkında daha fazla bilgi için aşağıdakileri ziyaret [edin, Sırları Yönet](service-fabric-mesh-howto-manage-secrets.md)

#### <a name="add-a-new-secretvalue-resource-to-your-application"></a>Uygulamanıza yeni bir secretvalue kaynağı ekleme
Bir secretvalue kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addsecretvalue -DsecretValue=someVal -DsecretValueName=secret1/v1
```

- Adlı klasörde `servicefabric->appresources` bir secretvalue YAML oluşturma`secretvalue_secret1_v1`

### <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Amaç `azure-sfmesh:deploytocluster`yardımı ile aşağıdaki komutu kullanarak uygulamayı yerel olarak çalıştırabilirsiniz:

```cmd
mvn azure-sfmesh:deploytocluster
```

Varsayılan olarak bu hedef kaynakları yerel kümeye dağır. Yerel kümeye dağıtım yapıyorsunuzsa, yerel bir Hizmet Kumaşı kümenizin çalışır durumda olduğunu varsayar. Kaynaklar için Yerel Hizmet Kumaş kümesi şu anda yalnızca [Windows'da](service-fabric-mesh-howto-setup-developer-environment-sdk.md)desteklenir.

- Service Fabric kümeleri için geçerli olan yaml'lardan JSON'lar oluşturur
- Daha sonra Küme bitiş noktasına dağılar

### <a name="deploy-application-to-azure-service-fabric-mesh"></a>Uygulamayı Azure Hizmet Kumaş Kafesi'ne dağıtın

Amaç `azure-sfmesh:deploy`yardımıyla, aşağıdaki komutu çalıştırarak Service Fabric Mesh Environment'a dağıtabilirsiniz:

```cmd
mvn azure-sfmesh:deploy -DresourceGroup=todoapprg -Dlocation=eastus
```

- Yoksa adında `todoapprg` bir kaynak grubu oluşturur.
- YAML'leri birleştirerek Bir Azure Kaynak Yöneticisi şablonu JSON oluşturur. 
- JSON'u Azure Hizmet Kumaş Kafesi ortamına dağıtıyor.
