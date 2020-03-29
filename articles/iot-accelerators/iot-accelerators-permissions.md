---
title: Azure IoT Çözümleri sitesini kullanma - Azure | Microsoft Dokümanlar
description: Çözüm hızlandırıcınızı dağıtmak için AzureIoTSolutions.com web sitesini nasıl kullanacağınızı açıklar.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: dobett
ms.openlocfilehash: 87f6b9cef50e4b8c388be835b2aa7bed8177ac4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447463"
---
# <a name="use-the-azureiotsolutionscom-site-to-deploy-your-solution-accelerator"></a>Çözüm hızlandırıcınızı dağıtmak için azureiotsolutions.com sitesini kullanın

Azure IoT çözüm hızlandırıcılarını Azure aboneliğinize [AzureIoTSolutions.com.'dan](https://www.azureiotsolutions.com/Accelerators)dağıtabilirsiniz. AzureIoTSolutions.com hem Microsoft açık kaynak hem de iş ortağı çözüm hızlandırıcılarına ev sahipliği yapmaktadır. Bu çözüm hızlandırıcıları [Azure IoT Başvuru Mimarisi](https://aka.ms/iotrefarchitecture)ile uyumludur. Bir çözüm hızlandırıcısını demo veya üretim ortamı olarak hızla dağıtmak için siteyi kullanabilirsiniz.

![AzureIoTSolutions.com](media/iot-accelerators-permissions/iotsolutionscom.png)

> [!TIP]
> Dağıtım işlemi üzerinde daha fazla denetime ihtiyacınız varsa, [bir çözüm hızlandırıcıdağıtmak için CLI](iot-accelerators-remote-monitoring-deploy-cli.md)kullanabilirsiniz.

Çözüm hızlandırıcılarını aşağıdaki yapılandırmalarda dağıtabilirsiniz:

* **Standart**: Üretim ortamı nı geliştirmek için genişletilmiş bir altyapı dağıtımı. Azure Kapsayıcı Hizmeti, mikro hizmetleri çeşitli Azure sanal makinelerine dağır. Kubernetes mikro hizmetleri tek tek barındıran Docker kapsayıcılarını düzenler.
* **Temel**: Bir gösteri veya dağıtımı sınamak için daha düşük maliyetli sürüm. Tüm mikro hizmetler tek bir Azure sanal makinesine dağıtılır.
* **Yerel**: Test ve geliştirme için yerel bir makine dağıtımı. Bu yaklaşım, mikro hizmetleri yerel bir Docker kapsayıcısına dağıtır ve buluttaki IoT Hub, Azure Cosmos DB ve Azure depolama hizmetlerine bağlanır.

Çözüm hızlandırıcılarının her biri, IoT Hub, Azure Akış Analizi ve Cosmos DB gibi Azure hizmetlerinin farklı bir birleşimini kullanır. Daha fazla bilgi için [AzureIoTSolutions.com'yi](https://www.azureiotsolutions.com/Accelerators) ziyaret edin ve bir çözüm hızlandırıcısı seçin.

## <a name="sign-in-at-azureiotsolutionscom"></a>azureiotsolutions.com'da oturum açın

Bir çözüm hızlandırıcısı dağıtmadan önce, azure aboneliğiyle ilişkili kimlik bilgilerini kullanarak AzureIoTSolutions.com oturum açmanız gerekir. Hesabınız birden fazla Microsoft Azure Etkin Dizin (AD) kiracısıyla ilişkiliyse, kullanılacak dizini seçmek için **Hesap seçimi açılır düşüşünü** kullanabilirsiniz.

Çözüm hızlandırıcıları dağıtma, kullanıcıları yönetme ve Azure hizmetlerini yönetme izinleriniz, seçili dizindeki rolünüze bağlıdır. Çözüm hızlandırıcılarıyla ilişkili ortak Azure AD rolleri şunlardır:

* **Genel yönetici**: Azure AD kiracı başına birçok [genel yönetici](../active-directory/users-groups-roles/directory-assign-admin-roles.md) olabilir:

  * Bir Azure AD kiracısı oluşturduğunuzda, varsayılan olarak bu kiracının genel yöneticisi olursunuz.
  * Genel yönetici temel ve standart çözüm hızlandırıcıları dağıtabilir.

* **Etki alanı kullanıcısı**: Azure AD kiracıbaşına birçok etki alanı kullanıcısı olabilir. Etki alanı kullanıcısı temel bir çözüm hızlandırıcısı dağıtabilir.

* **Konuk kullanıcı**: Azure AD kiracıbaşına çok sayıda konuk kullanıcı olabilir. Konuk kullanıcılar, Azure AD kiracısında bir çözüm hızlandırıcıdağıtamaz.

Azure AD'deki kullanıcılar ve roller hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Etkin Dizini'nde kullanıcı oluşturma](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
* [Kullanıcıları uygulamalara atama](../active-directory/manage-apps/assign-user-or-group-access-portal.md)

## <a name="choose-your-device"></a>Cihazınızı seçin

AzureIoTSolutions.com [sitesi, IoT aygıt kataloğu için Azure Sertifikalı'ya](https://catalog.azureiotsolutions.com/)bağlantılar bağlar.

Katalog, IoT çözümünüzü oluşturmaya başlamak için çözüm hızlandırıcılarınıza bağlanabileceğiniz yüzlerce sertifikalı IoT donanım aygıtını listeler.

![Cihaz kataloğu](media/iot-accelerators-permissions/devicecatalog.png)

Donanım üreticisiyseniz, Sertifikalı IoT programında Microsoft ile ortaklık yapma hakkında bilgi edinmek için **İş Ortağı Ol'u** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarından birini denemek için hızlı başlangıçları inceleyin:

* [Uzaktan izleme çözümü deneyin](quickstart-remote-monitoring-deploy.md)
* [Bağlı fabrika çözümünü deneyin](quickstart-connected-factory-deploy.md)
* [Tahmine dayalı bakım çözümünü deneyin](quickstart-predictive-maintenance-deploy.md)
* [Cihaz benzetimi çözümünü deneyin](quickstart-device-simulation-deploy.md)
