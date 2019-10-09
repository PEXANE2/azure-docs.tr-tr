---
title: Visual Studio Azure Kaynak grubu projelerini oluşturma & dağıtma
description: Azure Kaynak grubu projesi oluşturmak ve kaynakları Azure 'a dağıtmak için Visual Studio 'Yu kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: tomfitz
ms.openlocfilehash: c7f71dbadf24244756cf65f68ceea24547190737
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170252"
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Visual Studio aracılığıyla Azure Kaynak grupları oluşturma ve dağıtma

Visual Studio ile altyapınızı ve kodunuzu Azure 'a dağıtan bir proje oluşturabilirsiniz. Örneğin, Web sitesi için Web konağını, Web sitesini ve kodu dağıtabilirsiniz. Visual Studio, yaygın senaryoları dağıtmak için birçok farklı Başlatıcı şablonu sağlar. Bu makalede bir Web uygulaması dağıtırsınız.

Bu makalede [, Visual Studio 2019 veya sonraki bir sürümünü kullanarak Azure geliştirme ve ASP.NET iş yükleri yüklü](/visualstudio/install/install-visual-studio?view=vs-2019)olarak açıklanmaktadır. Visual Studio 2017 kullanıyorsanız, deneyiminiz büyük ölçüde aynıdır.

## <a name="create-azure-resource-group-project"></a>Azure Kaynak grubu projesi oluştur

Bu bölümde, bir **Web uygulaması** şablonuyla bir Azure Kaynak grubu projesi oluşturacaksınız.

