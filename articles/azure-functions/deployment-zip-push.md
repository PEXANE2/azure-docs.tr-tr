---
title: Azure Fonksiyonları için zip push dağıtımı
description: Azure İşlevlerinizi yayınlamak için Kudu dağıtım hizmetinin .zip dosya dağıtım olanaklarını kullanın.
ms.topic: conceptual
ms.date: 08/12/2018
ms.openlocfilehash: 6bda0859ca4741fe74f572b204e40130c56c46fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75769685"
---
# <a name="zip-deployment-for-azure-functions"></a>Azure İşlevleri için zip dosyasıyla dağıtım

Bu makalede, işlev uygulaması proje dosyalarınızı .zip (sıkıştırılmış) bir dosyadan Azure'a nasıl dağıtılacak açıklanmaktadır. Hem Azure CLI'yi kullanarak hem de REST API'lerini kullanarak anında laştırmayı nasıl yapacağınızı öğrenirsiniz. [Azure İşlevler Temel Araçları,](functions-run-local.md) yerel bir projeyi Azure'da yayınlarken bu dağıtım API'lerini de kullanır.

Azure İşlevler, Azure Uygulama Hizmeti tarafından sağlanan tüm sürekli dağıtım ve tümleştirme seçeneklerine sahiptir. Daha fazla bilgi için Azure [İşlevler için Sürekli dağıtım'a](functions-continuous-deployment.md)bakın.

Geliştirmeyi hızlandırmak için, işlev uygulaması proje dosyalarınızı doğrudan bir .zip dosyasından dağıtmanız daha kolay olabilir. .zip dağıtım API'si bir .zip dosyasının içeriğini `wwwroot` alır ve içeriği işlev uygulamanızın klasörüne ayıklar. Bu .zip dosyası dağıtımı, aşağıdakiler de dahil olmak üzere sürekli tümleştirme tabanlı dağıtımlara güç veren aynı Kudu hizmetini kullanır:

+ Önceki dağıtımlardan kalan dosyaların silinmesi.
+ Dağıtım komut dosyalarını çalıştırma da dahil olmak üzere dağıtım özelleştirme.
+ Dağıtım günlükleri.
+ Bir [Tüketim planı](functions-scale.md) işlevi uygulamasında eşitleme işlevi tetikler.

Daha fazla bilgi için [.zip dağıtım başvurusuna](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)bakın.

## <a name="deployment-zip-file-requirements"></a>Dağıtım .zip dosya gereksinimleri

Push dağıtım için kullandığınız .zip dosyası, işlevinizi çalıştırmak için gereken tüm dosyaları içermelidir.

>[!IMPORTANT]
> .zip dağıtımı kullandığınızda, .zip dosyasında bulunmayan varolan bir dağıtımdaki tüm dosyalar işlev uygulamanızdan silinir.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

İşlev uygulaması dizindeki tüm dosya ve `wwwroot` klasörleri içerir. .zip dosyası dağıtımı `wwwroot` dizinin içeriğini içerir, ancak dizinin kendisini içermez. C# sınıfı kitaplık projesini dağıtırken, derlenmiş kitaplık dosyalarını `bin` ve bağımlılıkları .zip paketinizde bir alt klasöre eklemeniz gerekir.

## <a name="download-your-function-app-files"></a>İşlev uygulama dosyalarınızı indirin

Yerel bir bilgisayarda geliştirme yaparken, geliştirme bilgisayarınızda işlev uygulaması proje klasörünün bir .zip dosyası oluşturmak kolaydır.

Ancak, Azure portalındaki düzenleyiciyi kullanarak işlevlerinizi oluşturmuş olabilirsiniz. Varolan bir işlev uygulama projesini şu yollardan biriyle indirebilirsiniz:

+ **Azure portalından:**

  1. [Azure portalında](https://portal.azure.com)oturum açın ve ardından işlev uygulamanıza gidin.

  2. Genel **Bakış** sekmesinde, **uygulama içeriğini indir'i**seçin. İndirme seçeneklerinizi seçin ve ardından **İndir'i**seçin.

      ![Fonksiyon uygulaması projesini indirin](./media/deployment-zip-push/download-project.png)

     İndirilen .zip dosyası ,zip push deployment kullanarak işlev uygulamanıza yeniden yayımlanmak üzere doğru formattadır. Portal indirme de doğrudan Visual Studio işlev uygulaması açmak için gerekli dosyaları ekleyebilirsiniz.

+ **REST API'lerini kullanma:**

    Dosyaları projenizden `<function_app>` indirmek için aşağıdaki dağıtım GET API'sını kullanın: 

        https://<function_app>.scm.azurewebsites.net/api/zip/site/wwwroot/

    Dahil `/site/wwwroot/` olmak üzere zip dosyası sadece işlev uygulaması proje dosyaları değil, tüm site içerir emin olun. Azure'da zaten oturum açmış değilseniz, bunu yapmanız istenir.  

GitHub deposundan bir .zip dosyası da indirebilirsiniz. Bir GitHub deposunu .zip dosyası olarak indirdiğinizde, GitHub şube için fazladan bir klasör düzeyi ekler. Bu ekstra klasör düzeyi, .zip dosyasını GitHub'dan indirirken doğrudan dağıtamadığınız anlamına gelir. İşlev uygulamanızı korumak için bir GitHub deposu kullanıyorsanız, uygulamanızı dağıtmak için [sürekli tümleştirme](functions-continuous-deployment.md) kullanmanız gerekir.  

## <a name="deploy-by-using-azure-cli"></a><a name="cli"></a>Azure CLI'yi kullanarak dağıtma

Anında niçin dağıtımını tetiklemek için Azure CLI'yi kullanabilirsiniz. [Az functionapp dağıtım kaynağı config-zip](/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config-zip) komutunu kullanarak işlev uygulamanıza bir .zip dosyası dağıtın. Bu komutu kullanmak için Azure CLI sürüm 2.0.21 veya sonraki sürümlerini kullanmanız gerekir. Hangi Azure CLI sürümünü kullandığınızı görmek `az --version` için komutu kullanın.

Aşağıdaki komutta, yer `<zip_file_path>` tutucuyu .zip dosyanızın konumuna giden yol ile değiştirin. Ayrıca, `<app_name>` işlev uygulamanızın benzersiz adı ile `<resource_group>` değiştirin ve kaynak grubunuzun adı ile değiştirin.

```azurecli-interactive
az functionapp deployment source config-zip -g <resource_group> -n \
<app_name> --src <zip_file_path>
```

Bu komut, indirilen .zip dosyasından Azure'daki işlev uygulamanıza proje dosyalarını dağıtır. Daha sonra uygulamayı yeniden başlatır. Bu işlev uygulamasının dağıtım listesini görüntülemek için REST API'lerini kullanmanız gerekir.

Yerel bilgisayarınızda Azure CLI kullanıyorsanız, `<zip_file_path>` bilgisayarınızdaki .zip dosyasına giden yoldur. Azure CLI'yi [Azure Bulut BulutU'da](../cloud-shell/overview.md)da çalıştırabilirsiniz. Cloud Shell'i kullandığınızda, öncelikle dağıtım .zip dosyanızı Bulut Shell'inizle ilişkili Azure Dosyaları hesabına yüklemeniz gerekir. Bu durumda, `<zip_file_path>` Bulut Shell hesabınızın kullandığı depolama konumudur. Daha fazla bilgi için [Azure Bulut BulutU'ndaki Devam dosyalarına](../cloud-shell/persisting-shell-storage.md)bakın.

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

## <a name="run-functions-from-the-deployment-package"></a>Dağıtım paketinden işlevleri çalıştırma

İşlevlerinizi doğrudan dağıtım paketi dosyasından çalıştırmayı da seçebilirsiniz. Bu yöntem, dosya kopyalama dağıtım adımını paketten `wwwroot` işlev uygulamanızın dizinine atlar. Bunun yerine, paket dosyası Işlevler çalışma zamanı tarafından `wwwroot` monte edilir ve dizinin içeriği salt okunur olur.  

Zip dağıtımı, işlev uygulaması ayarını `WEBSITE_RUN_FROM_PACKAGE` `1`bir değere ayarlayarak etkinleştirebileceğiniz bu özellik ile tümleşir. Daha fazla bilgi için bkz: [Dağıtım paketi dosyasından işlevlerinizi çalıştırın.](run-functions-from-deployment-package.md)

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevler için sürekli dağıtım](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
