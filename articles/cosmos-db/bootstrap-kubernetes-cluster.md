---
title: Azure Cosmos DB ile Azure Kubernetes nasıl kullanılır?
description: Azure'da Azure Cosmos DB (önizleme) kullanan bir Kubernetes kümesini nasıl önyüklemeyle karşınıza göre başlatabileceğinizi öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: sngun
ms.openlocfilehash: 9dbbc914580d8d80a3f9b7d730574e24b44827c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70093721"
---
# <a name="how-to-use-azure-kubernetes-with-azure-cosmos-db-preview"></a>Azure Cosmos DB ile Azure Kubernetes nasıl kullanılır (önizleme)

Azure Cosmos DB'deki ETCD API'sı, Azure Kubernetes için arka uç mağazası olarak Azure Cosmos DB'yi kullanmanıza olanak tanır. Azure Cosmos DB, ana düğümün API sunucularının azure cosmos DB'yi yerel olarak yüklenmiş bir etcd'ye erişeceği gibi kullanmasına olanak tanıyan etcd wire protokolünü uygular. Azure Cosmos DB'deki etcd API şu anda önizlemede. Azure Cosmos etcd API'yi Kubernetes'in destek deposu olarak kullandığınızda aşağıdaki avantajlardan yararlanırsınız: 

* El ile yapılandırmak ve vb yönetmek gerek yok.
* Cosmos tarafından garanti edilen etcd'nin yüksek kullanılabilirliği (tek bölgede %99,99, birden fazla bölgede %99,999).
* Etcd'nin elastik ölçeklenebilirliği.
* Varsayılan olarak güvenli & kuruluş hazır.
* Sektör lideri, kapsamlı SLA'lar.

Azure Cosmos DB'deki etcd API hakkında daha fazla bilgi edinmek için [genel bakış](etcd-api-introduction.md) makalesine bakın. Bu makalede, Azure'da yerel olarak yüklenmiş ve yapılandırılmış bir etcd yerine [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) kullanan bir Kubernetes kümesini önyükleme yapmak için [Azure Kubernetes Engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md) (aks motoru) nasıl kullanılacağı gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar

