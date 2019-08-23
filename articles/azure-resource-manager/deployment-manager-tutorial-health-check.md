---
title: Azure Deployment Manager’ı Resource Manager şablonlarıyla kullanma | Microsoft Docs
description: Azure kaynaklarını dağıtmak için Azure Deployment Manager ile Resource Manager şablonlarını kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.date: 05/31/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: def218564511b65809e776f97d0962b72f72adc2
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69898130"
---
# <a name="tutorial-use-health-check-in-azure-deployment-manager-public-preview"></a>Öğretici: Azure Dağıtım Yöneticisi sistem durumu denetimi kullanma (Genel Önizleme)

[Azure dağıtım Yöneticisi](./deployment-manager-overview.md)'de sistem durumu denetimini tümleştirmeyi öğrenin. Bu öğretici, [Kaynak Yöneticisi şablonları Ile Azure dağıtım Yöneticisi kullanma](./deployment-manager-tutorial.md) öğreticisine dayanır. Bu öğreticiye devam etmeden önce bu öğreticiyi tamamlamalısınız.

[Azure dağıtım Yöneticisi kullanma](./deployment-manager-tutorial.md)bölümünde kullanılan dağıtım şablonunda, Kaynak Yöneticisi şablonlarla bir bekleme adımı kullandınız. Bu öğreticide, bekleme adımını bir sistem durumu denetimi adımıyla değiştirirsiniz.

> [!IMPORTANT]
> Aboneliğiniz yeni Azure özelliklerini test etmek üzere işaretlenmişse, Azure Dağıtım Yöneticisi 'yi yalnızca Canary bölgelerine dağıtmak için kullanabilirsiniz. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Durum denetimi hizmeti simülatörü oluşturma
> * Piyasaya çıkma şablonunu gözden geçirin
> * Topolojiyi dağıtma
> * Dağıtımı sağlıksız durumuyla dağıt
> * Dağıtım dağıtımını doğrulama
> * Dağıtımı sağlıklı durumla dağıtma
> * Dağıtım dağıtımını doğrulama
> * Kaynakları temizleme

Ek kaynaklar:

