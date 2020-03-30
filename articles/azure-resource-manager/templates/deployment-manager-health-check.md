---
title: Sistem durumu tümleştirme dağıtımı - Azure Dağıtım Yöneticisi
description: Azure Dağıtım Yöneticisi ile bir hizmetin birçok bölgeye nasıl dağıtılangerektiğini açıklar. Tüm bölgelere gönderilmeden önce dağıtımınızın kararlılığını doğrulamak için güvenli dağıtım uygulamalarını gösterir.
author: mumian
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: jgao
ms.openlocfilehash: aa99bdfcbc2f42ae81bdd55c266bcd7d87808031
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273806"
---
# <a name="introduce-health-integration-rollout-to-azure-deployment-manager-public-preview"></a>Azure Dağıtım Yöneticisi'ne sistem durumu tümleştirme ürüntanıtımı (Genel önizleme)

[Azure Dağıtım Yöneticisi,](./deployment-manager-overview.md) Azure Kaynak Yöneticisi kaynaklarının aşamalı kullanıma sunulmasını gerçekleştirmenize olanak tanır. Kaynaklar bölgeye göre sıralı bir şekilde dağıtılır. Azure Dağıtım Yöneticisi'nin tümleşik sistem durumu denetimi, kullanıma sunulmasını izleyebilir ve sorunlu kullanıma son verebilir, böylece etki boyutunu sorun giderebilir ve küçültebilirsiniz. Bu özellik, güncelleştirmelerde yaşanan gerilemelerden kaynaklanan hizmet kullanılabilirliğini azaltabilir.

## <a name="health-monitoring-providers"></a>Sağlık izleme sağlayıcıları

Microsoft, sağlık tümleştirmesini olabildiğince kolay hale getirmek için, sağlık denetimlerini dağıtımlarınıza entegre etmek için size basit bir kopya/yapıştır çözümü sağlamak için en iyi hizmet durumu izleme şirketlerinden bazılarıyla birlikte çalışmaktadır. Zaten bir sistem durumu monitörü kullanmıyorsanız, bunlar başlamak için harika çözümlerdir:

