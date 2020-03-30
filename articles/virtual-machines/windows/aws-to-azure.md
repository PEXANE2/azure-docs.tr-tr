---
title: Windows AWS VM'leri Azure'a taşıma
description: Bir Amazon Web Hizmetleri (AWS) EC2 Windows örneğini Azure sanal makinesine taşıyın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: 9bd01f24ac2cada02f51089d238519cd6c7e0248
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74039280"
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-an-azure-virtual-machine"></a>Windows VM'yi Amazon Web Hizmetleri'nden (AWS) Azure sanal makinesine taşıma

İş yüklerinizi barındıracak şekilde Azure sanal makinelerini değerlendiriyorsanız, varolan bir Amazon Web Hizmetleri (AWS) EC2 Windows VM örneğini dışa aktarabilir ve ardından sanal sabit diski (VHD) Azure'a yükleyebilirsiniz. VHD yüklendikten sonra, VHD'den Azure'da yeni bir VM oluşturabilirsiniz. 

Bu makale, tek bir VM'nin AWS'den Azure'a taşınmasını kapsar. VM'leri AWS'den Azure'a ölçekte taşımak istiyorsanız, [Amazon Web Hizmetleri'ndeki (AWS) sanal makineleri Azure Site Kurtarma ile Azure'a geçir'e](../../site-recovery/site-recovery-migrate-aws-to-azure.md)bakın.

## <a name="prepare-the-vm"></a>VM’yi hazırlama 
 
Hem genelleştirilmiş hem de özelleştirilmiş VHD'leri Azure'a yükleyebilirsiniz. Her tür, AWS'den dışa aktarmadan önce VM'yi hazırlamanızı gerektirir. 

- **Genelleştirilmiş VHD** - genelleştirilmiş bir VHD tüm kişisel hesap bilgilerinizi Sysprep kullanarak kaldırıldı olmuştur. VHD'yi yeni VM'ler oluşturmak için bir görüntü olarak kullanmayı planlıyorsanız, şunları yapmalısınız: 
 
    * [Windows VM hazırlayın.](prepare-for-upload-vhd-image.md)  
    * Sanal makineyi Sysprep kullanarak genelleştirin.  

 
- **Özel leştirilmiş VHD** - özel bir VHD, kullanıcı hesaplarını, uygulamaları ve diğer durum verilerini orijinal VM'inizden korur. Yeni bir VM oluşturmak için Olduğu gibi VHD'yi kullanmayı planlıyorsanız, aşağıdaki adımların tamamlandığından emin olun.  
    * [Azure'a yüklemek için bir Windows VHD hazırlayın.](prepare-for-upload-vhd-image.md) Sysprep kullanarak VM genelleme **yapmayın.** 
    * VM'de yüklü olan konuk sanallaştırma araçlarını ve aracılarını kaldırın (örneğin VMware araçları). 
    * VM'nin IP adresini ve DNS ayarlarını DHCP üzerinden çekecek şekilde yapılandırıldığından emin olun. Bu, sunucunun başlatıldığında VNet içinde bir IP adresi almasını sağlar.  


## <a name="export-and-download-the-vhd"></a>VHD'yi dışa aktarma ve indirme 

EC2 örneğini Bir Amazon S3 kovasında bir VHD'ye aktarın. Amazon dokümantasyon makalesinde [VM Alma/Dışa Aktarma kullanarak VM Örneği Verme](https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) adımlarını izleyin ve EC2 örneğini bir VHD dosyasına aktarmak için örnek [oluşturma-verme görev](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) komutunu çalıştırın. 

Dışa aktarılan VHD dosyası belirttiğiniz Amazon S3 kovasında kaydedilir. VHD'yi dışa aktarmak için temel sözdizimi aşağıdadır, yalnızca parantez içinde \<yer tutucu metni ni> bilgilerinizle değiştirin.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

VHD dışa aktarıldıktan sonra, VHD dosyasını S3 kovasından indirmek için [Bir Nesneyi S3 Kovasından Nasıl İndirebilirim?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) 

> [!IMPORTANT]
> AWS, VHD'yi indirmek için veri aktarım ücretlerini ücretlendirmez. Daha fazla bilgi için [Amazon S3 Fiyatlandırma'ya](https://aws.amazon.com/s3/pricing/) bakın.


## <a name="next-steps"></a>Sonraki adımlar

Artık VHD'yi Azure'a yükleyebilir ve yeni bir VM oluşturabilirsiniz. 

- Sysprep'i dışa aktarmadan önce **genelleştirmek** için kaynağınızda çalıştırdıysanız, [genelleştirilmiş bir VHD Yükle'ye bakın ve Azure'da yeni bir VM oluşturmak için kullanın](upload-generalized-managed.md)
- İhraç etmeden önce Sysprep'i çalıştırmadıysanız, VHD **özel**olarak kabul edilir, [bkz.](create-vm-specialized.md)

 
