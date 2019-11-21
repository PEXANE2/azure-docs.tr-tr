---
title: Azure’da bir Service Fabric uygulamasını bir kümeye dağıtma | Microsoft Docs
description: Uygulamayı Visual Studio'dan bir kümeye dağıtmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/22/2019
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 37c27ae71eddcb5a35b9baeae250bee232c7acb7
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74213192"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Öğretici: Azure’da bir Service Fabric uygulamasını kümeye dağıtma

Bu öğretici, bir dizinin ikinci bölümüdür. Azure Service Fabric uygulamasının Azure’da yeni kümeye nasıl dağıtılacağı gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Create a cluster.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Bu öğretici serisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md).
> * Uygulamayı uzak kümeye dağıtma.
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md).
> * [Azure Pipelines kullanarak CI/CD yapılandırın](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Install Visual Studio 2019](https://www.visualstudio.com/), and install the **Azure development** and **ASP.NET and web development** workloads.
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md).

> [!NOTE]
> A free account may not meet the requirements to create a virtual machine. This will prevent the completion of the tutorial. In addition, a non-work or non-school account may encounter permission issues while creating the certificate on the keyvault associated with the cluster. If you experience an error related to certificate creation use the Portal to create the cluster instead. 

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-dotnet-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. In a command window, run the following code to clone the sample application repository to your local machine.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

Open the application in Visual Studio, running as administrator, and build the application.

## <a name="create-a-cluster"></a>Küme oluşturma

Now that the application is ready, you create a Service Fabric cluster and then deploy the application to the cluster. [Service Fabric kümesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-anywhere), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir.

In this tutorial, you create a new three node test cluster in the Visual Studio IDE and then publish the application to that cluster. See the [Create and manage a cluster tutorial](service-fabric-tutorial-create-vnet-and-windows-cluster.md) for information on creating a production cluster. You can also deploy the application to an existing cluster that you previously created through the [Azure portal](https://portal.azure.com), by using [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) or [Azure CLI](./scripts/cli-create-cluster.md) scripts, or from an [Azure Resource Manager template](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

> [!NOTE]
> The Voting application, and many other applications, use the Service Fabric reverse proxy to communicate between services. Clusters created from Visual Studio have the reverse proxy enabled by default. If you're deploying to an existing cluster, you must [enable the reverse proxy in the cluster](service-fabric-reverseproxy-setup.md) for the Voting application to work.


### <a name="find-the-votingweb-service-endpoint"></a>VotingWeb hizmet uç noktasını bulun

The front-end web service of the Voting application is listening on a specific port (8080 if you in followed the steps in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md). Uygulama Azure'daki bir kümeye dağıtıldığında hem küme hem de uygulama bir Azure yük dengeleyicinin arkasında çalışır. The application port must be opened in the Azure load balancer by using a rule. The rule sends inbound traffic through the load balancer to the web service. Bağlantı noktası **VotingWeb/PackageRoot/ServiceManifest.xml** dosyasının **Endpoint** öğesinde bulunur. 

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Take note of the service endpoint, which is needed in a later step.  If you're deploying to an existing cluster, open this port by creating a load-balancing rule and probe in the Azure load balancer using a [PowerShell script](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) or via the load balancer for this cluster in the [Azure portal](https://portal.azure.com).

### <a name="create-a-test-cluster-in-azure"></a>Create a test cluster in Azure
Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin.

In **Connection Endpoint**, select **Create New Cluster**.  If you're deploying to an existing cluster, select the cluster endpoint from the list.  The Create Service Fabric Cluster dialog opens.

In the **Cluster** tab, enter the **Cluster name** (for example, "mytestcluster"), select your subscription, select a region for the cluster (such as South Central US), enter the number of cluster nodes (we recommend three nodes for a test cluster), and enter a resource group (such as "mytestclustergroup"). **İleri**’ye tıklayın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

In the **Certificate** tab, enter the password and output path for the cluster certificate. A self-signed certificate is created as a PFX file and saved to the specified output path.  The certificate is used for both node-to-node and client-to-node security.  Don't use a self-signed certificate for production clusters.  This certificate is used by Visual Studio to authenticate with the cluster and deploy applications. Select **Import certificate** to install the PFX in the CurrentUser\My certificate store of your computer.  **İleri**’ye tıklayın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/certificate.png)

In the **VM Detail** tab, enter the **User name** and **Password** for the cluster admin account.  Select the **Virtual machine image** for the cluster nodes and the **Virtual machine size** for each cluster node.  Click the **Advanced** tab.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/vm-detail.png)

In **Ports**, enter the VotingWeb service endpoint from the previous step (for example, 8080).  When the cluster is created, these application ports are opened in the Azure load balancer to forward traffic to the cluster.  Click **Create** to create the cluster, which takes several minutes.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/advanced.png)

## <a name="publish-the-application-to-the-cluster"></a>Publish the application to the cluster

When the new cluster is ready, you can deploy the Voting application directly from Visual Studio.

Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin. **Yayımla** iletişim kutusu görüntülenir.

In **Connection Endpoint**, select the endpoint for the cluster you created in the previous step.  For example, "mytestcluster.southcentral.cloudapp.azure.com:19000". If you select **Advanced Connection Parameters**, the certificate information should be auto-filled.  
![Publish a Service Fabric application](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

**Yayımla**’yı seçin.

Once the application is deployed, open a browser and enter the cluster address followed by **:8080**. Veya başka bir bağlantı noktası yapılandırdıysanız onu girin. `http://mytestcluster.southcentral.cloudapp.azure.com:8080` bunun bir örneğidir. Artık Azure'da kümede çalıştırılan uygulamayı görüyor olmalısınız. Voting web sayfasında, oylama seçeneklerini ve bu seçeneklerden en az biri için oylama ekleyip silmeyi deneyin.

![Service Fabric Voting örneği](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Sonraki adımlar
Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Create a cluster.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [HTTPS'yi etkinleştirme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
