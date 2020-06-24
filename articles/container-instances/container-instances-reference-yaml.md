---
title: Kapsayıcı grubu için YAML başvurusu
description: Bir kapsayıcı grubunu yapılandırmak için Azure Container Instances tarafından desteklenen YAML dosyası başvurusu
ms.topic: article
ms.date: 08/12/2019
ms.openlocfilehash: be78c7d498187486a1502da17faa2b8faa5a0982
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84730535"
---
# <a name="yaml-reference-azure-container-instances"></a>YAML başvurusu: Azure Container Instances

Bu makalede bir [kapsayıcı grubunu](container-instances-container-groups.md)yapılandırmak için Azure Container Instances tarafından desteklenen YAML dosyasının sözdizimi ve özellikleri ele alınmaktadır. Azure CLı 'de [az Container Create][az-container-create] komutuna grup yapılandırmasını girmek IÇIN BIR YAML dosyası kullanın. 

YAML dosyası, tekrarlanabilir dağıtımlar için bir kapsayıcı grubu yapılandırmanın kolay bir yoludur. Bir kapsayıcı grubu oluşturmak veya güncelleştirmek için bir [Kaynak Yöneticisi şablonu](/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups) veya Azure Container Instances SDK 'ları kullanmanın kısa bir alternatifidir.

> [!NOTE]
> Bu başvuru, Azure Container Instances REST API sürümüne yönelik YAML dosyaları için geçerlidir `2018-10-01` .

## <a name="schema"></a>Şema 