1. Visual Studio 'da **Dosya**, **Yeni**ve **Proje**' yi seçin. **Azure Kaynak grubu** proje şablonunu ve Ileri ' **yi**seçin.

    ![Proje oluştur](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Projenize bir ad verin. Diğer varsayılan ayarlar muhtemelen iyidir, ancak ortamınız için çalışmak üzere bunları gözden geçirin. İşiniz bittiğinde **Oluştur**' u seçin.

    ![Proje oluştur](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/name-project.png)

1. Azure Resource Manager dağıtmak istediğiniz şablonu seçin. Dağıtmak istediğiniz proje türüne göre birçok farklı seçenek olduğuna dikkat edin. Bu makale için **Web uygulaması** şablonunu ve **Tamam**' ı seçin.

    ![Şablon seçin](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Seçtiğiniz şablon yalnızca bir başlangıç noktasıdır; Senaryonuzu karşılamak için kaynak ekleyebilir ve kaldırabilirsiniz.

1. Visual Studio, Web uygulaması için bir kaynak grubu dağıtım projesi oluşturur. Projenizin dosyalarını görmek için dağıtım projesindeki düğüme bakın.

    ![Düğümleri göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Web uygulaması şablonunu seçtiğinizden, aşağıdaki dosyaları görürsünüz:

   | Dosya adı | Description |
   | --- | --- |
   | Deploy-AzureResourceGroup. ps1 |Azure Resource Manager dağıtmak için PowerShell komutlarını çalıştıran bir PowerShell betiği. Visual Studio, şablonunuzu dağıtmak için bu PowerShell betiğini kullanır. |
   | Web sitesi. JSON |Azure 'a dağıtmak istediğiniz altyapıyı ve dağıtım sırasında sağlayabilmeniz gereken parametreleri tanımlayan Kaynak Yöneticisi şablonu. Ayrıca kaynaklar arasındaki bağımlılıkları tanımlar, böylece Kaynak Yöneticisi kaynakları doğru sırayla dağıtır. |
   | Web sitesi. Parameters. JSON |Şablon için gerekli değerlere sahip bir parametre dosyası. Her bir dağıtımı özelleştirmek için parametre değerlerini geçirirsiniz. |

    Tüm kaynak grubu dağıtım projelerinin bu temel dosyaları vardır. Diğer projelerde diğer işlevleri desteklemek için ek dosyalar bulunabilir.

## <a name="customize-resource-manager-template"></a>Kaynak Yöneticisi şablonu özelleştirme

Dağıtım projesini, dağıtmak istediğiniz kaynakları açıklayan Kaynak Yöneticisi şablonunu değiştirerek özelleştirebilirsiniz. Kaynak Yöneticisi şablonun öğeleri hakkında bilgi edinmek için bkz. [yazma Azure Resource Manager şablonları](resource-group-authoring-templates.md).

1. Şablonunuzda çalışmak için **Web sitesi. JSON**' u açın.

1. Visual Studio Düzenleyicisi, Kaynak Yöneticisi şablonu düzenleme konusunda size yardımcı olan araçlar sağlar. **JSON ana hat** penceresi, şablonunuzda tanımlı öğeleri görmeyi kolaylaştırır.

   ![JSON ana hattını göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

1. Şablonun ilgili bölümüne gitmek için anahatta bir öğe seçin.

   ![JSON 'a git](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

1. JSON ana hattı penceresinin üst kısmında bulunan **Kaynak Ekle** düğmesini seçerek veya **kaynaklar** ' a sağ tıklayıp **Yeni Kaynak Ekle**' yi seçerek bir kaynak ekleyebilirsiniz.

   ![Kaynak Ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

1. **Depolama hesabı** ' nı seçin ve bir ad verin. 11 karakterden uzun olmayan bir ad belirtin ve yalnızca sayılar ve küçük harfler içerir.

   ![Depolama Ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

1. Yalnızca kaynak eklenmemiş, aynı zamanda depolama hesabı türü için de bir parametre ve depolama hesabının adı için bir değişken olmadığına dikkat edin.

   ![Ana hattı göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

1. Depolama hesabı türünün parametresi, izin verilen türler ve varsayılan bir tür ile önceden tanımlanmıştır. Bu değerleri bırakabilir veya senaryolarınız için düzenleyebilirsiniz. Bu şablon aracılığıyla kimsenin bir **Premium_LRS** depolama hesabı dağıtmasını istemiyorsanız, izin verilen türlerden kaldırın.

   ```json
   "demoaccountType": {
     "type": "string",
     "defaultValue": "Standard_LRS",
     "allowedValues": [
       "Standard_LRS",
       "Standard_ZRS",
       "Standard_GRS",
       "Standard_RAGRS"
     ]
   }
   ```

1. Visual Studio, şablonu düzenlenirken kullanılabilen özellikleri anlamanıza yardımcı olmak için IntelliSense de sağlar. Örneğin, App Service planınızın özelliklerini düzenlemek için **hostingplan** kaynağına gidin ve **Özellikler**için bir değer ekleyin. IntelliSense 'in kullanılabilir değerleri gösterdiğini ve bu değere ilişkin bir açıklama sağladığını unutmayın.

   ![IntelliSense 'i göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

   **Numberofçalışanları** 1 olarak ayarlayabilir ve dosyayı kaydedebilirsiniz.

   ```json
   "properties": {
     "name": "[parameters('hostingPlanName')]",
     "numberOfWorkers": 1
   }
   ```

1. **Web sitesi. Parameters. JSON** dosyasını açın. Dağıtım sırasında dağıtılan kaynağı özelleştiren değerleri geçirmek için parametreler dosyasını kullanın. Barındırma planına bir ad verin ve dosyayı kaydedin.

   ```json
   {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "hostingPlanName": {
         "value": "demoHostPlan"
       }
     }
   }
   ```

## <a name="deploy-project-to-azure"></a>Projeyi Azure 'a dağıtma

Artık projenizi bir kaynak grubuna dağıtmaya hazırsınız.

Varsayılan olarak, projedeki PowerShell betiği (Deploy-AzureResourceGroup. ps1) Azurerd modülünü kullanır. AzureRM modülünü hala yüklediyseniz ve kullanmaya devam etmek istiyorsanız, bu varsayılan betiği kullanabilirsiniz. Bu komut dosyası ile çözümünüzü dağıtmak için Visual Studio arabirimini kullanabilirsiniz.

Ancak, yeni [az modüle](/powershell/azure/new-azureps-module-az)geçiş yaptıysanız projenize yeni bir komut dosyası eklemeniz gerekir. Az Module kullanan bir betik eklemek için [Deploy-AzTemplate. ps1](https://github.com/Azure/azure-quickstart-templates/blob/master/Deploy-AzTemplate.ps1) betiğini kopyalayın ve projenize ekleyin. Bu betiği dağıtım için kullanmak üzere, Visual Studio 'nun dağıtım arabirimini kullanmak yerine bir PowerShell konsolundan çalıştırmanız gerekir.

Her iki yaklaşım da bu makalede gösterilmektedir. Bu makale, Azurere modül betiği olarak varsayılan komut dosyası ve az Module betiği olarak yeni komut dosyası anlamına gelir.

### <a name="az-module-script"></a>Az Module betiği

Az Module betiği için bir PowerShell konsolu açın ve şunu çalıştırın:

```powershell
.\Deploy-AzTemplate.ps1 -ArtifactStagingDirectory . -Location centralus -TemplateFile WebSite.json -TemplateParametersFile WebSite.parameters.json
```

### <a name="azurerm-module-script"></a>Azurerd modülü betiği

Azurerd modül betiği için Visual Studio 'Yu kullanın:

1. Dağıtım projesi düğümünün kısayol menüsünde @no__t **Dağıt**-1**Yeni**' yi seçin.

    ![Yeni dağıtım menü öğesi](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

1. **Kaynak grubuna dağıt** iletişim kutusu görüntülenir. **Kaynak grubu** açılır kutusunda, var olan bir kaynak grubunu seçin veya yeni bir tane oluşturun. **Dağıt**' ı seçin.

    ![Kaynak grubuna Dağıt iletişim kutusu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. **Çıkış** penceresinde, dağıtımın durumunu görürsünüz. Dağıtım tamamlandığında, son ileti şuna benzer bir şekilde dağıtım başarılı olduğunu gösterir:

   ```output
   18:00:58 - Successfully deployed template 'website.json' to resource group 'ExampleAppDeploy'.
   ```

## <a name="view-deployed-resources"></a>Dağıtılan kaynakları görüntüle

Sonuçları kontrol edelim.

1. Bir tarayıcıda [Azure Portal](https://portal.azure.com/) açın ve hesabınızda oturum açın. Kaynak grubunu görmek için, **kaynak grupları** ve dağıttığınız kaynak grubunu seçin.

1. Dağıtılan tüm kaynakları görürsünüz. Depolama hesabı adının bu kaynağı eklerken tam olarak belirtdikdiğine dikkat edin. Depolama hesabının benzersiz olması gerekir. Şablon, benzersiz bir ad oluşturmak için girdiğiniz ada otomatik olarak bir karakter dizesi ekler.

    ![Kaynakları göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

## <a name="add-code-to-project"></a>Projeye kod Ekle

Bu noktada, uygulamanız için altyapıyı dağıttık, ancak projeyle birlikte dağıtılan gerçek kod yok.

1. Visual Studio çözümünüze bir proje ekleyin. Çözüme sağ tıklayın ve  > **Yeni proje** **Ekle**' yi seçin.

    ![Proje Ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. ASP.NET Core bir **Web uygulaması**ekleyin.

    ![Web uygulaması Ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)

1. Web uygulamanıza bir ad verin ve **Oluştur**' u seçin.

    ![Web uygulamasını Adlandır](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/name-web-app.png)

1. **Web uygulaması** ' nı seçin ve **oluşturun**.

    ![Web uygulaması Seç](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project-type.png)

1. Visual Studio Web uygulamanızı oluşturduktan sonra, çözümdeki her iki projeyi de görürsünüz.

    ![Projeleri göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Şimdi, kaynak grubu projenizin yeni projenin farkında olduğundan emin olmanız gerekir. Kaynak grubu projenize (ExampleAppDeploy) geri dönün. **Başvurular** ' a sağ tıklayın ve **Başvuru Ekle**' yi seçin.

    ![Başvuru ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Oluşturduğunuz Web uygulaması projesini seçin.

   ![Başvuru ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)

   Bir başvuru ekleyerek, Web uygulaması projesini kaynak grubu projesine bağlarsınız ve bazı özellikleri otomatik olarak ayarlar. Bu özellikleri başvurunun **Özellikler** penceresinde görürsünüz. **Içerme dosyası yolu** , paketin oluşturulduğu yolu içerir. Klasöre (ExampleApp) ve dosyaya (Package. zip) göz önünde. Bu değerleri, uygulamayı dağıtmada parametreler olarak sağladığınız için bilmeniz gerekir.

   ![Başvuruya bakın](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)

1. Şablonunuza (WebSite. JSON) geri dönün ve şablona bir kaynak ekleyin.

    ![Kaynak Ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Bu kez **Web Apps için Web dağıtımı**seçin.

    ![Web dağıtımı Ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)

   Şablonunuzu kaydedin.

1. Şablonunuzda bazı yeni parametreler vardır. Önceki adımda eklenirler. **_Artifactslocation** veya **_Artifactslocationsastoken** değerlerini, bu değerler otomatik olarak oluşturulduğundan sağlamanız gerekmez. Ancak, klasör ve dosya adını dağıtım paketini içeren yola ayarlamanız gerekir. Bu parametrelerin adları **PackageFolder** ve **packagefilename**ile biter. Adın ilk bölümü, eklediğiniz Web Dağıtımı kaynağının adıdır. Bu makalede, bunlar **Exampleapppackagefolder** ve **Exampleapppackagefilename**olarak adlandırılmaktadır.

   **Web sitesi. Parameters. JSON** ' u açın ve bu parametreleri başvuru özelliklerinde gördüğünüz değerlere ayarlayın. **Exampleapppackagefolder** değerini klasörün adına ayarlayın. **Exampleapppackagefilename** değerini ZIP dosyasının adına ayarlayın.

   ```json
   {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "hostingPlanName": {
         "value": "demoHostPlan"
       },
       "ExampleAppPackageFolder": {
         "value": "ExampleApp"
       },
       "ExampleAppPackageFileName": {
         "value": "package.zip"
       }
     }
   }
   ```

## <a name="deploy-code-with-infrastructure"></a>Altyapı ile kod dağıtma

Projeye kod eklediyseniz, dağıtımınız bu kez biraz farklı olur. Dağıtım sırasında, projeniz için yapıtları Kaynak Yöneticisi erişebileceği bir yere aşamalandırın. Yapıtlar bir depolama hesabına hazırlanır.

### <a name="az-module-script"></a>Az Module betiği

Az Module betiğini kullanıyorsanız, şablonunuzda yapmanız gereken küçük bir değişiklik vardır. Bu betik, yapıt konumuna eğik çizgi ekler, ancak şablonunuz bu eğik çizgiyi beklemez. Web sitesi. json ' yı açın ve MSDeploy uzantısının özelliklerini bulun. **PackageURI**adlı bir özelliğe sahiptir. Yapıt konumuyla paket klasörü arasındaki eğik çizgiyi kaldırın.

Şöyle görünmelidir:

```json
"packageUri": "[concat(parameters('_artifactsLocation'), parameters('ExampleAppPackageFolder'), '/', parameters('ExampleAppPackageFileName'), parameters('_artifactsLocationSasToken'))]",
```

Önceki örnekte **Parametreler (' _artifactsLocation ')** ve **Parametreler arasında (' ExampleAppPackageFolder ')** `'/',` olmadığını unutmayın.

Projeyi yeniden derleyin. Projeyi oluşturmak, dağıtmanız gereken dosyaların hazırlama klasörüne eklendiğinden emin olmanızı sağlar.

Şimdi bir PowerShell konsolu açın ve şunu çalıştırın:

```powershell
.\Deploy-AzTemplate.ps1 -ArtifactStagingDirectory .\bin\Debug\staging\ExampleAppDeploy -Location centralus -TemplateFile WebSite.json -TemplateParametersFile WebSite.parameters.json -UploadArtifacts -StorageAccountName <storage-account-name>
```

### <a name="azurerm-module-script"></a>Azurerd modülü betiği

Azurerd modül betiği için Visual Studio 'Yu kullanın:

1. Yeniden dağıtmak için **Dağıt**' ı ve daha önce dağıttığınız kaynak grubunu seçin.

    ![Projeyi yeniden Dağıt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

1. **Yapıt depolama hesabı**için bu kaynak grubuyla dağıttığınız depolama hesabını seçin.

   ![Web dağıtımını yeniden Dağıt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy-web-app.png)

## <a name="view-web-app"></a>Web uygulamasını görüntüle

1. Dağıtım bittikten sonra, portalda Web uygulamanızı seçin. Siteye gitmek için URL 'YI seçin.

   ![Siteye gözatamıyorum](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Varsayılan ASP.NET uygulamasını başarıyla dağıttığınıza dikkat edin.

   ![Dağıtılan uygulamayı göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="add-operations-dashboard"></a>İşlem panosu ekleme

Yalnızca Visual Studio arabirimi aracılığıyla kullanılabilen kaynaklarla sınırlı değilsiniz. Şablonunuza özel bir kaynak ekleyerek dağıtımınızı özelleştirebilirsiniz. Kaynak eklemeyi göstermek için, dağıttığınız kaynağı yönetmek üzere bir işlemsel Pano eklersiniz.

1. Web sitesi. json dosyasını açın ve depolama hesabı kaynağı ' nı kapatmadan önce, ancak kaynaklar bölümünün `]` ' ından önce Şu JSON 'yi ekleyin.

   ```json
    ,{
      "properties": {
        "lenses": {
          "0": {
            "order": 0,
            "parts": {
              "0": {
                "position": {
                  "x": 0,
                  "y": 0,
                  "colSpan": 4,
                  "rowSpan": 6
                },
                "metadata": {
                  "inputs": [
                    {
                      "name": "resourceGroup",
                      "isOptional": true
                    },
                    {
                      "name": "id",
                      "value": "[resourceGroup().id]",
                      "isOptional": true
                    }
                  ],
                  "type": "Extension/HubsExtension/PartType/ResourceGroupMapPinnedPart"
                }
              },
              "1": {
                "position": {
                  "x": 4,
                  "y": 0,
                  "rowSpan": 3,
                  "colSpan": 4
                },
                "metadata": {
                  "inputs": [],
                  "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                  "settings": {
                    "content": {
                      "settings": {
                        "content": "__Customizations__\n\nUse this dashboard to create and share the operational views of services critical to the application performing. To customize simply pin components to the dashboard and then publish when you're done. Others will see your changes when you publish and share the dashboard.\n\nYou can customize this text too. It supports plain text, __Markdown__, and even limited HTML like images <img width='10' src='https://portal.azure.com/favicon.ico'/> and <a href='https://azure.microsoft.com' target='_blank'>links</a> that open in a new tab.\n",
                        "title": "Operations",
                        "subtitle": "[resourceGroup().name]"
                      }
                    }
                  }
                }
              }
            }
          }
        },
        "metadata": {
          "model": {
            "timeRange": {
              "value": {
                "relative": {
                  "duration": 24,
                  "timeUnit": 1
                }
              },
              "type": "MsPortalFx.Composition.Configuration.ValueTypes.TimeRange"
            }
          }
        }
      },
      "apiVersion": "2015-08-01-preview",
      "name": "[concat('ARM-',resourceGroup().name)]",
      "type": "Microsoft.Portal/dashboards",
      "location": "[resourceGroup().location]",
      "tags": {
        "hidden-title": "[concat('OPS-',resourceGroup().name)]"
      }
    }
   ```

1. Projenizi yeniden dağıtın.

1. Dağıtım tamamlandıktan sonra panonuzu portalda görüntüleyin. **Panoyu** seçin ve dağıttığınız bir tane seçin.

   ![Özel Pano](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/view-custom-dashboards.png)

1. Özelleştirilmiş panoyu görürsünüz.

   ![Özel Pano](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/Ops-DemoSiteGroup-dashboard.png)

RBAC gruplarını kullanarak panoya erişimi yönetebilirsiniz. Ayrıca, panonun görüntüsünü dağıtıldıktan sonra özelleştirebilirsiniz. Ancak, kaynak grubunu yeniden dağıtırsanız, pano şablonunuzda varsayılan durumuna sıfırlanır. Panolar oluşturma hakkında daha fazla bilgi için bkz. [Program aracılığıyla Azure panoları oluşturma](../azure-portal/azure-portal-dashboards-create-programmatically.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarına artık ihtiyaç duyulmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grupları** ' nı seçin.

1. Kaynak grubu adını seçin.

1. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Visual Studio kullanarak şablon oluşturmayı ve dağıtmayı öğrendiniz. Şablon geliştirme hakkında daha fazla bilgi edinmek için bkz. Yeni Başlangıç Öğreticisi serisi:

> [!div class="nextstepaction"]
> [Başlangıç öğreticileri](./template-tutorial-create-first-template.md)
