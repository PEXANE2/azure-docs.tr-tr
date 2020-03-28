---
title: Öğretici - Azure Kırmızı Şapka OpenShift kümesi oluşturma
description: Azure CLI'yi kullanarak Microsoft Azure Kırmızı Şapka OpenShift kümesini nasıl oluşturabilirsiniz öğrenin
author: jimzim
ms.author: jzim
ms.topic: tutorial
ms.service: container-service
ms.date: 11/04/2019
ms.openlocfilehash: 58fc695707995aafe4d804ffab8beee7c52b4320
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79455307"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Öğretici: Azure Kırmızı Şapka OpenShift kümesi oluşturma

Bu öğretici, bir dizinin birinci bölümüdür. Azure CLI'yi kullanarak Microsoft Azure Red Hat OpenShift kümesini nasıl oluşturacağınız, ölçeklendirmenizi ve kaynakları temizlemek için nasıl sildiğinizi öğreneceksiniz.

Serinin birinci bölümünde, nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturma
> * [Azure Red Hat OpenShift kümesini ölçeklendirme](tutorial-scale-cluster.md)
> * [Azure Red Hat OpenShift kümesini silme](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Ön koşullar

> [!IMPORTANT]
> Bu öğretici, Azure CLI sürümü 2.0.65 gerektirir.

Bu öğreticiye başlamadan önce:

Aşağıdakileri içeren [geliştirme ortamınızı ayarladığınızdan](howto-setup-environment.md)emin olun:
- En son CLI'nin yüklenmesi (sürüm 2.0.65 veya üzeri)
- Zaten bir kiracınız yoksa kiracı oluşturma
- Zaten bir Azure Uygulama nesneniz yoksa Azure Uygulaması nesnesi oluşturma
- Güvenlik grubu oluşturma
- Kümede oturum açabilmek için Etkin Dizin kullanıcısı oluşturma.

## <a name="step-1-sign-in-to-azure"></a>Adım 1: Azure'da oturum açın

Azure CLI'yi yerel olarak çalıştırıyorsanız, Bir Bash `az login` komut kabuğu açın ve Azure'da oturum açmak için çalıştırın.

```azurecli
az login
```

 Birden çok aboneye erişiminiz `az account set -s {subscription ID}` varsa, `{subscription ID}` kullanmak istediğiniz abonelikle değiştirin.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>Adım 2: Azure Kırmızı Şapka OpenShift kümesi oluşturma

Bash komut penceresinde aşağıdaki değişkenleri ayarlayın:

> [!IMPORTANT]
> Benzersiz küme için bir ad seçin ve tüm küçük harf veya küme oluşturma başarısız olur.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

Kümenizi oluşturmak için bir konum seçin. Azure'da OpenShift'i destekleyen azure bölgelerinin listesi için [bkz.](supported-resources.md#azure-regions) Örneğin: `LOCATION=eastus`.

```bash
LOCATION=<location>
```

Azure `APPID` AD uygulama kaydı oluşturma adım 5'te kaydettiğiniz [değere](howto-aad-app-configuration.md#create-an-azure-ad-app-registration)ayarlayın.

```bash
APPID=<app ID value>
```

[Bir Azure REKLAM güvenlik grubu oluşturma](howto-aad-app-configuration.md#create-an-azure-ad-security-group)adım 10'da kaydettiğiniz değere 'GROUPID' ayarlayın.

```bash
GROUPID=<group ID value>
```

`SECRET` [İstemci sırrı oluştur'un](howto-aad-app-configuration.md#create-a-client-secret)8.

```bash
SECRET=<secret value>
```

Yeni `TENANT` bir kiracı oluşturma adım 7'de kaydettiğiniz kiracı kimliği [değerine](howto-create-tenant.md#create-a-new-azure-ad-tenant) ayarlayın

```bash
TENANT=<tenant ID>
```

Küme için kaynak grubu oluşturun. Yukarıdaki değişkenleri tanımlamak için kullandığınız aynı Bash kabuğundan aşağıdaki komutu çalıştırın:

```azurecli
az group create --name $CLUSTER_NAME --location $LOCATION
```

### <a name="optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network"></a>İsteğe bağlı: Kümenin sanal ağını varolan bir sanal ağa bağlayın

Oluşturduğunuz kümenin sanal ağını (VNET) gözleme yoluyla varolan bir VNET'e bağlamanız gerekmiyorsa, bu adımı atlayın.

Varsayılan abonelik dışında bir ağa bakıyorsanız, bu abonelikte microsoft.containerservice sağlayıcısını da kaydetmeniz gerekir. Bunu yapmak için, bu abonelikteki aşağıdaki komutu çalıştırın. Aksi takdirde, bakmakta olduğunuz VNET aynı abonelikte bulunuyorsa, kayıt adımını atlayabilirsiniz.

`az provider register -n Microsoft.ContainerService --wait`

İlk olarak, varolan VNET'in tanımlayıcısını alın. Tanımlayıcı formda olacak: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Ağ adını veya varolan VNET'in ait olduğu kaynak grubunu bilmiyorsanız, [Sanal ağlar bıçağına](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks) gidin ve sanal ağınızı tıklayın. Sanal ağ sayfası görüntülenir ve ağın adını ve ait olduğu kaynak grubunu listeler.

Bir BASH kabuğunda aşağıdaki CLI komutunu kullanarak bir VNET_ID değişkeni tanımlayın:

```azurecli
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Örneğin, `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="optional-connect-the-cluster-to-azure-monitoring"></a>İsteğe bağlı: Kümeyi Azure İzleme'ye bağlayın

İlk olarak, **varolan** günlük analitiği çalışma alanının tanımlayıcısını alın. Tanımlayıcı formdan olacaktır:

`/subscriptions/{subscription}/resourceGroups/{resourcegroup}/providers/Microsoft.OperationalInsights/workspaces/{workspace-id}`.

Günlük analitiği çalışma alanı adını veya varolan günlük analitiği çalışma alanının ait olduğu kaynak grubunu bilmiyorsanız, [Log-Analytics Çalışma Alanına](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2Fworkspaces) gidin ve günlük analizi çalışma alanlarınıza tıklayın. Günlük analitiği çalışma alanı sayfası görüntülenir ve çalışma alanının adını ve ait olduğu kaynak grubunu listeler.

_Günlük analizi çalışma alanı oluşturmak için [bkz.](../azure-monitor/learn/quick-create-workspace-cli.md)_

Bir BASH kabuğunda aşağıdaki CLI komutunu kullanarak bir WORKSPACE_ID değişkeni tanımlayın:

```azurecli
WORKSPACE_ID=$(az monitor log-analytics workspace show -g {RESOURCE_GROUP} -n {NAME} --query id -o tsv)
```

### <a name="create-the-cluster"></a>Kümeyi oluşturma

Artık bir küme oluşturmaya hazırsınız. Aşağıda, belirtilen Azure AD kiracısında küme oluşturulur, güvenlik sorumlusu olarak kullanılacak Azure AD uygulaması nesnesini ve gizlisini ve kümeye yönetici erişimi olan üyeleri içeren güvenlik grubunu belirtin.

> [!IMPORTANT]
> Kümeoluşturmadan önce Azure AD uygulaması için uygun izinleri burada ayrıntılı olarak [belirttiğiniz](howto-aad-app-configuration.md#add-api-permissions) şekilde doğru şekilde eklediğinizden emin olun

Kümenizi sanal bir ağa **bakmıyorsanız** veya Azure İzleme **istemiyorsanız** aşağıdaki komutu kullanın:

```azurecli
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID
```

Kümenizi sanal bir ağa **bakıyorsanız,** bayrağı ekleyen aşağıdaki `--vnet-peer` komutu kullanın:

```azurecli
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID --vnet-peer $VNET_ID
```

Kümenizle Azure İzleme **istiyorsanız,** bayrağı ekleyen `--workspace-id` aşağıdaki komutu kullanın:

```azurecli
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID --workspace-id $WORKSPACE_ID
```

> [!NOTE]
> Ana bilgisayar adının kullanılamaması yla ilgili bir hata alırsanız, bunun nedeni küme adınız benzersiz olmayabilir. Yeni bir kullanıcı ve güvenlik grubu oluşturma adımını atlayarak, özgün uygulama [kaydınızı](howto-aad-app-configuration.md#create-an-azure-ad-app-registration)silmeyi ve yeni bir uygulama kaydı oluştur'da farklı bir küme adı ile adımları yeniden yapmayı deneyin.




Birkaç dakika sonra, `az openshift create` tamamlanacak.

### <a name="get-the-sign-in-url-for-your-cluster"></a>Kümeniz için oturum açma URL'sini alın

Aşağıdaki komutu çalıştırarak kümenizde oturum açabilmek için URL'yi alın:

```azurecli
az openshift show -n $CLUSTER_NAME -g $CLUSTER_NAME
```

Örneğin, `publicHostName` çıktıdakine bakın:`"publicHostname": "openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io"`

Kümenizin oturum url'sini `https://` `publicHostName` ardından değer takip edecektir.  Örneğin: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`.  Bu URI'yi, uygulama kaydı yönlendirme URI'nin bir parçası olarak bir sonraki adımda kullanacaksınız.

## <a name="step-3-update-your-app-registration-redirect-uri"></a>Adım 3: Uygulama kaydınızı yeniden yönlendirme URI'yi güncelleyin

Küme için URL'de oturum açilebildiğinize göre, uygulama kaydı yönlendirme Kullanıcı Arabirimi'ni ayarlayın:

1. Uygulama [kayıtlarını aç.](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)
2. Uygulama kayıt nesnenize tıklayın.
3. Yeniden **yönlendirme URI ekle'ye**tıklayın.
4. **Tİp'in** **Web** olduğundan emin olun ve aşağıdaki `https://<public host name>/oauth2callback/Azure%20AD`deseni kullanarak **REDIRECT URI'yi** ayarlayın: . Örneğin, `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io/oauth2callback/Azure%20AD`
5. **Kaydet'i** tıklatın

## <a name="step-4-sign-in-to-the-openshift-console"></a>Adım 4: OpenShift konsolunda oturum açın

Artık yeni kümeniz için OpenShift konsolunda oturum açmaya hazırsınız. [OpenShift Web Konsolu, OpenShift](https://docs.openshift.com/aro/architecture/infrastructure_components/web_console.html) projelerinizin içeriğini görselleştirmenizi, göz atmanızı ve yönetmenizsağlar.

Azure portalında oturum açarken normalde kullandığınız kimliği önbelleğe almamış yeni bir tarayıcı örneğine ihtiyacınız olacak.

1. *Gizli* pencere (Chrome) veya *Özel Özel* Pencere (Microsoft Edge) açın.
2. Yukarıda elde ettiğiniz oturum açma URL'sine gidin, örneğin:`https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`

[Yeni bir Azure Active Directory kullanıcısı oluştur'un](howto-aad-app-configuration.md#create-a-new-azure-active-directory-user)adım 3'ünde oluşturduğunuz kullanıcı adını kullanarak oturum açın.

**İstenen İzinler** iletişim kutusu görüntülenir. **Kuruluşunuz adına İzin'i** tıklatın ve sonra **Kabul Et'i**tıklatın.

Şimdi küme konsoluna giriş yaptınız.

![OpenShift küme konsolunun ekran görüntüsü](./media/aro-console.png)

 [Red Hat OpenShift](https://docs.openshift.com/aro/welcome/index.html) belgelerinde görüntüler oluşturmak ve oluşturmak için [OpenShift konsolunu kullanma](https://docs.openshift.com/aro/getting_started/developers_console.html) hakkında daha fazla bilgi edinin.

## <a name="step-5-install-the-openshift-cli"></a>Adım 5: OpenShift CLI'yi yükleyin

[OpenShift CLI](https://docs.openshift.com/aro/cli_reference/get_started_cli.html) (veya *OC Araçları),* OpenShift kümenizin çeşitli bileşenleriyle etkileşim kurmak için uygulamalarınızı ve alt düzey yardımcı programları yönetmek için komutlar sağlar.

OpenShift konsolunda, oturum açma adınız ile sağ üst köşedeki soru işaretini tıklatın ve **Komut Satırı Araçlarını**seçin.  Linux, MacOS veya Windows için desteklenen oc CLI'yi indirmek ve yüklemek için **En Son Sürüm** bağlantısını izleyin.

> [!NOTE]
> Sağ üst köşedeki soru işareti simgesini görmüyorsanız, sol üst köşeden *Hizmet Kataloğu* veya *Uygulama Konsolu'nu* seçin.
>
> Alternatif olarak, [oc CLI'yi](https://www.okd.io/download.html) doğrudan indirebilirsiniz.

**Komut Satırı Araçları** sayfası formun `oc login https://<your cluster name>.<azure region>.cloudapp.azure.com --token=<token value>`komutunu sağlar.  Bu komutu kopyalamak *için panoya* kopyala düğmesini tıklatın.  Terminal penceresinde, [yolunuzu](https://docs.okd.io/latest/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli) oc araçlarının yerel yüklemesini içerecek şekilde ayarlayın. Ardından kopyaladiğiniz oc CLI komutunu kullanarak kümede oturum açın.

Yukarıdaki adımları kullanarak belirteç değerini alamadıysanız, belirteç değerini `https://<your cluster name>.<azure region>.cloudapp.azure.com/oauth/token/request`şu şekilde alın: .

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesini ölçeklendirme](tutorial-scale-cluster.md)