1. [Azure CLI'nin](/cli/azure/install-azure-cli?view=azure-cli-latest)en son sürümünü yükleyin. Azure CLI'yi işletim sisteminize özel olarak indirebilir ve yükleyebilirsiniz.

1. Azure Kubernetes Engine'in [en son sürümünü](https://github.com/Azure/aks-engine/releases) yükleyin. Farklı işletim sistemlerine yönelik kurulum talimatları [Azure Kubernetes Engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md#install-aks-engine) sayfasında mevcuttur. Sadece bağlı **dokümanıN AKS Engine** bölümünden adımları yüklemeniz gerekir. İndirdikten sonra zip dosyasını ayıklayın.

   Azure Kubernetes Engine **(aks-engine),** Azure'daki Kubernetes kümeleri için Azure Kaynak Yöneticisi şablonları oluşturur. Aks motoruna giriş, orkestratör, özellikler ve aracılar da dahil olmak üzere istenen kümeyi açıklayan bir küme tanım dosyasıdır. Giriş dosyalarının yapısı, Azure Kubernetes Hizmeti'nin genel API'sine benzer.

1. Azure Cosmos DB'deki ETCD API şu anda önizlemede. Önizleme sürümünü kullanmak için kaydolun: https://aka.ms/cosmosetcdapi-signup. Formu gönderdikten sonra aboneliğiniz Azure Cosmos etcd API'yi kullanmak üzere beyaz listeye alınacaktır. 

## <a name="deploy-the-cluster-with-azure-cosmos-db"></a>Azure Cosmos DB ile kümeyi dağıtma

1. Komut istemi penceresi açın ve aşağıdaki komutla Azure'da oturum açın:

   ```azurecli-interactive
   az login 
   ```

1. Birden fazla aboneliğiniz varsa, Azure Cosmos DB etcd API için beyaz listeye alınmış olan aboneliği geçin. Aşağıdaki komutu kullanarak gerekli aboneliği geçebilirsiniz:

   ```azurecli-interactive
   az account set --subscription "<Name of your subscription>"
   ```
1. Ardından, Azure Kubernetes kümesinin gerektirdiği kaynakları dağıtacağınız yeni bir kaynak grubu oluşturun. Kaynak grubunu "centralus" bölgesinde oluşturduğunuzdan emin olun. Kaynak grubunun "centralus" bölgesinde olması zorunlu değildir, ancak Azure Cosmos etcd API şu anda yalnızca "centralus" bölgesinde dağıtılabilir. Bu yüzden En iyisi Kubernetes kümesinin Cosmos etcd örneği ile birlikte yer almaktır:

   ```azurecli-interactive
   az group create --name <Name> --location "centralus"
   ```

1. Ardından, aynı kaynak grubunun parçası olan kaynaklarla iletişim kurabilmek için Azure Kubernetes kümesi için bir hizmet ilkesi oluşturun. Azure CLI, PowerShell veya Azure portalLarını kullanarak bir hizmet sorumlusu oluşturabilirsiniz, bu örnekte bunu oluşturmak için CLI'yi oluşturabilirsiniz.

   ```azurecli-interactive
   az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<Your_Azure_subscription_ID>/resourceGroups/<Your_resource_group_name>"
   ```
   Bu komut, örneğin bir hizmet sorumlusunun ayrıntılarını verir:
   
   ```cmd
   Retrying role assignment creation: 1/36
   {
     "appId": "8415a4e9-4f83-46ca-a704-107457b2e3ab",
     "displayName": "azure-cli-2019-04-19-19-01-46",
     "name": "http://azure-cli-2019-04-19-19-01-46",
     "password": "102aecd3-5e37-4f3d-8738-2ac348c2e6a7",
     "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
   }
   ```
   
   Sonraki adımlarda bu parametreleri kullanacağınız için **appId** ve **parola** alanlarını not alın. 

1. Komut isteminden Azure Kubernetes Engine çalıştırılabilir klasörüne gidin. Örneğin, komut isteminizde klasöre aşağıdaki gibi gidebilirsiniz:

   ```cmd
   cd "\aks-engine-v0.36.3-windows-amd64\aks-engine-v0.36.3-windows-amd64"
   ```

1. Seçtiğiniz bir metin düzenleyicisini açın ve Azure Cosmos DB etcd API ile Azure Kubernetes kümesini dağıtan bir Kaynak Yöneticisi şablonu tanımlayın. Aşağıdaki JSON tanımını metin düzenleyicinize kopyalayın `apiModel.json`ve dosyayı aşağıdaki şekilde kaydedin:

   ```json

   {
    "apiVersion": "vlabs",
    "properties": {
        "orchestratorProfile": {
            "orchestratorType": "Kubernetes",
            "kubernetesConfig": {
                "useManagedIdentity": false
            }
        },
        "masterProfile": {
            "count": 1,
            "dnsPrefix": "",
            "vmSize": "Standard_D2_v3",
            "cosmosEtcd": true
        },
        "agentPoolProfiles": [
            {
                "name": "agent",
                "count": 1,
                "vmSize": "Standard_D2_v3",
                "availabilityProfile": "AvailabilitySet"
            }
        ],
        "linuxProfile": {
            "adminUsername": "azureuser",
            "ssh": {
                "publicKeys": [
                    {
                        "keyData": ""
                    }
                ]
            }
        }
    }
   }
   ```

   JSON/küme tanım dosyasında, dikkat edilmesi gereken anahtar parametre **"cosmosEtcd": doğru**. Bu parametre "masterProfile" özelliklerindedir ve dağıtımı düzenli etcd yerine Azure Cosmos etcd API kullanmak üzere gösterir. 

1. Azure Cosmos DB kullanan Azure Kubernetes kümesini aşağıdaki komutla dağıtın:

   ```cmd
   aks-engine deploy \
     --subscription-id <Your_Azure_subscription_ID> \
     --client-id <Service_ principal_appId> \
     --client-secret <Service_ principal_password> \
     --dns-prefix <Region_unique_dns_name > \
     --location centralus \
     --resource-group <Resource_Group_Name> \
     --api-model <Fully_qualified_path_to_the_template_file>  \
     --force-overwrite
   ```

   Azure Kubernetes Engine, Azure Kubernetes'in istenen şeklini, boyutunu ve yapılandırmasını özetleyen bir küme tanımı nı tüketir. Küme tanımı aracılığıyla etkinleştirilebilen çeşitli özellikler vardır. Bu örnekte aşağıdaki parametreleri kullanacaksınız:

   * **abonelik kimliği:** Azure Cosmos DB etcd API özellikli Azure abonelik kimliği.
   * **istemci kimliği:** Servis müdürünün appId'i. 4. `appId` adımda çıktı olarak döndürüldü.
   * **Müşteri sırrı:** Hizmet sorumlusunun parolası veya rasgele oluşturulan parola. Bu değer, adım 4'teki 'parola' parametresinde çıktı olarak döndürüldü. 
   * **dnsÖnek:** Bölgeye özgü bir DNS adı. Bu değer, ana bilgisayar adının bir parçasını oluşturur (örnek değerler- myprod1, evreleme).
   * **konum:**  Kümenin dağıtılması gereken konum, şu anda yalnızca "centralus" desteklenir.

   > [!Note]
   > Azure Cosmos etcd API şu anda yalnızca "centralus" bölgesinde dağıtılabilir. 
 
   * **api modeli:** Şablon dosyasına tam nitelikli yol.
   * **kuvvet üzerine yazma:** Bu seçenek, çıktı dizinindeki varolan dosyaların üzerine otomatik olarak yazmak için kullanılır.
 
   Aşağıdaki komut örnek bir dağıtım gösterir:

   ```cmd
   aks-engine deploy \
     --subscription-id 1234fc61-1234-1234-1234-be1234d12c1b \
     --client-id 1234a4e9-4f83-46ca-a704-107457b2e3ab \
     --client-secret 123aecd3-5e37-4f3d-8738-2ac348c2e6a7 \
     --dns-prefix aks-sg-test \
     --location centralus \
     --api-model "C:\Users\demouser\Downloads\apiModel.json"  \
     --force-overwrite
   ```

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Şablon dağıtımının tamamlanması birkaç dakika sürer. Dağıtım başarıyla tamamlandıktan sonra, komut isteminde aşağıdaki çıktıyı görürsünüz:

```cmd
WARN[0006] apimodel: missing masterProfile.dnsPrefix will use "aks-sg-test"
WARN[0006] --resource-group was not specified. Using the DNS prefix from the apimodel as the resource group name: aks-sg-test
INFO[0025] Starting ARM Deployment (aks-sg-test-546247491). This will take some time...
INFO[0587] Finished ARM Deployment (aks-sg-test-546247491). Succeeded
```

Kaynak grubu artık sanal makine, Azure Cosmos hesabı(etcd API), sanal ağ, kullanılabilirlik kümesi ve Kubernetes kümesi tarafından gerekli diğer kaynaklar gibi kaynakları içerir. 

Azure Cosmos hesabının adı, k8'lerle eklenen belirtilen DNS önekile eşleşir. Azure Cosmos hesabınız ailecek olan **EtcdDB** adlı bir veritabanı ve **EtcdData**adında bir kapsayıcı. Kapsayıcı, etcd ile ilgili tüm verileri saklar. Kapsayıcı belirli sayıda istek birimiyle birlikte verilir ve iş yükünüze göre [iş yükünü ölçeklendirebilir (artırabilirsiniz/azaltabilirsiniz).](scaling-throughput.md) 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos veritabanı, kapsayıcılar ve öğelerle](databases-containers-items.md) nasıl çalışacağınızı öğrenin
* Sağlanan iş maliyeti maliyetlerini nasıl [optimize edebilirsiniz](optimize-cost-throughput.md) öğrenin

