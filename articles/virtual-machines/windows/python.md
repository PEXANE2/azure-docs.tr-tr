---
title: Python'u kullanarak Azure'da bir Windows VM oluşturma ve yönetme
description: Azure'da bir Windows VM oluşturmak ve yönetmek için Python'u kullanmayı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 06/22/2017
ms.author: cynthn
ms.openlocfilehash: a85a9c28acd2d50d95159883a01b27c8ed1d2f1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461084"
---
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a>Python'u kullanarak Azure'da Windows VM'leri oluşturma ve yönetme

[Azure Sanal Makine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) birkaç destekleyici Azure kaynağına ihtiyaç duyar. Bu makalede, Python kullanarak VM kaynakları oluşturma, yönetme ve silme kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Visual Studio projesi oluşturma
> * Paketleri yükleme
> * Kimlik bilgileri oluşturma
> * Kaynak oluşturma
> * Yönetim görevlerini gerçekleştirme
> * Kaynakları silme
> * Uygulamayı çalıştırma

Bu adımları yapmak yaklaşık 20 dakika sürer.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Henüz yapmadıysanız Visual [Studio'yı](https://docs.microsoft.com/visualstudio/install/install-visual-studio)yükleyin. İş Yükleri sayfasında **Python geliştirme'yi** seçin ve sonra **Yükle'yi**tıklatın. Özette, Python 3 **64 bit (3.6.0)** tarafından otomatik olarak seçildiğini görebilirsiniz. Visual Studio'yu zaten yüklediyseniz, Visual Studio Başlatıcısı'nı kullanarak Python iş yükünü ekleyebilirsiniz.
2. Visual Studio'yı yükledikten ve kurduktan sonra **Dosya** > **Yeni** > **Projesi'ni**tıklatın.
3. **Templates** > **Python** > **Python Application'ı**tıklatın, projenin adı için *myPythonProject'e* girin, projenin konumunu seçin ve ardından **Tamam'ı**tıklatın.

## <a name="install-packages"></a>Paketleri yükleme

1. Solution Explorer'da, *myPythonProject*altında **Python Ortamları'na**sağ tıklayın ve ardından **sanal ortam ekle'yi**seçin.
2. Sanal Ortam Ekle *ekranında, env*varsayılan adını kabul , *Python 3.6 (64-bit)* temel yorumlayıcı için seçili olduğundan emin olun ve sonra **Oluştur'u**tıklatın .
3. Oluşturduğunuz *env* ortamına sağ tıklayın, **Python Paketini Yükle'yi**tıklatın, arama kutusuna *azure* girin ve sonra Enter tuşuna basın.

Çıkış pencerelerinde azure paketlerinin başarıyla yüklenmiş olduğunu görmeniz gerekir. 

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Bu adımı başlatmadan önce, etkin [dizin hizmet yöneticisi](../../active-directory/develop/howto-create-service-principal-portal.md)olduğundan emin olun. Ayrıca uygulama kimliğini, kimlik doğrulama anahtarını ve daha sonraki bir adımda gereksinim duyduğunuz kiracı kimliğini de kaydetmeniz gerekir.

1. Oluşturulan *myPythonProject.py* dosyayı açın ve uygulamanızın çalışmasını sağlamak için bu kodu ekleyin:

    ```python
    if __name__ == "__main__":
    ```

2. Gerekli kodu almak için bu ifadeleri .py dosyasının üstüne ekleyin:

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. .py dosyasında sonraki, kodda kullanılan ortak değerleri belirtmek için alma deyimlerinden sonra değişkenler ekleyin:
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    **Abonelik kimliğinizi** abonelik tanımlayıcınızla değiştirin.

4. İstekte bulunmanız gereken Active Directory kimlik bilgilerini oluşturmak için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    **Uygulama kimliği,** **kimlik doğrulama anahtarı**ve kiracı **kimliği'ni** Azure Active Directory hizmet yöneticiliği yöneticinizi oluşturduğunuzda daha önce topladığınız değerlerle değiştirin.

5. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a>Kaynak oluşturma
 
### <a name="initialize-management-clients"></a>Yönetim istemcilerini başlatma

Azure'da Python SDK'yı kullanarak kaynak oluşturmak ve yönetmek için yönetim istemcileri gereklidir. Yönetim istemcilerini oluşturmak için, .py dosyasının sonundaki **if** deyiminin altına bu kodu ekleyin:

```python
resource_group_client = ResourceManagementClient(
    credentials,
    SUBSCRIPTION_ID
)
network_client = NetworkManagementClient(
    credentials,
    SUBSCRIPTION_ID
)
compute_client = ComputeManagementClient(
    credentials,
    SUBSCRIPTION_ID
)
```

### <a name="create-the-vm-and-supporting-resources"></a>VM ve destekleyici kaynaklar oluşturma

Tüm kaynaklar bir [Kaynak grubunda](../../azure-resource-manager/management/overview.md)yer almalıdır.

1. Bir kaynak grubu oluşturmak için,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter to continue...')
    ```

[Kullanılabilirlik kümeleri,](tutorial-availability-sets.md) uygulamanız tarafından kullanılan sanal makineleri korumanızı kolaylaştırır.

1. Kullanılabilirlik kümesi oluşturmak için,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:
   
    ```python
    def create_availability_set(compute_client):
        avset_params = {
            'location': LOCATION,
            'sku': { 'name': 'Aligned' },
            'platform_fault_domain_count': 3
        }
        availability_set_result = compute_client.availability_sets.create_or_update(
            GROUP_NAME,
            'myAVSet',
            avset_params
        )
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter to continue...')
    ```

Sanal makineyle iletişim kurmak için [genel bir IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) gereklidir.

1. Sanal makine için genel bir IP adresi oluşturmak için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def create_public_ip_address(network_client):
        public_ip_addess_params = {
            'location': LOCATION,
            'public_ip_allocation_method': 'Dynamic'
        }
        creation_result = network_client.public_ip_addresses.create_or_update(
            GROUP_NAME,
            'myIPAddress',
            public_ip_addess_params
        )

        return creation_result.result()
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

Sanal bir makine, [Sanal ağın](../../virtual-network/virtual-networks-overview.md)alt ağında olmalıdır.

1. Sanal ağ oluşturmak için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def create_vnet(network_client):
        vnet_params = {
            'location': LOCATION,
            'address_space': {
                'address_prefixes': ['10.0.0.0/16']
            }
        }
        creation_result = network_client.virtual_networks.create_or_update(
            GROUP_NAME,
            'myVNet',
            vnet_params
        )
        return creation_result.result()
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

3. Sanal ağa bir alt ağ eklemek için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:
    
    ```python
    def create_subnet(network_client):
        subnet_params = {
            'address_prefix': '10.0.0.0/24'
        }
        creation_result = network_client.subnets.create_or_update(
            GROUP_NAME,
            'myVNet',
            'mySubnet',
            subnet_params
        )

        return creation_result.result()
    ```
        
4. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

Sanal makinenin sanal ağda iletişim kurabilmesi için ağ arabirimine ihtiyacı vardır.

1. Ağ arabirimi oluşturmak için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def create_nic(network_client):
        subnet_info = network_client.subnets.get(
            GROUP_NAME, 
            'myVNet', 
            'mySubnet'
        )
        publicIPAddress = network_client.public_ip_addresses.get(
            GROUP_NAME,
            'myIPAddress'
        )
        nic_params = {
            'location': LOCATION,
            'ip_configurations': [{
                'name': 'myIPConfig',
                'public_ip_address': publicIPAddress,
                'subnet': {
                    'id': subnet_info.id
                }
            }]
        }
        creation_result = network_client.network_interfaces.create_or_update(
            GROUP_NAME,
            'myNic',
            nic_params
        )

        return creation_result.result()
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

Artık tüm destekleyici kaynakları oluşturduğunuziçin, sanal bir makine oluşturabilirsiniz.

1. Sanal makineyi oluşturmak için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:
   
    ```python
    def create_vm(network_client, compute_client):  
        nic = network_client.network_interfaces.get(
            GROUP_NAME, 
            'myNic'
        )
        avset = compute_client.availability_sets.get(
            GROUP_NAME,
            'myAVSet'
        )
        vm_parameters = {
            'location': LOCATION,
            'os_profile': {
                'computer_name': VM_NAME,
                'admin_username': 'azureuser',
                'admin_password': 'Azure12345678'
            },
            'hardware_profile': {
                'vm_size': 'Standard_DS1'
            },
            'storage_profile': {
                'image_reference': {
                    'publisher': 'MicrosoftWindowsServer',
                    'offer': 'WindowsServer',
                    'sku': '2012-R2-Datacenter',
                    'version': 'latest'
                }
            },
            'network_profile': {
                'network_interfaces': [{
                    'id': nic.id
                }]
            },
            'availability_set': {
                'id': avset.id
            }
        }
        creation_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm_parameters
        )
    
        return creation_result.result()
    ```

    > [!NOTE]
    > Bu öğretici, Windows Server işletim sisteminin bir sürümünü çalıştıran sanal bir makine oluşturur. Diğer resimleri seçme hakkında daha fazla bilgi edinmek için [Windows PowerShell ve Azure CLI ile Gezin ve Azure sanal makine resimlerini seçin.](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
    > 
    > 

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

## <a name="perform-management-tasks"></a>Yönetim görevlerini gerçekleştirme

Bir sanal makinenin yaşam döngüsü boyunca, sanal makineyi başlatmak, durdurmak veya silmek gibi yönetim görevleri gerçekleştirmek isteyebilirsiniz. Ayrıca, yinelenen veya karmaşık görevleri otomatikleştirmek için kod oluşturmak isteyebilirsiniz.

### <a name="get-information-about-the-vm"></a>VM hakkında bilgi alın

1. Sanal makine hakkında bilgi almak için .py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def get_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME, expand='instanceView')
        print("hardwareProfile")
        print("   vmSize: ", vm.hardware_profile.vm_size)
        print("\nstorageProfile")
        print("  imageReference")
        print("    publisher: ", vm.storage_profile.image_reference.publisher)
        print("    offer: ", vm.storage_profile.image_reference.offer)
        print("    sku: ", vm.storage_profile.image_reference.sku)
        print("    version: ", vm.storage_profile.image_reference.version)
        print("  osDisk")
        print("    osType: ", vm.storage_profile.os_disk.os_type.value)
        print("    name: ", vm.storage_profile.os_disk.name)
        print("    createOption: ", vm.storage_profile.os_disk.create_option.value)
        print("    caching: ", vm.storage_profile.os_disk.caching.value)
        print("\nosProfile")
        print("  computerName: ", vm.os_profile.computer_name)
        print("  adminUsername: ", vm.os_profile.admin_username)
        print("  provisionVMAgent: {0}".format(vm.os_profile.windows_configuration.provision_vm_agent))
        print("  enableAutomaticUpdates: {0}".format(vm.os_profile.windows_configuration.enable_automatic_updates))
        print("\nnetworkProfile")
        for nic in vm.network_profile.network_interfaces:
            print("  networkInterface id: ", nic.id)
        print("\nvmAgent")
        print("  vmAgentVersion", vm.instance_view.vm_agent.vm_agent_version)
        print("    statuses")
        for stat in vm_result.instance_view.vm_agent.statuses:
            print("    code: ", stat.code)
            print("    displayStatus: ", stat.display_status)
            print("    message: ", stat.message)
            print("    time: ", stat.time)
        print("\ndisks");
        for disk in vm.instance_view.disks:
            print("  name: ", disk.name)
            print("  statuses")
            for stat in disk.statuses:
                print("    code: ", stat.code)
                print("    displayStatus: ", stat.display_status)
                print("    time: ", stat.time)
        print("\nVM general status")
        print("  provisioningStatus: ", vm.provisioning_state)
        print("  id: ", vm.id)
        print("  name: ", vm.name)
        print("  type: ", vm.type)
        print("  location: ", vm.location)
        print("\nVM instance status")
        for stat in vm.instance_view.statuses:
            print("  code: ", stat.code)
            print("  displayStatus: ", stat.display_status)
    ```
