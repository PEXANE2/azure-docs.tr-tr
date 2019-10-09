---
title: Azure yay bulutu 'nda kalıcı depolamayı kullanma | Microsoft Docs
description: Azure yay bulutu 'nda kalıcı depolamayı kullanma
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: jeconnoc
ms.openlocfilehash: 8c57698471d1363438c10e5806f9ed6f1da5333f
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72039094"
---
# <a name="how-to-use-persistent-storage-in-azure-spring-cloud"></a>Azure yay bulutu 'nda kalıcı depolamayı kullanma

Azure yay bulutu, uygulamanız için iki tür depolama alanı sağlar: kalıcı ve geçici.  Azure yay bulutu, her uygulama örneği için varsayılan olarak geçici depolama alanı sunar. Geçici depolama, 5 GB ile sınırlıdır ve varsayılan bağlama yolu `/tmp` ' dır.

> [!WARNING]
> Bir uygulama örneğini yeniden başlatmak, ilişkili geçici depolamayı kalıcı olarak siler.

Kalıcı depolama, Azure tarafından yönetilen ve uygulama kapsamı başına ayrılan bir dosya paylaşma kapsayıcısıdır. Kalıcı depolamada depolanan veriler tüm uygulamanın örnekleri arasında paylaşılır. Azure yay bulut hizmeti örneği, kalıcı disk etkin olan en fazla 10 uygulama içerebilir ve her uygulamaya 50 GB kalıcı depolama alanı verilir. Kalıcı depolama için varsayılan bağlama yolu `/persistent` ' dır.

## <a name="enable-persistent-storage-using-the-azure-portal"></a>Azure portal kullanarak kalıcı depolamayı etkinleştirme

1. Azure yay bulut hizmeti sayfasında, **uygulama panosu**' nu seçin ve ardından kalıcı depolama gerektiren uygulamayı seçin.
1. **Genel bakış** sekmesinde **kalıcı depolama** özniteliğini bulun ve **devre dışı**' yı seçin.
1. Kalıcı depolamayı etkinleştirmek için **Etkinleştir** ' e tıklayın ve değişikliği uygulamak için **Tamam** düğmesini seçin.

Kalıcı depolama etkinleştirildiğinde, **genel bakış** sayfasının **kalıcı depolama** özniteliğinde boyut ve yol gösterilir.

> [!WARNING]
> Kalıcı depolamayı *devre dışı bırakmak* , bu uygulama için depolamayı serbest bırakır.  Bu depolama hesabındaki tüm veriler kaybedilecek. 

## <a name="enable-persistent-storage-using-the-azure-cli"></a>Azure CLı kullanarak kalıcı depolamayı etkinleştirme

Kalıcı disk etkin olan bir uygulama oluşturun:
 
```azurecli
az spring-cloud app create -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
```

Mevcut bir uygulamada kalıcı depolamayı etkinleştir:

```azurecli
az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
```

Mevcut bir uygulamada kalıcı depolamayı devre dışı bırak:

> [!WARNING]
> Kalıcı depolamayı devre dışı bırakmak, bu uygulamanın depolama alanını serbest bırakır ve burada depolanan tüm verileri kalıcı olarak kaybedilir. 

```azurecli
az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage false
```

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ve hizmet kotaları](spring-cloud-quotas.md)hakkında bilgi edinin ya da [uygulamanızı el ile ölçeklendirmenin](spring-cloud-tutorial-scale-manual.md)nasıl yapılacağını öğrenin.