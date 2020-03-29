---
title: FabricTransport ayarlarını değiştirme
description: Azure Service Fabric aktör iletişim ayarlarını farklı aktör yapılandırmaları için yapılandırma hakkında bilgi edinin.
author: suchiagicha
ms.topic: conceptual
ms.date: 04/20/2017
ms.author: pepogors
ms.openlocfilehash: 182b35194a647f5de706f9ada07b8e9db51bb4c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645591"
---
# <a name="configure-fabrictransport-settings-for-reliable-actors"></a>Güvenilir Aktörler için FabricTransport ayarlarını yapılandırın

Yapılandırabileceğiniz ayarlar şunlardır:
- C#: [FabricTransportRemotingAyarlar](
https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.fabrictransport.fabrictransportremotingsettings)
- Java: [FabricTransportRemotingAyarlar](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.fabrictransport.fabrictransportremotingsettings)

FabricTransport'un varsayılan yapılandırmasını aşağıdaki yollarla değiştirebilirsiniz.

## <a name="assembly-attribute"></a>Montaj özniteliği

[FabricTransportActorRemotingProvider](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.actors.remoting.fabrictransport.fabrictransportactorremotingproviderattribute?redirectedfrom=MSDN) özniteliği aktör istemci ve aktör hizmet meclisleri üzerinde uygulanması gerekir.

Aşağıdaki örnek, FabricTransport OperationTimeout ayarlarının varsayılan değerinin nasıl değiştirilebildiğini gösterir:

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600)]
   ```

   İkinci örnek, FabricTransport MaxMessageSize ve OperationTimeoutInSeconds varsayılan değerlerini değiştirir.

  ```csharp
    using Microsoft.ServiceFabric.Actors.Remoting.FabricTransport;
    [assembly:FabricTransportActorRemotingProvider(OperationTimeoutInSeconds = 600,MaxMessageSize = 134217728)]
   ```

## <a name="config-package"></a>Config paketi

Varsayılan yapılandırmayı değiştirmek için [bir config paketi](service-fabric-application-and-service-manifests.md) kullanabilirsiniz.

> [!IMPORTANT]
> Linux düğümlerinde sertifikalar PEM biçimlendirilmiş olmalıdır. Linux için sertifikaları bulma ve yapılandırma hakkında daha fazla bilgi edinmek için [bkz.](./service-fabric-configure-certificates-linux.md) 
> 

### <a name="configure-fabrictransport-settings-for-the-actor-service"></a>Aktör hizmeti için FabricTransport ayarlarını yapılandırın

settings.xml dosyasına TransportSettings bölümü ekleyin.

Varsayılan olarak, aktör kodu SectionName'i&gt;"ActorName&lt;TransportSettings" olarak arar. Bu bulunmazsa, SectionName'i "TransportSettings" olarak denetler.

  ```xml
  <Section Name="MyActorServiceTransportSettings">
       <Parameter Name="MaxMessageSize" Value="10000000" />
       <Parameter Name="OperationTimeoutInSeconds" Value="300" />
       <Parameter Name="SecurityCredentialsType" Value="X509" />
       <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
       <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
       <Parameter Name="CertificateRemoteThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
       <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
       <Parameter Name="CertificateStoreName" Value="My" />
       <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
       <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
   </Section>
  ```

### <a name="configure-fabrictransport-settings-for-the-actor-client-assembly"></a>Aktör istemci derlemesi için FabricTransport ayarlarını yapılandırın

İstemci bir hizmetin parçası olarak çalışmıyorsa,&lt;istemci .exe dosyasıyla aynı konumda "Client Exe Name&gt;.settings.xml" dosyası oluşturabilirsiniz. Ardından bu dosyaya bir TransportSettings bölümü ekleyin. SectionName "TransportSettings" olmalıdır.

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
    <Section Name="TransportSettings">
      <Parameter Name="SecurityCredentialsType" Value="X509" />
      <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
      <Parameter Name="CertificateFindValue" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
      <Parameter Name="CertificateRemoteThumbprints" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
      <Parameter Name="OperationTimeoutInSeconds" Value="300" />
      <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
      <Parameter Name="CertificateStoreName" Value="My" />
      <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
    </Section>
  </Settings>
   ```

* İkincil Sertifika ile Güvenli Aktör Hizmeti/İstemci için FabricTransport Ayarlarını Yapılandırma.
  İkincil sertifika bilgileri parametre CertificateFindValuebySecondary ekleyerek eklenebilir.
  Aşağıda Dinleyici Taşıma Ayarları için örnek verilmiştir.

  ```xml
  <Section Name="TransportSettings">
  <Parameter Name="SecurityCredentialsType" Value="X509" />
  <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
  <Parameter Name="CertificateFindValue" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
  <Parameter Name="CertificateFindValuebySecondary" Value="h9449b018d0f6839a2c5d62b5b6c6ac822b6f690" />
  <Parameter Name="CertificateRemoteThumbprints" Value="4FEF3950642138446CC364A396E1E881DB76B48C,a9449b018d0f6839a2c5d62b5b6c6ac822b6f667" />
  <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
  <Parameter Name="CertificateStoreName" Value="My" />
  <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
  </Section>
   ```
   Aşağıda İstemci Taşıma Ayarları için örnek verilmiştir.

  ```xml
  <Section Name="TransportSettings">
  <Parameter Name="SecurityCredentialsType" Value="X509" />
  <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
  <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
  <Parameter Name="CertificateFindValuebySecondary" Value="a9449b018d0f6839a2c5d62b5b6c6ac822b6f667" />
  <Parameter Name="CertificateRemoteThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662,h9449b018d0f6839a2c5d62b5b6c6ac822b6f690" />
  <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
  <Parameter Name="CertificateStoreName" Value="My" />
  <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
  </Section>
   ```
  * Konu Adı Kullanarak Aktör Hizmetini/İstemciyi Güvence Altına Almak için FabricTransport Ayarlarını Yapılandırma.
    Kullanıcı findBySubjectName olarak findType sağlamak için, SertifikaIssuerThumbprints ve SertifikaRemoteCommonNames değerleri ekleyin.
    Aşağıda Dinleyici Taşıma Ayarları için örnek verilmiştir.

    ```xml
    <Section Name="TransportSettings">
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindBySubjectName" />
    <Parameter Name="CertificateFindValue" Value="CN = WinFabric-Test-SAN1-Alice" />
    <Parameter Name="CertificateIssuerThumbprints" Value="b3449b018d0f6839a2c5d62b5b6c6ac822b6f662" />
    <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Bob" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    </Section>
    ```
    Aşağıda İstemci Taşıma Ayarları için örnek verilmiştir.

  ```xml
   <Section Name="TransportSettings">
  <Parameter Name="SecurityCredentialsType" Value="X509" />
  <Parameter Name="CertificateFindType" Value="FindBySubjectName" />
  <Parameter Name="CertificateFindValue" Value="CN = WinFabric-Test-SAN1-Bob" />
  <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
  <Parameter Name="CertificateStoreName" Value="My" />
  <Parameter Name="CertificateRemoteCommonNames" Value="WinFabric-Test-SAN1-Alice" />
  <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
  </Section>
   ```