2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter to continue...')
    ```

### <a name="stop-the-vm"></a>VM’yi durdurma

Sanal bir makineyi durdurabilir ve tüm ayarlarını saklayabilirsiniz, ancak bunun için ücretlendirilmeye devam edebilir veya sanal bir makineyi durdurup yerini saptayabilirsiniz. Sanal bir makine devre dışı edildiğinde, onunla ilişkili tüm kaynaklar da ayrılır ve faturalandırma sona erer.

1. Sanal makineyi ayırmadan durdurmak için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    Sanal makineyi bulmak istiyorsanız, power_off aramayı bu kodla değiştirin:

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    stop_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="start-the-vm"></a>VM’yi başlatma

1. Sanal makineyi başlatmak için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    start_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="resize-the-vm"></a>VM'yi yeniden boyutlandırın

Sanal makineniz için bir boyut alırken dağıtımın birçok yönü göz önünde bulundurulmalıdır. Daha fazla bilgi için [VM boyutlarına](sizes.md)bakın.

1. Sanal makinenin boyutunu değiştirmek için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:

    ```python
    def update_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        vm.hardware_profile.vm_size = 'Standard_DS3'
        update_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm
        )

    return update_result.result()
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter to continue...')
    ```

### <a name="add-a-data-disk-to-the-vm"></a>VM’ye veri diski ekleme

Sanal makinelerde VHD olarak depolanan bir veya daha fazla [veri diski](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) olabilir.

1. Sanal makineye bir veri diski eklemek için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin: 

    ```python
    def add_datadisk(compute_client):
        disk_creation = compute_client.disks.create_or_update(
            GROUP_NAME,
            'myDataDisk1',
            {
                'location': LOCATION,
                'disk_size_gb': 1,
                'creation_data': {
                    'create_option': DiskCreateOption.empty
                }
            }
        )
        data_disk = disk_creation.result()
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        add_result = vm.storage_profile.data_disks.append({
            'lun': 1,
            'name': 'myDataDisk1',
            'create_option': DiskCreateOption.attach,
            'managed_disk': {
                'id': data_disk.id
            }
        })
        add_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME,
            VM_NAME,
            vm)

        return add_result.result()
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter to continue...')
    ```

## <a name="delete-resources"></a>Kaynakları silme

Azure'da kullanılan kaynaklar için ücretlendirildiğiniz için, artık ihtiyaç duyulmamış kaynakları silmek her zaman iyi bir uygulamadır. Sanal makineleri ve tüm destekleyici kaynakları silmek istiyorsanız, tek yapmanız gereken kaynak grubunu silmektir.

1. Kaynak grubunu ve tüm kaynakları silmek için ,.py dosyasındaki değişkenlerden sonra bu işlevi ekleyin:
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. Daha önce eklediğiniz işlevi aramak için bu kodu .py dosyasının sonundaki **if** deyiminin altına ekleyin:
   
    ```python
    delete_resources(resource_group_client)
    ```

3. *myPythonProject.py*kaydedin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

1. Konsol uygulamasını çalıştırmak için Visual Studio'da **Başlat'ı** tıklatın.

2. Her kaynağın durumu döndürülürkten sonra **Enter** tuşuna basın. Durum bilgilerinde, **Başarılı** bir sağlama durumu görmeniz gerekir. Sanal makine oluşturulduktan sonra, oluşturduğunuz tüm kaynakları silme fırsatına sahipsiniz. Kaynakları silmeye başlamak için **Enter** tuşuna basmadan önce, bunların Azure portalında oluşturulmasını doğrulamak birkaç dakikanızı alabilir. Azure portalı açıksa, yeni kaynakları görmek için bıçağı yenilemeniz gerekebilir.  

    Bu konsol uygulamasının baştan sona tamamen çalışması yaklaşık beş dakika sürer. Tüm kaynaklar ve kaynak grubu silinmeden önce uygulama nın tamamlanması birkaç dakika sürebilir.


## <a name="next-steps"></a>Sonraki adımlar

- Dağıtımla ilgili sorunlar varsa, bir sonraki [adım, Azure portalı ile Sorun Giderme kaynak grubu dağıtımlarına](../../resource-manager-troubleshoot-deployments-portal.md) bakmak olacaktır
- Azure Python [Kitaplığı](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python) hakkında daha fazla bilgi edinin

