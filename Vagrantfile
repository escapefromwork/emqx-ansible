ENV["LC_ALL"] = "en_US.UTF-8"

# emqx variables
EMQX_COUNT = 3
EMQX_CPU = 1
EMQX_MEMORY = 700
EMQX_NETWORK = "192.168.63.1"

# haproxy variables
HAPROXY_COUNT = 1
HAPROXY_CPU = 1
HAPROXY_MEMORY = 512
HAPROXY_NETWORK = "192.168.63.2"

# Add something here, if you want to modify the init script
$init_script = <<SCRIPT
    sudo apt update --fix-missing
SCRIPT

Vagrant.configure("2") do |config|
    config.vm.box = "debian/bookworm64"

    (1..EMQX_COUNT).each do |i|
        config.vm.define "emqx-#{i}" do |emqx|
            emqx.vm.hostname = "#{i}-emqx-internal"
            emqx.vm.provider "virtualbox" do |vbox|
                vbox.cpus = EMQX_CPU
                vbox.memory = EMQX_MEMORY
            end
            emqx.vm.network :private_network, ip: "#{EMQX_NETWORK}#{i}", auto_config: true
            emqx.vm.provision "shell", inline: $init_script
        end
    end
end

Vagrant.configure("2") do |config|
    config.vm.box = "debian/bookworm64"

    (1..HAPROXY_COUNT).each do |i|
        config.vm.define "haproxy-#{i}" do |haproxy|
            haproxy.vm.hostname = "haproxy-#{i}"
            haproxy.vm.provider "virtualbox" do |vbox|
                vbox.cpus = HAPROXY_CPU
                vbox.memory = HAPROXY_MEMORY
            end
            haproxy.vm.network :private_network, ip: "#{HAPROXY_NETWORK}#{i}", auto_config: true
            haproxy.vm.provision "shell", inline: $init_script
        end
    end
end