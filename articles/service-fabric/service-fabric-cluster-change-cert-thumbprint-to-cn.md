---
title: Bir kümeyi sertifika ortak adını kullanacak şekilde güncelleştirme
description: Bir Service Fabric kümesi sertifikasını parmak izi tabanlı bildirimlerden ortak adlara dönüştürmeyi öğrenin.
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: 224798565921593d3c91dfcc187efa71a71b1fdd
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92368069"
---
# <a name="convert-cluster-certificates-from-thumbprint-based-declarations-to-common-names"></a>Küme sertifikalarını parmak izi tabanlı bildirimlerden ortak adlara Dönüştür
Bir sertifikanın imzası (' parmak izi ' olarak da bilinir) benzersizdir ve bu, parmak izi tarafından tanımlanan bir küme sertifikası bir sertifikanın belirli bir örneğine başvurur. Bu, sırasıyla Sertifika geçişi ve yönetimini genel ve açık ve açık yapar: her değişiklik, kümenin ve temel bilgi işlem konaklarının yükseltmelerini düzenlemenizi gerektirir. Bir Service Fabric kümesinin sertifika bildirimlerini, sertifikanın konu ortak adına dayanan bildirimlere dönüştürme, belirli bir şekilde, bir sertifikayı başka bir küme yükseltmesi için önemli ölçüde basitleştirir. Bu makalede, var olan bir kümenin kapalı kalma süresi olmadan ortak ad tabanlı bildirimlere nasıl dönüştürüleceği açıklanır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="moving-to-certificate-authority-ca-signed-certificates"></a>Sertifika yetkilisi (CA) tarafından imzalanan sertifikalara geçme
Sertifikası, parmak izi tarafından belirtilen bir kümenin güvenliği, başka bir imzayla aynı imzaya sahip bir sertifikayı yasaklamadan emin olur. Bu durumda, sertifikanın bir düzeyi daha az önemlidir ve bu nedenle otomatik olarak imzalanan sertifikalar yeterlidir. Bunun aksine, ortak ad tarafından belirtilen sertifikalara sahip bir kümenin güvenliği, bu sertifikayı veren ortak anahtar altyapısı hizmetinden (PKI) akar ve kendi işletimsel güvenlik denetimli ve diğer birçok farklı şekilde sertifika uygulamaları gibi yönleri içerir. Bu nedenle, bir PKI seçimi önemlidir, intimate verenler (sertifika yetkilisi veya CA) gerekir ve otomatik olarak imzalanan sertifikalar temelde düşüktür. Ortak ad (CN) tarafından tanımlanan bir sertifika, genellikle zinciri başarıyla derleniyorsa, konunun beklenen CN öğesine sahip olması ve veren (zincirdeki anında veya daha yüksek), doğrulamayı gerçekleştiren aracı tarafından güvenilir olarak değerlendirilir. Service Fabric, ' Örtük ' veren (zincirin bir güven çıpası içinde bitmesi gerekir) veya parmak izi ("verenin sabitleme") tarafından bildirilenlerin daha fazla bilgi için lütfen bu  [makaleye](cluster-security-certificates.md#common-name-based-certificate-validation-declarations) bakın. Parmak izi tarafından ortak ada göre belirtilen otomatik olarak imzalanan bir sertifika kullanarak bir kümeyi dönüştürmek için, hedef, CA imzalı sertifikanın önce parmak izine göre kümeye tanıtılmalıdır; yalnızca, TP 'den CN 'ye dönüştürme yapılabilir.

Sınama amacıyla, kendinden imzalı bir sertifika CN tarafından bildirilebilecek ve veren kendi parmak izine sabitlenebilir; bir güvenlik açısından, bu, TP tarafından aynı sertifikayı bildirmek için neredeyse eşdeğerdir. Ancak, bu tür başarılı bir dönüştürmenin, bir CA ile imzalanmış bir sertifika ile TP 'den CN 'ye başarılı bir şekilde dönüştürülmesini garanti vermediğini unutmayın. Bu nedenle, dönüştürme işleminin uygun, CA imzalı bir sertifikayla test olması önerilir (ücretsiz seçenekler mevcuttur).

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Sertifikayı karşıya yükleyin ve ölçek kümesine yükleyin
Azure 'da, sertifika edinme ve sağlama için önerilen mekanizma Azure Key Vault hizmeti ve araç araçlarını içerir. Küme sertifikası bildirimiyle eşleşen bir sertifika, kümelerinizi kapsayan sanal makine ölçek kümelerinin her düğümüne sağlanmalıdır; daha fazla ayrıntı için lütfen [sanal makine ölçek kümelerinde gizli dizileri](../virtual-machine-scale-sets/virtual-machine-scale-sets-faq.md#how-do-i-securely-ship-a-certificate-to-the-vm) inceleyin. Kümenin sertifika bildirimlerinde değişiklik yapmadan önce, kümenin her düğüm türünün VM 'lerinde hem geçerli hem de hedef küme sertifikalarının yüklü olması önemlidir. Bir Service Fabric düğümünü sağlamak için sertifika verme işleminden gelen yolculuğun ayrıntıları [burada](cluster-security-certificate-management.md#the-journey-of-a-certificate)açıklanmıştır.

## <a name="bring-cluster-to-an-optimal-starting-state"></a>Kümeyi en iyi başlangıç durumuna getir
Parmak izi tabanlı bir sertifika bildirimini, her ikisini de ortak ad tabanlı etkiler olarak dönüştürme:

- Kümedeki her düğüm, kimlik bilgilerini diğer düğümlere nasıl bulur ve gösterir
- Her düğüm, güvenli bağlantı kurulduktan sonra karşılık gelen kimlik bilgilerini nasıl doğrular  

Devam etmeden önce [her iki yapılandırma için de sunuyu ve doğrulama kurallarını](cluster-security-certificates.md#certificate-configuration-rules) gözden geçirin. Bir parmak izi genel ad dönüştürmesi gerçekleştirirken en önemli fark, yükseltilen ve henüz yükseltilmemiş düğümlerin (yani, farklı yükseltme etki alanlarına ait düğümlerin) yükseltme sırasında her zaman karşılıklı kimlik doğrulaması gerçekleştirebilmelidir. Bunu elde etmek için önerilen yol, ilk yükseltmede parmak izine göre hedef/hedef sertifikayı bildirmektir ve sonraki bir ada yapılan ortak ada geçişi tamamlar. Küme zaten önerilen bir başlangıç durumundaysa, bu bölüm atlanabilir.

Bir dönüştürme için birden çok geçerli başlangıç durumu vardır; Bu, kümenin ortak ada Yükseltmenin başlangıcında hedef sertifikayı (parmak izi ile belirtilen) zaten kullanıyor olması. Şunları göz önünde bulundurun `GoalCert` `OldCert1` `OldCert2` :

#### <a name="valid-starting-states"></a>Geçerli başlangıç durumları
- `Thumbprint: GoalCert, ThumbprintSecondary: None`
- `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1`, `GoalCert` bu değerden sonraki bir `NotAfter` tarihe sahip `OldCert1`
- `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert`, `GoalCert` bu değerden sonraki bir `NotAfter` tarihe sahip `OldCert1`

Kümeniz yukarıda açıklanan durumlardan birinde değilse, bu durumun sonunda bu durumu elde etmek için lütfen eke bakın.

## <a name="select-the-desired-common-name-based-certificate-validation-scheme"></a>İstenen ortak ad tabanlı sertifika doğrulama şemasını seçin
Daha önce açıklandığı gibi Service Fabric, bir örtük güven çıpası ile CN tarafından veya verenin parmak izlerinin açıkça sabitlenmesi için sertifikaların bildirimini destekler. Ayrıntılar için lütfen [Bu makaleye](cluster-security-certificates.md#common-name-based-certificate-validation-declarations) bakın ve farkları ve iki mekanizma arasından seçim yapma etkilerine ilişkin iyi bir fikir sahibi olduğunuzdan emin olun. Sözdizimi, bu fark/seçim parametre değeri tarafından belirlenir `certificateIssuerThumbprintList` : boş, güvenilen kök CA 'ya (güven çıpası) bağlı olarak, bir parmak izi kümesi, küme sertifikalarının izin verilen doğrudan verenler kısıtlar.

   > [!NOTE]
   > ' Certificateıssuerparmak Izi ' alanı, konu ortak adına göre belirtilen sertifikaların beklenen doğrudan verenler belirtilmesine izin verir. Kabul edilebilir değerler bir veya daha fazla virgülle ayrılmış SHA1 parmak izdir. Bu, sertifika doğrulamasının güçlendirildiği bir sertifikadır. hiçbir veren belirtilmemişse/liste boşsa, zinciri oluşturulup doğrulanıyorsa, sertifika kimlik doğrulaması için kabul edilecektir ve doğrulayıcı tarafından güvenilen bir kökte sona erer. Bir veya daha fazla veren parmak izi belirtilirse, bir veya daha fazla verenin parmak izi, zincirden ayıklanarak, bu alanda belirtilen değerlerden biriyle eşleşir. Bu, kökün güvenilir olup olmadığına bakılmaksızın, sertifika kabul edilir. Bir PKI 'nın belirli bir konuyla sertifikaların imzalanabilmesi için farklı sertifika yetkilileri (' verenler ') kullanabileceğini ve bu konu için beklenen tüm veren parmak izlerinin belirtilmesi açısından önemli olduğunu unutmayın. Diğer bir deyişle, bir sertifikanın yenilenmesi, yenilenmekte olan sertifikayla aynı veren tarafından imzalanabilmesi garanti edilmez.
   >
   > Verenin belirtilmesi en iyi yöntem olarak kabul edilir; Bunu yoksayarak, güvenilir bir köke kadar olan sertifikalar için çalışmaya devam eder. bu davranışın sınırlamaları vardır ve yakın gelecekte kullanıma sunulacaktır. Ayrıca, PKI 'nın sertifika Ilkesi keşfedilemez, kullanılabilir ve erişilebilir değilse, Azure 'da dağıtılan ve özel bir PKI tarafından verilen ve konuya göre belirtilen x509 sertifikalarıyla güvenli hale getirilmiş kümelerin Azure Service Fabric hizmeti (kümeden hizmete iletişim için) tarafından onaylanamayabilir. 

## <a name="update-the-clusters-azure-resource-management-arm-template-and-deploy"></a>Kümenin Azure Kaynak Yönetimi (ARM) şablonunu güncelleştirme ve dağıtma
ARM şablonlarıyla Azure Service Fabric kümelerini yönetmeniz önerilir; diğer bir seçenek de JSON yapıtları kullanılarak [Azure Kaynak Gezgini (Önizleme)](https://resources.azure.com). Şu anda Azure portal bir eşdeğer deneyim bulunmamaktadır. Var olan bir kümeye karşılık gelen özgün şablon yoksa, kümeyi içeren kaynak grubuna giderek Azure portal eşdeğer bir şablon, **Otomasyon** sol menüsünde **şablonu dışarı aktar** ' ı seçerek ve ardından istediğiniz kaynakları daha sonra seçerek elde edilebilir. En azından, sanal makine ölçek kümesi ve küme kaynakları, sırasıyla verilmelidir. Oluşturulan şablon da indirilebilir. Not Bu şablon, tamamen dağıtılabilir olmadan önce değişiklikler yapılmasını gerektirebilir ve tamamen orijinal bir ile eşleşmeyebilir, küme kaynağının geçerli durumunun bir yansımasından biridir.

Gerekli değişiklikler aşağıdaki gibidir:
    - Service Fabric düğüm uzantısının tanımı güncelleştiriliyor (sanal makine kaynağı altında); küme birden çok düğüm türünü tanımlıyorsa, karşılık gelen her bir sanal makine ölçek kümesinin tanımını güncelleştirmeniz gerekir
    - küme kaynak tanımı güncelleştiriliyor

Ayrıntılı örnekler aşağıda verilmiştir.

### <a name="updating-the-virtual-machine-scale-set-resources"></a>Sanal makine ölçek kümesi kaynakları güncelleştiriliyor
Kaynak
```json
"virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            ...
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            ...
                            "certificate": {
                                "thumbprint": "[parameters('certificateThumbprint')]",
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        ...
                    }
                },
```
Amaç
```json
"virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            ...
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            ...
                            "certificate": {
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        ...
                    }
                },
```

### <a name="updating-the-cluster-resource"></a>Küme kaynağı güncelleştiriliyor
**Microsoft. ServiceFabric/kümeler** kaynağında, **Commonnames** ayarıyla bir **certificatecommonnames** özelliği ekleyin ve **sertifika** özelliğini tamamen kaldırın (tüm ayarları):

Kaynak
```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            ...
        ],
        "properties": {
            "addonFeatures": [
                ...
            ],
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
```
Amaç
```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            ...
        ],
        "properties": {
            "addonFeatures": [
                ...
            ],
            "certificateCommonNames": {
                "commonNames": [
                    {
                        "certificateCommonName": "[parameters('certificateCommonName')]",
                        "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprintList')]"
                    }
                ],
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
```

Daha fazla bilgi için bkz [. parmak izi yerine sertifika ortak adı kullanan Service Fabric kümesi dağıtma.](./service-fabric-create-cluster-using-cert-cn.md)

## <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Değişiklikleri yaptıktan sonra güncelleştirilmiş şablonu yeniden dağıtın.

```powershell
$groupname = "sfclustertutorialgroup"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="appendix-achieve-a-valid-starting-state-for-converting-a-cluster-to-cn-based-certificate-declarations"></a>Ek: kümeyi CN tabanlı sertifika bildirimlerine dönüştürmek için geçerli bir başlangıç durumu elde edin

| Başlangıç durumu | Yükseltme 1 | Yükseltme 2 |
| :--- | :--- | :--- |
| `Thumbprint: OldCert1, ThumbprintSecondary: None` ve `GoalCert` sonraki bir `NotAfter` tarihe sahiptir `OldCert1` | `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert` | - |
| `Thumbprint: OldCert1, ThumbprintSecondary: None` ve `OldCert1` sonraki bir `NotAfter` tarihe sahiptir `GoalCert` | `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1` | `Thumbprint: GoalCert, ThumbprintSecondary: None` |
| `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert`, burada `OldCert1` şundan sonraki bir `NotAfter` Tarih vardır `GoalCert` | Sürümüne yükselt `Thumbprint: GoalCert, ThumbprintSecondary: None` | - |
| `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1`, burada `OldCert1` şundan sonraki bir `NotAfter` Tarih vardır `GoalCert` | Sürümüne yükselt `Thumbprint: GoalCert, ThumbprintSecondary: None` | - |
| `Thumbprint: OldCert1, ThumbprintSecondary: OldCert2` | `OldCert1` `OldCert2` Durumuna ulaşmak için veya birini kaldırın`Thumbprint: OldCertx, ThumbprintSecondary: None` | Yeni başlangıç durumundan devam et |

Bu yükseltmelerden herhangi birini nasıl gerçekleştireceğinizi gösteren yönergeler için [Bu belgeye](service-fabric-cluster-security-update-certs-azure.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar
* [Küme güvenliği](service-fabric-cluster-security.md)hakkında bilgi edinin.
* [Bir küme sertifikasını ortak ad ile başa](service-fabric-cluster-rollover-cert-cn.md) alma hakkında bilgi edinin
* [Touchless oto geçişi için bir kümeyi yapılandırmayı](cluster-security-certificate-management.md) öğrenin

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
