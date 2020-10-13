---
title: Azure Stack Edge Pro cihaz hatasından kurtarma
description: Azure Stack Edge Pro başarısız bir cihazdan nasıl kurtarılacağı açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 10/06/2020
ms.author: alkohli
ms.openlocfilehash: ef99ff874376e0d76e45e4b0b2c36a78d938130b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91844013"
---
# <a name="recover-from-a-failed-azure-stack-edge-pro-gpu-device"></a>Başarısız Azure Stack Edge Pro GPU cihazından kurtarma 

Bu makalede Azure Stack Edge Pro GPU cihazınızdan toleransız olmayan bir hatadan nasıl kurtarılacağı açıklanır. Azure Stack Edge Pro GPU cihazındaki toleransız bir hata, cihazın değiştirilmesini gerektirir.

## <a name="before-you-begin"></a>Başlamadan önce

Bilgisayarınızda yüklü olduğundan emin olun:

- Cihaz hatası ile ilgili olarak Microsoft Desteği ile iletişim kurar ve cihaz değişikliği önerilir. 
- Cihaz yapılandırmanızı [bir cihaz hatası Için hazırlama](azure-stack-edge-gpu-prepare-device-failure.md)bölümünde anlatıldığı şekilde yedekledi.


## <a name="configure-replacement-device"></a>Değiştirme cihazını yapılandırma

Cihazınız toleransız bir hata ile karşılaştığında, bir değiştirme cihazı sipariş etmeniz gerekir. Değiştirme cihazının yapılandırma adımları aynı kalır. 

Başarısız olan cihazdan yedeklediğiniz cihaz yapılandırma bilgilerini alın. Bu bilgileri, değiştirme cihazını yapılandırmak için kullanacaksınız.  

Değiştirme cihazını yapılandırmak için aşağıdaki adımları izleyin:

1. [Dağıtım denetim listesinde](azure-stack-edge-gpu-deploy-checklist.md)gereken bilgileri toplayın. Önceki cihaz yapılandırmasından kaydettiğiniz bilgileri kullanmanız gerekir. 
1. Başarısız olan yapılandırmayla aynı yapılandırmaya sahip yeni bir cihaz sıralayın.  Bir sipariş yerleştirmek için, Azure portal [Yeni bir Azure Stack Edge kaynağı oluşturun](azure-stack-edge-gpu-deploy-prep.md#) .
1. [Paketi](azure-stack-edge-gpu-deploy-install.md#unpack-the-device)açın, [raf takın](azure-stack-edge-gpu-deploy-install.md#rack-the-device) ve [cihazınızı bağlayın](azure-stack-edge-gpu-deploy-install.md#cable-the-device). 
1. [Cihazın yerel kullanıcı arabirimine bağlanın](azure-stack-edge-gpu-deploy-connect.md).
1. Ağı, eski cihazınız için kullandığınız IP adreslerini kullanarak yapılandırın. Bu, ortamınızda kullanılan tüm istemci makinelerde etkiyi en aza indirir. Bkz. [ağ ayarlarını yapılandırma](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).
1. Eski cihazınız ile aynı cihaz adı ve DNS etki alanını atayın. Bu, istemcilerinizin yeni cihazla konuşmak için aynı cihaz adını kullanmasını sağlar. Bkz. [cihaz ayarlarını yapılandırma](azure-stack-edge-gpu-deploy-set-up-device-update-time.md).
1. Yeni cihazdaki sertifikaları eski cihazla yaptığınız şekilde yapılandırın. Yeni cihazın yeni bir düğüm seri numarası olduğunu aklınızda bulundurun. Eski cihazda kendi sertifikalarınızı kullandıysanız yeni bir düğüm sertifikası almanız gerekecektir. Bkz. [sertifikaları yapılandırma](azure-stack-edge-gpu-deploy-configure-certificates.md).
1. Azure portal etkinleştirme anahtarını alın ve yeni cihazı etkinleştirin. Bkz. [cihazı etkinleştirme](azure-stack-edge-gpu-deploy-activate.md).

Artık eski cihazda çalıştırdığınız iş yüklerini dağıtmaya hazırsınız.

## <a name="restore-edge-cloud-shares"></a>Uç bulut paylaşımlarını geri yükleme

Cihazınızdaki uç bulut paylaşımlarındaki verileri geri yüklemek için şu adımları izleyin:

1. Daha önce başarısız olan cihazda oluşturulmuş aynı paylaşım adlarına sahip [Paylaşımlar ekleyin](azure-stack-edge-j-series-manage-shares.md#add-a-share) . Paylaşımlar oluştururken, **BLOB kapsayıcısı** ' nın **var olan seçeneği kullanacak** şekilde ayarlandığından emin olun ve ardından önceki cihazla kullanılan kapsayıcıyı seçin.
1. Önceki cihaza erişimi olan [kullanıcıları ekleyin](azure-stack-edge-j-series-manage-users.md#add-a-user) .
1. Daha önce cihazda bulunan paylaşımlarla ilişkili [depolama hesaplarını ekleyin](azure-stack-edge-j-series-manage-storage-accounts.md#add-an-edge-storage-account) . Sınır depolama hesapları oluştururken, mevcut bir kapsayıcı arasından seçim yapın ve önceki cihazda eşlenen Azure depolama hesabıyla eşleştirilmiş kapsayıcıyı işaret edin. Cihazdan önceki cihazdaki Edge depolama hesabına yazılan tüm veriler, eşlenen Azure depolama hesabındaki seçili depolama kapsayıcısına yüklendi.
1. Azure 'daki [Share verilerini yenileyin](azure-stack-edge-j-series-manage-shares.md#refresh-shares) . Bu, mevcut kapsayıcıdan tüm bulut verilerini paylaşımlara çeker.

## <a name="restore-edge-local-shares"></a>Uç yerel paylaşımları geri yükleme

Olası bir cihaz hatasına hazırlanmak için, Kubernetes veya IoT iş yüklerinizde Yerel paylaşım verilerini korumak üzere aşağıdaki yedekleme çözümlerini dağıtmış olabilirsiniz:

| Üçüncü taraf yazılım           | Çözüme başvuru                               |
|--------------------------------|---------------------------------------------------------|
| Cohesity                       | [https://www.cohesity.com/solution/cloud/azure/](https://www.cohesity.com/solution/cloud/azure/) <br> Ayrıntılar için Cohesity başvurun.          |
| Veritas                        | Ayrıntılar için VERITAS ile iletişim kurun.   |

Değiştirme aygıtı tam olarak yapılandırıldıktan sonra, cihazı yerel depolama için etkinleştirin. 

Yerel paylaşımlardaki verileri kurtarmak için şu adımları izleyin: 

1. [Cihazda Işlem yapılandırma](azure-stack-edge-gpu-deploy-configure-compute.md).
1. [Yerel bir paylaşımdan geri ekleyin](azure-stack-edge-j-series-manage-shares.md#add-a-local-share) .
1. Tercih edilen veri koruma çözümü tarafından sunulan kurtarma yordamını çalıştırın. Yukarıdaki tablodaki başvurulara bakın.

## <a name="restore-vm-files-and-folders"></a>VM dosyalarını ve klasörlerini geri yükleme

Potansiyel bir cihaz hatasına hazırlanmak için, VM 'lerdeki verileri korumak üzere aşağıdaki Yedekleme çözümlerinden birini dağıttıysanız:



| Yedekleme çözümleri        | Desteklenen işletim sistemi   | Başvuru                                                                |
|-------------------------|----------------|--------------------------------------------------------------------------|
| Azure Backup için Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı | Windows        | [MARS aracısı hakkında](/azure/backup/backup-azure-about-mars)    |
| Cohesity                | Windows, Linux | [Microsoft Azure tümleştirme, yedekleme ve kurtarma çözümü kısa](https://www.cohesity.com/solution/cloud/azure) <br>Ayrıntılar için Cohesity başvurun.                          |
| Veritas                 | Windows, Linux | Ayrıntılar için VERITAS ile iletişim kurun.                    |

Değiştirme aygıtı tam olarak yapılandırıldıktan sonra VM 'Leri daha önce kullanılan VM görüntüsü ile yeniden dağıtabilirsiniz. 

VM 'lerdeki verileri kurtarmak için şu adımları izleyin:
 
1. Cihazdaki [VM görüntüsünden BIR VM dağıtın](azure-stack-edge-gpu-deploy-virtual-machine-templates.md) . 
1. SANAL makineye tercih edilen veri koruma çözümünü yükler.
1. Tercih edilen veri koruma çözümü tarafından sunulan kurtarma yordamını çalıştırın. Yukarıdaki tablodaki başvurulara bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Pro cihazının nasıl döneceğinizi](azure-stack-edge-return-device.md)öğrenin.
