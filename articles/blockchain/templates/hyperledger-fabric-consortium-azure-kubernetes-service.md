---
title: Azure Kubernetes Service (AKS) üzerinde hiper muhasebe doku Consortium
description: Azure Kubernetes hizmetinde hiper muhasebe doku Consortium ağını dağıtma ve yapılandırma
ms.date: 07/07/2020
ms.topic: how-to
ms.reviewer: ravastra
ms.openlocfilehash: e1cbfa56f1e4ea9f8cbaa0ad973d06e8b8d486ca
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86085822"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) üzerinde hiper muhasebe doku Consortium

Azure Kubernetes Service (AKS) şablonunda hiper muhasebe dokusunu (HLF) Azure 'da bir hiper muhasebe doku Consortium ağını dağıtmak ve yapılandırmak için kullanabilirsiniz.

Bu makaleyi okuduktan sonra şunları yapabilir olacaksınız:

- Hiper muhasebe dokusuna ve hiper muhasebe doku blok zinciri ağının yapı taşlarını oluşturan çeşitli bileşenlere sahip çalışma hakkında bilgi edinin.
- Üretim senaryolarınız için Azure Kubernetes hizmetinde bir hiper muhasebe doku konsorsiyusunu dağıtmayı ve yapılandırmayı öğrenin.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="choose-an-azure-blockchain-solution"></a>Bir Azure blok zinciri çözümü seçin

Bir çözüm şablonu kullanmayı seçmeden önce, senaryonuzu, kullanılabilir Azure blok zinciri seçeneklerinin ortak kullanım durumları ile karşılaştırın.

Seçenek | Hizmet modeli | Yaygın kullanım durumu
-------|---------------|-----------------
Çözüm şablonları | IaaS | Çözüm şablonları, tam olarak yapılandırılmış bir blok zinciri ağ topolojisi sağlamak için kullanabileceğiniz Azure Resource Manager şablonlardır. Şablonlar, belirli bir blok zinciri ağ türü için Microsoft Azure işlem, ağ ve depolama hizmetleri dağıtır ve yapılandırır. Çözüm şablonları, bir hizmet düzeyi sözleşmesi olmadan sağlanır. Destek için [Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-blockchain-workbench.html) kullanın.
[Azure Blok Zinciri Hizmeti](../service/overview.md) | PaaS | Azure blok zinciri hizmeti önizlemesi, konsorsiyum blok zinciri ağlarının yönetimini, yönetimini ve yönetimini basitleştirir. PaaS, konsorsiyum yönetimi veya sözleşme ve işlem gizliliği gerektiren çözümler için Azure blok zinciri hizmetini kullanın.
[Azure Blockchain Workbench](../workbench/overview.md) | IaaS ve PaaS | Azure blok zinciri çalışma ekranı önizlemesi, iş süreçlerini ve verileri diğer kuruluşlarla paylaşmak üzere blok zinciri uygulamaları oluşturmanıza ve dağıtmanıza yardımcı olmak üzere tasarlanmış bir Azure hizmetleri ve özellikleri koleksiyonudur. Bir blok zinciri çözümünü veya blok zinciri uygulaması kavram kanıtı 'nı prototip için Azure blok zinciri çalışma ekranı 'nı kullanın. Azure Blockchain Workbench hizmet düzeyi anlaşması olmadan sunulur. Destek için [Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-blockchain-workbench.html) kullanın.

## <a name="hyperledger-fabric-consortium-architecture"></a>Hiper muhasebe Fabric Consortium mimarisi

Azure 'da hiper muhasebe doku ağı oluşturmak için, iş düğümleri ile sıralama hizmetini ve organizasyonu dağıtmanız gerekir. Şablon dağıtımının bir parçası olarak oluşturulan farklı temel bileşenler şunlardır:

- **Orderer düğümleri**: muhasebedeki işlem sıralamaktan sorumlu bir düğüm. Diğer düğümlerle birlikte, sıralı düğümler hiper muhasebe doku ağının sıralama hizmetini oluşturur.

- **Eş düğümleri**: Öncelikle, ağın bu temel öğeleri ile ilgili temel elemanlar ve akıllı sözleşmeleri barındıran bir düğümdür.

