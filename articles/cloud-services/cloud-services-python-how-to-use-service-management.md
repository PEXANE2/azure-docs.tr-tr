---
title: Hizmet Yönetimi API'sini (Python) kullanın - özellik kılavuzu
description: Python'dan ortak hizmet yönetimi görevlerini programlı olarak nasıl gerçekleştireceklerini öğrenin.
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: ''
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: 50501413a63921a9a34be1c04ed259990922b686
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70141476"
---
# <a name="use-service-management-from-python"></a>Python'un hizmet yönetimini kullanma
Bu kılavuz, Python'dan ortak hizmet yönetimi görevlerini nasıl programlı bir şekilde gerçekleştirdiğinizi gösterir. [Python için Azure SDK'daki](https://github.com/Azure/azure-sdk-for-python) **ServiceManagementService** sınıfı, [Azure portalında][management-portal]bulunan hizmet yönetimiyle ilgili işlevlerin çoğuna programlı erişimi destekler. Bulut hizmetleri, dağıtımlar, veri yönetimi hizmetleri ve sanal makineler oluşturmak, güncelleştirmek ve silmek için bu işlevi kullanabilirsiniz. Bu işlevsellik, hizmet yönetimine programlı erişim gerektiren uygulamalar oluşturmada yararlı olabilir.

## <a name="what-is-service-management"></a><a name="WhatIs"> </a>Hizmet yönetimi nedir?
Azure Hizmet Yönetimi API'si, [Azure portalı][management-portal]aracılığıyla kullanılabilen hizmet yönetimi işlevlerinin çoğuna programlı erişim sağlar. Bulut hizmetleri ve depolama hesaplarınızı yönetmek için Python için Azure SDK'yı kullanabilirsiniz.

Hizmet Yönetimi API'sini kullanmak için [bir Azure hesabı oluşturmanız](https://azure.microsoft.com/pricing/free-trial/)gerekir.

## <a name="concepts"></a><a name="Concepts"> </a>Kavramlar
Python için Azure SDK, REST API olan [Hizmet Yönetimi API'sini][svc-mgmt-rest-api]sarar. Tüm API işlemleri SSL üzerinden gerçekleştirilir ve X.509 v3 sertifikaları kullanılarak karşılıklı kimlik doğrulaması yapılır. Yönetim hizmetine Azure'da çalışan bir hizmetin içinden erişilebilir. Ayrıca, https isteği gönderebilen ve HTTPS yanıtı alabilen herhangi bir uygulamadan doğrudan Internet üzerinden erişilebilir.

## <a name="installation"></a><a name="Installation"> </a>Kurulum
Bu makalede açıklanan tüm özellikler pip `azure-servicemanagement-legacy` kullanarak yükleyebilirsiniz paket, mevcuttur. Yükleme hakkında daha fazla bilgi için (örneğin, Python'da yeniyseniz), [Bkz. Python ve Azure SDK'sı.](/azure/python/python-sdk-azure-install)

## <a name="connect-to-service-management"></a><a name="Connect"> </a>Hizmet yönetimine bağlanın
Hizmet yönetimi bitiş noktasına bağlanmak için Azure abonelik kimliğinize ve geçerli bir yönetim sertifikasına ihtiyacınız var. Abonelik kimliğinizi [Azure portalı][management-portal]üzerinden alabilirsiniz.

> [!NOTE]
> Artık Windows'da çalışırken OpenSSL ile oluşturulan sertifikaları kullanabilirsiniz. Python 2.7.4 veya daha sonra gereklidir. .pfx sertifikalarıiçin destek gelecekte kaldırılacak gibi olabileceğinden, .pfx yerine OpenSSL kullanmanızı öneririz.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Windows/Mac/Linux(OpenSSL) üzerinde yönetim sertifikaları
Yönetim sertifikanızı oluşturmak için [OpenSSL'yi](https://www.openssl.org/) kullanabilirsiniz. Biri sunucu (dosya) `.cer` ve diğeri istemci (dosya) `.pem` için olmak üzere iki sertifika oluşturmanız gerekir. Dosyayı `.pem` oluşturmak için şunları uygulayın:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Sertifikayı `.cer` oluşturmak için şunları uygulayın:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Azure sertifikaları hakkında daha fazla bilgi için Azure [Bulut Hizmetleri için Sertifikalara genel bakış bilgisine](cloud-services-certs-create.md)bakın. OpenSSL parametrelerinin tam açıklaması için [https://www.openssl.org/docs/apps/openssl.html](https://www.openssl.org/docs/apps/openssl.html), .

Bu dosyaları oluşturduktan sonra, dosyayı Azure'a yükleyin. `.cer` **Ayarlar** sekmesinde [Azure portalında][management-portal] **Yükle'yi**seçin. Dosyayı nereye `.pem` kaydettiğinize dikkat edin.

Abonelik kimliğinizi aldıktan, bir sertifika oluşturduktan `.cer` ve dosyayı Azure'a yükledikten sonra Azure yönetim bitiş noktasına bağlanın. Abonelik kimliğini ve `.pem` dosyaya giden yolu **ServiceManagementService'e**geçirerek bağlanın.

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte, `sms` bir **ServiceManagementService** nesnesidir. **ServiceManagementService** sınıfı, Azure hizmetlerini yönetmek için kullanılan birincil sınıftır.

### <a name="management-certificates-on-windows-makecert"></a>Windows'da yönetim sertifikaları (MakeCert)
Makinenizde kendi imzalı bir yönetim sertifikası `makecert.exe`oluşturabilirsiniz. **Yönetici** olarak **Visual Studio komut istemini** açın ve *AzureCertificate'u* kullanmak istediğiniz sertifika adıyla değiştirerek aşağıdaki komutu kullanın:

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Komut dosyayı `.cer` oluşturur ve **Kişisel** sertifika deposuna yükler. Daha fazla bilgi için [Azure Bulut Hizmetleri için Sertifikalara genel bakış bilgisine](cloud-services-certs-create.md)bakın.

Sertifikayı oluşturduktan sonra `.cer` dosyayı Azure'a yükleyin. **Ayarlar** sekmesinde [Azure portalında][management-portal] **Yükle'yi**seçin.

Abonelik kimliğinizi aldıktan, bir sertifika oluşturduktan `.cer` ve dosyayı Azure'a yükledikten sonra Azure yönetim bitiş noktasına bağlanın. Abonelik kimliğini ve sertifikanın konumunu **Kişisel** sertifika mağazanızdaki **ServiceManagementService'e** geçirerek bağlanın (yine *AzureCertificate'u* sertifikanızın adıyla değiştirin).

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte, `sms` bir **ServiceManagementService** nesnesidir. **ServiceManagementService** sınıfı, Azure hizmetlerini yönetmek için kullanılan birincil sınıftır.

## <a name="list-available-locations"></a><a name="ListAvailableLocations"> </a>Kullanılabilir konumları listele
Barındırma hizmetleri için kullanılabilen konumları listelemek için **\_liste konumları** yöntemini kullanın.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Bir bulut hizmeti veya depolama hizmeti oluşturduğunuzda, geçerli bir konum sağlamanız gerekir. **\_Liste konumları** yöntemi her zaman kullanılabilir konumların güncel bir listesini döndürür. Bu yazı itibariyle, kullanılabilir konumlar şunlardır:

* Batı Avrupa
* Kuzey Avrupa
* Güneydoğu Asya
* Doğu Asya
* Orta ABD
* Orta Kuzey ABD
* Orta Güney ABD
* Batı ABD
* Doğu ABD
* Doğu Japonya
* Batı Japonya
* Güney Brezilya
* Doğu Avustralya
* Güneydoğu Avustralya

## <a name="create-a-cloud-service"></a><a name="CreateCloudService"> </a>Bulut hizmeti oluşturma
Bir uygulama oluşturup Azure'da çalıştırdığınızda, kod ve yapılandırma birlikte Azure [bulut hizmeti][cloud service]olarak adlandırılır. (Önceki Azure sürümlerinde *barındırılan hizmet* olarak biliniyordu.) Yeni bir **\_barındırılan\_hizmet** oluşturmak için barındırılan hizmet yöntemini kullanabilirsiniz. Barındırılan bir hizmet adı (Azure'da benzersiz olması gereken), bir etiket (otomatik olarak base64'e kodlanmış), bir açıklama ve bir konum sağlayarak hizmeti oluşturun.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Aboneliğiniz için barındırılan tüm hizmetleri **\_barındırılan\_hizmetler yöntemiyle listeleyebilirsiniz.**

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Belirli bir barındırılan hizmet hakkında bilgi almak için, barındırılan hizmet adını **barındırılan\_\_hizmet\_özelliklerini alma** yöntemine geçirin.

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Bir bulut hizmeti oluşturduktan sonra, **dağıtım oluşturma\_** yöntemiyle kodunuzu hizmete dağıtın.

## <a name="delete-a-cloud-service"></a><a name="DeleteCloudService"> </a>Bulut hizmetini silme
Hizmet adını **silin barındırılan\_\_hizmet** yöntemine geçirerek bir bulut hizmetini silebilirsiniz.

    sms.delete_hosted_service('myhostedservice')

Bir hizmeti silmeden önce, hizmetin tüm dağıtımlarının önce silinmesi gerekir. Daha fazla bilgi için [bkz.](#DeleteDeployment)

## <a name="delete-a-deployment"></a><a name="DeleteDeployment"> </a>Dağıtımı silme
Bir dağıtımı silmek **için, sil\_dağıtım** yöntemini kullanın. Aşağıdaki örnek, adlı `v1`bir dağıtımın nasıl silinir:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="create-a-storage-service"></a><a name="CreateStorageService"> </a>Depolama hizmeti oluşturma
Depolama [hizmeti,](../storage/common/storage-create-storage-account.md) Azure [blobs,](../storage/blobs/storage-python-how-to-use-blob-storage.md) [tablolar](../cosmos-db/table-storage-how-to-use-python.md)ve [kuyruklar](../storage/queues/storage-python-how-to-use-queue-storage.md)erişim sağlar. Bir depolama hizmeti oluşturmak için hizmet için bir ad gerekir (3 ila 24 küçük karakter ve Azure içinde benzersiz). Ayrıca bir açıklama, bir etiket (en fazla 100 karakter, otomatik olarak base64 kodlanmış) ve bir konuma ihtiyacınız vardır. Aşağıdaki örnekte, bir konum belirterek bir depolama hizmetinin nasıl oluşturulutur:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Önceki örnekte, **create\_depolama\_hesabı** işleminin durumu, depolama **\_\_hesabı oluştur** tarafından döndürülen sonucu alma **\_işlemi\_durumu** yöntemine geçirerek alınabilir. 

Depolama hesaplarınızı ve özelliklerini **liste\_depolama\_hesapları** yöntemiyle listeleyebilirsiniz.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="delete-a-storage-service"></a><a name="DeleteStorageService"> </a>Depolama hizmetini silme
Bir depolama hizmetini silmek için depolama hizmeti adını **silme\_depolama\_hesabı** yöntemine geçirin. Depolama hizmetini silmek, hizmette depolanan tüm verileri (blobs, tablolar ve kuyruklar) siler.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="list-available-operating-systems"></a><a name="ListOperatingSystems"> </a>Kullanılabilir işletim sistemlerini listele
Barındırma hizmetleri için kullanılabilen işletim sistemlerini listelemek için **liste\_işletim\_sistemleri** yöntemini kullanın.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternatif olarak, işletim **\_\_sistemlerini\_** aileye göre gruplayan liste işletim sistemi aileleri yöntemini kullanabilirsiniz.

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="create-an-operating-system-image"></a><a name="CreateVMImage"> </a>İşletim sistemi görüntüsü oluşturma
Görüntü deposuna işletim sistemi görüntüsü eklemek için **os\_görüntü\_ekle** yöntemini kullanın.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Kullanılabilir işletim sistemi görüntülerini listelemek için **liste\_\_işletim sistemi görüntüleri** yöntemini kullanın. Tüm platform görüntüleri ve kullanıcı görüntüleri içerir.

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="delete-an-operating-system-image"></a><a name="DeleteVMImage"> </a>İşletim sistemi görüntüsünü silme
Bir kullanıcı görüntüsünü silmek için **silmek\_os\_görüntü** yöntemini kullanın.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="create-a-virtual-machine"></a><a name="CreateVM"> </a>Sanal bir makine oluşturma
Sanal bir makine oluşturmak için öncelikle bir [bulut hizmeti](#CreateCloudService)oluşturmanız gerekir. Ardından, **sanal\_\_makine dağıtım ını oluştur\_** yöntemini kullanarak sanal makine dağıtımını oluşturun.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="delete-a-virtual-machine"></a><a name="DeleteVM"> </a>Sanal makineyi silme
Sanal bir makineyi silmek için, önce **\_dağıtım silme** yöntemini kullanarak dağıtımı silebilirsiniz.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Bulut hizmeti daha sonra **silme\_barındırılan\_hizmet** yöntemi kullanılarak silinebilir.

    sms.delete_hosted_service(service_name='myvm')

## <a name="create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Yakalanan sanal makine görüntüsünden sanal makine oluşturma
VM görüntüsünü yakalamak için önce **yakalama\_vm\_görüntü** yöntemini çağırırsınız.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Görüntüyü başarıyla çektiğinden emin olmak için **\_vm\_görüntüleri** API listesini kullanın. Görüntünüzün sonuçlarda görüntülendiğinden emin olun.

    images = sms.list_vm_images()

Sonunda yakalanan görüntüyü kullanarak sanal makine oluşturmak için, daha önce olduğu gibi **sanal\_\_makine\_dağıtım yöntemini oluşturun,** ancak bu kez bunun yerine vm_image_name geçirin.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Klasik dağıtım modelinde bir Linux sanal makinesini nasıl yakalayacaklarınız hakkında daha fazla bilgi edinmek için [bkz.](../virtual-machines/linux/classic/capture-image-classic.md)

Klasik dağıtım modelinde Windows sanal makinesini nasıl yakalayacaklarınız hakkında daha fazla bilgi edinmek için [bkz.](../virtual-machines/windows/classic/capture-image-classic.md)

## <a name="next-steps"></a><a name="What's Next"> </a>Sonraki adımlar
Artık hizmet yönetiminin temellerini öğrendiğinize göre, [Azure Python SDK'nın Tam API başvuru belgelerine](https://azure-sdk-for-python.readthedocs.org/) erişebilir ve Python uygulamanızı yönetmek için karmaşık görevleri kolayca gerçekleştirebilirsiniz.

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/).

[What is service management?]: #WhatIs
[Concepts]: #Concepts
[Connect to service management]: #Connect
[List available locations]: #ListAvailableLocations
[Create a cloud service]: #CreateCloudService
[Delete a cloud service]: #DeleteCloudService
[Create a deployment]: #CreateDeployment
[Update a deployment]: #UpdateDeployment
[Move deployments between staging and production]: #MoveDeployments
[Delete a deployment]: #DeleteDeployment
[Create a storage service]: #CreateStorageService
[Delete a storage service]: #DeleteStorageService
[List available operating systems]: #ListOperatingSystems
[Create an operating system image]: #CreateVMImage
[Delete an operating system image]: #DeleteVMImage
[Create a virtual machine]: #CreateVM
[Delete a virtual machine]: #DeleteVM
[Next steps]: #NextSteps
[management-portal]: https://portal.azure.com/
[svc-mgmt-rest-api]: https://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/azure/cloud-services/