- [Azure Dağıtım Yöneticisi REST API başvurusu](https://docs.microsoft.com/rest/api/deploymentmanager/).
- [Azure dağıtım Yöneticisi örneği](https://github.com/Azure-Samples/adm-quickstart).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Azure Dağıtım Yöneticisi kaynak yöneticisi şablonlarla kullanın](./deployment-manager-tutorial.md).
* Bu öğretici tarafından kullanılan [şablonları ve yapıtları](https://armtutorials.blob.core.windows.net/admtutorial/ADMTutorial.zip) indirin.

## <a name="create-a-health-check-service-simulator"></a>Durum denetimi hizmeti simülatörü oluşturma

Üretimde, genellikle bir veya daha fazla izleme sağlayıcısı kullanırsınız. Sistem durumu tümleştirmesini mümkün olduğunca kolay hale getirmek için, Microsoft, en önemli hizmet durumu izleme şirketleriyle birlikte çalışarak dağıtımlarınızla durum denetimlerini tümleştirmek üzere basit bir kopyalama/yapıştırma çözümü sağlar. Bu şirketlerin listesi için bkz. [sistem durumu izleme sağlayıcıları](./deployment-manager-health-check.md#health-monitoring-providers). Bu öğreticinin amacı doğrultusunda, bir sistem durumu izleme hizmetinin benzetimini yapmak için bir [Azure işlevi](/azure/azure-functions/) oluşturursunuz. Bu işlev bir durum kodu alır ve aynı kodu döndürür. Azure Dağıtım Yöneticisi şablonunuz dağıtım ile devam etmek için durum kodunu kullanır.

Azure Işlevini dağıtmak için aşağıdaki iki dosya kullanılır. Öğreticiye gitmek için bu dosyaları indirmeniz gerekmez.

* Konumunda [https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json](https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json)bulunan bir kaynak yöneticisi şablonu. Bu şablonu, bir Azure Işlevi oluşturmak için dağıtırsınız.
* Azure Işlevi kaynak kodunun [https://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip](https://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip)bir ZIP dosyası. Çağrılan bu zip Kaynak Yöneticisi şablonu tarafından çağırılır.

Azure işlevini dağıtmak için **dene** ' yi seçerek Azure Cloud Shell 'i açın ve ardından aşağıdaki betiği kabuk penceresine yapıştırın.  Kodu yapıştırmak için kabuk penceresine sağ tıklayıp **Yapıştır**' ı seçin.

> [!IMPORTANT]
> PowerShell betiğinin **ProjectName** , bu öğreticide dağıtılan Azure hizmetlerinin adlarını oluşturmak için kullanılır. Farklı Azure Hizmetleri, adlar üzerinde farklı gereksinimlere sahiptir. Dağıtımın başarılı olduğundan emin olmak için, yalnızca küçük harf ve rakam içeren 12 karakterden daha az bir ad seçin.
> Proje adının bir kopyasını kaydedin. Öğreticide aynı projectName 'yi kullanırsınız.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json" -projectName $projectName
```

Azure işlevini doğrulamak ve test etmek için:

1. [Azure portalı](https://portal.azure.com) açın.
1. Kaynak grubunu açın.  Varsayılan ad, **RG** eklenmiş proje adıdır.
1. Kaynak grubundan App Service ' i seçin.  App Service 'in varsayılan adı, **WebApp** eklenen proje adıdır.
1. **İşlevler**' i genişletin ve ardından **HttpTrigger1**' ı seçin.

    ![Azure Dağıtım Yöneticisi sistem durumu denetimi Azure Işlevi](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-function.png)

1. **&lt;İşlev URL 'sini al/>** seçin.
1. URL 'YI panoya kopyalamak için **Kopyala** ' yı seçin.  URL şuna benzer:

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/{healthStatus}?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    URL `{healthStatus}` 'de bir durum kodu ile değiştirin. Bu öğreticide, sağlıksız senaryoyu test etmek için **sağlıksız** kullanın ve sağlıklı senaryoyu test etmek için **sağlıklı** ya da **uyarıyı** kullanın. Biri sağlıksız durum ve diğeri sağlıklı durumda olan iki URL oluşturun. Örnekler için:

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/healthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    Bu öğreticinin tamamlanması için her iki URL 'ye de ihtiyacınız vardır.

1. Sistem durumu izleme simülatörünü test etmek için, son adımda oluşturduğunuz URL 'Leri açın.  Sağlıksız durum sonuçları şuna benzer olacaktır:

    ```
    Status: unhealthy
    ```

## <a name="revise-the-rollout-template"></a>Piyasaya çıkma şablonunu gözden geçirin

Bu bölümün amacı, dağıtım şablonunda bir sistem durumu denetimi adımının nasıl ekleneceğini gösterir. Bu öğreticiyi tamamlayabilmeniz için kendi Createadmpiyasaya çıkma. JSON dosyanızı oluşturmanız gerekmez. Düzeltilen dağıtım şablonu, sonraki bölümlerde kullanılan bir depolama hesabında paylaşılır.

1. **Createadmpiyasaya çıkma. JSON**dosyasını açın. Bu JSON dosyası, indirmenin bir parçasıdır.  [Ön koşullara](#prerequisites) bakın.
1. İki parametre daha ekleyin:

    ```json
    "healthCheckUrl": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check URL."
        }
    },
    "healthCheckAuthAPIKey": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check Azure Function function authorization key."
        }
    }
    ```

1. Bekleme adımı kaynak tanımını bir sistem durumu denetimi adımı kaynak tanımıyla değiştirin:

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT0M",
          "maxElasticDuration": "PT0M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "ApiKey",
                    "name": "code",
                    "in": "Query",
                    "value": "[parameters('healthCheckAuthAPIKey')]"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "Status: healthy",
                      "Status: warning"
                    ],
                    "matchQuantifier": "Any"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

    Tanım temelinde, dağıtım sistem durumu *sağlıklı* veya *Uyarı*olursa devam eder.

1. Yeni tanımlanan sistem durumu denetimi adımını dahil etmek için, piyasaya çıkma tanımının bağımlılığını güncelleştirin:

    ```json
    "dependsOn": [
        "[resourceId('Microsoft.DeploymentManager/artifactSources', variables('rolloutArtifactSource').name)]",
        "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
    ],
    ```

1. **StepGroups** durumunu, sistem durumu denetimi adımını içerecek şekilde güncelleştirin. **Healthcheckstep** , **StepGroup2**'in **postdeploymentsteps** içinde çağırılır. **stepGroup3** ve **stepGroup4** yalnızca sağlıklı durum *sağlıklı* veya *Uyarı*olduğunda dağıtılır.

    ```json
    "stepGroups": [
        {
            "name": "stepGroup1",
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup2",
            "dependsOnStepGroups": ["stepGroup1"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit1.name)]",
            "postDeploymentSteps": [
                {
                    "stepId": "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
                }
            ]
        },
        {
            "name": "stepGroup3",
            "dependsOnStepGroups": ["stepGroup2"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup4",
            "dependsOnStepGroups": ["stepGroup3"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit1.name)]",
            "postDeploymentSteps": []
        }
    ]
    ```

    **StepGroup3** bölümünü, yeniden güncelleştirildikten önce ve sonra karşılaştırırsanız, bu bölüm artık **stepGroup2**'e bağımlıdır.  Bu, **stepGroup3** ve sonraki adım grupları sistem durumu izlemenin sonuçlarına bağlı olduğunda gereklidir.

    Aşağıdaki ekran görüntüsünde, değiştirilen bölgeler ve sistem durumu denetimi adımının nasıl kullanıldığı gösterilmektedir:

    ![Azure Dağıtım Yöneticisi sistem durumu denetimi şablonu](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-rollout-template.png)

## <a name="deploy-the-topology"></a>Topolojiyi dağıtma

Öğreticiyi basitleştirmek için, topoloji şablonu ve yapıtlar aşağıdaki konumlarda paylaşılır, böylece kendi kopyanızı hazırlamanız gerekmez. Kendinizinkini kullanmak istiyorsanız [öğreticideki yönergeleri izleyin: Kaynak Yöneticisi şablonlarla](./deployment-manager-tutorial.md)Azure dağıtım Yöneticisi kullanın.

* Topoloji şablonu:\\[https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplates/CreateADMServiceTopology.json](https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplates/CreateADMServiceTopology.json)
* Yapıt deposu:\\[https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore](https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore)

Topolojiyi dağıtmak için, Cloud Shell 'i açmak için **dene** ' yi seçin ve ardından PowerShell betiğini yapıştırın.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the service topology
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMServiceTopology.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation
```