- **Fabrıc CA**: doku CA 'Sı, hiper muhasebe dokusu Için sertifika yetkilisdir (CA). Doku CA 'sı, sertifika yetkilisini barındıran sunucu işlemini başlatıp başlatmanıza olanak tanır. Kimlikleri ve sertifikaları yönetmenizi sağlar. Şablonun bir parçası olarak dağıtılan her bir AKS kümesi, varsayılan olarak bir doku CA Pod 'ı olacaktır.

- **Couşdb veya leveldb**: Eş düğümleri için World durum veritabanı, Leveldb veya couşdb içinde depolanabilir. LevelDB, eş düğümüne eklenmiş olan varsayılan durum veritabanıdır ve chaincode verilerini basit anahtar-değer çiftleri olarak depolar ve yalnızca anahtar, anahtar aralığı ve bileşik anahtar sorgularını destekler. Couşdb, chaincode veri değerleri JSON olarak modellendiği zaman zengin sorguları destekleyen, isteğe bağlı alternatif bir durum veritabanıdır.

Dağıtım üzerindeki şablon, aboneliğinizdeki çeşitli Azure kaynaklarını alır. Dağıtılan farklı Azure kaynakları şunlardır:

- **Aks kümesi**: müşteri tarafından sunulan giriş parametrelerine göre yapılandırılan Azure Kubernetes kümesi. AKS kümesinde, hiper muhasebe doku ağı bileşenlerini çalıştırmak için yapılandırılmış çeşitli yığınlar vardır. Oluşturulan farklı pod 'ler şunlardır:

  - **Yapı Araçları**: doku Aracı, hiper muhasebe doku bileşenlerinin yapılandırılmasından sorumludur.
  - **Orderer/peer Pod**: HLF ağının düğümleri.
  - **Proxy**: istemci uygulamalarının aks kümesiyle arabirim içinde yer aldığı bir ngnx proxy Pod 'si.
  - **Fabrıc CA**: doku CA 'sını çalıştıran Pod.
- **PostgreSQL**: Fabric CA kimliklerini sürdürmek Için PostgreSQL örneği dağıtıldı.

- **Azure Anahtar Kasası**: yapı CA kimlik bilgilerini ve müşteri tarafından sunulan kök sertifikaları kaydetmek için bir Anahtar Kasası örneği dağıtılır. Bu, şablonun mekanizması idare etmek için şablon dağıtımı yeniden denemesi durumunda kullanılır.
- **Azure yönetilen disk**: Azure yönetilen disk, muhasebe ve eş düğüm dünya durumu veritabanı için kalıcı mağazaya yöneliktir.
- **Genel IP**: küme ile arabirim oluşturma için dağıtılan aks KÜMESININ genel IP uç noktası.

## <a name="hyperledger-fabric-blockchain-network-setup"></a>Hiper muhasebe doku blok zinciri ağı kurulumu

