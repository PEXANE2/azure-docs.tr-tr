---
title: 'Hızlı başlangıç: ARM şablonunu kullanarak FHıR için Azure IoT bağlayıcısını dağıtma (Önizleme)'
description: Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak FHIR (Önizleme) için Azure IoT bağlayıcısını dağıtmayı öğrenin (ARM şablonu).
author: rbhaiya
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: quickstart
ms.author: rabhaiya
ms.date: 01/21/2021
ms.openlocfilehash: b241484b8e8d981036fff4998d475b8b80fae42d
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020574"
---
# <a name="quickstart-use-an-azure-resource-manager-arm-template-to-deploy-azure-iot-connector-for-fhir-preview"></a>Hızlı başlangıç: FHıR için Azure IoT bağlayıcısını dağıtmak üzere bir Azure Resource Manager (ARM) şablonu kullanma (Önizleme)

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu (ARM şablonu) kullanarak hızlı sağlık birlikte çalışabilirlik kaynakları için Azure IoT bağlayıcısını (FHıR&#174;) *, FHıR için Azure API 'nin bir özelliğini nasıl kullanacağınızı öğreneceksiniz. Bu şablon, FHıR için Azure IoT bağlayıcısının çalışan bir örneğini dağıtmak için Ayrıca, fhır hizmeti için bir üst Azure API 'SI ve bir cihaz benzeticisinden FHıR için Azure IoT Bağlayıcısı 'na telemetri veren bir Azure IoT Central uygulaması dağıtır. Azure portal, PowerShell veya CLı aracılığıyla FHıR için Azure IoT bağlayıcısını dağıtmak üzere ARM şablonunu çalıştırabilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Bu şablon, oturum açıldıktan sonra Azure portal açılır.

[:::image type="content" source="../../media/template-deployments/deploy-to-azure.svg" alt-text="Azure portal bir ARM şablonu kullanarak FHıR için Azure IoT bağlayıcısını Azure 'a dağıtın.":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2Fiomt-fhir%2Fmaster%2Fdeploy%2Ftemplates%2Fmanaged%2Fazuredeploy.json)

## <a name="prerequisites"></a>Önkoşullar

# <a name="portal"></a>[Portal](#tab/azure-portal)

Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/).

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/).
* Kodu yerel olarak çalıştırmak istiyorsanız, [Azure PowerShell](/powershell/azure/install-az-ps).

