---
title: Uygulamaları şablonlarla dağıtma
description: Uygulama Hizmeti uygulamalarını sağlamak ve dağıtmak için Azure Kaynak Yöneticisi şablonları oluşturma kılavuzubulun.
author: tfitzmac
ms.topic: article
ms.date: 01/03/2019
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: 0a282a412823207e5f662441158000e8c6121796
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637920"
---
# <a name="guidance-on-deploying-web-apps-by-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak web uygulamalarını dağıtma kılavuzu

Bu makalede, Azure Uygulama Hizmeti çözümlerini dağıtmak için Azure Kaynak Yöneticisi şablonları oluşturmak için öneriler sağlanmaktadır. Bu öneriler, sık karşılaşılan sorunları önlemenize yardımcı olabilir.

## <a name="define-dependencies"></a>Bağımlılıkları tanımlama

Web uygulamaları için bağımlılıkların tanımlanması, bir web uygulamasındaki kaynakların nasıl etkileşimde olduğunu niçin anlamasını gerektirir. Bağımlılıkları yanlış sırada belirtirseniz, dağıtım hatalarına neden olabilir veya dağıtımı duraklayan bir yarış koşulu oluşturabilirsiniz.

> [!WARNING]
> Şablonunuza bir MSDeploy site uzantısı eklerseniz, tüm yapılandırma kaynaklarını MSDeploy kaynağına bağımlı olarak ayarlamanız gerekir. Yapılandırma değişiklikleri, sitenin eşsenkronize olarak yeniden başlatılmasına neden olur. Yapılandırma kaynaklarını MSDeploy'a bağımlı hale getirerek, site yeniden başlatmadan önce MSDeploy'ın bitmesini sağlarsınız. Bu bağımlılıklar olmadan, site MSDeploy dağıtım işlemi sırasında yeniden başlatAbilir. Örnek bir şablon için, [Web Dağıtma Bağımlılığı olan WordPress Şablonu'na](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json)bakın.

Aşağıdaki resim, çeşitli Uygulama Hizmeti kaynakları için bağımlılık sırasını gösterir:

![Web uygulaması bağımlılıkları](media/web-sites-rm-template-guidance/web-dependencies.png)

Kaynakları aşağıdaki sırayla dağıtın:

**Katman 1**
* Uygulama Hizmeti planı.
* Veritabanları veya depolama hesapları gibi diğer ilgili kaynaklar.

**Katman 2**
* Web uygulaması-- App Service planına bağlıdır.
* Sunucu ekinini hedefleyen Azure Application Insights örneği, Uygulama Hizmeti planına bağlıdır.

**Seviye 3**
* Kaynak denetimi-- web uygulamasına bağlıdır.
* MSDeploy site uzantısı-- web uygulamasına bağlıdır.
* Web uygulamasını hedefleyen Azure Application Insights örneği web uygulamasına bağlıdır.

**Seviye 4**
* Uygulama Hizmeti sertifikası, kaynak denetimine veya varsa MSDeploy'a bağlıdır. Aksi takdirde, web uygulamasına bağlıdır.
* Yapılandırma ayarları (bağlantı dizeleri, web.config değerleri, uygulama ayarları)-- kaynak denetimine veya varsa MSDeploy'a bağlıdır. Aksi takdirde, web uygulamasına bağlıdır.

**Seviye 5**
* Ana bilgisayar adı bağlamaları-- varsa sertifikaya bağlıdır. Aksi takdirde, daha üst düzey bir kaynağa bağlıdır.
* Site uzantıları-- varsa yapılandırma ayarlarına bağlıdır. Aksi takdirde, daha üst düzey bir kaynağa bağlıdır.

Genellikle, çözümünüz bu kaynakların ve katmanların yalnızca bazılarını içerir. Eksik katmanlar için, kaynakları bir sonraki-daha yüksek katmana göre eşle.

Aşağıdaki örnek, şablonun bir bölümünü gösterir. Bağlantı dizesi yapılandırmasının değeri MSDeploy uzantısı bağlıdır. MSDeploy uzantısı web uygulamasına ve veritabanına bağlıdır. 

```json
{
    "name": "[parameters('appName')]",
    "type": "Microsoft.Web/Sites",
    ...
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'))]",
            "[concat('Microsoft.Sql/servers/', parameters('dbServerName'), '/databases/', parameters('dbName'))]",
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

Yukarıdaki kodu kullanan çalışmaya hazır bir örnek için [Şablon: Basit bir Umbraco Web Uygulaması oluşturun.](https://github.com/Azure/azure-quickstart-templates/tree/master/umbraco-webapp-simple)

## <a name="find-information-about-msdeploy-errors"></a>MSDeploy hataları hakkında bilgi edinin

Kaynak Yöneticisi şablonunuz MSDeploy kullanıyorsa, dağıtım hatası iletilerini anlamak zor olabilir. Başarısız bir dağıtımdan sonra daha fazla bilgi almak için aşağıdaki adımları deneyin:

1. Sitenin [Kudu konsoluna](https://github.com/projectkudu/kudu/wiki/Kudu-console)gidin.
2. D:\home\LogFiles\SiteExtensions\MSDeploy adresindeki klasöre göz atın.
3. appManagerStatus.xml ve appManagerLog.xml dosyalarını arayın. İlk dosya durumu kaydeder. İkinci dosya hata yla ilgili bilgileri kaydeder. Hata sizin için açık değilse, [forumda](https://docs.microsoft.com/answers/topics/azure-webapps.html)yardım isterken ekleyebilirsiniz.

## <a name="choose-a-unique-web-app-name"></a>Benzersiz bir web uygulaması adı seçin

Web uygulamanızın adı genel olarak benzersiz olmalıdır. Benzersiz olması muhtemel bir adlandırma kuralı kullanabilir veya benzersiz bir ad oluşturmaya yardımcı olmak için [uniqueString işlevini](../azure-resource-manager/templates/template-functions-string.md#uniquestring) kullanabilirsiniz.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="deploy-web-app-certificate-from-key-vault"></a>Key Vault'tan web uygulaması sertifikası dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Şablonunuz TLS/SSL bağlama için bir [Microsoft.Web/sertifika](/azure/templates/microsoft.web/certificates) kaynağı içeriyorsa ve sertifika Bir Key Vault'ta depolanmışsa, Uygulama Hizmeti kimliğinin sertifikaya erişebileceğinden emin olmalısınız.

Genel Azure'da, App Service hizmet sorumlusu **abfa0a7c-a6b6-4736-8310-5855508787cd**kimliğine sahiptir. App Service servis sorumlusu için Key Vault'a erişim sağlamak için şunları kullanın:

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
  -VaultName KEY_VAULT_NAME `
  -ServicePrincipalName abfa0a7c-a6b6-4736-8310-5855508787cd `
  -PermissionsToSecrets get `
  -PermissionsToCertificates get
```

Azure Kamu'da, Uygulama Hizmeti müdürü **6a02c803-dafd-4136-b4c3-5a6f318b4714**kimliğine sahiptir. Bu kimliği önceki örnekte kullanın.

Anahtar Kasası'nızda, sertifikayı yüklemek için **Sertifikalar** ve **Oluştur/İçe Aktar'ı** seçin.

![Sertifikayı içeri aktarma](media/web-sites-rm-template-guidance/import-certificate.png)

Şablonunuzda, sertifikanın `keyVaultSecretName`adını

Örnek bir şablon [için](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)bkz.

## <a name="next-steps"></a>Sonraki adımlar

* Web uygulamalarını şablonla dağıtma konusunda bir öğretici için, [Azure'da tahmin edilebilir şekilde Sağlama'ya bakın ve mikro hizmetleri dağıtın.](deploy-complex-application-predictably.md)
* Şablonlarda kaynak türleri için JSON sözdizimi ve özellikleri hakkında bilgi edinmek için Azure [Kaynak Yöneticisi şablon başvurusuna](/azure/templates/)bakın.