| ![azure dağıtım yöneticisi sağlık monitörü sağlayıcısı datadog](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-datadog.svg) | ![azure dağıtım yöneticisi sağlık monitörü sağlayıcısı site24x7](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-site24x7.svg) | ![azure dağıtım yöneticisi sağlık monitörü sağlayıcısı wavefront](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-wavefront.svg) |
|-----|------|------|
|Datadog, modern bulut ortamları için önde gelen izleme ve analiz platformu. [Datadog'un Azure Dağıtım Yöneticisi ile nasıl entegre olduğunu](https://www.datadoghq.com/azure-deployment-manager/)görün.|Site24x7, hepsi bir arada özel ve genel bulut hizmetleri izleme çözümü. [Site24x7'nin Azure Dağıtım Yöneticisi ile nasıl entegre olduğunu](https://www.site24x7.com/azure/adm.html)görün.| Wavefront, çoklu bulut uygulama ortamları için izleme ve analiz platformu. [Wavefront'un Azure Dağıtım Yöneticisi ile nasıl entegre olduğunu](https://go.wavefront.com/wavefront-adm/)görün.|

## <a name="how-service-health-is-determined"></a>Hizmet sağlığı nasıl belirlenir?

[Sağlık izleme sağlayıcıları,](#health-monitoring-providers) hizmetleri izlemek ve herhangi bir hizmet durumu sorunları hakkında sizi uyarmak için çeşitli mekanizmalar sunar. [Azure Monitor](../../azure-monitor/overview.md) bu tür bir teklife örnektir. Azure Monitor, belirli eşikler aşıldığında uyarı oluşturmak için kullanılabilir. Örneğin, hizmetinize yeni bir güncelleştirme dağıttığınızda bellek ve CPU kullanımınız beklenen düzeyin ötesine yükselir. Bilgilendirildiğinde, düzeltici eylemlerde olabilirsiniz.

Bu sağlık sağlayıcıları genellikle REST API'leri sunar, böylece hizmetinizin monitörlerinin durumu programlı olarak incelenebilir. REST API'leri basit bir sağlıklı/sağlıksız sinyalle (HTTP yanıt kodu tarafından belirlenir) ve/veya aldığı sinyaller hakkında ayrıntılı bilgilerle geri gelebilir.

Azure Dağıtım Yöneticisi'ndeki yeni *sağlıkDenetimi* adımı, sağlıklı bir hizmeti gösteren HTTP kodlarını bildirmenize olanak tanır veya daha karmaşık REST sonuçları için, eşleşirlerse sağlıklı bir yanıt gösteren normal ifadeler bile belirtebilirsiniz.

Azure Dağıtım Yöneticisi sistem durumu denetimleri ile kurulum akışın akışı:

1. Sağlık monitörlerinizi seçtiğiniz bir sağlık hizmeti sağlayıcısı aracılığıyla oluşturun.
1. Azure Dağıtım Yöneticisi kullanıma sunulmasının bir parçası olarak bir veya daha fazla sağlık Denetimi adımı oluşturun. HealthCheck adımlarını aşağıdaki bilgilerle doldurun:

    1. Sağlık monitörleriniz için REST API'si için URI (sağlık hizmeti sağlayıcınız tarafından tanımlandığı şekilde).
    1. Kimlik doğrulama bilgileri. Şu anda yalnızca API tuşu stili kimlik doğrulaması desteklenir.
    1. [SAĞLıKLı](https://www.wikipedia.org/wiki/List_of_HTTP_status_codes) bir yanıt tanımlayan HTTP durum kodları veya düzenli ifadeler. Yanıtın sağlıklı kabul edilemesi için ALL'ın eşleştirmesi gereken düzenli ifadeler sağlayabileceğiniveya yanıtın sağlıklı kabul edilebilmek için ANY'nin eşleşmesi gereken ifadeler sağlayabileceğini unutmayın. Her iki yöntem de desteklenir.

    Aşağıdaki Json bir örnektir:

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

1. Azure Dağıtım Yöneticisi lansmanınızda sağlıkOnay adımlarını uygun zamanda çağırın. Aşağıdaki örnekte, **stepGroup2'nin** **DeploymentSteps sonrası** bir sistem durumu denetimi adımı çağrılır.

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

Bir örnek üzerinde yürümek için [Bkz. Öğretici: Azure Dağıtım Yöneticisi'nde sistem durumu denetimini kullanın.](./deployment-manager-health-check.md)

## <a name="phases-of-a-health-check"></a>Sağlık kontrolünün aşamaları

Bu noktada Azure Dağıtım Yöneticisi, hizmetinizin durumunu nasıl sorgulayacaklarını ve bunu yapmak için kullanıma sununuzda hangi aşamalarda sorguyapacağını bilir. Ancak, Azure Dağıtım Yöneticisi, bu denetimlerin zamanlamasıhakkında derin yapılandırmaya da olanak tanır. Bir healthCheck adımı, yapılandırılan sürelere sahip 3 ardışık aşamada yürütülür: 

1. Wait

    1. Bir dağıtım işlemi tamamlandıktan sonra, VM'ler yeniden başlatılabilir, yeni verilere göre yeniden yapılandırılabilir veya hatta ilk kez başlatılabilir. Ayrıca, hizmetlerin sağlık sinyalleri yaymaya başlaması nın sağlık izleme sağlayıcısı tarafından yararlı bir şeye toplanması zaman alır. Bu çalkantılı işlem sırasında, güncelleştirme henüz sabit bir duruma ulaşmadığından hizmet durumunu denetlemek mantıklı olmayabilir. Gerçekten de, hizmet kaynakları yerleşmek gibi sağlıklı ve sağlıksız devletler arasında salınım olabilir. 
    1. Bekleme aşamasında, hizmet durumu izlenmez. Bu, dağıtılan kaynaklara sistem durumu denetimi işlemine başlamadan önce pişirme zamanı sağlamak için kullanılır. 
1. Elastik

    1. Her durumda kaynakların kararlı hale gelmeden önce ne kadar süre pişireceğini bilmek mümkün olmadığından, Elastik faz, kaynakların potansiyel olarak kararsız olduğu ve sağlıklı bir sabit bakımı için gerekli olan arasında esnek bir zaman dilimi sağlar Durum.
    1. Elastik aşama başladığında, Azure Dağıtım Yöneticisi hizmet durumu için sağlanan REST bitiş noktasını düzenli aralıklarla yoklamaya başlar. Yoklama aralığı yapılandırılabilir. 
    1. Sistem durumu monitörü, hizmetin sağlıksız olduğunu gösteren sinyallerle geri dönerse, bu sinyaller yoksayılır, Elastik aşama devam edilir ve yoklama devam edilir. 
    1. Sağlık monitörü, hizmetin sağlıklı olduğunu gösteren sinyallerle geri döner dönmez, Elastik faz sona erer ve HealthyState aşaması başlar. 
    1. Bu nedenle, Elastik faz için belirtilen süre, sağlıklı bir yanıt zorunlu kabul edilmeden önce hizmet sağlığı için yoklama geçirilebilen maksimum süredir. 
1. Sağlıklı Durum

    1. HealthyState aşamasında, hizmet sağlığı sürekli elastik faz ile aynı aralıkta yoklanır. 
    1. Hizmetin, sağlık izleme sağlayıcısından gelen sağlıklı sinyalleri belirtilen süre boyunca tutması beklenir. 
    1. Herhangi bir noktada sağlıksız bir yanıt algılanırsa, Azure Dağıtım Yöneticisi tüm dağıtımı durdurur ve sağlıksız hizmet sinyallerini taşıyan REST yanıtını döndürer.
    1. HealthyState süresi sona erdiğinde, healthCheck tamamlanır ve dağıtım bir sonraki adıma devam edilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sistem durumu izlemeyi Azure Dağıtım Yöneticisi'ne nasıl entegre edinacağınızı öğrendiniz. Dağıtım Yöneticisi ile nasıl dağıtılacağınızı öğrenmek için bir sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Dağıtım Yöneticisi'nde sistem durumu denetimini tümleştirme](./deployment-manager-tutorial-health-check.md)