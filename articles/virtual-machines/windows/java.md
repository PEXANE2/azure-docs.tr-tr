---
title: Java Kullanarak Bir Azure Sanal Makinesi Oluşturma ve Yönetme
description: Sanal bir makineyi ve tüm destekleyici kaynaklarını dağıtmak için Java ve Azure Kaynak Yöneticisi'ni kullanın.
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.topic: how-to
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: a99924983bf0e78bd8c8901e25819a363583169a
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870021"
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Java'yı kullanarak Azure'da Windows VM'leri oluşturma ve yönetme

[Azure Sanal Makine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) birkaç destekleyici Azure kaynağına ihtiyaç duyar. Bu makalede, Java kullanarak VM kaynakları oluşturma, yönetme ve silme yer alıyor. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Maven projesi oluşturma
> * Bağımlılıkekleme
> * Kimlik bilgileri oluşturma
> * Kaynak oluşturma
> * Yönetim görevlerini gerçekleştirme
> * Kaynakları silme
> * Uygulamayı çalıştırma

Bu adımları yapmak yaklaşık 20 dakika sürer.

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturma

1. Bunu zaten yapmadıysanız, [Java'yı](https://aka.ms/azure-jdks)yükleyin.
2. [Maven yükleyin.](https://maven.apache.org/download.cgi)
3. Yeni bir klasör ve proje oluşturun:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Bağımlılıkekleme

1. Klasörün `testAzureApp` altında, `pom.xml` dosyayı açın ve &lt;uygulamanızın oluşturulmasını etkinleştirmek için projeye&gt; yapı yapılandırmasını ekleyin:

    ```xml
    <build>
      <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.App</mainClass>
            </configuration>
        </plugin>
      </plugins>
    </build>
    ```

2. Azure Java SDK'ya erişmek için gereken bağımlılıkları ekleyin.

    ```xml
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-compute</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-resources</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-network</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.squareup.okio</groupId>
      <artifactId>okio</artifactId>
      <version>1.13.0</version>
    </dependency>
    <dependency>
      <groupId>com.nimbusds</groupId>
      <artifactId>nimbus-jose-jwt</artifactId>
      <version>3.6</version>
    </dependency>
    <dependency>
      <groupId>net.minidev</groupId>
      <artifactId>json-smart</artifactId>
      <version>1.0.6.3</version>
    </dependency>
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4.5</version>
    </dependency>
    ```

3. Dosyayı kaydedin.

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Bu adımı başlatmadan önce, [Etkin Dizin hizmet ilkesine](../../active-directory/develop/howto-create-service-principal-portal.md)erişebildiğinizden emin olun. Ayrıca uygulama kimliğini, kimlik doğrulama anahtarını ve daha sonraki bir adımda gereksinim duyduğunuz kiracı kimliğini de kaydetmeniz gerekir.

### <a name="create-the-authorization-file"></a>Yetkilendirme dosyasını oluşturma

1. Adlandırılmış `azureauth.properties` bir dosya oluşturun ve bu özellikleri ekleyin:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.microsoft.com/
    ```

    ** &lt;Abonelik kimliğiniz&gt; abonelik** tanımlayıcınızla, ** &lt;uygulama kimliğini&gt; ** Active Directory uygulama tanımlayıcısıyla, ** &lt;kimlik doğrulama anahtarını&gt; ** uygulama anahtarıyla ve ** &lt;kiracı&gt; ** kimliğiyle kiracı kimliğiyle değiştirin.

2. Dosyayı kaydedin.
3. Kimlik doğrulama dosyasına giden tam yol ile kabuğunuzda AZURE_AUTH_LOCATION adlı bir ortam değişkeni ayarlayın.

### <a name="create-the-management-client"></a>Yönetim istemcisini oluşturma

1. Dosyayı `App.java` altında `src\main\java\com\fabrikam` açın ve bu paket deyiminin en üstte olduğundan emin olun:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. Paket deyiminin altına şu alma ekstrelerini ekleyin:
   
    ```java
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.compute.AvailabilitySet;
    import com.microsoft.azure.management.compute.AvailabilitySetSkuTypes;
    import com.microsoft.azure.management.compute.CachingTypes;
    import com.microsoft.azure.management.compute.InstanceViewStatus;
    import com.microsoft.azure.management.compute.DiskInstanceView;
    import com.microsoft.azure.management.compute.VirtualMachine;
    import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
    import com.microsoft.azure.management.network.PublicIPAddress;
    import com.microsoft.azure.management.network.Network;
    import com.microsoft.azure.management.network.NetworkInterface;
    import com.microsoft.azure.management.resources.ResourceGroup;
    import com.microsoft.azure.management.resources.fluentcore.arm.Region;
    import com.microsoft.azure.management.resources.fluentcore.model.Creatable;
    import com.microsoft.rest.LogLevel;
    import java.io.File;
    import java.util.Scanner;
    ```

2. İstekte bulunmak için gereken Active Directory kimlik bilgilerini oluşturmak için bu kodu Uygulama sınıfının ana yöntemine ekleyin:
   
    ```java
    try {
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
            .withLogLevel(LogLevel.BASIC)
            .authenticate(credFile)
            .withDefaultSubscription();
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    ```

## <a name="create-resources"></a>Kaynak oluşturma

### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Tüm kaynaklar bir [Kaynak grubunda](../../azure-resource-manager/management/overview.md)yer almalıdır.

Uygulama için değerleri belirtmek ve kaynak grubu oluşturmak için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-the-availability-set"></a>Kullanılabilirlik kümesini oluşturma

[Kullanılabilirlik kümeleri,](tutorial-availability-sets.md) uygulamanız tarafından kullanılan sanal makineleri korumanızı kolaylaştırır.

Kullanılabilirlik kümesini oluşturmak için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-the-public-ip-address"></a>Genel IP adresini oluşturma

Sanal makineyle iletişim kurmak için [genel bir IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) gereklidir.

Sanal makinenin genel IP adresini oluşturmak için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Sanal bir makine, [Sanal ağın](../../virtual-network/virtual-networks-overview.md)alt ağında olmalıdır.

Bir alt ağ ve sanal ağ oluşturmak için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Creating virtual network...");
Network network = azure.networks()
    .define("myVN")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withAddressSpace("10.0.0.0/16")
    .withSubnet("mySubnet","10.0.0.0/24")
    .create();
```

### <a name="create-the-network-interface"></a>Ağ arabirimini oluşturma

Sanal makinenin sanal ağda iletişim kurabilmesi için ağ arabirimine ihtiyacı vardır.

Ağ arabirimi oluşturmak için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Creating network interface...");
NetworkInterface networkInterface = azure.networkInterfaces()
    .define("myNIC")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetwork(network)
    .withSubnet("mySubnet")
    .withPrimaryPrivateIPAddressDynamic()
    .withExistingPrimaryPublicIPAddress(publicIPAddress)
    .create();
```

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Artık tüm destekleyici kaynakları oluşturduğunuziçin, sanal bir makine oluşturabilirsiniz.

Sanal makineyi oluşturmak için bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Creating virtual machine...");
VirtualMachine virtualMachine = azure.virtualMachines()
    .define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("azureuser")
    .withAdminPassword("Azure12345678")
    .withComputerName("myVM")
    .withExistingAvailabilitySet(availabilitySet)
    .withSize("Standard_DS1")
    .create();
Scanner input = new Scanner(System.in);
System.out.println("Press enter to get information about the VM...");
input.nextLine();
```

> [!NOTE]
> Bu öğretici, Windows Server işletim sisteminin bir sürümünü çalıştıran sanal bir makine oluşturur. Diğer resimleri seçme hakkında daha fazla bilgi edinmek için [Windows PowerShell ve Azure CLI ile Gezin ve Azure sanal makine resimlerini seçin.](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
>

Pazar yeri görüntüsü yerine varolan bir disk kullanmak istiyorsanız, şu kodu kullanın: 

```java
ManagedDisk managedDisk = azure.disks.define("myosdisk")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .withSizeInGB(128)
    .withSku(DiskSkuTypes.PremiumLRS)
    .create();

azure.virtualMachines.define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .withExistingAvailabilitySet(availabilitySet)
    .withSize(VirtualMachineSizeTypes.StandardDS1)
    .create();
```

## <a name="perform-management-tasks"></a>Yönetim görevlerini gerçekleştirme

Bir sanal makinenin yaşam döngüsü boyunca, sanal makineyi başlatmak, durdurmak veya silmek gibi yönetim görevleri gerçekleştirmek isteyebilirsiniz. Ayrıca, yinelenen veya karmaşık görevleri otomatikleştirmek için kod oluşturmak isteyebilirsiniz.

VM ile bir şey yapmanız gerektiğinde, bunun bir örneğini almanız gerekir. Bu kodu ana yöntemin try bloğuna ekleyin:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-the-vm"></a>VM hakkında bilgi alın

Sanal makine hakkında bilgi almak için bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("hardwareProfile");
System.out.println("    vmSize: " + vm.size());
System.out.println("storageProfile");
System.out.println("  imageReference");
System.out.println("    publisher: " + vm.storageProfile().imageReference().publisher());
System.out.println("    offer: " + vm.storageProfile().imageReference().offer());
System.out.println("    sku: " + vm.storageProfile().imageReference().sku());
System.out.println("    version: " + vm.storageProfile().imageReference().version());
System.out.println("  osDisk");
System.out.println("    osType: " + vm.storageProfile().osDisk().osType());
System.out.println("    name: " + vm.storageProfile().osDisk().name());
System.out.println("    createOption: " + vm.storageProfile().osDisk().createOption());
System.out.println("    caching: " + vm.storageProfile().osDisk().caching());
System.out.println("osProfile");
System.out.println("    computerName: " + vm.osProfile().computerName());
System.out.println("    adminUserName: " + vm.osProfile().adminUsername());
System.out.println("    provisionVMAgent: " + vm.osProfile().windowsConfiguration().provisionVMAgent());
System.out.println("    enableAutomaticUpdates: " + vm.osProfile().windowsConfiguration().enableAutomaticUpdates());
System.out.println("networkProfile");
System.out.println("    networkInterface: " + vm.primaryNetworkInterfaceId());
System.out.println("vmAgent");
System.out.println("  vmAgentVersion: " + vm.instanceView().vmAgent().vmAgentVersion());
System.out.println("    statuses");
for(InstanceViewStatus status : vm.instanceView().vmAgent().statuses()) {
    System.out.println("    code: " + status.code());
    System.out.println("    displayStatus: " + status.displayStatus());
    System.out.println("    message: " + status.message());
    System.out.println("    time: " + status.time());
}
System.out.println("disks");
for(DiskInstanceView disk : vm.instanceView().disks()) {
    System.out.println("  name: " + disk.name());
    System.out.println("  statuses");
    for(InstanceViewStatus status : disk.statuses()) {
        System.out.println("    code: " + status.code());
        System.out.println("    displayStatus: " + status.displayStatus());
        System.out.println("    time: " + status.time());
    }
}
System.out.println("VM general status");
System.out.println("  provisioningStatus: " + vm.provisioningState());
System.out.println("  id: " + vm.id());
System.out.println("  name: " + vm.name());
System.out.println("  type: " + vm.type());
System.out.println("VM instance status");
for(InstanceViewStatus status : vm.instanceView().statuses()) {
    System.out.println("  code: " + status.code());
    System.out.println("  displayStatus: " + status.displayStatus());
}
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="stop-the-vm"></a>VM’yi durdurma

Sanal bir makineyi durdurabilir ve tüm ayarlarını saklayabilirsiniz, ancak bunun için ücretlendirilmeye devam edebilir veya sanal bir makineyi durdurup yerini saptayabilirsiniz. Sanal bir makine devre dışı edildiğinde, onunla ilişkili tüm kaynaklar da ayrılır ve faturalandırma sona erer.

Sanal makineyi ayırmadan durdurmak için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter to continue...");
input.nextLine();
```

Sanal makineyi bulmak istiyorsanız, PowerOff çağrısını bu kodla değiştirin:

```java
vm.deallocate();
```

### <a name="start-the-vm"></a>VM’yi başlatma

Sanal makineyi başlatmak için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="resize-the-vm"></a>VM'yi yeniden boyutlandırın

Sanal makineniz için bir boyut alırken dağıtımın birçok yönü göz önünde bulundurulmalıdır. Daha fazla bilgi için [VM boyutlarına](sizes.md)bakın.  

Sanal makinenin boyutunu değiştirmek için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>VM’ye veri diski ekleme

Sanal makineye 2 GB boyutunda, LUN 0 lun'a ve önbelleğe alma türüne sahip bir veri diski eklemek için, bu kodu ana yöntemdeki try bloğuna ekleyin:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter to delete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Kaynakları silme

Azure'da kullanılan kaynaklar için ücretlendirildiğiniz için, artık gerekmeyen kaynakları silmek her zaman iyi bir uygulamadır. Sanal makineleri ve tüm destekleyici kaynakları silmek istiyorsanız, tek yapmanız gereken kaynak grubunu silmektir.

1. Kaynak grubunu silmek için, bu kodu ana yöntemdeki try bloğuna ekleyin:
   
    ```java
    System.out.println("Deleting resources...");
    azure.resourceGroups().deleteByName("myResourceGroup");
    ```

2. App.java dosyasını kaydedin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bu konsol uygulamasının baştan sona tamamen çalışması yaklaşık beş dakika sürer.

1. Uygulamayı çalıştırmak için bu Maven komutunu kullanın:

    ```
    mvn compile exec:java
    ```

2. Kaynakları silmeye başlamak için **Enter** tuşuna basmadan önce, Azure portalındaki kaynakların oluşturulmasını doğrulamak birkaç dakikanızı alabilir. Dağıtım la ilgili bilgileri görmek için dağıtım durumunu tıklatın.


## <a name="next-steps"></a>Sonraki adımlar
* [Java için Azure kitaplıklarını](https://docs.microsoft.com/java/azure/java-sdk-azure-overview)kullanma hakkında daha fazla bilgi edinin.

