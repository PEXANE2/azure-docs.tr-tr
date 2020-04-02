---
title: Parçalı rol tabanlı erişim Azure HDInsight küme yapılandırmaları
description: HDInsight küme yapılandırmaları için parçalı rol tabanlı erişime geçişin bir parçası olarak gereken değişiklikler hakkında bilgi edinin.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/22/2019
ms.openlocfilehash: bb78d84aa0f9a2832b6599edeac9d50e0e226437
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546349"
---
# <a name="migrate-to-granular-role-based-access-for-cluster-configurations"></a>Küme yapılandırmaları için ayrıntılı rol tabanlı erişime geçme

Hassas bilgileri elde etmek için daha ince taneli rol tabanlı erişimi desteklemek için bazı önemli değişiklikler sıyoruz. Bu değişikliklerin bir parçası olarak, [etkilenen varlıklardan/senaryolardan](#am-i-affected-by-these-changes)birini kullanıyorsanız, **3 Eylül 2019'a kadar** bazı eylemler gerekebilir.

## <a name="what-is-changing"></a>Ne değişiyor?

Daha önce, sırlar HDInsight API'si aracılığıyla, `*/read` sahibi, Katılımcısı veya Reader [RBAC rollerine](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)sahip küme kullanıcıları tarafından elde edilebilirdi, çünkü bu bilgiler izin alan herkes tarafından kullanılabilirhale sunulmuştur. Sırlar, kullanıcının rolünün izin verdiğinden daha yüksek erişim elde etmek için kullanılabilecek değerler olarak tanımlanır. Bunlar küme ağ geçidi HTTP kimlik bilgileri, depolama hesabı anahtarları ve veritabanı kimlik bilgileri gibi değerleri içerir.

3 Eylül 2019'dan itibaren, bu sırlara erişmek `Microsoft.HDInsight/clusters/configurations/action` için izin gerekir, bu da artık Reader rolüne sahip kullanıcılar tarafından erişilemeyecekleri anlamına gelir. Bu izne sahip roller Katkıda Bulunan, Sahip ve yeni HDInsight Cluster Operator rolüdür (aşağıda daha fazla bilgi).

Ayrıca, Katılımcı veya Sahibin yönetim izinleri verilmeden sırları geri alabilecek yeni bir [HDInsight Cluster Operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#hdinsight-cluster-operator) rolünü de sıyoruz. Özetlersek:

| Rol                                  | Önceden                                                                                       | İleriye Gitme       |
|---------------------------------------|--------------------------------------------------------------------------------------------------|-----------|
| Okuyucu                                | - Sırlar da dahil olmak üzere erişimi okuyun                                                                   | - Sırlar **hariç,** erişim okuyun |           |   |   |
| HDInsight Küme Operatörü<br>(Yeni Rol) | Yok                                                                                              | - Sırlar da dahil olmak üzere okuma/yazma erişimi         |   |   |
| Katılımcı                           | - Sırlar da dahil olmak üzere okuma/yazma erişimi<br>- Tüm Azure kaynakları türleri oluşturun ve yönetin.     | düzeltme sınıfı, |
| Sahip                                 | - Sırlar da dahil olmak üzere erişim okuma/yazma<br>- Tüm kaynaklara tam erişim<br>- Başkalarına temsilci erişimi | düzeltme sınıfı, |

Bir kullanıcıya küme sırlarına okuma/yazma erişimi sağlamak için HDInsight Cluster Operator rol atamasının nasıl eklendirilen hakkında bilgi için aşağıdaki bölüme bakın, [BIR kullanıcıya HDInsight Cluster Operator rol atamasını ekleyin.](#add-the-hdinsight-cluster-operator-role-assignment-to-a-user)

## <a name="am-i-affected-by-these-changes"></a>Bu değişikliklerden etkileniyor muyum?

Aşağıdaki varlıklar ve senaryolar etkilenir:

- [API](#api): Kullanıcılar `/configurations` `/configurations/{configurationName}` veya uç noktaları kullanır.
- Visual Studio Code sürüm 1.1.1 veya altında [için Azure HDInsight Araçları.](#azure-hdinsight-tools-for-visual-studio-code)
- [IntelliJ](#azure-toolkit-for-intellij) sürüm 3.20.0 veya altında için Azure Araç Seti.
- [Visual Studio için Azure Veri Gölü ve Akış Analizi Araçları,](#azure-data-lake-and-stream-analytics-tools-for-visual-studio) sürüm 2.3.9000.1'in altında.
- Eclipse sürüm 3.15.0 veya altında [için Azure Araç Seti.](#azure-toolkit-for-eclipse)
- [.NET için SDK](#sdk-for-net)
    - [1.x veya 2.x sürümleri](#versions-1x-and-2x) `GetClusterConfigurations`: `GetConnectivitySettings` `ConfigureHttpSettings`ConfigurationsOperationsExtensions sınıfından , , `EnableHttp` veya `DisableHttp` yöntemleri kullanan kullanıcılar.
    - [3.x ve üzeri versiyonlar](#versions-3x-and-up) `Update`: `EnableHttp` `DisableHttp` `ConfigurationsOperationsExtensions` Sınıftan , `Get`, veya yöntemleri kullanan kullanıcılar.
- [Python için SDK](#sdk-for-python): `get` `update` `ConfigurationsOperations` Sınıftan veya yöntemlerden kullanan kullanıcılar.
- [Java için SDK](#sdk-for-java): `update` `get` `ConfigurationsInner` Sınıftan veya yöntemlerden kullanan kullanıcılar.
- [Go için SDK](#sdk-for-go): `Get` `Update` `ConfigurationsClient` Yapıya veya yönteme göre kullanan kullanıcılar.
- [Az.HDInsight PowerShell](#azhdinsight-powershell) sürüm 2.0.0 altında.
Senaryonuz için geçiş adımlarını görmek için aşağıdaki bölümlere bakın (veya yukarıdaki bağlantıları kullanın).

### <a name="api"></a>API

Aşağıdaki API'ler değiştirilecek veya amortismana dönüştürülür:

- [**GET /configurations/{configurationName}**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (hassas bilgiler kaldırıldı)
    - Daha önce tek tek yapılandırma türleri (sırlar dahil) elde etmek için kullanılır.
    - 3 Eylül 2019'dan itibaren, bu API çağrısı artık atlanan sırlar içeren tek tek yapılandırma türlerini döndürecektir. Sırlar da dahil olmak üzere tüm yapılandırmaları elde etmek için yeni POST /configurations çağrısını kullanın. Sadece ağ geçidi ayarlarını elde etmek için yeni POST /getGatewaySettings çağrısını kullanın.
- [**GET /yapılandırmaları**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (amortismana uğradı)
    - Daha önce tüm yapılandırmaları elde etmek için kullanılan (sırlar dahil)
    - 3 Eylül 2019'dan itibaren bu API çağrısı küçümsenir ve artık desteklenmeyen. İleriye dönük tüm yapılandırmaları elde etmek için yeni POST /configurations çağrısını kullanın. Hassas parametreleri atlanan yapılandırmaları elde etmek için GET /configurations/{configurationName} çağrısını kullanın.
- [**POST /configurations/{configurationName}**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings) (amortismana alındı)
    - Ağ geçidi kimlik bilgilerini güncelleştirmek için daha önce kullanılır.
    - 3 Eylül 2019'dan itibaren bu API çağrısı küçümsenir ve artık desteklenmeyen. Bunun yerine yeni POST /updateGatewaySettings'ı kullanın.

Aşağıdaki yedek API'ler eklenmiştir:</span>

- [**POST /yapılandırmalar**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#list-configurations)
    - Sırlar da dahil olmak üzere tüm yapılandırmaları elde etmek için bu API'yi kullanın.
- [**POST /getGatewaySettings**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-gateway-settings)
    - Ağ geçidi ayarlarını elde etmek için bu API'yi kullanın.
- [**POST /updateGatewaySettings**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings)
    - Ağ geçidi ayarlarını (kullanıcı adı ve/veya parola) güncelleştirmek için bu API'yi kullanın.

### <a name="azure-hdinsight-tools-for-visual-studio-code"></a>Visual Studio Kodu için Azure HDInsight Araçları

Sürüm 1.1.1 veya altında kullanıyorsanız, kesintileri önlemek [için Visual Studio Code için Azure HDInsight Tools'un en son sürümüne](https://marketplace.visualstudio.com/items?itemName=mshdinsight.azure-hdinsight&ssr=false) güncelleyin.

### <a name="azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ

Sürüm 3.20.0 veya altında kullanıyorsanız, kesintileri önlemek [için IntelliJ eklentisi için Azure Araç Seti'nin en son sürümüne](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij) güncelleyin.

### <a name="azure-data-lake-and-stream-analytics-tools-for-visual-studio"></a>Visual Studio için Azure Veri Gölü ve Akış Analizi Araçları

Kesintileri önlemek için Visual Studio için Azure Veri [Gölü ve Akış Analizi Araçları'nın](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools&ssr=false#overview) 2.3.9000.1 veya sonraki sürümüne güncelleştirin.  Güncelleme ile ilgili yardım için, belgelerimize bakın, [Visual Studio için Veri Göl Araçlarını Güncelleyin.](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-visual-studio-tools-get-started#update-data-lake-tools-for-visual-studio)

### <a name="azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse

Sürüm 3.15.0 veya altında kullanıyorsanız, kesintileri önlemek [için Eclipse için Azure Araç Seti'nin en son sürümüne](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) güncelleyin.

### <a name="sdk-for-net"></a>.NET için SDK

#### <a name="versions-1x-and-2x"></a>Sürümler 1.x ve 2.x

.NET için HDInsight SDK sürümü [2.1.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/2.1.0) güncelleştirin. Bu değişikliklerden etkilenen bir yöntem kullanıyorsanız, en az kod değişiklikleri gerekebilir:

- `ClusterOperationsExtensions.GetClusterConfigurations`artık depolama anahtarları (çekirdek site) veya HTTP kimlik bilgileri (ağ geçidi) gibi **hassas parametreleri döndürmez.**
    - Hassas parametreler de dahil olmak üzere `ClusterOperationsExtensions.ListConfigurations` tüm yapılandırmaları almak için ileriye doğru kullanın.  'Reader' rolüne sahip kullanıcıların bu yöntemi kullanamayacağını unutmayın. Bu, kullanıcıların bir küme için hassas bilgilere erişebileceği parçalı denetime olanak tanır.
    - Sadece HTTP ağ geçidi `ClusterOperationsExtensions.GetGatewaySettings`kimlik bilgilerini almak için.

- `ClusterOperationsExtensions.GetConnectivitySettings`şimdi amortismana uğradı ve yerini `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.ConfigureHttpSettings`şimdi amortismana uğradı ve yerini `ClusterOperationsExtensions.UpdateGatewaySettings`.

- `ConfigurationsOperationsExtensions.EnableHttp`ve `DisableHttp` şimdi küçümsenen vardır. HTTP artık her zaman etkinolduğundan, bu yöntemlere artık gerek yoktur.

#### <a name="versions-3x-and-up"></a>Sürümler 3.x ve yukarısı

.NET için HDInsight SDK [sürümü 5.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/5.0.0) veya daha sonra güncelleştirin. Bu değişikliklerden etkilenen bir yöntem kullanıyorsanız, en az kod değişiklikleri gerekebilir:

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet)artık depolama anahtarları (çekirdek site) veya HTTP kimlik bilgileri (ağ geçidi) gibi **hassas parametreleri döndürmez.**
    - Hassas parametreler de dahil olmak üzere [`ConfigurationOperationsExtensions.List`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.list?view=azure-dotnet) tüm yapılandırmaları almak için ileriye doğru kullanın.'Reader' rolüne sahip kullanıcıların bu yöntemi kullanamayacağını unutmayın. Bu, kullanıcıların bir küme için hassas bilgilere erişebileceği parçalı denetime olanak tanır. 
    - Sadece HTTP ağ geçidi [`ClusterOperationsExtensions.GetGatewaySettings`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.getgatewaysettings?view=azure-dotnet)kimlik bilgilerini almak için. 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet)şimdi amortismana uğradı ve yerini [`ClusterOperationsExtensions.UpdateGatewaySettings`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.updategatewaysettings?view=azure-dotnet). 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet)ve [`DisableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) şimdi küçümsenen vardır. HTTP artık her zaman etkinolduğundan, bu yöntemlere artık gerek yoktur.

### <a name="sdk-for-python"></a>Python için SDK

Python için HDInsight SDK sürümü [1.0.0](https://pypi.org/project/azure-mgmt-hdinsight/1.0.0/) veya daha sonra güncelleştirin. Bu değişikliklerden etkilenen bir yöntem kullanıyorsanız, en az kod değişiklikleri gerekebilir:

- [`ConfigurationsOperations.get`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#get-resource-group-name--cluster-name--configuration-name--custom-headers-none--raw-false----operation-config-)artık depolama anahtarları (çekirdek site) veya HTTP kimlik bilgileri (ağ geçidi) gibi **hassas parametreleri döndürmez.**
    - Hassas parametreler de dahil olmak üzere [`ConfigurationsOperations.list`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#list-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) tüm yapılandırmaları almak için ileriye doğru kullanın.'Reader' rolüne sahip kullanıcıların bu yöntemi kullanamayacağını unutmayın. Bu, kullanıcıların bir küme için hassas bilgilere erişebileceği parçalı denetime olanak tanır. 
    - Sadece HTTP ağ geçidi [`ClusterOperations.get_gateway_settings`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clustersoperations#get-gateway-settings-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-)kimlik bilgilerini almak için.
- [`ConfigurationsOperations.update`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#update-resource-group-name--cluster-name--configuration-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-)şimdi amortismana uğradı ve yerini [`ClusterOperations.update_gateway_settings`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clustersoperations#update-gateway-settings-resource-group-name--cluster-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="sdk-for-java"></a>Java için SDK

Java için HDInsight SDK'nın [1.0.0](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight/1.0.0/jar) veya sonraki sürümüne güncelleştirin. Bu değişikliklerden etkilenen bir yöntem kullanıyorsanız, en az kod değişiklikleri gerekebilir:

- `ConfigurationsInner.get`artık depolama anahtarları (çekirdek site) veya HTTP kimlik bilgileri (ağ geçidi) gibi **hassas parametreleri döndürmez.**
- `ConfigurationsInner.update`şimdi küçümsüyor.

### <a name="sdk-for-go"></a>Gitmek İçin SDK

HDInsight SDK for Go sürümü [27.1.0](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight) veya daha sonrasına güncelleştirin. Bu değişikliklerden etkilenen bir yöntem kullanıyorsanız, en az kod değişiklikleri gerekebilir:

- [`ConfigurationsClient.get`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Get)artık depolama anahtarları (çekirdek site) veya HTTP kimlik bilgileri (ağ geçidi) gibi **hassas parametreleri döndürmez.**
    - Hassas parametreler de dahil olmak üzere [`ConfigurationsClient.list`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.List) tüm yapılandırmaları almak için ileriye doğru kullanın.'Reader' rolüne sahip kullanıcıların bu yöntemi kullanamayacağını unutmayın. Bu, kullanıcıların bir küme için hassas bilgilere erişebileceği parçalı denetime olanak tanır. 
    - Sadece HTTP ağ geçidi [`ClustersClient.get_gateway_settings`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.GetGatewaySettings)kimlik bilgilerini almak için.
- [`ConfigurationsClient.update`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Update)şimdi amortismana uğradı ve yerini [`ClustersClient.update_gateway_settings`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.UpdateGatewaySettings).

### <a name="azhdinsight-powershell"></a>Az.HDInsight PowerShell
Kesintileri önlemek için [Az PowerShell sürüm 2.0.0](https://www.powershellgallery.com/packages/Az) veya daha sonragüncelleştirin.  Bu değişikliklerden etkilenen bir yöntem kullanıyorsanız, en az kod değişikliği gerekebilir.
- `Grant-AzHDInsightHttpServicesAccess`şimdi amortismana lı ve yeni `Set-AzHDInsightGatewayCredential` cmdlet ile değiştirildi.
- `Get-AzHDInsightJobOutput`parçalı rol tabanlı depolama anahtarına erişimi destekleyecek şekilde güncelleştirildi.
    - HDInsight Küme Operatörü, Katkıda Bulunan veya Sahip rolleri olan kullanıcılar etkilenmeyecek.
    - Yalnızca Reader rolüne sahip kullanıcıların `DefaultStorageAccountKey` parametreyi açıkça belirtmeleri gerekir.
- `Revoke-AzHDInsightHttpServicesAccess`şimdi küçümsüyor. HTTP artık her zaman etkindir, bu nedenle bu cmdlet artık gerekli değildir.
 [Az'a bak. ](https://github.com/Azure/azure-powershell/blob/master/documentation/migration-guides/Az.2.0.0-migration-guide.md#azhdinsight)Daha fazla ayrıntı için HDInsight geçiş kılavuzu.

## <a name="add-the-hdinsight-cluster-operator-role-assignment-to-a-user"></a>HDInsight Cluster Operator rol atamasını kullanıcıya ekleme

[Sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rolüne sahip bir kullanıcı, [hdinsight cluster operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#hdinsight-cluster-operator) rolünü, hassas HDInsight küme yapılandırma değerlerine (küme ağ geçidi kimlik bilgileri ve depolama hesabı anahtarları gibi) okuma/yazma erişimi olmasını istediğiniz kullanıcılara atayabilir.

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Bu rol atamasını eklemenin en basit `az role assignment create` yolu Azure CLI'deki komutu kullanmaktır.

> [!NOTE]
> Bu komut, yalnızca onlar bu izinleri verebilir gibi, Sahibi rolü olan bir kullanıcı tarafından çalıştırılmalıdır. HDInsight `--assignee` Cluster Operator rolünü atamak istediğiniz kullanıcının hizmet sorumlusunun veya e-posta adresinin adıdır. Yetersiz izin hatası alırsanız, aşağıdaki SSS bölümüne bakın.

#### <a name="grant-role-at-the-resource-cluster-level"></a>Kaynak (küme) düzeyinde hibe rolü

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee <user@domain.com> --scope /subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.HDInsight/clusters/<ClusterName>
```

#### <a name="grant-role-at-the-resource-group-level"></a>Kaynak grubu düzeyinde hibe rolü

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com -g <ResourceGroupName>
```

#### <a name="grant-role-at-the-subscription-level"></a>Abonelik düzeyinde hibe rolü

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com
```

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Bir kullanıcıya HDInsight Cluster Operator rol atamasını eklemek için Azure portalını alternatif olarak kullanabilirsiniz. Belgelere bakın, [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetin - Rol ataması ekleyin.](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment)

## <a name="faq"></a>SSS

### <a name="why-am-i-seeing-a-403-forbidden-response-after-updating-my-api-requests-andor-tool"></a>API isteklerimi ve/veya aracımı güncelleştirdikten sonra neden 403 (Yasak) yanıtını görüyorum?

Küme yapılandırmaları artık parçalı rol tabanlı erişim denetiminin `Microsoft.HDInsight/clusters/configurations/*` arkasındadır ve bunlara erişmek için izin gerektirir. Bu izni almak için, yapılandırmalara erişmeye çalışan kullanıcı veya hizmet sorumlusuna HDInsight Cluster Operatörü, Katılımcı veya Sahip rolünü atayın.

### <a name="why-do-i-see-insufficient-privileges-to-complete-the-operation-when-running-the-azure-cli-command-to-assign-the-hdinsight-cluster-operator-role-to-another-user-or-service-principal"></a>HDInsight Cluster Operator rolünü başka bir kullanıcıya veya hizmet ilkesine atamak için Azure CLI komutunu çalıştırırken neden "İşlemi tamamlamak için yetersiz ayrıcalıklar" görüyorum?

Sahibi rolüne sahip olmanın yanı sıra, komutu yürüten kullanıcı veya hizmet sorumlusunun, devralan kullanıcının nesne adlarını aramak için yeterli Azure AD izinlerine sahip olması gerekir. Bu ileti, yetersiz Azure REKLAM izinlerini gösterir. Bağımsız değişkeni `-–assignee` değiştirmeyi `–assignee-object-id` deneyin ve ad (veya yönetilen kimlik durumunda asıl kimlik) yerine parametre olarak devralan nesne kimliğini sağlayın. Az rol atamasının isteğe bağlı parametreler bölümüne bakın daha fazla bilgi için [dokümantasyon oluşturun.](https://docs.microsoft.com/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create)

Bu işlem hala işe yaramazsa, doğru izinleri almak için Azure AD yöneticinize başvurun.

### <a name="what-will-happen-if-i-take-no-action"></a>Harekete geçmezsem ne olur?

3 Eylül 2019'dan `GET /configurations` `POST /configurations/gateway` itibaren aramalar artık hiçbir bilgiyi `GET /configurations/{configurationName}` döndürmez ve arama artık depolama hesabı anahtarları veya küme parolası gibi hassas parametreleri döndürmez. Aynı karşılık gelen SDK yöntemleri ve PowerShell cmdlets için de geçerlidir.

Yukarıda bahsedilen Visual Studio, VSCode, IntelliJ veya Eclipse araçlarından birinin eski bir sürümünü kullanıyorsanız, siz güncelleyene kadar artık çalışmaz.

Daha ayrıntılı bilgi için, senaryonuz için bu belgenin ilgili bölümüne bakın.
