IMAGE_NAME = "bento/ubuntu-20.04"

N=1

Vagrant.configure("2") do |config|
    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.56.2"
        master.vm.hostname = "kube-master-1"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "provisioning/master.yaml"
        end
    end

    (1..N).each do |i|
        config.vm.define "k8s-worker-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.56.#{i + 10}"
            node.vm.hostname = "kube-worker-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "provisioning/worker.yaml"
            end
        end
    end
end