Başlamak için birkaç sanal makine ve standart depolama hesabı dağıtımı destekleyebilen bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı oluşturabilirsiniz](https://azure.microsoft.com/free/).

Aşağıdaki adımları kullanarak hiper muhasebe doku blok zinciri ağını ayarlayın:

- [Sipariş/eş kuruluşu dağıtma](#deploy-the-ordererpeer-organization)
- [Consortium oluşturma](#build-the-consortium)

## <a name="deploy-the-ordererpeer-organization"></a>Sipariş/eş kuruluşu dağıtma

HLF ağ bileşenleri dağıtımına başlamak için [Azure Portal](https://portal.azure.com)gidin. **Azure Kubernetes hizmetinde hiper muhasebe dokusunu**aramak > **blok zinciri > kaynak oluştur** ' u seçin.

1. Şablon dağıtımını başlatmak için **Oluştur** ' u seçin. **Azure Kubernetes hizmetinde hiper defter oluşturma dokusu** görüntülenir.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-fabric-aks.png)

2. **Temel bilgiler** sayfasında proje ayrıntılarını girin.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-basics.png)

3. Şu ayrıntıları girin:
    - **Abonelik**: HLF ağ bileşenlerini dağıtmak istediğiniz abonelik adını seçin.
    - **Kaynak grubu**: yeni bir kaynak grubu oluşturun veya var olan boş bir kaynak grubunu seçin, kaynak grubu, şablonun bir parçası olarak dağıtılan tüm kaynakları tutar.
    - **Bölge**: HLF bileşenleri Için Azure Kubernetes kümesini dağıtmak istediğiniz Azure bölgesini seçin. Şablon, AKS 'nin kullanılabilir olduğu tüm bölgelerde kullanılabilir, aboneliğinizin sanal makine (VM) kota sınırına ulaşamadığı bir bölge seçtiğinizden emin olun.
    - **Kaynak öneki**: dağıtılan kaynakların adlandırılmasının ön eki. Kaynak ön eki altıdan az karakter uzunluğunda olmalı ve karakterlerin birleşimi hem sayı hem de harf içermelidir.
4. Dağıtılacak HLF ağ bileşenlerini tanımlamak için **doku ayarları** sekmesini seçin.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-settings.png)

5. Şu ayrıntıları girin:
    - **Kuruluş adı**: çeşitli veri düzlemi işlemleri için gerekli olan doku kuruluşunun adı. Kuruluş adının dağıtım başına benzersiz olması gerekir.
    - **Fabric ağ bileşeni**: ayarlamak Istediğiniz blok zinciri ağ bileşenine göre hizmet veya eş düğümleri sıralama seçeneğini belirleyin.
    - **Düğüm sayısı** -aşağıdakiler iki düğüm türüdür:
        - Hizmeti sıralama-ağ için hataya dayanıklılık sağlanacak düğüm sayısını seçin. Yalnızca 3, 5 ve 7 desteklenen orderer düğüm sayısıdır.
        - Eş düğümleri-gereksiniminize göre 1-10 düğümleri seçebilirsiniz.
    - **Eş düğüm dünya durumu veritabanı**: leveldb Ile CoucbDB arasında seçim yapın. Bu alan, Kullanıcı doku ağ bileşeni açılan kutusunda eş düğüm seçtiğinde görüntülenir.
    - **Fabric Kullanıcı adı**: doku CA kimlik doğrulaması için kullanılan Kullanıcı adını girin.
    - **Fabrıc CA parolası**: doku CA kimlik doğrulaması için parolayı girin.
    - **Parolayı onaylayın**: doku CA parolasını onaylayın.
    - **Sertifikalar**: doku CA 'sını başlatmak için kendi kök sertifikalarınızı kullanmak Istiyorsanız, doku CA 'sı için kök sertifikayı karşıya yükle seçeneğini belirleyin. Aksi takdirde, varsayılan doku CA 'sı otomatik olarak imzalanan sertifikalar oluşturur.
    - **Kök sertifika**: doku CA 'sının başlatılması gereken kök sertifikayı (ortak anahtar) karşıya yükleyin. . Pek biçimindeki sertifikalar desteklenir, sertifikaların UTC saat diliminde geçerli olması gerekir.
    - **Kök sertifika özel anahtarı**: kök sertifikanın özel anahtarını karşıya yükleyin. Hem ortak hem de özel anahtar birleştirilmiş bir. ped sertifikanız varsa, bu sertifikayı da buraya yükleyin.


6. Doku ağ bileşenlerinin oluşturulacağı temeldeki altyapı olan Azure Kubernetes kümesi yapılandırmasını tanımlamak için **aks küme ayarları** sekmesini seçin.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-aks-cluster-settings-1.png)

7. Şu ayrıntıları girin:
    - **Kubernetes küme adı**: oluşturulan aks kümesinin adı. Bu alan, belirtilen kaynak ön ekine göre önceden doldurulur, gerekirse değiştirebilirsiniz.
    - **Kubernetes sürümü**: kümede dağıtılacak Kubernetes sürümü. **Temel bilgiler** sekmesinde seçilen bölgeye bağlı olarak, kullanılabilir desteklenen sürümler değişebilir.
    - **DNS öneki**: aks kümesi için etki alanı adı SISTEMI (DNS) adı ön eki. Kümeyi oluşturduktan sonra kapsayıcıları yönetirken Kubernetes API 'sine bağlanmak için DNS kullanırsınız.
    - **Düğüm boyutu**: Kubernetes düğümünün boyutu, Azure 'DA bulunan VM stok tutma birimi (SKU 'lar) listesinden seçim yapabilirsiniz. En iyi performansı elde etmek için standart DS3 v2 önerilir.
    - **Düğüm sayısı**: kümede dağıtılacak Kubernetes düğümlerinin sayısı. Bu düğüm sayısının doku ayarlarında belirtilen HLF düğümlerinin en az birine eşit veya daha fazla tutulması önerilir.
    - **Hizmet sorumlusu ISTEMCI kimliği**: mevcut bir hizmet SORUMLUSUNUN istemci kimliğini girin veya aks kimlik doğrulaması için gerekli olan yeni bir oluştur. Bkz. [hizmet sorumlusu oluşturma](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal)adımları.
    - **Hizmet sorumlusu istemci parolası**: hizmet sorumlusu istemci kimliğinde belirtilen hizmet sorumlusunun istemci gizli anahtarını girin.
    - **İstemci parolasını onaylayın**: hizmet sorumlusu istemci gizli anahtarı 'nda belirtilen istemci gizliliğini onaylayın.
    - **Kapsayıcı Izlemeyi etkinleştir**: aks izlemesini etkinleştirmek için seçin. Bu, aks günlüklerinin belirtilen Log Analytics çalışma alanına gönderim yapmasını sağlar.
    - **Log Analytics çalışma alanı**: Log Analytics çalışma alanı, izleme etkinse oluşturulan varsayılan çalışma alanı ile doldurulur.

