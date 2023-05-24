MACHINES = {
  :"Prometheus" => {
              :box_name => "centos/8",
              :box_version => "0",
              :cpus => 4,
              :memory => 4096,
#              :ip_addr => '192.168.0.120',
            }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.synced_folder "/test_vm/upload", "/download", disabled: true

    config.vm.network "forwarded_port", guest: 9090, host: 9090
    config.vm.network "forwarded_port", guest: 9100, host: 9100
    config.vm.network "forwarded_port", guest: 9093, host: 9093
    config.vm.network "forwarded_port", guest: 3000, host: 3000
    

    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s

#      box.vm.network "public_network", ip: boxconfig[:ip_addr]

      box.vm.provider "virtualbox" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
      end
    end
  end
end