# <a name="cli"></a>[CLI](#tab/CLI)

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/).
* Kodu yerel olarak çalıştırmak istiyorsanız:
    * Bash kabuğu ( [Windows Için git](https://gitforwindows.org)'e dahil olan git Bash gibi).
    * [Azure CLI](/cli/azure/install-azure-cli).

---

## <a name="review-the-template"></a>Şablonu gözden geçirme

[Şablon](https://raw.githubusercontent.com/microsoft/iomt-fhir/master/deploy/templates/managed/azuredeploy.json) , aşağıdaki Azure kaynaklarını tanımlar:

* **Microsoft. Healthgelişme API 'leri/Hizmetleri**
* **Microsoft. Healthgelişme API 'leri/Hizmetleri/ıomtbağlayıcıları**
* **Microsoft. ıotcentral/ıotapps**

## <a name="deploy-the-template"></a>Şablonu dağıtma

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal 'de ARM şablonunu kullanarak FHıR için Azure IoT bağlayıcısını dağıtmak üzere aşağıdaki bağlantıyı seçin:

[:::image type="content" source="../../media/template-deployments/deploy-to-azure.svg" alt-text="Azure portal ARM şablonunu kullanarak FHıR hizmeti için Azure IoT bağlayıcısını Azure 'a dağıtın.":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2Fiomt-fhir%2Fmaster%2Fdeploy%2Ftemplates%2Fmanaged%2Fazuredeploy.json)

**FHıR Için Azure API dağıtma** sayfasında:

1. İsterseniz, **aboneliği** varsayılan değerinden farklı bir aboneliğe değiştirin.

2. **Kaynak grubu** Için **Yeni oluştur**' u seçin, yeni kaynak grubu için bir ad girin ve **Tamam**' ı seçin.

3. Yeni bir kaynak grubu oluşturduysanız, kaynak grubu için bir **bölge** seçin.

4. **Fhır hizmeti adı**' nda fhır Için yenı Azure API 'niz için bir ad girin.

5. FHıR için Azure API 'nizin **konumunu** seçin. Konum, kaynak grubunun bölgesiyle aynı veya farklı olabilir.

6. **IoT Bağlayıcısı adında** fhır örneği Için Azure IoT Bağlayıcınız için bir ad sağlayın.

7. Azure IoT Bağlayıcısı 'nda **bağlantı adı** içindeki fhır için oluşturulan bağlantı için bir ad girin. Bu bağlantı, Azure IoT Central uygulaması tarafından, sanal cihaz telemetrisini FHıR için Azure IoT bağlayıcısına göndermek üzere kullanılır.

8. **IoT merkezi adında** yeni Azure IoT Central uygulamanız için bir ad girin. Bu uygulama, bir cihazın benzetimini yapmak için *sürekli hasta izleme* şablonunu kullanacaktır.

9. **IoT Central konumu** açılan listesinden IoT Central uygulamanızın konumunu seçin. 

10. **Gözden geçir ve oluştur**’u seçin.

11. Hüküm ve koşulları okuyun ve ardından **Oluştur**' u seçin.

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

> [!NOTE]
> PowerShell betiklerini yerel olarak çalıştırmak istiyorsanız, ilk olarak `Connect-AzAccount` Azure kimlik bilgilerinizi kurmak için girin.

`Microsoft.HealthcareApis`Kaynak sağlayıcısı aboneliğiniz için zaten kayıtlı değilse, aşağıdaki etkileşimli kodla kaydedebilirsiniz. Kodu Azure Cloud Shell çalıştırmak için herhangi bir kod bloğunun üst köşesinde **deneyin** ' i seçin.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

`Microsoft.IoTCentral`Kaynak sağlayıcısı aboneliğiniz için zaten kayıtlı değilse, aşağıdaki etkileşimli kodla kaydedebilirsiniz. Kodu Azure Cloud Shell çalıştırmak için herhangi bir kod bloğunun üst köşesinde **deneyin** ' i seçin.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.IoTCentral
```

ARM şablonunu kullanarak FHıR hizmeti için Azure IoT bağlayıcısını dağıtmak üzere aşağıdaki kodu kullanın.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter a name for the new resource group to contain the service"
$resourceGroupRegion = Read-Host -Prompt "Enter an Azure region (for example, centralus) for the resource group"
$fhirServiceName = Read-Host -Prompt "Enter a name for the new Azure API for FHIR service"
$location = Read-Host -Prompt "Enter an Azure region (for example, westus2) for the service"
$iotConnectorName = Read-Host -Prompt "Enter a name for the new Azure IoT Connector for FHIR"
$connectionName = Read-Host -Prompt "Enter a name for the connection with Azure IoT Connector for FHIR"
$iotCentralName = Read-Host -Prompt "Enter a name for the new Azure IoT Central Application"
$iotCentralLocation = Read-Host -Prompt "Enter a location for the new Azure IoT Central Application"

Write-Verbose "New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupRegion" -Verbose
New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupRegion
Write-Verbose "Run New-AzResourceGroupDeployment to create an Azure IoT Connector for FHIR service using an ARM template" -Verbose
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateUri https://raw.githubusercontent.com/microsoft/iomt-fhir/master/deploy/templates/managed/azuredeploy.json `
    -fhirServiceName $fhirServiceName `
    -location $location
    -iotConnectorName $iotConnectorName
    -connectionName $connectionName
    -iotCentralName $iotCentralName
    -iotCentralLocation $iotCentralLocation
Read-Host "Press [ENTER] to continue"
```

# <a name="cli"></a>[CLI](#tab/CLI)

`Microsoft.HealthcareApis`Kaynak sağlayıcısı aboneliğiniz için zaten kayıtlı değilse, aşağıdaki etkileşimli kodla kaydedebilirsiniz. Kodu Azure Cloud Shell çalıştırmak için herhangi bir kod bloğunun üst köşesinde **deneyin** ' i seçin.

```azurecli-interactive
az extension add --name healthcareapis
```

`Microsoft.IoTCentral`Kaynak sağlayıcısı aboneliğiniz için zaten kayıtlı değilse, aşağıdaki etkileşimli kodla kaydedebilirsiniz.

```azurecli-interactive
az extension add --name azure-iot
```

ARM şablonunu kullanarak FHıR hizmeti için Azure IoT bağlayıcısını dağıtmak üzere aşağıdaki kodu kullanın.

```azurecli-interactive
read -p "Enter a name for the new resource group to contain the service: " resourceGroupName &&
read -p "Enter an Azure region (for example, centralus) for the resource group: " resourceGroupRegion &&
read -p "Enter a name for the new Azure API for FHIR service: " fhirServiceName &&
read -p "Enter an Azure region (for example, westus2) for the service: " location &&
read -p "Enter a name for the new Azure IoT Connector for FHIR: " iotConnectorName &&
read -p "Enter a name for the connection with Azure IoT Connector for FHIR: " connectionName &&
read -p "Enter a name for the new Azure IoT Central Application: " iotCentralName &&
read -p "Enter a location for the new Azure IoT Central Application: " iotCentralLocation &&

params='fhirServiceName='$fhirServiceName' location='$location' iotConnectorName='$iotConnectorName' connectionName='$connectionName' iotCentralName='$iotCentralName' iotCentralLocation='$iotCentralLocation &&
echo "CREATE RESOURCE GROUP:  az group create --name $resourceGroupName --location $resourceGroupRegion" &&
az group create --name $resourceGroupName --location $resourceGroupRegion &&
echo "RUN az deployment group create, which creates an Azure IoT Connector for FHIR service using an ARM template" &&
az deployment group create --resource-group $resourceGroupName --parameters $params --template-uri https://raw.githubusercontent.com/microsoft/iomt-fhir/master/deploy/templates/managed/azuredeploy.json &&
read -p "Press [ENTER] to continue: "
```

---

> [!NOTE]
> Dağıtımın tamamlanabilmesi birkaç dakika sürer. Daha sonra dağıtılan kaynakları gözden geçirmek için kullandığınız FHıR hizmeti, Azure IoT Central uygulaması ve kaynak grubu için Azure API 'sinin adlarını aklınızda bulabilirsiniz.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

# <a name="portal"></a>[Portal](#tab/azure-portal)

FHıR hizmetine yönelik yeni Azure API 'nize genel bir bakış için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **Fhır IÇIN Azure API 'yi** arayın ve seçin.

2. FHıR listesinde yeni hizmetinizi seçin. FHıR hizmeti için yeni Azure API 'SI için **genel bakış** sayfası görüntülenir.

3. Yeni FHıR API hesabının sağlandığını doğrulamak için fhır API 'SI özelliğini getirmek üzere **fhır meta veri uç noktası** ' nın yanındaki bağlantıyı seçin. Bağlantının biçimi vardır `https://<service-name>.azurehealthcareapis.com/metadata` . Hesap sağlandıysa, büyük bir JSON dosyası görüntülenir.

4. FHıR için yeni Azure IoT bağlayıcısının sağlandığını doğrulamak için, sol gezinti menüsünden **IoT Bağlayıcısı (Önizleme)** ' ni seçerek **IoT bağlayıcıları** sayfasını açın. Sayfada *durum* değeri *çevrimiçi*, *1* olarak *bağlantı* değeri ve hem *cihaz eşleme* hem de *fhır eşlemesi* *başarı* simgesini gösterecek şekilde, fhır için sağlanan Azure IoT Bağlayıcısı 'nın gösterilmesi gerekir.

5. [Azure Portal](https://portal.azure.com), **IoT Central uygulamaları** arayın ve seçin.

6. IoT Central uygulamalar listesinde yeni hizmetinizi seçin. Yeni IoT Central uygulaması için **genel bakış** sayfası görüntülenir.

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

FHıR hizmeti için Azure API 'niz hakkındaki ayrıntıları görüntülemek için aşağıdaki etkileşimli kodu çalıştırın. Yeni FHıR hizmetinin ve kaynak grubunun adını girmeniz gerekir.

```azurepowershell-interactive
$fhirServiceName = Read-Host -Prompt "Enter the name of your Azure API for FHIR service"
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Write-Verbose "Get-AzHealthcareApisService -ResourceGroupName $resourceGroupName -Name $serviceName" -Verbose
Get-AzHealthcareApisService -ResourceGroupName $resourceGroupName -Name $serviceName
Read-Host "Press [ENTER] to fetch the FHIR API capability statement, which shows that the new FHIR service has been provisioned"

$requestUri="https://" + $fhirServiceName + ".azurehealthcareapis.com/metadata"
$metadata = Invoke-WebRequest -Uri $requestUri
$metadata.RawContent
Read-Host "Press [ENTER] to continue"
```

> [!NOTE]
> FHıR için Azure IoT Bağlayıcısı Şu anda PowerShell komutları sağlamıyor. Azure IoT bağlayıcısının FHAR için doğru şekilde sağlandığını doğrulamak için **Portal** sekmesinde sağlanan doğrulama işlemini kullanın.

Azure IoT Central uygulamanız hakkındaki ayrıntıları görüntülemek için aşağıdaki etkileşimli kodu çalıştırın. Yeni IoT Central uygulamasının ve kaynak grubunun adını girmeniz gerekir.

```azurepowershell-interactive
$iotCentralName = Read-Host -Prompt "Enter the name of your Azure IoT Central application"
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Write-Verbose "Get-AzIotCentralApp -ResourceGroupName $resourceGroupName -Name $iotCentralName" -Verbose
Get-AzIotCentralApp -ResourceGroupName $resourceGroupName -Name $iotCentralName
```

# <a name="cli"></a>[CLI](#tab/CLI)

FHıR hizmeti için Azure API 'niz hakkındaki ayrıntıları görüntülemek için aşağıdaki etkileşimli kodu çalıştırın. Yeni FHıR hizmetinin ve kaynak grubunun adını girmeniz gerekir.

```azurecli-interactive
read -p "Enter the name of your Azure API for FHIR service: " fhirServiceName &&
read -p "Enter the resource group name: " resourceGroupName &&
echo "SHOW SERVICE DETAILS:  az healthcareapis service show --resource-group $resourceGroupName --resource-name $fhirServiceName" &&
az healthcareapis service show --resource-group $resourceGroupName --resource-name $fhirServiceName &&
read -p "Press [ENTER] to fetch the FHIR API capability statement, which shows that the new service has been provisioned: " &&
requestUrl='https://'$fhirServiceName'.azurehealthcareapis.com/metadata' &&
curl --url $requestUrl &&
read -p "Press [ENTER] to continue: "
```

> [!NOTE]
> FHıR için Azure IoT Bağlayıcısı Şu anda CLı komutları sağlamıyor. Azure IoT bağlayıcısının FHAR için doğru şekilde sağlandığını doğrulamak için **Portal** sekmesinde sağlanan doğrulama işlemini kullanın.

Azure IoT Central uygulamanız hakkındaki ayrıntıları görüntülemek için aşağıdaki etkileşimli kodu çalıştırın. Yeni IoT Central uygulamasının ve kaynak grubunun adını girmeniz gerekir.

```azurecli-interactive
read -p "Enter the name of your IoT Central application: " iotCentralName &&
read -p "Enter the resource group name: " resourceGroupName &&
echo "SHOW SERVICE DETAILS:  az iot central app show -g $resourceGroupName -n $iotCentralName" &&
az iot central app show -g $resourceGroupName -n $iotCentralName &&
```

---

## <a name="connect-your-iot-data-with-the-azure-iot-connector-for-fhir-preview"></a>IoT verilerinizi FHıR için Azure IoT Bağlayıcısı (Önizleme) ile bağlama
> [!IMPORTANT]
> Bu kılavuzda verilen cihaz eşleme şablonu, IoT Central içinde veri dışa aktarma (eski) ile çalışmak üzere tasarlanmıştır.

IoT Central uygulama şu anda, veri dışa aktarma ayarlamak için ARM şablonu veya PowerShell ve CLı komutları sağlamıyor. Bu nedenle, Azure portal kullanarak aşağıdaki yönergeleri izleyin.  

IoT Central Uygulamanızı dağıttıktan sonra, iki kullanıma hazır sanal aygıtınız telemetri oluşturmaya başlar. Bu öğreticide, FHıR için Azure IoT Bağlayıcısı aracılığıyla *akıllı Vintals düzeltme* simülatörü simülatisini fhır 'ye aktaracağız. IoT verilerinizi FHıR için Azure IoT bağlayıcısına aktarmak için [IoT Central içinde bir veri dışarı aktarma (eski) kurmak](../../iot-central/core/howto-export-data-legacy.md)istiyoruz. Veri dışa aktarma (eski) sayfasında:
- Dışarı aktarma hedefi olarak *Azure Event Hubs* 'yi seçin.
- **Event Hubs ad alanı** alanı için *bağlantı dizesi değeri kullan* ' ı seçin.
- FHıR **'nin bağlantı dizesi alanı için** önceki bir adımda elde edilen bağlantı dizesi Için Azure IoT Bağlayıcısı 'nı sağlayın.
- **Verilerin dışarı aktarılması** için **telemetri** seçeneğini *Açık* tutun.

---

## <a name="view-device-data-in-azure-api-for-fhir"></a>FHıR için Azure API 'de cihaz verilerini görüntüleme

PMAN kullanarak FHıR sunucunuzda fhır için Azure IoT Bağlayıcısı tarafından oluşturulan FHıR tabanlı gözlemler kaynağını görüntüleyebilirsiniz. [Fhır Için Azure API 'sine erişmek üzere Postman](access-fhir-postman-tutorial.md) 'nizi ayarlayın ve `GET` `https://your-fhir-server-url/Observation?code=http://loinc.org|8867-4` sinyal oranı değeri olan gözehır kaynaklarını görüntülemek için bir istek yapın.

> [!TIP]
> Kullanıcının FHıR veri düzlemi için Azure API 'sine uygun erişimi olduğundan emin olun. Gerekli veri düzlemi rollerini atamak için [Azure rol tabanlı erişim denetimi 'ni (Azure RBAC)](configure-azure-rbac.md) kullanın.

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubundaki kaynakları silen kaynak grubunu silin.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com), **kaynak gruplarını** arayıp seçin.

2. Kaynak grubu listesinde, kaynak grubunuzun adını seçin.

3. Kaynak grubunuzun **genel bakış** sayfasında **kaynak grubunu sil**' i seçin.

4. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to delete"
Write-Verbose "Remove-AzResourceGroup -Name $resourceGroupName" -Verbose
Remove-AzResourceGroup -Name $resourceGroupName
Read-Host "Press [ENTER] to continue"
```

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
read -p "Enter the name of the resource group to delete: " resourceGroupName &&
echo "DELETE A RESOURCE GROUP (AND ITS RESOURCES):  az group delete --name $resourceGroupName" &&
az group delete --name $resourceGroupName &&
read -p "Press [ENTER] to continue: "
```

---

ARM şablonu oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için, [Ilk ARM şablonunuzu oluşturma ve dağıtma öğreticisine](../../azure-resource-manager/templates/template-tutorial-create-first-template.md) bakın

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda fhır için Azure API 'niz için Azure IoT Bağlayıcısı özelliğini dağıttık. FHıR için Azure IoT Bağlayıcısı hakkında daha fazla bilgi edinmek için aşağıdaki adımları aşağıdan seçin:

Azure IoT Bağlayıcısı içindeki FHIR için farklı veri akışı aşamalarını anlayın.

>[!div class="nextstepaction"]
>[FHıR veri akışı için Azure IoT Bağlayıcısı](iot-data-flow.md)

Cihaz ve FHıR eşleme şablonlarını kullanarak IoT bağlayıcısını nasıl yapılandıracağınızı öğrenin.

>[!div class="nextstepaction"]
>[FHıR eşleme şablonları için Azure IoT Bağlayıcısı](iot-mapping-templates.md)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır. FHıR, HL7 'in tescilli ticari markasıdır ve HL7 izniyle kullanılır.