8. Yukarıdaki tüm ayrıntıları sağladıktan sonra **gözden geçir ve oluştur** sekmesini seçin. İnceleme ve oluşturma, verdiğiniz değerler için doğrulamayı tetikler.
9. Doğrulama başarılı olduktan sonra **Oluştur**' u seçebilirsiniz.
Dağıtım genellikle 10-12 dakika sürer, belirtilen AKS düğümlerinin boyutuna ve sayısına bağlı olarak değişebilir.
10. Başarılı dağıtımdan sonra, sağ üst köşedeki Azure bildirimleri aracılığıyla bilgilendirilirsiniz.
11. Şablon dağıtımının bir parçası olarak oluşturulan tüm kaynakları denetlemek için **kaynak grubuna git** ' i seçin. Tüm kaynak adları, **temel bilgiler** ayarında belirtilen önekle başlayacaktır.

## <a name="build-the-consortium"></a>Consortium oluşturma

Sıralama hizmetini ve eş düğümlerini dağıtan blok zinciri Konsorsiyumu gönderisini oluşturmak için aşağıdaki adımları sırayla gerçekleştirmeniz gerekir. Azure HLF betiği (azhlf), bu, Konsorsiyumu ayarlama, kanal oluşturma ve chaincode işlemleri yapmanıza yardımcı olur.

> [!NOTE]
> Betikte bir güncelleştirme var, bu güncelleştirme Azure HLF betiğine daha fazla işlevsellik sağlamaktır. Eski betiğe başvurmak istiyorsanız [buraya bakın](https://github.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/blob/master/consortiumScripts/README.md). Bu betik, Azure Kubernetes hizmet şablonu sürüm 2.0.0 ve üzeri için hiper muhasebe dokuyla uyumludur. Dağıtımın sürümünü denetlemek için [sorun giderme](#troubleshoot)bölümündeki adımları izleyin.

> [!NOTE]
> Sağlanan Azure HLF (azhlf) betiği yalnızca demo/DevTest senaryolarıyla yardımcı olur. Bu betik tarafından oluşturulan kanal ve konsorsiyumun demo/DevTest senaryosunu basitleştirecek temel HLF ilkeleri vardır. Üretim Kurulumu için, yerel HLF API 'Lerini kullanarak, kuruluşunuzun uyumluluk ihtiyaçlarına uygun şekilde Channel/Consortium HLF ilkelerini güncelleştirmenizi öneririz.


Azure HLF betiğini çalıştırmaya yönelik tüm komutlar Azure Bash komut satırı aracılığıyla yürütülebilir. Arabirim (CLı). Azure Shell web sürümünde oturum açabilirsiniz  ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) seçeneğinin sağ üst köşesinde bulunan seçeneğini Azure portal. Komut isteminde Bash CLı yazın.