YAML dosyasının şeması, anahtar özelliklerini vurgulamak için yorumlar da dahil olmak üzere aşağıdaki gibidir. Bu şemadaki özelliklerin bir açıklaması için, bkz. [özellik değerleri](#property-values) bölümü.

```yml
name: string  # Name of the container group
apiVersion: '2018-10-01'
location: string
tags: {}
identity: 
  type: string
  userAssignedIdentities: {}
properties: # Properties of container group
  containers: # Array of container instances in the group
  - name: string # Name of an instance
    properties: # Properties of an instance
      image: string # Container image used to create the instance
      command:
      - string
      ports: # External-facing ports exposed on the instance, must also be set in group ipAddress property 
      - protocol: string
        port: integer
      environmentVariables:
      - name: string
        value: string
        secureValue: string
      resources: # Resource requirements of the instance
        requests:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
        limits:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
      volumeMounts: # Array of volume mounts for the instance
      - name: string
        mountPath: string
        readOnly: boolean
      livenessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
      readinessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
  imageRegistryCredentials: # Credentials to pull a private image
  - server: string
    username: string
    password: string
  restartPolicy: string
  ipAddress: # IP address configuration of container group
    ports:
    - protocol: string
      port: integer
    type: string
    ip: string
    dnsNameLabel: string
  osType: string
  volumes: # Array of volumes available to the instances
  - name: string
    azureFile:
      shareName: string
      readOnly: boolean
      storageAccountName: string
      storageAccountKey: string
    emptyDir: {}
    secret: {}
    gitRepo:
      directory: string
      repository: string
      revision: string
  diagnostics:
    logAnalytics:
      workspaceId: string
      workspaceKey: string
      logType: string
      metadata: {}
  networkProfile: # Virtual network profile for container group
    id: string
  dnsConfig: # DNS configuration for container group
    nameServers:
    - string
    searchDomains: string
    options: string
```

## <a name="property-values"></a>Özellik değerleri

Aşağıdaki tablolarda, şemada ayarlamanız gereken değerler açıklanır.



### <a name="microsoftcontainerinstancecontainergroups-object"></a>Microsoft. Containerınstance/containerGroups nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Yes | Kapsayıcı grubunun adı. |
|  apiVersion | enum | Yes | 2018-10-01 |
|  location | dize | No | Kaynak konumu. |
|  etiketler | nesne | No | Kaynak etiketleri. |
|  identity | nesne | No | Yapılandırılmışsa kapsayıcı grubunun kimliği. - [Containergroupıdentity nesnesi](#containergroupidentity-object) |
|  properties | nesne | Yes | [ContainerGroupProperties nesnesi](#containergroupproperties-object) |




### <a name="containergroupidentity-object"></a>Containergroupıdentity nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  tür | enum | No | Kapsayıcı grubu için kullanılan kimlik türü. ' SystemAssigned, Useratandı ' türü hem örtük olarak oluşturulan bir kimliği hem de Kullanıcı tarafından atanan kimlikleri içerir. ' None ' türü, kapsayıcı grubundaki tüm kimlikleri kaldırır. -SystemAssigned, Useratandı, SystemAssigned, Useratandı, None |
|  Userassignedıdentities | nesne | No | Kapsayıcı grubuyla ilişkili kullanıcı kimliklerinin listesi. Kullanıcı kimlik sözlüğü anahtar başvuruları Azure Resource Manager kaynak kimlikleri şu biçimde olacaktır: '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName} '. |




### <a name="containergroupproperties-object"></a>ContainerGroupProperties nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  containers | array | Yes | Kapsayıcı grubu içindeki kapsayıcılar. - [Kapsayıcı nesnesi](#container-object) |
|  imageRegistryCredentials | array | No | Kapsayıcı grubunun oluşturulduğu görüntü kayıt defteri kimlik bilgileri. - [ImageRegistryCredential nesnesi](#imageregistrycredential-object) |
|  restartPolicy | enum | No | Kapsayıcı grubundaki tüm kapsayıcılar için ilkeyi yeniden başlatın. - `Always`Her zaman yeniden Başlat-hata durumunda yeniden başlatma `OnFailure` `Never` . -Always, OnFailure, hiçbir zaman |
|  Belirlenemiyor | nesne | No | Kapsayıcı grubunun IP adresi türü. - [IPAddress nesnesi](#ipaddress-object) |
|  osType | enum | Yes | Kapsayıcı grubundaki kapsayıcılar için gereken işletim sistemi türü. -Windows veya Linux |
|  volumes | array | No | Bu kapsayıcı grubundaki kapsayıcılarla bağlankalebilecek birimlerin listesi. - [Birim nesnesi](#volume-object) |
|  tanılama | nesne | No | Bir kapsayıcı grubu için tanılama bilgileri. - [ContainerGroupDiagnostics nesnesi](#containergroupdiagnostics-object) |
|  networkProfile | nesne | No | Bir kapsayıcı grubu için ağ profili bilgileri. - [ContainerGroupNetworkProfile nesnesi](#containergroupnetworkprofile-object) |
|  dnsConfig | nesne | No | Bir kapsayıcı grubu için DNS yapılandırma bilgileri. - [DnsConfiguration nesnesi](#dnsconfiguration-object) |




### <a name="container-object"></a>Kapsayıcı nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Yes | Kapsayıcı örneğinin Kullanıcı tarafından sağlanmış adı. |
|  properties | nesne | Yes | Kapsayıcı örneğinin özellikleri. - [ContainerProperties nesnesi](#containerproperties-object) |




### <a name="imageregistrycredential-object"></a>ImageRegistryCredential nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  sunucu | string | Yes | "Http" ve "https" gibi bir protokol olmadan Docker görüntü kayıt defteri sunucusu. |
|  kullanıcı adı | string | Yes | Özel kayıt defteri için Kullanıcı adı. |
|  password | dize | No | Özel kayıt defteri için parola. |




### <a name="ipaddress-object"></a>IPAddress nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ports | array | Yes | Kapsayıcı grubunda kullanıma sunulan bağlantı noktalarının listesi. - [Bağlantı noktası nesnesi](#port-object) |
|  tür | enum | Yes | IP 'nin genel İnternet 'e veya özel VNET 'e açık olup olmadığını belirtir. -Public veya Private |
|  IP | dize | No | Genel internet 'e sunulan IP. |
|  dnsNameLabel | dize | No | IP için DNS ad etiketi. |




### <a name="volume-object"></a>Birim nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Yes | Birimin adı. |
|  azureFile | nesne | No | Azure dosya birimi. - [AzureFileVolume nesnesi](#azurefilevolume-object) |
|  emptyDir | nesne | No | Boş dizin birimi. |
|  gizli dizi | nesne | No | Gizli birim. |
|  gitRepo | nesne | No | Git depo birimi. - [GitRepoVolume nesnesi](#gitrepovolume-object) |




### <a name="containergroupdiagnostics-object"></a>ContainerGroupDiagnostics nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Günlüğe kaydetme Analizi | nesne | No | Kapsayıcı grubu Günlük Analizi bilgileri. - [LogAnalytics nesnesi](#loganalytics-object) |




### <a name="containergroupnetworkprofile-object"></a>ContainerGroupNetworkProfile nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  kimlik | string | Yes | Bir ağ profili için tanımlayıcı. |




### <a name="dnsconfiguration-object"></a>DnsConfiguration nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Kullanır | array | Yes | Kapsayıcı grubu için DNS sunucuları. -dize |
|  searchDomains | dize | No | Kapsayıcı grubunda ana bilgisayar adı araması için DNS arama etki alanları. |
|  seçenekler | dize | No | Kapsayıcı grubu için DNS seçenekleri. |




### <a name="containerproperties-object"></a>ContainerProperties nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  image | string | Yes | Kapsayıcı örneğini oluşturmak için kullanılan görüntünün adı. |
|  command | array | No | Exec form 'da kapsayıcı örneği içinde yürütülecek komutlar. -dize |
|  ports | array | No | Kapsayıcı örneğindeki açığa çıkarılan bağlantı noktaları. - [ContainerPort nesnesi](#containerport-object) |
|  environmentVariables | array | No | Kapsayıcı örneğinde ayarlanacak ortam değişkenleri. - [EnvironmentVariable nesnesi](#environmentvariable-object) |
|  kaynaklar | nesne | Yes | Kapsayıcı örneğinin kaynak gereksinimleri. - [ResourceRequirements nesnesi](#resourcerequirements-object) |
|  Birimsiz bağlama | array | No | Kapsayıcı örneği için kullanılabilir birim bağlama. - [VolumeMount nesnesi](#volumemount-object) |
|  Livenessaraştırması | nesne | No | Lizleştirme araştırması. - [Containeraraştırma nesnesi](#containerprobe-object) |
|  Readinessaraştırması | nesne | No | Hazırlık araştırması. - [Containeraraştırma nesnesi](#containerprobe-object) |




### <a name="port-object"></a>Bağlantı noktası nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  protokol | enum | No | Bağlantı noktasıyla ilişkili protokol. -TCP veya UDP |
|  port | integer | Yes | Bağlantı noktası numarası. |




### <a name="azurefilevolume-object"></a>AzureFileVolume nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  madı | string | Yes | Birim olarak takılacak Azure dosya paylaşımının adı. |
|  Özelliğinin | boole | No | Bir birim olarak bağlanmış Azure dosyasının salt okunurdur olup olmadığını belirten bayrak. |
|  storageAccountName | string | Yes | Azure dosya paylaşımının bulunduğu depolama hesabının adı. |
|  storageAccountKey | dize | No | Azure dosya paylaşımınıza erişmek için kullanılan depolama hesabı erişim anahtarı. |




### <a name="gitrepovolume-object"></a>GitRepoVolume nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  dizin | dize | No | Hedef dizin adı. '.. ' İle içermemelidir veya başlamamalıdır.  '. ' Sağlanırsa, birim dizini git deposu olur.  Aksi halde, belirtilmişse, belirtilen ada sahip alt dizinde Git deposunu içerir. |
|  depo | string | Yes | Depo URL 'SI |
|  revision | dize | No | Belirtilen düzeltme için karmayı işleyin. |




### <a name="loganalytics-object"></a>LogAnalytics nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  workspaceId | string | Yes | Log Analytics için çalışma alanı kimliği |
|  workspaceKey | string | Yes | Log Analytics için çalışma alanı anahtarı |
|  Günlüğe kaydetme türü | enum | No | Kullanılacak günlük türü. -Containerınsights veya Containerınstancelogs |
|  meta veriler | nesne | No | Log Analytics için meta veriler. |




### <a name="containerport-object"></a>ContainerPort nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  protokol | enum | No | Bağlantı noktasıyla ilişkili protokol. -TCP veya UDP |
|  port | integer | Yes | Kapsayıcı grubu içinde ortaya çıkarılan bağlantı noktası numarası. |




### <a name="environmentvariable-object"></a>EnvironmentVariable nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Yes | Ortam değişkeninin adı. |
|  değer | dize | No | Ortam değişkeninin değeri. |
|  secureValue | dize | No | Güvenli ortam değişkeninin değeri. |




### <a name="resourcerequirements-object"></a>ResourceRequirements nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  istekleri | nesne | Yes | Bu kapsayıcı örneğinin kaynak istekleri. - [ResourceRequests nesnesi](#resourcerequests-object) |
|  değerleri | nesne | No | Bu kapsayıcı örneğinin kaynak sınırları. - [Resourcelimit nesnesi](#resourcelimits-object) |




### <a name="volumemount-object"></a>VolumeMount nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Yes | Birim bağlama adı. |
|  Bağlamayolu | string | Yes | Birimin bağlanması gereken kapsayıcı içindeki yol. İki nokta (:) içermemelidir. |
|  Özelliğinin | boole | No | Birim bağlamamı salt okunurdur olduğunu belirten bayrak. |




### <a name="containerprobe-object"></a>Containeraraştırma nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Exec | nesne | No | Araştırma- [Containerexec nesnesine](#containerexec-object) yürütme komutu |
|  httpGet | nesne | No | Http get ayarları araştırma- [Containerhttpget nesnesine](#containerhttpget-object) |
|  ınitialdelayseconds | integer | No | İlk gecikme süresi. |
|  periodSeconds | integer | No | Süre saniyesi. |
|  failureThreshold | integer | No | Hata eşiği. |
|  Başarılı eşik | integer | No | Başarı eşiği. |
|  timeoutSeconds | integer | No | Zaman aşımı süresi. |




### <a name="resourcerequests-object"></a>ResourceRequests nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | sayı | Yes | Bu kapsayıcı örneğinin GB cinsinden bellek isteği. |
|  'suna | sayı | Yes | Bu kapsayıcı örneğinin CPU isteği. |
|  GPU | nesne | No | Bu kapsayıcı örneğinin GPU isteği. - [GpuResource nesnesi](#gpuresource-object) |




### <a name="resourcelimits-object"></a>Resourcelimit nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | sayı | No | Bu kapsayıcı örneğinin GB cinsinden bellek sınırı. |
|  'suna | sayı | No | Bu kapsayıcı örneğinin CPU sınırı. |
|  GPU | nesne | No | Bu kapsayıcı örneğinin GPU sınırı. - [GpuResource nesnesi](#gpuresource-object) |




### <a name="containerexec-object"></a>ContainerExec nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  command | array | No | Kapsayıcı içinde yürütülecek komutlar. -dize |




### <a name="containerhttpget-object"></a>ContainerHttpGet nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  yol | dize | No | Araştırmanın yolu. |
|  port | integer | Yes | Araştırmanın bağlantı noktası numarası. |
|  düzen | enum | No | Düzen. -http veya https |




### <a name="gpuresource-object"></a>GpuResource nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  count | integer | Yes | GPU kaynağının sayısı. |
|  isteyin | enum | Yes | GPU kaynağının SKU 'SU. -K80, P100, V100 |


## <a name="next-steps"></a>Sonraki adımlar

[YAML dosyası kullanarak çok kapsayıcılı bir grup dağıtma](container-instances-multi-container-yaml.md)öğreticisine bakın.

Bir [Sanal ağda](container-instances-vnet.md) kapsayıcı grupları dağıtmak veya [harici bir birimi bağlamak](container-instances-volume-azure-files.md)için bir YAML dosyası kullanma örneklerine bakın.

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create

