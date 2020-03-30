---
title: Azure Hizmet Kumaşı uygulamalarını sfctl kullanarak yönetme
description: Azure Hizmet Kumaşı CLI'yi kullanarak uygulamaları Azure Hizmet Kumaşı kümesinden nasıl dağıtıp kaldırabilirsiniz öğrenin
author: Christina-Kang
ms.topic: conceptual
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 7d361d44c349bc7a6e3c041f78d00ad66182fa15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259077"
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli-sfctl"></a>Azure Hizmet Kumaşı CLI (sfctl) kullanarak Azure Hizmet Kumaşı uygulamasını yönetme

Azure Hizmet Kumaşı kümesinde çalışan uygulamaların nasıl oluşturulup silindiğini öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* Servis Kumaş CLI yükleyin. Ardından, Service Fabric kümenizi seçin. Daha fazla bilgi için Service [Fabric CLI ile başlayın.](service-fabric-cli.md)

* Kullanıma hazır bir Service Fabric uygulama paketi hazır olsun. Bir uygulamanın nasıl yazılave paketlenebildiğini öğrenmek için [Service Fabric uygulama modeli](service-fabric-application-model.md)hakkında daha fazla bilgi edinin.

## <a name="overview"></a>Genel Bakış

Yeni bir uygulama dağıtmak için şu adımları tamamlayın:

1. Hizmet Kumaşı resim mağazasına bir uygulama paketi yükleyin.
2. Bir uygulama türünü sağlama.
3. Resim deposu içeriğini silin.
4. Bir uygulama belirtin ve oluşturun.
5. Hizmetleri belirtin ve oluşturun.

Varolan bir uygulamayı kaldırmak için şu adımları tamamlayın:

1. Uygulamayı silin.
2. İlişkili uygulama türünü sağlama.

## <a name="deploy-a-new-application"></a>Yeni bir uygulama dağıtma

Yeni bir uygulama dağıtmak için aşağıdaki görevleri tamamlayın:

### <a name="upload-a-new-application-package-to-the-image-store"></a>Resim mağazasına yeni bir uygulama paketi yükleme

Bir uygulama oluşturmadan önce, uygulama paketini Service Fabric resim mağazasına yükleyin.

Örneğin, uygulama paketiniz dizindeyse, `app_package_dir` dizini yüklemek için aşağıdaki komutları kullanın:

```shell
sfctl application upload --path ~/app_package_dir
```

Büyük uygulama paketleri için, `--show-progress` yüklemenin ilerlemesini görüntüleme seçeneğini belirtebilirsiniz.

### <a name="provision-the-application-type"></a>Uygulama türünü sağlama

Yükleme tamamlandığında, uygulamayı hükmüne başvurun. Uygulamayı sağlamak için aşağıdaki komutu kullanın:

```shell
sfctl application provision --application-type-build-path app_package_dir
```

`application-type-build-path` Değer, başvuru paketinizi yüklediğiniz dizinin adıdır.

### <a name="delete-the-application-package"></a>Uygulama paketini silme

Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmanız önerilir.  Uygulama paketlerini görüntü deposundan silerse sistem kaynaklarını boşaltAr.  Kullanılmayan uygulama paketlerinin tutulması disk depolamayı tüketir ve uygulama performansı sorunlarına yol açar. 

Uygulama paketini resim deposundan silmek için aşağıdaki komutu kullanın:

```shell
sfctl store delete --content-path app_package_dir
```

`content-path`uygulamayı oluşturduğunuzda yüklediğiniz dizinin adı olmalıdır.

### <a name="create-an-application-from-an-application-type"></a>Uygulama türünden uygulama oluşturma

Uygulamayı oluşturduktan sonra, uygulamanızı adlandırmak ve oluşturmak için aşağıdaki komutu kullanın:

```shell
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`uygulama örneği için kullanmak istediğiniz addır. Daha önce sağlanan uygulama bildiriminden ek parametreler alabilirsiniz.

Uygulama adı önek `fabric:/`ile başlamalıdır.

### <a name="create-services-for-the-new-application"></a>Yeni uygulama için hizmetler oluşturma

Bir uygulama oluşturduktan sonra, uygulamadan hizmetler oluşturun. Aşağıdaki örnekte, uygulamamızdan yeni bir devletsiz hizmet oluşturuyoruz. Bir uygulamadan oluşturabileceğiniz hizmetler, önceden sağlanan uygulama paketindeki bir hizmet bildiriminde tanımlanır.

```shell
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Uygulama dağıtımını ve sistem durumunu doğrulama

Her şeyin sağlıklı olduğunu doğrulamak için aşağıdaki sistem durumu komutlarını kullanın:

```shell
sfctl application list
sfctl service list --application-id TestApp
```

Hizmetin sağlıklı olduğunu doğrulamak için, hem hizmetin hem de uygulamanın durumunu almak için benzer komutları kullanın:

```shell
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

Sağlıklı hizmet ve uygulamaların `HealthState` bir `Ok`değeri vardır.

## <a name="remove-an-existing-application"></a>Varolan bir uygulamayı kaldırma

Bir uygulamayı kaldırmak için aşağıdaki görevleri tamamlayın:

### <a name="delete-the-application"></a>Uygulamayı silme

Uygulamayı silmek için aşağıdaki komutu kullanın:

```shell
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a>Uygulama türünü sağlama

Uygulamayı sildikten sonra, artık ihtiyacınız yoksa uygulama türünü unprovision'ı geri leyebilirsiniz. Uygulama türünü sağlamak için aşağıdaki komutu kullanın:

```shell
sfctl application unprovision --application-type-name TestAppType --application-type-version 1.0
```

Tür adı ve tür sürümü, önceden sağlanan uygulama bildirimindeki ad ve sürümle eşleşmelidir.

## <a name="upgrade-application"></a>Yükseltme uygulaması

Uygulamanızı oluşturduktan sonra, uygulamanızın ikinci bir sürümünü sağlamak için aynı adım kümesini yineleyebilirsiniz. Daha sonra, Service Fabric uygulama yükseltmesi ile uygulamanın ikinci sürümünü çalıştırmaya geçebilirsiniz. Daha fazla bilgi için [Service Fabric uygulama yükseltmeleri](service-fabric-application-upgrade.md)ile ilgili belgelere bakın.

Yükseltme gerçekleştirmek için, önce olduğu gibi aynı komutları kullanarak uygulamanın bir sonraki sürümünü ilk hükmü:

```shell
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
sfctl store delete --content-path app_package_dir_2
```

Daha sonra izlenen otomatik yükseltme gerçekleştirmek için tavsiye edilir, aşağıdaki komutu çalıştırarak yükseltme başlatmak:

```shell
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Yükseltmeler, belirtilen kümeyle varolan parametreleri geçersiz kılar. Uygulama parametreleri gerekirse yükseltme komutuna bağımsız değişken olarak geçirilmelidir. Uygulama parametreleri JSON nesnesi olarak kodlanmalıdır.

Daha önce belirtilen parametreleri almak için `sfctl application info` komutu kullanabilirsiniz.

Bir uygulama yükseltmesi devam ederken, durum komutu `sfctl application upgrade-status` kullanılarak alınabilir.

Son olarak, yükseltme devam ediyorsa ve iptal edilmesi `sfctl application upgrade-rollback` gerekiyorsa, yükseltmeyi geri almak için bu yükseltmeyi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Servis Kumaş CLI temelleri](service-fabric-cli.md)
* [Linux'ta Service Fabric ile başlarken](service-fabric-get-started-linux.md)
* [Service Fabric uygulama yükseltmesi başlatma](service-fabric-application-upgrade.md)