Daha fazla bilgi için bkz. [Azure kabuğu](https://docs.microsoft.com/azure/cloud-shell/overview) .

![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


Aşağıdaki görüntüde, bir orderer organizasyonu ve eş kuruluş arasında konsorsiyumun derlenmesi için adım adım işlem gösterilmektedir. Bu adımları yürütmeye yönelik ayrıntılı komutlar aşağıdaki bölümlerde yakalanır.

![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/process-to-build-consortium-flow-chart.png)

İstemci uygulamasının ilk kurulumu için aşağıdaki komutları izleyin: 

1.  [İstemci uygulama dosyalarını indirin](#download-client-application-files)
2.  [Ortam değişkenlerini belirleme](#setup-environment-variables)
3.  [Kuruluş bağlantısı profilini, yönetici kullanıcıyı ve MSP 'yi içeri aktarma](#import-organization-connection-profile-admin-user-identity-and-msp)

İlk kurulumu tamamladıktan sonra, aşağıdaki işlemleri gerçekleştirmek için istemci uygulamasını kullanabilirsiniz:  

- [Kanal yönetimi komutları](#channel-management-commands)
- [Konsorsiyum yönetim komutları](#consortium-management-commands)
- [Chaincode yönetim komutları](#chaincode-management-commands)

### <a name="download-client-application-files"></a>İstemci uygulama dosyalarını indirin

İlk kurulum, istemci uygulama dosyalarını indirdir. Gerekli tüm dosya ve paketleri indirmek için aşağıdaki komutu yürütün:

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/azhlfToolSetup.sh | bash
cd azhlfTool
npm install
npm run setup

```
Bu komutlar, Azure HLF istemci uygulaması kodunu genel GitHub deposundan, ardından tüm bağımlı NPM paketlerini yükleyerek kopyalayacaktır. Komutun başarılı bir şekilde yürütülmesi tamamlandıktan sonra, geçerli dizinde bir node_modules klasörü görebilirsiniz. Gerekli tüm paketler node_modules klasörüne yüklenir.


### <a name="setup-environment-variables"></a>Ortam değişkenlerini belirleme

> [!NOTE]
> Tüm ortam değişkenleri Azure Kaynak adlandırma kuralını izler.


**Orderer kuruluş istemcisi için ortam değişkenlerinin altına ayarla**


```bash
ORDERER_ORG_SUBSCRIPTION=<ordererOrgSubscription>
ORDERER_ORG_RESOURCE_GROUP=<ordererOrgResourceGroup>
ORDERER_ORG_NAME=<ordererOrgName>
ORDERER_ADMIN_IDENTITY="admin.$ORDERER_ORG_NAME"
CHANNEL_NAME=<channelName>
```
**Eş kuruluş istemcisi için aşağıdaki ortam değişkenlerini ayarlayın**

```bash
PEER_ORG_SUBSCRIPTION=<peerOrgSubscritpion>
PEER_ORG_RESOURCE_GROUP=<peerOrgResourceGroup>
PEER_ORG_NAME=<peerOrgName>
PEER_ADMIN_IDENTITY="admin.$PEER_ORG_NAME"
CHANNEL_NAME=<channelName>
```

> [!NOTE]
> Konsorsiyumdaki eş/çalışma sayısına bağlı olarak, eş komutlarını tekrarlamanız ve ortam değişkenini uygun şekilde ayarlamanız gerekebilir.

**Azure Depolama hesabını ayarlamak için aşağıdaki ortam değişkenlerini ayarlayın**

```bash
STORAGE_SUBSCRIPTION=<subscriptionId>
STORAGE_RESOURCE_GROUP=<azureFileShareResourceGroup>
STORAGE_ACCOUNT=<azureStorageAccountName>
STORAGE_LOCATION=<azureStorageAccountLocation>
STORAGE_FILE_SHARE=<azureFileShareName>
```

Azure depolama hesabı oluşturma adımlarını izleyin. Zaten oluşturulmuş Azure depolama hesabınız varsa, bu adımları atlayın

```bash
az account set --subscription $STORAGE_SUBSCRIPTION
az group create -l $STORAGE_LOCATION -n $STORAGE_RESOURCE_GROUP
az storage account create -n $STORAGE_ACCOUNT -g  $STORAGE_RESOURCE_GROUP -l $STORAGE_LOCATION --sku Standard_LRS
```

Azure depolama hesabında bir dosya paylaşımının oluşturulması için aşağıdaki adımları izleyin. Zaten oluşturulmuş bir dosya paylaşımınız varsa, bu adımları atlayın

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
az storage share create  --account-name $STORAGE_ACCOUNT  --account-key $STORAGE_KEY  --name $STORAGE_FILE_SHARE
```

Azure dosya paylaşma bağlantı dizesi oluşturmak için aşağıdaki adımları izleyin

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
SAS_TOKEN=$(az storage account generate-sas --account-key $STORAGE_KEY --account-name $STORAGE_ACCOUNT --expiry `date -u -d "1 day" '+%Y-%m-%dT%H:%MZ'` --https-only --permissions lruwd --resource-types sco --services f | tr -d '"')
AZURE_FILE_CONNECTION_STRING=https://$STORAGE_ACCOUNT.file.core.windows.net/$STORAGE_FILE_SHARE?$SAS_TOKEN

```

### <a name="import-organization-connection-profile-admin-user-identity-and-msp"></a>Kuruluş bağlantısı profilini, yönetici kullanıcı kimliğini ve MSP 'yi içeri aktar

Kuruluşun bağlantı profilini, yönetici kullanıcı kimliğini ve MSP 'yi Azure Kubernetes kümesinden getirmek ve bu kimlikleri istemci uygulama yerel deposunda ("azhlfTool/depoları" dizininde) depolamak için komutların altına sorun.

Orderer organizasyonu için:

```bash
./azhlf adminProfile import fromAzure -o $ORDERER_ORG_NAME -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION
./azhlf connectionProfile import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME   
./azhlf msp import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME
```

Eş kuruluş için:

```bash
./azhlf adminProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf connectionProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf msp import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
```

### <a name="channel-management-commands"></a>Kanal yönetimi komutları

> [!NOTE]
> Herhangi bir kanal işlemine başlamadan önce, istemci uygulamasının ilk kurulumunun tamamlandığından emin olun.  

Aşağıdaki iki kanal yönetim komutu verilmiştir:

1. [Kanal Oluştur komutu](#create-channel-command)
2. [Çapa eşleri ayarlanıyor komutu](#setting-anchor-peers-command)


#### <a name="create-channel-command"></a>Kanal Oluştur komutu

Orderer kuruluş istemcisinden yeni bir kanal oluşturmak için komutunu verin. Bu komut, yalnızca içinde orderer organizasyonu olan bir kanal oluşturur.  

```bash
./azhlf channel create -c $CHANNEL_NAME -u $ORDERER_ADMIN_IDENTITY -o $ORDERER_ORG_NAME
```

#### <a name="setting-anchor-peers-command"></a>Çapa eşleri ayarlanıyor komutu
Eş kuruluş istemcisinden, belirtilen kanalda eş kuruluşa ait bağlantı eşleri ayarlamak için aşağıdaki komutu verin.

>[!NOTE]
> Bu komutu yürütmeden önce, eşler arası kuruluşun, konsorsiyum yönetim komutları kullanılarak kanala eklendiğinden emin olun.

```bash
./azhlf channel setAnchorPeers -c $CHANNEL_NAME -p <anchorPeersList> -o $PEER_ORG_NAME -u $PEER_ADMIN_IDENTITY
```

`<anchorPeersList>`, bir çapa eşi olarak ayarlanacak eş düğümlerinin boşlukla ayrılmış bir listesidir. Örneğin,

  - `<anchorPeersList>`Yalnızca peer1 düğümünü bağlayıcı eşi olarak ayarlamak istiyorsanız "peer1" olarak ayarlayın.
  - `<anchorPeersList>`Hem peer1 hem de peer3 düğümünü bağlayıcı eşi olarak ayarlamak istiyorsanız "peer1" "peer3" olarak ayarlayın.

### <a name="consortium-management-commands"></a>Konsorsiyum yönetim komutları

>[!NOTE]
> Herhangi bir konsorsiyum işlemini başlatmadan önce, istemci uygulamasının ilk kurulumunun tamamlandığından emin olun.  

Bir kanala ve konsorsiyuma bir eş kuruluş eklemek için aşağıdaki komutları belirtilen sırada yürütün
1.  Eş kuruluş istemcisinden, Azure depolama 'da eş kuruluş MSP 'yi karşıya yükleyin

      ```bash
      ./azhlf msp export toAzureStorage -f  $AZURE_FILE_CONNECTION_STRING -o $PEER_ORG_NAME
      ```
2.  Orderer kuruluş istemcisinde, eşdüzey kuruluş MSP 'yi Azure Storage 'dan indirin ve ardından kanal/konsorsiyuma şirket içinde eşdüzey kuruluş eklemek için komutunu verin.

      ```bash
      ./azhlf msp import fromAzureStorage -o $PEER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel join -c  $CHANNEL_NAME -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ./azhlf consortium join -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ```

3.  Orderer kuruluş istemcisinde, eş kuruluşun bu bağlantı profilini kullanarak sipariş düğümlerine bağlanabilmesi için Azure Storage 'da sipariş bağlantı profilini karşıya yükleyin

      ```bash
      ./azhlf connectionProfile  export toAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ```

4.  Eş kuruluş istemcisinden, Azure depolama 'dan orderer bağlantı profilini indirin ve ardından kanala eşdüzey düğümler eklemek için komut verin

      ```bash
      ./azhlf connectionProfile  import fromAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel joinPeerNodes -o $PEER_ORG_NAME  -u $PEER_ADMIN_IDENTITY -c $CHANNEL_NAME --ordererOrg $ORDERER_ORG_NAME
      ```

Benzer şekilde, kanala daha fazla eş kurum eklemek için, eş ortam değişkenlerini gerekli eş kuruluşa göre güncelleştirin ve 1 ile 4 arasındaki adımları yürütün.


### <a name="chaincode-management-commands"></a>Chaincode yönetim komutları

>[!NOTE]
> Herhangi bir chaincode işlemine başlamadan önce, istemci uygulamasının ilk kurulumunun tamamlandığından emin olun.  

**Aşağıdaki chaincode 'a özgü ortam değişkenlerini ayarlayın**

```bash
# peer organization name where chaincode operation is to be performed
ORGNAME=<PeerOrgName>
USER_IDENTITY="admin.$ORGNAME"  
# If you are using chaincode_example02 then set CC_NAME=“chaincode_example02”
CC_NAME=<chaincodeName>  
# If you are using chaincode_example02 then set CC_VERSION=“1” for validation
CC_VERSION=<chaincodeVersion>
# Language in which chaincode is written. Supported languages are 'node', 'golang' and 'java'  
# Default value is 'golang'  
CC_LANG=<chaincodeLanguage>  
# CC_PATH contains the path where your chaincode is place.
# If you are using chaincode_example02 to validate then CC_PATH=“/home/<username>/azhlfTool/chaincode/src/chaincode_example02/go”
CC_PATH=<chaincodePath>  
# Channel on which chaincode is to be instantiated/invoked/queried  
CHANNEL_NAME=<channelName>  
```

Aşağıdaki chaincode işlemleri gerçekleştirilebilir:  

- [Chaincode 'u yükler](#install-chaincode)  
- [Chaincode örneği oluşturma](#instantiate-chaincode)  
- [Chaincode çağırma](#invoke-chaincode)
- [Sorgu chaincode](#query-chaincode)


### <a name="install-chaincode"></a>Chaincode 'u yükler  

Eş kuruluşa chaincode 'u yüklemek için aşağıdaki komutu yürütün.  

```bash
./azhlf chaincode install -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION  

```
ORGNAME ortam değişkeninde ayarlanan eşdüzey kuruluşun tüm eşdüzey düğümlerine chaincode yükler. Kanalınıza iki veya daha fazla eş kuruluş varsa ve bunların tümüne chaincode yüklemek istiyorsanız, her eş kuruluş için bu komutu ayrı olarak yürütün.  

Adımları izleyin:  

1.  `ORGNAME` `USER_IDENTITY` PeerOrg1 ve sorun komutuna göre ayarlayın `./azhlf chaincode install` .  
2.  `ORGNAME` `USER_IDENTITY` PeerOrg2 ve sorun komutuna göre ayarlayın `./azhlf chaincode install` .  

### <a name="instantiate-chaincode"></a>Chaincode örneği oluşturma  

Eş istemci uygulamasından, kanaldaki chaincode 'u başlatmak için aşağıdaki komutu yürütün.  

```bash
./azhlf chaincode instantiate -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -v $CC_VERSION -l $CC_LANG -c $CHANNEL_NAME -f <instantiateFunc> --args <instantiateFuncArgs>  
```
Örnek oluşturma işlevi adı ve bağımsız değişken ayrılmış listesi ' ni `<instantiateFunc>` ve `<instantiateFuncArgs>` sırasıyla geçirin. Örneğin, chaincode_example02. go chaincode 'da, ' `<instantiateFunc>` `init` a ve " `<instantiateFuncArgs>` a" "2000" "b" "1000" olarak ayarlanmış chaincode 'u oluşturmak için.

> [!NOTE]
> Kanalda herhangi bir eş kuruluştan bir kez komutunu yürütün. İşlem düzenli olarak sipariş 'e gönderildikten sonra, sipariş bu işlemi kanaldaki tüm eş kuruluşlara dağıtır. Bu nedenle, chaincode, kanaldaki tüm eş kuruluşlardaki tüm eşdüzey düğümlerde oluşturulur.  


### <a name="invoke-chaincode"></a>Chaincode çağırma  

Eş kuruluş istemcisinden, chaincode işlevini çağırmak için aşağıdaki komutu yürütün:  

```bash
./azhlf chaincode invoke -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <invokeFunc> -a <invokeFuncArgs>  
```

' In ve sırasıyla bağımsız değişkenlerin çağırma işlev adını ve boşlukla ayrılmış listesini geçirin  `<invokeFunction>`    `<invokeFuncArgs>`   . Çağırma işlemini gerçekleştirmek için chaincode_example02. go chaincode örneğine devam edin,  `<invokeFunction>`    `invoke`    `<invokeFuncArgs>`   "a" "b" "10" olarak ayarlayın.  

>[!NOTE]
> Kanalda herhangi bir eş kuruluştan bir kez komutunu yürütün. İşlem düzenli olarak sipariş 'e gönderildikten sonra, sipariş bu işlemi kanaldaki tüm eş kuruluşlara dağıtır. Bu nedenle, dünyanın durumu, kanaldaki tüm eş kuruluşların tüm eşdüzey düğümlerinde güncelleştirilir.  


### <a name="query-chaincode"></a>Sorgu chaincode  

Chaincode 'u sorgulamak için aşağıdaki komutu yürütün:  

```bash
./azhlf chaincode query -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <queryFunction> -a <queryFuncArgs>  
```
Sorgu işlevi adı ve bağımsız değişken ayrılmış listesini içinde  `<queryFunction>`   ve sırasıyla geçirin  `<queryFuncArgs>`   . Yeniden chaincode_example02. Bu, "a"  `<queryFunction>`    `query` ve  `<queryArgs>` "a" olarak ayarlanan dünya durumundaki "a" değerini sorgulamak için.  

## <a name="troubleshoot"></a>Sorun giderme

**Çalışan şablon sürümünü doğrulamak için**

Şablon dağıtımınızın sürümünü bulmak için aşağıdaki komutları çalıştırın.

Şablonun dağıtıldığı kaynak grubuna göre ortam değişkenlerini aşağıda ayarlayın.

```bash

SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
AKS_CLUSTER_SUBSCRIPTION=<AKSClusterSubscriptionID>
AKS_CLUSTER_RESOURCE_GROUP=<AKSClusterResourceGroup>
AKS_CLUSTER_NAME=<AKSClusterName>
```
Şablon sürümünü yazdırmak için aşağıdaki komutu çalıştırın
```bash
SWITCH_TO_AKS_CLUSTER $AKS_CLUSTER_RESOURCE_GROUP $AKS_CLUSTER_NAME $AKS_CLUSTER_SUBSCRIPTION
kubectl describe pod fabric-tools -n tools | grep "Image:" | cut -d ":" -f 3

```

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure blok zinciri haberleri için Azure blok zinciri [blogu](https://azure.microsoft.com/blog/topics/blockchain/) ' nı ziyaret ederek, blok zinciri hizmeti tekliflerini ve Azure blok zinciri Mühendisliği ekibinin bilgilerini güncel tutun.

Ürün geri bildirimi sağlamak veya yeni özellikler istemek için, [blok zinciri Için Azure geri bildirim Forumu](https://aka.ms/blockchainuservoice)aracılığıyla bir fikir gönderin veya oylayın.

### <a name="community-support"></a>Topluluk desteği

Microsoft mühendisleri ve Azure blok zinciri topluluk uzmanlarıyla birlikte katılın.

- [Microsoft Q&soru sayfası](https://docs.microsoft.com/answers/topics/azure-blockchain-workbench.html). Blok zinciri şablonları için mühendislik desteği, dağıtım sorunlarıyla sınırlıdır.
- [Microsoft Teknoloji Topluluğu](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-blockchain-workbench)