Azure portalı kullanarak hizmet topolojisinin ve temel kaynakların başarıyla oluşturulduğunu doğrulayın:

![Azure Deployment Manager öğreticisi dağıtılan hizmet topolojisi kaynakları](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

Kaynakları görmek için **Gizli türleri göster** seçeneği belirlenmelidir.

## <a name="deploy-the-rollout-with-the-unhealthy-status"></a>Dağıtımı sağlıksız durumuyla dağıtma

Öğreticiyi basitleştirmek için, düzeltilmiş piyasaya çıkma şablonu aşağıdaki konumlarda paylaşılır, böylece kendi kopyanızı hazırlamanız gerekmez. Kendinizinkini kullanmak istiyorsanız [öğreticideki yönergeleri izleyin: Kaynak Yöneticisi şablonlarla](./deployment-manager-tutorial.md)Azure dağıtım Yöneticisi kullanın.

* Topoloji şablonu:\\[https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json](https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json)
* Yapıt deposu:\\[https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore](https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore)

[Durum denetimi hizmeti simülatörü oluşturma](#create-a-health-check-service-simulator)bölümünde oluşturduğunuz sağlıksız durum URL 'sini kullanın. **Managedıdentityıd**için bkz. [Kullanıcı tarafından atanan yönetilen kimliği oluşturma](./deployment-manager-tutorial.md#create-the-user-assigned-managed-identity).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$managedIdentityID = Read-Host -Prompt "Enter a user-assigned managed identity"
$healthCheckUrl = Read-Host -Prompt "Enter the health check Azure function URL"
$healthCheckAuthAPIKey = $healthCheckUrl.Substring($healthCheckUrl.IndexOf("?code=")+6, $healthCheckUrl.Length-$healthCheckUrl.IndexOf("?code=")-6)
$healthCheckUrl = $healthCheckUrl.Substring(0, $healthCheckUrl.IndexOf("?"))

$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the rollout
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation `
    -managedIdentityID $managedIdentityID `
    -healthCheckUrl $healthCheckUrl `
    -healthCheckAuthAPIKey $healthCheckAuthAPIKey
```

> [!NOTE]
> `New-AzResourceGroupDeployment`zaman uyumsuz bir çağrıdır. Başarı iletisi yalnızca dağıtımın başarıyla başladığını gösterir. Dağıtımı doğrulamak için kullanın `Get-AZDeploymentManagerRollout`.  Sonraki yordama bakın.

Aşağıdaki PowerShell betiğini kullanarak dağıtım ilerlemesini denetlemek için:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$resourceGroupName = "${projectName}rg"
$rolloutName = "${projectName}Rollout"

# Get the rollout status
Get-AzDeploymentManagerRollout `
    -ResourceGroupName $resourceGroupName `
    -Name $rolloutName `
    -Verbose
```

Aşağıdaki örnek çıktı, sağlıksız durum nedeniyle dağıtımın başarısız olduğunu gösterir:

```output
Service: myhc0417ServiceWUSrg
    TargetLocation: WestUS
    TargetSubscriptionId: <Subscription ID>

    ServiceUnit: myhc0417ServiceWUSWeb
        TargetResourceGroup: myhc0417ServiceWUSrg

        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
Get-AzDeploymentManagerRollout :
Service: myhc0417ServiceWUSrg
    ServiceUnit: myhc0417ServiceWUSWeb
        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
At line:1 char:1
+ Get-AzDeploymentManagerRollout `
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Get-AzDeploymentManagerRollout], Exception
+ FullyQualifiedErrorId : RolloutFailed,Microsoft.Azure.Commands.DeploymentManager.Commands.GetRollout


ResourceGroupName       : myhc0417rg
BuildVersion            : 1.0.0.0
ArtifactSourceId        : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/artifactSources/myhc0417ArtifactSourceRollout
TargetServiceTopologyId : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/serviceTopologies/myhc0417ServiceTopology
Status                  : Failed
TotalRetryAttempts      : 0
Identity                : Microsoft.Azure.Commands.DeploymentManager.Models.PSIdentity
OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
Services                : {myhc0417ServiceWUS, myhc0417ServiceWUSrg}
Name                    : myhc0417Rollout
Type                    : Microsoft.DeploymentManager/rollouts
Location                : centralus
Id                      : /subscriptions/<Subscription ID>/resourcegroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/rollouts/myhc0417Rollout
Tags                    :
```

Dağıtım tamamlandıktan sonra, Batı ABD için oluşturulmuş bir ek kaynak grubu görürsünüz.

## <a name="deploy-the-rollout-with-the-healthy-status"></a>Dağıtımı sağlıklı durumuyla dağıtma

Dağıtımı sağlıklı durum URL 'SI ile yeniden dağıtmak için bu bölümü tekrarlayın.  Dağıtım tamamlandıktan sonra, Doğu ABD için bir kaynak grubunun oluşturulduğunu daha görürsünüz.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure portalı](https://portal.azure.com) açın.
2. Piyasaya çıkarma dağıtımı tarafından oluşturulan yeni kaynak gruplarındaki yeni oluşturulan web uygulamalarına gidin.
3. Web uygulamasını bir web tarayıcıda açın. Index.html dosyasında konumu ve sürümü doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. Bu öğreticide oluşturulan kaynak gruplarını daraltmak için **Ada göre filtrele** alanını kullanın. 3-4 adet olacaktır:

    * **&lt;namePrefix>rg**: Deployment Manager kaynaklarını içerir.
    * **&lt;namePrefix>ServiceWUSrg**: ServiceWUS tarafından tanımlanan kaynakları içerir.
    * **&lt;namePrefix>ServiceEUSrg**: ServiceEUS tarafından tanımlanan kaynakları içerir.
    * Kullanıcı tanımlı yönetilen kimlik için kaynak grubu.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.
5. Bu öğretici tarafından oluşturulan diğer kaynak gruplarını silmek için son iki adımı tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Dağıtım Yöneticisi sistem durumu denetimi özelliğini kullanmayı öğrendiniz. Daha fazla bilgi edinmek için bkz. [Azure Resource Manager belgeleri](/azure/azure-resource-manager/).
