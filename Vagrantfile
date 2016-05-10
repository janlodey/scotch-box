# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "scotch/box"
  config.vm.network "private_network", ip: "192.168.33.66"
  config.vm.hostname = "sandbox.dev"
  
  # Optional NFS. Make sure to remove other synced_folder line too
  config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

	config.vm.provision "shell", privileged: false, inline: <<-SHELL

	    echo "======= COMPOSER UPDATE ========="
	    sudo composer self-update 
	    echo "------- composer updated -------"

	    echo "======= INSTALL DRUPAL CONSOLE ========="
	    cd ~
	    # Install Drupal Console
      composer global require drupal/console:@stable
      echo sudo "PATH=$PATH:~/.composer/vendor/bin" >> ~/.bash_profile
	    echo "------- drupal console installed -------"

	    echo "======= INSTALL DRUSH ========="
	    # Install Drush
	    if [ ! -f "/usr/local/bin/drush" ]; then
	      wget https://github.com/drush-ops/drush/releases/download/8.0.0-rc4/drush.phar
	      chmod +x drush.phar
	      sudo mv drush.phar /usr/local/bin/drush
	      drush init
	    fi
	    echo "------- drush installed -------"

		  echo "======= INSTALL XDEBUG ========="
		  sudo apt-get update
		  sudo apt-get upgrade
		  sudo apt-get install php5-xdebug php5-dev
		  echo "------- xdebug installed -------"

	    echo "======= SET PHP CONFIG ========="
	    ## Set PHP memory limit to 1024M
	    php_memory_limit=1024M
	    php_post_max_size=2000M
	    php_upload_max_filesize=2000M
	    php_error_reporting=E_ALL
	    php_display_errors=On
	    php_display_startup_errors=On

	    sudo sed -i 's/memory_limit = .*/memory_limit = '${php_memory_limit}'/' /etc/php5/apache2/php.ini
	    sudo sed -i 's/post_max_size = .*/post_max_size = '${php_post_max_size}'/' /etc/php5/apache2/php.ini
	    sudo sed -i 's/upload_max_filesize = .*/upload_max_filesize = '${php_upload_max_filesize}'/' /etc/php5/apache2/php.ini
	    sudo sed -i 's/error_reporting = .*/error_reporting = '${php_error_reporting}'/' /etc/php5/apache2/php.ini
	    sudo sed -i 's/display_errors = .*/display_errors = '${php_display_errors}'/' /etc/php5/apache2/php.ini
	    sudo sed -i 's/display_startup_errors = .*/display_startup_errors = '${php_display_startup_errors}'/' /etc/php5/apache2/php.ini

		  echo "------- finished php config -------"
  
	    echo "Let's restart apache..."
	    sudo service apache2 restart

	    echo "BOOM... you should be ready to go"

	  SHELL

end